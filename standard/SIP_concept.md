
Whitepaper

Der semantische Farbraum als persönlicher Passkey

Implementierungsarchitektur, mathematische Struktur, Trainingsverfahren und Sicherheitsdesign

---

1 Einleitung

Digitale Authentifizierungssysteme stehen in einem Spannungsfeld:

- Sie sollen sicher sein.  
- Sie sollen benutzbar sein.  
- Sie sollen möglichst wenig in die Privatsphäre eingreifen.  

Klassische Ansätze:

- Passwörter: vergesslich, unsicher, stressig.  
- Token: verlierbar, manipulierbar.  
- Biometrie: invasiv, nicht widerrufbar.  

Dieses Dokument beschreibt ein alternatives Paradigma:

Authentifizierung über individuelle Bedeutungsbildung, operationalisiert durch den semantischen Farbraum mit 27 Konzeptklassen.

Der Kern:

> Nicht „Was weißt du?“ oder „Wie siehst du aus?“,  
> sondern: „Wie formst du Bedeutung?“

---

2 Theoretische Grundlage

2.1 Zahlen als Formprinzipien

Das Modell basiert auf der Idee, dass bestimmte Zahlen strukturelle Eigenschaften von Formen beschreiben:

`
2  → Binarität, Grenze, Entweder-Oder
3  → Vermittlung, Übergang, Dynamik
pi → Symmetrie, Kreis, geschlossene Form
phi → Selbstähnlichkeit, Proportion, emergente Ordnung
e  → Wachstum, Kontinuum, Grenzprozesse
c  → Grenzrate, maximale Geschwindigkeit
`

Diese Zahlen werden nicht als „magisch“, sondern als abstrakte Formoperatoren verstanden.

2.2 Vom Formprinzip zum Farbraum

Aus diesen Prinzipien wird ein minimaler, aber vollständiger semantischer Raum konstruiert:

- 3 Achsen (drei grundlegende Formdomänen)  
- 3 Stufen pro Achse (diskrete Approximation von Kontinua)  
- 3 x 3 x 3 = 27 Konzeptklassen  

Der Raum ist:

- klein genug, um interpretierbar zu bleiben,  
- groß genug, um komplexe Bedeutungsunterschiede abzubilden.

---

3 Struktur des semantischen Farbraums

3.1 Tensorstruktur

Der Farbraum wird als Tensor beschrieben:

`
Tensor T: 3 x 3 x 3
Konzeptklasse C(i,j,k)
i, j, k ∈ {0, 1, 2}
`

Jede Kombination (i, j, k) entspricht einer Konzeptklasse.

3.2 Semantische Interpretation der Achsen

Eine mögliche Achsenzuordnung:

- Achse A: Symmetriegrad / Formgeschlossenheit  
- Achse B: Musterhaftigkeit / Selbstähnlichkeit  
- Achse C: Prozesshaftigkeit / Dynamik / Grenzverhalten  

Stufenbeispiel pro Achse:

`
0 → niedrig / lokal / statisch
1 → mittel / meso / intermediär
2 → hoch / global / dynamisch
`

Die genaue Benennung kann domänenspezifisch angepasst werden, die Struktur bleibt gleich.

---

4 Datenrepräsentation und Eingabe

4.1 Unterstützte Eingabetypen

Der Passkey kann auf zwei primäre Eingabeformen angewendet werden:

1. Geschriebener Text  
2. Gesprochene Sprache (Audio)

Beide werden in eine gemeinsame semantische Repräsentation überführt.

4.2 Normalisierung

Vorverarbeitungsschritte:

- bei Text:
  - Tokenisierung  
  - Normalisierung (Kleinbuchstaben, Entfernen von Artefakten, optional Lemmatierung)  

- bei Audio:
  - Spracherkennung (ASR) → Text  
  - optional: parallele Extraktion prosodischer Merkmale (Tonhöhe, Rhythmus, Pausen)

---

5 Embedding und Projektion in den Farbraum

5.1 Semantisches Embedding

Ein Sprachmodell (z. B. Transformer) erzeugt einen Vektor:

`
v ∈ R^n
`

Typische Dimensionen:

`
n = 256, 512 oder 768
`

