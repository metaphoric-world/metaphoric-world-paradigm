### Appendix 4 - The Semantic Passkey - Technical Implementation

Overview

This appendix describes the technical architecture of the Semantic Passkey: an authentication system that identifies a person not through biometric signals or stored secrets, but through their individual pattern of meaning-formation - their semantic trajectory through the 27 form-regions of the field.

The appendix is structured in two parts:

Part I - Core Architecture - describes the modality-independent components that remain constant regardless of input type.

Part II - Worked Example - develops a complete implementation for a specific scenario: audio input, medium security level, online login.

Design decision points are explicitly marked throughout. Where multiple approaches are viable, each is described with its tradeoffs.

---

Part I - Core Architecture

### 1. - Foundational Concept

Every utterance, text, or expressive act can be projected into the 27-region form-space. This projection produces a form-region profile: a 27-dimensional vector whose components represent the activation strength of each region for that input.

```
Input → [Channel Extraction] → [Per-Channel Projection] → 27-dim profile
```

A single input produces one profile. A sequence of inputs produces a trajectory: an ordered series of profiles in the 27-dimensional space.

The semantic signature of a person is not a point in this space. It is a characteristic trajectory pattern - the way their profiles move, cluster, and co-vary across inputs and across channels.

Authentication asks: *Does this new trajectory match the enrolled pattern?*

### 2. - The Three-channel model

Each input is analyzed along up to three independent channels:

| Channel | What it captures | Required for |
|---------|-----------------|--------------|
| **Content** | Semantic field — what conceptual regions are activated | All modalities |
| **Style** | Expressive structure — how distinctions are drawn and organized | Text and audio |
| **Prosody** | Temporal-dynamic signal — rhythm, emphasis, pace | Audio only |

The channels are structurally independent: they tap different aspects of the same act of meaning-formation. Their co-movement across a sequence of inputs is the primary authentication signal.

> *Design decision 1 — Modality selection*
>
> The channel set depends on the input modality:
>
> - Audio: all three channels available → richest signature
> - Text: content + style only → compensated by targeted prompting (see below)
> - Other modalities (gesture, drawing, symbolic input): content channel must be re-defined per modality; style and prosody analogues may exist but require separate specification
>
> The core architecture is identical across modalities. Only the extraction layer changes.

### 3. - Projection onto Form-Regions

Each channel produces a raw feature vector. This must be projected onto the 27-region space to produce a form-region profile.

```
raw_vector_channel → [Projection Function] → profile ∈ R^27
```

The profile components are non-negative and sum to 1 (a probability distribution over regions).

> *Design decision 2 — Projection method*
>
> Option A - Trained classifier:
> A supervised model is trained to map feature vectors to form-region activations. Requires labeled training data. More accurate but depends on quality of labels.
>
> Option B - Unsupervised embedding alignment:
> An embedding model is used; its dimensions are clustered and aligned to form-regions via unsupervised methods (e.g., k-means with 27 clusters, then region assignment). No labeled data required but alignment to theoretical regions is approximate.
>
> Option C - Rule-based projection:
> Explicit mapping rules derived from the theoretical framework. Most interpretable, least dependent on data, but requires careful manual design.
>
> For the worked example below, Option B is used as the most practical starting point.

### 4. - Trajectory formation

Over a sequence of N inputs, each channel produces N profiles. The result is a trajectory per channel:

```
Content trajectory:  [p_c1, p_c2, ..., p_cN]   each p_ci ∈ R^27
Style trajectory:    [p_s1, p_s2, ..., p_sN]   each p_si ∈ R^27
Prosody trajectory:  [p_r1, p_r2, ..., p_rN]   each p_ri ∈ R^27
```

Together these form a trajectory in the 3×27-dimensional product space.

The **signature** is not a single point but a statistical model of this trajectory:

```
Signature = {
    μ_c, σ_c,          # mean and variance of content trajectory
    μ_s, σ_s,          # mean and variance of style trajectory
    μ_r, σ_r,          # mean and variance of prosody trajectory
    Cov(c,s),          # co-movement between content and style
    Cov(c,r),          # co-movement between content and prosody
    Cov(s,r),          # co-movement between style and prosody
    temporal_pattern   # characteristic ordering / transition pattern
}
```

The covariance terms capture the co-movement between channels - this is the core authentication signal that is hardest to falsify.

Temporal_pattern captures the characteristic transition sequences between form-regions - not where the trajectory goes on average, but how it tends to move from one input to the next.

### 5. - Authentication algorithm

Given an enrolled signature S and a new input sequence producing trajectory T_new:

