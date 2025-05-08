### Intrinsische und extrinsische Kameraparameter
- Berechnung der Pose der Marke im Verhältnis zur Kamera
  - auf Basis der Abbildung der Markeneckpunktkoordinaten -> Größe der Marke muss bekannt sein

#### intrinsische Kameraparameter
- durch Kamerakalibrierung -> erhält man intrinsische Kameraparameter -> entsteht Kalibrierungsmatrix (Abbildung der Kamerakoordinaten auf die Bildebene bestimmt)
#### extrinsische Kameraparameter
- mithilfe von Kalibrierungsmatrix K, bejabteb Avstabds zwischen Eckpunkte, Berücksichtigung der aufgrund der Markenausrichtung bekannten orientierung
  - lassen sich 3x3 Rotationsmatrix R (extrinsische Kameraparameter)
  - und Translationsvektor Tcm bestimmen (extrinsische Kameraparameter)


### Merkmalsbasierte Tracking-Verfahren
- Merkmale im Kamerabild erkennen
  - werden verglichen mit bekannten Modelle aus einer Datenbank 
  - kann 2d- oder 3d-Modelle sein
- kann als eine Verallgemeinerung des markenbasierten Ansatz gesehen werden
#### Geometriebasiertes Tracking
- aus Kamerabild werden Merkmale extrahiert
  - Kanten und/oder Eckpunkt
- basierend auf Fortschreibung (Extrapolation)
  - aus dem vorangegangenen Kamerabild extrahierten Transformation 
    - -> werden Abstände zwischen Linien und Ecken des errechneten und des aktuellen Bildes als Grundlage für die Veränderung der Transformation verwendet
- Vergleich Würfel -> einzelne Merkmale meist nicht eindeutig 
  - -> existieren meist mehrere gültige Posen zu einem aktuellen Kamerabild (Würfel mit sechs gleich langen Seiten)
  - basierend auf der zuletzt verwendeten Pose wird diejenige ausgewählt
    - mit Verwendung von mehreren Transformationen 
    - die kleinste Veränderung zur zuvor berechneten Transformation aufweist
  - wichtig hierbei ist dadurch die korrekte Initialisierung des Trackings
    - werden alle nachfolgende Posen falsch berechnet / entschieden
  - zur korrekten Initialisierung -> Markenbasiertes Tracking Verfahren verwendet
  - zusätzlich kommen Neuronale Netze zum Abgleich mit dem verwendeten Modell zum Einsatz
  - Merkmalsbasierte Ansätze (Kanten und / Ecken verwendet) eignen sich gut für gleichförmiger geometrischer Formen
    -  gerade wenn die Bereiche sonst keine Merkmale hat
### Weitere merkmalbasierte Tracking-Verfahren
- andere visuelle Merkmale (Features) lassen sich vom menschlichen Betrachter nicht leicht erkennen
- Vorteil
  - schnell und zuverlässig durch Merkmalsdetektoren in einem Kamerabild finden lassen
- wenn genügend solcher Merkmale aus dem Kamerabild extrahiert werden kann
 - -> im Anschluss basierend auf ihrer individuellen Beschreibungen (Deskription) 
   - mit vorhanden beschreibungen der Merkmale der zu verfolgenden 2D- oder 3D Geometrie verglichen
 - Ausreißer werden aussortiert -> mit **RANSAC-Verfahren** 
   - kann die korrekte Zuordnung die Pose der Kamera im Verhältnis zu den bekannten Merkmalsgruppen berechnet werden (Bild zeigen S. 141)
#### Merkmalsdetekoren
- unterscheiden sich in ihrer Geschwindigkeit
- nicht jeder von denen bieten entsprechende Deskriptoren
- Vorteilhaft ist wenn:
  - Erkennung der Merkmale unabhängig von:
    - Rotation (Rotationsinvarianz)
    - Entfernung (Skalierungsinvarianz) ist
- wenn das nicht ist dann:
  - Merkmale mithilfe von:
    - unterschiedlichen Winkel
    - unterschiedlichen Auflösungen berechnet werden
- für die Merkmalbasierte verwendeten Detektoren gehört:
  - **SIFT (Scale Invariant Feature Transform)**
  - **SURF (Speeded Up Robust Features)**

#### andere Möglichkeit kamerabasiertes Tracking umzusetzen
- kombinierte Verwendung von Farbkameras und Tiefenkameras
  - **RGBD-Kmaeras** -> verwenden zur Tiefenerkennung ein mit Infrarot projiziertes Muster oder ein Laufzeitverfahren
    - bei denen die Laufzeit des reflektierten Lichts bestimmt wird
    - bekannt durch Kinect geworden
  - Tiefeninformation -> zum Tracking von Kameraposition und Tracking von Nutzerinteraktion verwendet werden
- Nutzerinteraktion -> Abschätzung in wie weit Skelette in aufgenommenen Tiefendaten passen können
 - dadurch werden winken mit den Arm erlaubt / erkannt


### Visual SLAM-Verfahren S. 143 
