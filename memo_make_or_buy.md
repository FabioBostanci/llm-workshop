### Empfehlung

Aufgrund der Masse der Anzeigen (60 + Anzeigen) wird ein hybrider Ansatz (human in the loop) empfohlen. Die erste Annotation sollte von einem Frontier-Modell durchgeführt werden. Die Masse der Anzeige nimmt bei rein menschlicher Bearbeitung viel Zeit in Anspruch und stellt sich als nicht wirtschaftlich dar. Das Risiko, dass der Mensch beim Lesen der vielen Anzeigen aufgrund von abnehmender Konzentration Fehler produziert, ist zudem erhöht. Selbst wenn das Frontier-Modell anfangs Fehler macht, ist der Output meist durch wenige Korrektur-Turns nahezu fehlerfrei. Zudem kann aus dem Frontier-Modell heraus direkt das gewünschte Dateiformat (.csv) erstellt werden. 

Nachdem das Frontier-Modell die erste Annotation erstellt hat, ist eine Sichtung des Ergebnisses durch den Menschen notwendig, um die Qualität der Annotation zu sichern. Insbesondere bei Feldern, die keinen Standards unterliegen und sich in ihrer Beschreibung von Anzeige zu Anzeige stark unterscheiden können, muss überprüft werden. Aber auch dazu wäre es nötig, sich vorher darauf festzulegen, wie die Bewertung erfolgen soll (bspw. was genau unter "junior" oder "senior" etc. in dem Projekt verstanden wird).

Eine vollständige Eigenleistung ist wirtschaftlich ineffizient; ein blinder Zukauf ohne Kontrolle gefährdet die Datenintegrität massiv.

### Begründung am κ

Dem Frontier kann man bei Feldern (eher) vertrauen, die einer eindeutigen Logik folgen und einfach zu benennen sind. Bei Gehaltsangaben kann man bspw. vorher Buckets festlegen, das Frontier-Modell kann mit Zahlen dann gut umgehen. Dort ist weniger Spielraum für Interpretationen als bei Angaben zu Homeoffice oder dem Erfahrungslevel.


### Schwellwert - Logik

Die Grenze liegt bei dem, was mit den eingesetzten finanziellen Mittel und dem Zeitrahmen möglich ist. Allerdings sind die Modelle mittlerweile so leistungsfähig, dass selbst eine große Anzahl von Anzeigen in einem Bruchteil der Geschwindigkeit abgearbeitet werden kann

Die projektweite Grenze für den unkontrollierten produktiven Einsatz des LLMs wird auf ein **$\kappa \ge 0.75$** festgelegt. 

* Jedes kritische Feld unterhalb von $\kappa = 0.40$ (`erfahrungslevel` sowie der harte Set-Match der `skills_top3`) verbleibt in der menschlichen Hoheit (*Make*). Ein Absinken des Gesamt-Kappas unter $0.60$ würde den sofortigen Stopp des Hybrid-Verfahrens und eine Rückkehr zur rein manuellen Kuration erzwingen.


### Risiko-Sicherung & Qualitäts-Gate
Sollte die KI im Restkorpus halluzinieren oder systematisch vom Schema abweichen, droht ein unbemerktes Drift-Problem in den nachgelagerten Machine-Learning-Modellen. Um dieses Risiko abzusichern, etablieren wir ein zweistufiges Qualitäts-Gate:

1. **Randomisierte Stichproben-Kontrolle:** Ein fixer Anteil von **15%** des Restkorpus wird parallel und blind von einem menschlichen Annotator gegengelesen. Tritt hierbei eine Abweichung auf, greift eine Schiedsrichter-Logik.
2. **Automatisches Flagging:** Datensätze, bei denen das LLM Informationen extrahiert, die im API-Rohdatenblock (`raw`) als `KEINE_ANGABEN` deklariert sind, werden automatisch für die manuelle Prüfung markiert. Dies fängt potenzielle Parametrisierungsfehler oder Halluzinationen des Modells kostengünstig und effektiv ab.