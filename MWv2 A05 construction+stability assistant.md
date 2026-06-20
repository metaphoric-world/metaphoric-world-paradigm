
### Appendix 5 - A qualitative construction & stability assistant 

This Appendix (as well as Appendix 6) sketches an application example for the implementation architecture of the Metaphoric World (Chapter 25).

1. Motivation

Everyday life is full of improvised or semi‑improvised constructions:
- garden walkways  
- small bridges  
- pergolas and trellises  
- children's play structures  
- temporary shelters  
- ad‑hoc repairs  
- makeshift load‑bearing assemblies  

These structures are rarely documented, rarely inspected, and often built by people who lack the vocabulary to describe what they have created - let alone to assess its stability.

At the same time, professional engineers face a communication gap:  
they are often approached too late, or with incomplete information, or with a sense of embarrassment ("I built something… can you check if it's safe?").

A qualitative, topological assessment tool can bridge this gap.

2. Purpose of the system

The goal is not to replace structural engineering.  
Instead, the system provides:
- a qualitative stability assessment  
- a topological understanding of the construction  
- a structured documentation package for professionals  
- a low‑threshold interface for non‑experts  
- a shared language between DIY practice and engineering practice  

The system avoids numerical claims, legal implications, and normative requirements.  
It operates entirely within the qualitative domain of the Metaphoric World (MW).

3. MW Foundations

The assistant is built on the Form‑Region architecture of the MW - a 3×3×3 semantic‑topological field that classifies structural elements not by numbers, but by roles, relations, and compatibilities.

3.1 - Form‑Regions used in construction analysis

- Material Form‑Regions  
  (anisotropy, brittleness, ductility, moisture sensitivity, etc.)

- Geometric Form‑Regions  
  (beam, plate, column, shell, cable, frame)

- Coupling Form‑Regions  
  (nails, screws, knots, clamps, welds, friction joints)

- System Form‑Regions  
  (truss, beam‑and‑post, hanging system, frame system)

- Context Form‑Regions  
  (soil, anchoring, boundary conditions, environmental loads)

These regions define the roles that elements play within the structure.

4. Why a nonlinear model is appropriate for "statics"

At first glance, it may seem counterintuitive to use a model for nonlinear systems in a domain traditionally associated with linear statics.  
However, this intuition only applies to professional engineering contexts.

4.1 - Professional statics is linearized by design

Engineering statics assumes:
- standardized materials  
- controlled geometries  
- known load cases  
- well‑defined couplings  
- safety factors that absorb uncertainties  

This is a linearized world.

4.2 - Improvised constructions are not linear systems

DIY structures exhibit:
- unknown material properties  
- irregular geometries  
- inconsistent couplings  
- variable environmental loads  
- aging, moisture, and wear  
- asymmetric supports  
- mixed materials  
- "it holds because it holds" logic  

This is a nonlinear system.

4.3 - MW is the correct model for this domain

The MW provides:
- K‑coherence instead of "stability"
- Drift‑fields instead of "uncertainty margins"  
- Attractor basins instead of "equilibrium states"  
- Catastrophe patterns instead of "failure modes"  
- qualitative Alpha‑coupling instead of "load propagation equations"

Thus, the MW is not a replacement for engineering statics - it is the correct qualitative model for nonlinear, improvised, real‑world constructions.

5. Technical architecture (Qualitative)

The system consists of five interacting modules:

5.1 - Form‑Region Extraction (Computer Vision)

The app identifies:
- geometric primitives (beams, plates, cables)  
- material categories (wood, metal, rope, plastic)  
- coupling types (nails, screws, knots, clamps)  
- boundary conditions (ground contact, wall attachment, suspension)

This produces a Form‑Region map of the structure.

> Real‑world video data - even low‑quality, single‑perspective footage - is surprisingly rich in the qualitative patterns that the MW requires.  
> Catastrophe forms, drift behaviour, coupling failures and material‑specific break patterns remain visually identifiable even under poor imaging conditions.  
> A training corpus of improvised construction failures therefore provides exactly the kind of nonlinear, qualitative information that a Form‑Region‑based system needs.

5.2 - System‑FR Matching

The structure is matched against known System Form‑Regions, such as:
- truss‑like  
- beam‑and‑post  
- hanging system  
- frame system  
- hybrid systems  

This provides the topological skeleton of the construction.

5.3 - K‑Coherence Analysis

The system evaluates:
- compatibility of roles  
- alignment of load paths  
- redundancy vs. fragility  
- presence of competing roles  
- coherence of the overall configuration  

Output: a K‑profile showing coherent and incoherent zones.

5.4 - Drift‑Field Analysis

Drift represents:
- uncertainty  
- variability  
- environmental sensitivity  
- material degradation  
- coupling looseness  
- geometric irregularity  

The app identifies Drift‑zones, which correspond to areas where the structure is likely to behave unpredictably.

5.5 - Qualitative Alpha‑Coupling

Alpha is not used numerically.  
Instead, the system classifies coupling strength qualitatively:
- local (failure remains confined)  
- regional (failure propagates to a subsystem)  
- global (failure destabilizes the entire structure)

This identifies Alpha‑hotspots - points where local issues can become systemic.

6. Attractors and Catastrophes

Improvised constructions often sit in shallow attractor basins.  
Small perturbations can cause:
- tipping (fold catastrophe)  
- buckling (pitchfork catastrophe)  
- tearing or pull‑out (cusp catastrophe)  
- cable‑slip or knot failure (swallowtail catastrophe)

The app recognizes these qualitative catastrophe patterns and highlights them visually.

7. Output

The system does not provide:
- load capacities  
- numerical safety factors  
- normative compliance  
- legal statements  

Instead, it provides qualitative categories:
- "Suitable for decorative use"
- "Suitable for light loads" 
- "Not suitable for human load" 
- "High instability likelihood"
- "Professional assessment recommended"

Along with:
- Form‑Region map  
- K‑coherence profile  
- Drift‑field visualization  
- Alpha‑hotspot map  
- Catastrophe pattern indicators  
- A structured report for engineers  

8. Societal Value

For non‑experts
- low‑threshold safety awareness  
- no embarrassment  
- intuitive visualizations  
- better self‑assessment  

For professionals
- structured pre‑documentation  
- reduced ambiguity  
- faster assessments  
- new client groups  

For society
- fewer dangerous DIY structures  
- improved communication between laypeople and experts  
- increased technical literacy  
- reduced regulatory burden  

9. Summary

This example demonstrates that the MW is not limited to academic or ecological contexts.  
It can be applied to everyday, nonlinear, improvised systems in a way that is:
- technically plausible  
- conceptually rigorous  
- socially useful  
- qualitatively grounded  
- and fully MW‑consistent  

---