Dieser Vektor repräsentiert den Inhalt und Stil der Eingabe.

5.2 Lineare Projektion

Der Vektor v wird in den 3D‑Farbraum projiziert:

`
x = W * v + b
`

mit:

- W: Matrix der Größe 3 x n  
- b: Bias‑Vektor der Länge 3  
- x: Vektor (x1, x2, x3)

x ist ein Punkt im semantischen Farbraum.

---

6 Diskretisierung in 27 Konzeptklassen

6.1 Schwellenbasierte Diskretisierung

Für jede Dimension x_d (d = 1, 2, 3) werden zwei Schwellenwerte definiert:

`
Schwelle1_d
Schwelle2_d
`

Diskretisierung:

`
Wenn xd < Schwelle1d → 0
Wenn Schwelle1d <= xd < Schwelle2_d → 1
Wenn xd >= Schwelle2d → 2
`

Ergebnis:

`
(i, j, k)
`

6.2 Konzeptklassen‑Label

Eine Label‑Funktion ordnet jeder Kombination (i, j, k) eine semantische Beschreibung zu:

Beispiel:

`
C(0,0,0) → "niedrig symmetrisch, wenig musterhaft, statisch"
C(2,2,2) → "hoch symmetrisch, stark musterhaft, hochdynamisch"
C(2,0,1) → "hoch symmetrisch, wenig musterhaft, mittlere Prozesshaftigkeit"
`

Diese Labels sind interpretierbar, aber für die Authentifizierung nicht zwingend erforderlich – sie dienen der Diagnose und Visualisierung.

---

7 Erweiterte Merkmalsräume

Der Farbraum bleibt 3D.  
Die Eingabe wird jedoch um zusätzliche Merkmale erweitert, die ebenfalls in diesen Raum projiziert werden.

7.1 Stilometrische Merkmale

Beispiele:

- durchschnittliche Satzlänge  
- Varianz der Satzlänge  
- Häufigkeit von Metaphern  
- lexikalische Diversität  
- syntaktische Tiefe  
- Präferenz für bestimmte Strukturen (z. B. Nebensätze, Einschübe)

Diese Merkmale werden zu einem erweiterten Vektor kombiniert:

`
v_ext ∈ R^m
`

und ebenfalls über eine Matrix W_ext in den Farbraum projiziert:

`
xstyle = Wext * vext + bext
`

7.2 Prosodische Merkmale

Bei Audio können zusätzlich extrahiert werden:

- Tonhöhenverlauf  
- Betonungsmuster  
- Sprechtempo  
- Pausenstruktur  

Auch diese werden in einen Vektor überführt und in den Farbraum projiziert:

`
xprosody = Wprosody * vprosody + bprosody
`

7.3 Fusion der Merkmale

Die verschiedenen Projektionen können kombiniert werden, z. B.:

`
xtotal = α  xsemantik + β  xstyle + γ * xprosody
`

mit:

- α, β, γ als Gewichtungsfaktoren.

---

8 Individuelle Signatur im Farbraum

8.1 Signatur als Punktwolke

Eine Person erzeugt über viele Interaktionen eine Menge von Punkten:

`
S = {x1, x2, ..., xm}
`

Diese Punktwolke bildet die semantische Signatur der Person.

8.2 Statistische Kennwerte

Aus S werden berechnet:

`
Zentrum μ (Mittelwert)
Varianz σ (pro Dimension)
Toleranzfenster τ
Driftparameter δ
`

Beispiel:

`
μ = (μ1, μ2, μ3)
σ = (σ1, σ2, σ3)
τ = f(σ, gewünschte Sicherheit)
`

---

9 Trainingsverfahren für den persönlichen Schlüssel

Das ist der Teil, den du explizit angesprochen hast:  
Wie trainiert man den Schlüssel?

9.1 Ziel des Trainings

Ziel:

- eine stabile, individuelle Signatur im Farbraum zu erzeugen,  
- die robust gegen Rauschen ist,  
- aber sensibel für echte Identitätsabweichungen.

9.2 Phase 1 – Initiales Einschwingen

In dieser Phase wird der Schlüssel „eingelernt“.

Ablauf:

1. Nutzer interagiert mehrfach mit dem System (z. B. 20–50 kurze Sätze oder Sprachproben).  
2. Jede Probe wird in den Farbraum projiziert → x_i.  
3. Alle xi werden gesammelt → Sinitial.  
4. Aus Sinitial werden μinitial und σ_initial berechnet.  

Ergebnis:

`
μ_initial = erste Schätzung der Signatur
σ_initial = erste Schätzung der Varianz
τ_initial = initiales Toleranzfenster
`

9.3 Phase 2 – Adaptives Verfeinern

Nach der Initialphase wird der Schlüssel kontinuierlich verfeinert.

Prinzip:

- Jede erfolgreiche Authentifizierung liefert neue Punkte x_neu.  
- Diese werden in die Signatur integriert.  

Update‑Regel (vereinfacht):

`
μneu = (1 - η)  μalt + η  x_neu
σ_neu = aktualisiert auf Basis der neuen Punkte
`

mit:

- η = Lernrate (z. B. 0.01 bis 0.1)

9.4 Drift‑Management

Sprache und Stil verändern sich über Zeit.

Drift‑Erkennung:

- Wenn sich μ langsam verschiebt, aber σ stabil bleibt → natürliche Entwicklung.  
- Wenn sich μ sprunghaft verschiebt oder σ stark ansteigt → mögliche Anomalie.

Strategie:

- langsame Drift wird akzeptiert und integriert.  
- starke Drift führt zu:
  - Rückfragen,  
  - zusätzlicher Verifikation,  
  - oder temporärer Sperre.

9.5 Rauschtoleranz

Rauschen (Müdigkeit, Stress, Alkohol) wird über τ abgefangen:

- τ wird so gewählt, dass normale Schwankungen akzeptiert werden.  
- τ kann adaptiv sein, z. B. abhängig von Tageszeit oder Kontext.  

Optional:

- Mehrere Proben in Folge verlangen und mitteln.

---

10 Authentifizierungsalgorithmus

10.1 Distanzbasierte Methode

Für eine neue Probe x_neu:

1. Berechne Distanz zu μ:

`
d = Distanz(x_neu, μ)
`

z. B. euklidisch oder Mahalanobis‑Distanz.

2. Vergleiche mit Toleranz:

`
Wenn d < τ → akzeptieren
Wenn d ≥ τ → Rückfrage oder Ablehnung
`

10.2 Siamese‑Netz‑Methode

Alternative:

- Ein neuronales Netz lernt direkt eine Ähnlichkeitsfunktion zwischen zwei Proben.  

Input:

`
(xref, xneu)
`

Output:

`
Similarity-Wert zwischen 0 und 1
`

Entscheidung:

`
Wenn Similarity > Schwelle → akzeptieren
sonst → ablehnen
`

---

11 Sicherheitsarchitektur

11.1 Daten, die nicht gespeichert werden

Das System speichert explizit nicht:

- Roh‑Audio  
- Roh‑Text  
- biometrische Merkmale  
- Metadaten wie Ort, Zeit, Gerät (optional konfigurierbar)

Stattdessen:

- nur abstrakte Strukturparameter (μ, σ, ggf. Modellparameter)

11.2 Nicht‑Rückführbarkeit

Aus μ und σ kann man nicht rekonstruieren:

- was gesagt wurde,  
- wie die Stimme klingt,  
- wer die Person biografisch ist.

Die Signatur ist:

- wiedererkennend,  
- aber nicht identifizierend.

11.3 Angriffsszenarien

Mögliche Angriffe:

1. Diebstahl der Signaturdaten (μ, σ, τ).  
2. Versuch, die Signatur zu imitieren.  
3. Manipulation des Geräts.

Bewertung:

- 1: Liefert nur abstrakte Muster, keine Identität.  
- 2: Imitation der Bedeutungsbildung ist praktisch unmöglich, da hochdimensional und unbewusst.  
- 3: Kann durch klassische Sicherheitsmaßnahmen (Secure Enclave, Hardware‑Bindung) abgefangen werden.

---

12 Systemarchitektur

12.1 Hauptmodule