```
FUNCTION authenticate(T_new, S, security_level):

    # Step 1: Extract per-channel trajectories from T_new
    T_content, T_style, T_prosody = extract_channels(T_new)

    # Step 2: Compute distance metrics
    d_content  = trajectory_distance(T_content, S.μ_c, S.σ_c)
    d_style    = trajectory_distance(T_style, S.μ_s, S.σ_s)
    d_prosody  = trajectory_distance(T_prosody, S.μ_r, S.σ_r)
    d_covar    = covariance_distance(T_new, S.Cov)

    # Step 3: Weighted combination
    d_total = α * d_content + β * d_style + γ * d_prosody + δ * d_covar

    # Step 4: Compare to threshold
    τ = threshold(security_level)

    IF d_total < τ:
        RETURN accept
    ELSE IF d_total < τ * extension_factor:
        REQUEST additional_input
        # recurse with extended sequence
    ELSE:
        RETURN reject
```

> *Design decision 3 — Distance metric*
>
> Option A - Mahalanobis distance: accounts for variance structure; well-suited when σ is well-estimated.
>
> Option B - Dynamic Time Warping (DTW): handles temporal variation in sequence length; useful when input pace varies.
>
> Option C - Learned similarity (Siamese network): most powerful but requires training data pairs.
>
> For medium security, Option A is recommended as a robust baseline.

> *Design decision 4 — Channel weights (α, β, γ, δ)*
>
> Fixed weights: simple, transparent, auditable.
>
> Adaptive weights: adjusted per user based on which channels show highest individual stability. More accurate but more complex.
>
> Security-level dependent: higher security levels increase δ (covariance weight), making co-movement the dominant factor.

### 6. - Security level and sequence length

| Security Level | Training inputs | Auth inputs | Primary use case |
|---------------|-----------------|-------------|-----------------|
| Low | 10–20 | 1–2 | Device unlock |
| Medium | 20–40 | 3–5 | Online login |
| High | 50+ | 8–12 | Institutional / governmental |

At any level, the system may request additional inputs if d_total falls in the ambiguous zone.

### 7. - Drift management

Semantic style evolves slowly over time. The signature must adapt without becoming vulnerable.

```
FUNCTION update_signature(S, T_new, drift_params):

    d_shift = distance(T_new.μ, S.μ)
    d_var   = change_in_variance(T_new.σ, S.σ)

    IF d_shift < drift_params.slow_threshold AND d_var stable:
        # Natural drift: integrate smoothly
        S.μ = (1 - η) * S.μ + η * T_new.μ
        S.σ = update_variance(S.σ, T_new.σ)

    ELSE IF d_shift > drift_params.fast_threshold OR d_var unstable:
        # Anomaly: flag, do not integrate
        FLAG anomaly
        REQUEST additional_verification

    ELSE:
        # Ambiguous: hold, accumulate evidence
        HOLD pending further observations
```

> *Design decision 5 - learning rate η*
>
> A low η (0.01–0.05) makes the signature stable but slow to adapt. A higher η (0.1–0.2) adapts faster but is more vulnerable to gradual impersonation. Security-critical deployments should use low η with periodic manual re-enrollment triggers.

### 8. - Text-modality compensation

Without prosody, the text modality loses one channel. This is compensated by targeted prompting:

The system generates questions designed to activate specific form-regions, effectively guiding the trajectory through the space rather than passively observing it.

```
prompt_strategy = {
    low_security:    free_response(1 question),
    medium_security: targeted_sequence(3–5 questions, 
                        covering different form-region clusters),
    high_security:   adaptive_sequence(dynamically generated 
                        based on partial trajectory so far)
}
```

The adaptive sequence is the most powerful: it observes where the trajectory has gone so far, identifies under-sampled regions, and generates prompts likely to activate them - producing a richer trajectory from fewer inputs.

> *Design decision 6 - User identification prior to matching**
>
> A fundamental architectural choice concerns whether the system requires the user to identify themselves before authentication begins.
>
> Option A - Username + Signature (Classical Model)
>
> The user provides a username (or equivalent identifier) before the authentication sequence. The system retrieves the corresponding signature and matches only against it.
>
> ```
> user provides: username → system retrieves: 
> S_username
> authentication: match(T_new, S_username)
> ```
>
> Advantages: fast, scalable to arbitrary user 
> numbers, simple implementation.
>
> Tradeoffs: username and signature become separate attack surfaces. Username must be provided in potentially public contexts (spoken aloud, typed visibly).
>
> Option B - Signature-only (identifier-free model)
>
> No identifier is provided. The system matches the incoming trajectory against all stored signatures and returns the best fit above threshold.
>
> ```
> authentication: best_match(T_new, {S_1, S_2, ...,
> S_n})
> → if best_match.distance < τ: identify and 
> authenticate
> → if ambiguous: request additional input
>```
>
> Advantages: conceptually consistent with the framework - identity is in the signature, not a label attached to it. No public disclosure of identity required.
>
> Tradeoffs: computationally expensive at scale. Requires careful handling of near-ties between similar signatures.
>
> Option C - Anonymous pre-filtering (Hybrid model)
>
> A short initial input - too brief for full authentication - is used to narrow the candidate set to a small number of signatures. Full authentication then proceeds against this reduced set only.
>
> ```
> pre-filter input → candidate_set = 
> top_k_matches(T_brief, all_signatures)
> full authentication: match(T_new, candidate_set)
> ```
>
> Advantages: scalable without requiring explicit identification. Preserves anonymity while keeping matching tractable.
>
> Tradeoffs: requires careful calibration of the pre-filter threshold to avoid excluding the genuine user from the candidate set. Adds one processing step.
>
> For most deployment contexts in the introduction phase of the technology, Option A is the pragmatic baseline. Option C is recommended where anonymity is a design requirement or where spoken usernames are contextually problematic.

