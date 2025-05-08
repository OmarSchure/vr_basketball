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
  - -> existieren meist mehrere gültige Posen zu einem aktuellen Kamera
