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
- die vorherigen Verfahren basieren auf **Computer Vision**
  - haben vorausgesetzt dass Marken, Bilder oder Objekte basierend auf ihren Merkmalen bekannt sind

- wie aber wird ein Tracking realisiert in unbekannter Umgebung
  - ohne Marken, Bilder oder Objekte 
  - ohne Infos über Anordung dieser oder Kameras im Raum
- **SLAM (Simultanous Localization and Mapping)** kommt ursprünglich aus der Robotik
- am Anfang weder Position und Lage der Kamera bekannt
- in Robotik werden **LIDAR (light detection and ranging)** hauptsächlich verwendet
- in AR
  - werden hauptsächlich Kameras eingesetzt
  - nennt sich dann **Visual SLAM**
  - auf Merkmale (z.B. SIFT, SURF, FAST etc.) oder
    - dünn besetzte Karten (sparse maps) mit vergleichsweise wenigen Merkmale erzeugt
  - Tiefeninformationen (z.B. Kinect, Intel RealSense, Google Tango, Structure.io) zurückgegriffen
    - dich besetzte (dense maps) Volumendarstellungen verwenden
  - anfangs keine Karte existiert
    - Startposition des Koordinatensystem frei gewählt
  - Sukzessive basierend auf Bewegung der Kamera die Karte erzeugt
    - aktuellen Kamerabild gefundene Merkmale werden mit der existierenden abgeglichen
    - neue Merkmale werden in der Karte verortet
    - basierend auf bekannte Teile der Karte wird Position und Lage der Kamera neu geschätzt (anhand detektioerten Merkmalen)
    - führt alles erstmal zu vielen Fehler -> Karte wurde erst erzeugt
    - wichtig ist das bekannte Umgebungsteile zuverlässig wieder erkannt werden (auch wenn Position und Lage abweichen zu den aktuellen Karteninfos)
    - beim **loop closing**
      - Kartendaten angepasst werden -> aktuelle und gespeicherten Infos konsistent sind
    - Schwierigkeit sind dynamische Objekte 
      - müssen identifiziert werden und rausgefilltert werden -> sonst gibt es Fehler in der Kartenerstellung
### Hybride Tracking-Techniken
- für AR -> kombinierte  und unterschiedliche Tracking-Verfahren zum Einsatz
  - Grund ist das nicht für jede Situation ein Verfahren genutzt werden kann
    - unterschiedlich gute Ergebnisse -> manche machen in bestimmten Situation gar keine verwendbaren Ergebnisse liefern
- bsp.: markenbasierten Ansatz
  - funktioniert in der Regel nur gut wenn:
    - für alle virtuellen Inhalte mindestens eine Marke die Position und Lage im Verhältnis zur Kamera bestimmt werden kann
    - wenns dabei auch nur kurzeitig zu einer Verdeckung -> wird die Marke nicht erkannt -> Registrierung des / der virtuellen Objekte in der realen Szene ist nicht mehr möglich
- um die AR Illusion nicht zu verlieren
  - die Veränderung der Position und Lage auf Basis alternativer Tracking-Verfahren einzuschätzen
- wenn Tablet oder Smartphone verwendet wird
  - kann die Änderung der Lage mithile der integrierten Lagesensoren ermittelt werden
  - Marke nicht mehr im Sichtfeld -> durch Lagesensoren eine korrekte Transformation angegeben werden
- andere Möglichkeit kurzzeitige Ausfälle zu kompensieren:
  - Verwendung von **Vorhersagetechniken**
    - hierbei eignen sich Extrapolationsverfahren
  - **Kalman-Filter** bessere Alternative
    - je nachdem ob Rotation oder Position geschätzt werden soll -> kommen andere Kalman-Filter zum Einsatz
  - andere Möglichkeit besteht bei **Parsafikation** (TODO kann ich nicht lesen S145 zweiter Absatz letztes Wort)

### Tracking der Microsoft Hololens
- verwendet SLAM-Ansatz
  - Besonderheiten -> gerade bei Kombination unterschiedlicher Hardware-Sensoren
- vier Kameras die ausschließlich zum Tracking verwendet werden 
  - Blickfeld von ca. 180° stereoskopisch erfasst
  - arbeiten mit geringen Bildwiederholrate (30 Hz) -> schnelle Kopfbewegungen -> nicht ohne merkliche latenz erfasst werden können
  - zur Kompensation werden bei schnellen Bewegungen die Tracking-Daten mit denen einer IMU (Update-Rate 1000Hz) kombiniert
    - erlaubt Berechnungen von Zwischenwerten zwischen den ermittelten Kameraposen mit 240 Hz 
    - erlaubt einen Ausgleich von Farbverschiebungen (late-stage reprojection) aufgrund der farbsequenziellen Ausgabe
- nutzt hochauflösende Frontkamera
- nutzt Tiefenkamera
  - wird nur zur Gestensteuerung verwendet (nicht fürs Tracking verwendet)
- anstatt eines globalen Koordinatensystems kommt ein Graph von Lageschätzungen zum Einsatz 
  - die einzelnen lokalen Koordinatensysteme sind über relative Posen verbunden
  - wenn diese nicht oder noch nicht vorliegen -> zerfällt der Graph in mehrere Teilgraphen
    - ein Loop Closing findet nicht statt -> Graph ist nicht zwingend global konsistent