---

Part II - Worked example

Scenario: Online login via audio input, medium security level.

User: has completed enrollment (30 audio samples during registration).

Task: authenticate with 3–5 spoken responses.

### Step 1 - Enrollment (done prior)

User spoke responses to 30 open questions. For each response:

```
audio_file → ASR → text
text       → embedding_model → v_content ∈ R^768
text       → stylometric_extractor → v_style ∈ R^42
audio_file → prosody_extractor → v_prosody ∈ R^18

v_content → projection_B → p_content ∈ R^27
v_style   → projection_B → p_style   ∈ R^27
v_prosody → projection_B → p_prosody ∈ R^27
```

After 30 samples:

```
S = compute_signature(
    [p_content_1..30],
    [p_style_1..30],
    [p_prosody_1..30]
)
```

Signature stored locally on device (not on server).

### Step 2 - Authentication request

User attempts login. System presents 3 open prompts:

```
Prompt 1: "Describe something you find genuinely interesting right now."
Prompt 2: "What does a good decision feel like to you?"
Prompt 3: "Tell me about a place that matters to you."
```

Questions are chosen to activate different form-region clusters (evaluative/relational, process-oriented, identity-anchored).

### Step 3 - Processing

For each of the 3 responses:

```
FOR each response_i IN [response_1, response_2, response_3]:
    text_i     = ASR(audio_i)
    v_c_i      = embed(text_i)
    v_s_i      = stylometry(text_i)
    v_r_i      = prosody(audio_i)

    p_c_i      = project(v_c_i)
    p_s_i      = project(v_s_i)
    p_r_i      = project(v_r_i)

T_new = {
    content:  [p_c_1, p_c_2, p_c_3],
    style:    [p_s_1, p_s_2, p_s_3],
    prosody:  [p_r_1, p_r_2, p_r_3]
}
```

### Step 4 - Distance computation

```
d_content = mahalanobis(mean(T_new.content), S.μ_c, S.σ_c)
d_style   = mahalanobis(mean(T_new.style),   S.μ_s, S.σ_s)
d_prosody = mahalanobis(mean(T_new.prosody), S.μ_r, S.σ_r)
d_covar   = covariance_distance(T_new, S.Cov)

Medium security weights
d_total = 0.30 * d_content + 0.25 * d_style + 
          0.20 * d_prosody + 0.25 * d_covar
```

### Step 5 - Decision

```
τ_medium = 0.45   # example threshold, to be calibrated empirically

IF d_total < τ_medium:
    → LOGIN GRANTED
    → update_signature(S, T_new, η=0.03)

ELSE IF d_total < τ_medium * 1.4:
    → REQUEST one additional response
    → recompute with 4 inputs

ELSE:
    → LOGIN DENIED
    → flag for review if repeated
```

### Step 6 - Post-authentication

If accepted:
- Signature updated with low learning rate
- No raw audio retained
- Only updated μ, σ, Cov stored

If denied:
- No update
- Attempt logged (locally)
- After N failed attempts: fallback to alternative verification

### Summary of Design Decisions

| # | Decision | Options | Recommended baseline |
|---|----------|---------|---------------------|
| 1 | Modality | Audio / Text / Other | Audio for richest signature |
| 2 | Projection method | Trained / Unsupervised / Rule-based | Unsupervised (Option B) |
| 3 | Distance metric | Mahalanobis / DTW / Siamese | Mahalanobis for medium security |
| 4 | Channel weights | Fixed / Adaptive / Level-dependent | Fixed with δ elevated for high security |
| 5 | Learning rate η | 0.01–0.2 | 0.03 for medium security |

---

### Open questions for future development

1. Calibration of thresholds: τ values must be empirically determined through testing across diverse speaker populations. The theoretical architecture does not constrain these values.

2. Cross-lingual stability: whether form-region profiles are stable across languages for the same person is an open empirical question with significant implications for multilingual deployments.

3. Minimum viable enrollment: the minimum number of enrollment samples needed for reliable authentication at each security level requires empirical determination.

4. Adversarial robustness: systematic testing against adversaries with access to the enrolled signature (but not raw data) should be conducted before deployment in high-security contexts.

5. Projection validation: the central empirical question to what degree the projection onto form-regions captures individual meaning-formation patterns (rather than generic stylistic variation) requires dedicated validation study.

---