`
Input-Modul (Text/Audio)
Vorverarbeitung
Embedding-Modul
Projektionsmodul
Diskretisierungsmodul
Signatur-Verwaltung
Authentifizierungslogik
Drift-Management
Sicherheitslayer
Federated-Learning-Modul
`

12.2 Lokale Verarbeitung

Ziel:

- möglichst viel lokal auf dem Gerät ausführen.  
- keine zentrale Speicherung sensibler Muster.  

Federated Learning:

- Modelle werden lokal trainiert.  
- Nur Modellupdates (nicht Rohdaten) werden optional aggregiert.

---

13 Trainingspipeline im Detail

13.1 Initiales Setup

1. Nutzer entscheidet sich für semantischen Passkey.  
2. System erklärt kurz das Prinzip.  
3. Nutzer durchläuft eine Trainingsphase:

Beispiel:

- 30 kurze Sätze sprechen oder schreiben, z. B. Antworten auf offene Fragen:
  - „Beschreibe einen Ort, an dem du dich wohlfühlst.“  
  - „Was bedeutet für dich Sicherheit?“  
  - „Was nervt dich im Alltag?“  

Diese Fragen sind so gewählt, dass sie:

- Emotion,  
- Metaphern,  
- individuelle Gewichtungen,  
- Stil,  
- Rhythmus  

aktivieren.

13.2 Berechnung der ersten Signatur

Für jede Antwort:

- Embedding → v  
- Projektion → x  
- Sammlung in S_initial  

Dann:

- Berechnung von μinitial, σinitial  
- Setzen von τ_initial (z. B. basierend auf einem gewünschten Sicherheitsniveau)

13.3 Laufende Anpassung

Bei jeder erfolgreichen Authentifizierung:

- x_neu wird in die Signatur integriert.  
- μ und σ werden leicht angepasst.  
- τ kann adaptiv nachjustiert werden.

Optional:

- Gewichtung neuer Proben höher, um aktuelle Gestimmtheit stärker zu berücksichtigen.  
- Ältere Proben können langsam „ausblenden“.

---

14 Evaluationsmetriken

14.1 False Acceptance Rate

Wie oft wird eine fremde Person fälschlich akzeptiert?

14.2 False Rejection Rate

Wie oft wird der echte Nutzer fälschlich abgelehnt?

14.3 Equal Error Rate

Punkt, an dem FAR und FRR gleich sind – dient als Vergleichsmaß.

14.4 Drift‑Stabilität

Wie stabil bleibt die Signatur über:

- Tage  
- Wochen  
- Monate  

14.5 Kontext‑Robustheit

Wie robust ist das System gegenüber:

- Müdigkeit  
- Stress  
- Alkohol  
- emotionalen Ausnahmesituationen  

---

15 Beispielworkflow

15.1 Gerät entsperren

1. Nutzer sagt: „Öffne mein Gerät.“  
2. System extrahiert Merkmale.  
3. Embedding → Projektion → x_neu.  
4. Vergleich mit μ, σ, τ.  
5. Entscheidung:  
   - d < τ → Gerät öffnet sich.  
   - d ≥ τ → Rückfrage: „Bitte wiederhole einen Satz.“  

15.2 Online‑Login

1. Nutzer spricht oder schreibt eine kurze, freie Antwort.  
2. Lokale Verarbeitung wie oben.  
3. Server erhält nur: „authentifiziert“ oder „nicht authentifiziert“ – keine Rohdaten.

---

16 Gesellschaftliche Implikationen

- weniger Passwortstress  
- keine biometrische Invasion  
- Technik, die sich an menschliche Ausdrucksweise anpasst  
- Identität als lebendiger Prozess, nicht als statischer Datensatz  

---

17 Fazit

Der semantische Farbraum als Passkey ist:

- mathematisch klar strukturiert,  
- technisch mit heutigen Mitteln implementierbar,  
- sicherheitstechnisch robust,  
- datenschutzfreundlich,  
- und kulturell anschlussfähig.

Er ersetzt nicht nur Passwörter,  
sondern verschiebt das Paradigma von:

> „Beweise, dass du berechtigt bist“  
hin zu  
> „Zeig, wie du Bedeutung gibst – und das System erkennt dich.“

---

