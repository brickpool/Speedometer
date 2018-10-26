---
layout: page
title: Lade- und Öldruckkontrollanzeige mit Single-Chip-Anschaltung
description: Universal LCD Motorrad Tachometer
---

Die diskret aufgebaute Lade- und Öldruckkontrollanzeige kann auch mit einem einzelnen NE555 und einem Dutzend passiver Komponenten realsiert werden. Aufgabe der Diskreten Schaltung ist es, die Generatorspannung zu überwachen und bei Unterspannung die Ladekontrolllampe mit 12V zu versorgen. Zusätzlich wird bei schalten des Öldruckschalter gegen Masse die Kontrolllampe ebenfalls zur Anzeige gebracht. 

## Schaltung Ladekontrollanzeige
Der allgegenwärtige _Timer_-Baustein [NE555](http://de.wikipedia.org/wiki/NE555) ist ein sehr vielseitiger Chip. Es enthält zwei [Spannungsvergleicher](http://de.wikipedia.org/wiki/Komparator_(Analogtechnik)), die gegen eine interne oder über Pin 5 zugeführte Referenz vergleichen, ein [RS-FlipFlop](http://de.wikipedia.org/wiki/Flipflop#RS-Flipflop) und ein [Totem-Pole-Ausgang](http://de.wikipedia.org/wiki/Totem-Pole-Ausgang), der je nach IC-Ausführung bis zu 200 mA liefern bzw. aufnehmen kann. Alle diese Teile werden für die Realisierung einer Ladekontrollanzeige mittels NE555 verwendet.

Mit einer geeigneten [Referenzspannung](https://de.wikipedia.org/wiki/Referenzspannungsquelle) und zwei [Spannungsteilern](http://de.wikipedia.org/wiki/Spannungsteiler), kann die Funktion der Ladekontrollanzeige inkl. [Hysterese](http://de.wikipedia.org/wiki/Schmitt-Trigger) erfüllt werden. 

![Kontrollanzeige NE555 Abb.1](../images/Kontrollanzeige_mit_555_1.png)

Grundsätzlich funktionieren die drei für die Ladekontrollanzeige verwendeten Eingänge _Trigger_, _Treshold_ und _Control Voltage_ vom NE555 wie folgt:
- Wenn die Spannung am _Trigger_-Pin 2 weniger als die Hälfte der Referenzspannung vom _Control Voltage_-Pin 5 des IC's beträgt, wird das interne RS-FlipFlop gesetzt und der Ausgang _Output_-Pin 3 geht auf _High_ (Spannung erreicht fast Vcc: Kontolllampe leuchtet).
- Wenn die Spannung am _Treshold_-Pin 6 mehr als die Referenzspannung vom _Control Voltage_-Pin 5 beträgt, wird das interne RS-FlipFlop zurückgesetzt und der Ausgang Pin 3 geht in den _Low_-Zustand (Spannung erreicht fast 0V: Kontolllampe erlischt).

Der Pin _Control Voltage_ benötigt eine stabile Spannung, um die korrekten Umschaltpunkte für die zwei interen Komparatoren, welche zusätzlich mit den Pins _Trigger_ und _Treshold_ verbunden sind, zu erhalten. Eine 6,2V Zenerdiode (0,5W) zusammen mit einem Vorwiderstand ist eine einfache und genügend genaue Spannungsreferenz. Der Vorwiderstand ist mit 1k so gewählt, dass die Zener-Spannung auch noch bei 10V Eingangsspannung oder darunter gehalten wird.

Der Einsatz des Kondensator C5 schafft Abhilfe gegen Störungen beim Wechsel vom Zustand _High_ in den Zustand _Low_. Aufgabe der PNP-Schaltstufe war es, die Versorgungsspannung zu schalten. Der Ausgangsstrom des _Timer_-Bausteins reicht ausreicht, um den Kontrollanzeige zum Leuchten zu bringen. Das Datenblatt verrät den maximalen Ausgangstrom der beim NE555 bis zu 200 mA betragen darf. Sollte der Ausgang sogar gegen Masse oder die Betriebsspannung kurzgeschlossen sein, fließt der eben genannte Strom aus bzw. in den Ausgangspin und die Spannung bricht zusammen.

Ein am Gleichrichter Klemme 61 bzw. D+ angeschlossener Spannungsteiler wird so ausgelegt und an Pin _Trigger_ angeschlossen, dass die Spannung an _Trigger_ gleich die Hälfte von der Referenzspannung von _Control Voltage_ ist. Dies ist der gewünschte Unterspannungsschaltpunkt. Wenn die Spannung an Klemme 61 unter diesen Sollwert sinkt, geht die Spannung am Ausgang _Output_ auf [High](http://de.wikipedia.org/wiki/Logikpegel).

Ein zweiter Spannungsteiler wird an _Treshold_ angeschlossen. Er ist so eingestellt, dass die Spannung an _Treshold_ gleich der Spannung von _Control Voltage_ ist, wenn die Batteriespannung auf dem gewünschten Sollwert ist. Wenn die Spannung an Klemme 61 diesen Sollwert überschreitet, geht die Spannung an Pin  _Output_ auf [Low](http://de.wikipedia.org/wiki/Logikpegel).

![Kontrollanzeige NE555 Abb.2](../images/Kontrollanzeige_mit_555_2.png)

Die Widerstände R1, R2 und R3, R4 bilden die beiden Spannungsteiler. Betreffs der Größe vom Widerstand kommt es ganz auf die Anwendung an. In der Praxis eignen sich gut Werte von 10k bis einigen 100k. Der Wert von R1 beträgt 68k und der Wert von R2 beträgt 24k für den Umschaltpunkt der Unterspannung. Der Widerstand R3 ist mit 39k und R4 mit 33k festgelegt, um den Umschalztpunkt für die Sollspannung festzulegen.

## Erweiterung Öldruckkontrollanzeige
Die Kontrollanzeige La1 soll wie bisher bei Verlust des Öldrucks ebenfalls zur Anzeige kommen. Die Anschaltung des Sensors (Schalten der Masse bei Öldruckverlust) erfolgt einfach an den _Trigger_ Eingang vom _Timer_-Baustein. Zu beachten ist jedoch, dass nur bei der CMOS-Version des _Timer_-Bausteins der Eingang _Trigger_ vorrangig dem Eingang _Treshold_ ist.

Was fehlt ist eine passende Beschaltung, um unempfindlich gegen Störspannung zu sein. Und obwohl der _Timer_-Baustein eine Eingangsschutzschaltung besitzt, soll zusätzlich ein _Überspannungsschutz_ eingebaut werden. 

![Kontrollanzeige NE555 Abb.3](../images/Kontrollanzeige_mit_555_3.png)

## Gesamtschaltung Lade-/Öldruckkontrollanzeige
Zur Anwendung kommt der CMOS Timer-Baustein TLC556 (Datenblatt: [tlc556m.pdf](http://www.ti.com/lit/gpn/tlc556m)), welcher zwei RC-Timer mit gemeinsamer Spannungsvorsorgung hat. An den Betriebsspannungsanschlüssen darf eine Spannung für Vcc (= Betriebsspannung) von +2V bis +18V und für Vss (= Masse) 0V angelegt werden.

Die gesamte Schaltung für die Kontrollanzeige mit zusätzlicher _Blinkstufe_ sieht wie folgt aus:

![Kontrollanzeige (CMOS TLC556)](../images/Kontrollanzeige_556.png)

Die Schaltug wurde durch eine nachgelagerte [Astabile Kippstufe](http://de.wikipedia.org/wiki/Multivibrator#Astabiler_Multivibrator_mit_NE555) ergänzt, damit die Anzeige nicht nur einfach leuchtet sondern blinkt. Zudem wurde die Schaltung angepasst, sodass zwischen den beiden unterschiedliche Ereignissen - _Störung Generator_ und _Öldruckverlust_ - unterschieden werden kann. Jedes Erreignis hat eine eigene Blinkfrequenz.

Die Beeinflussung der Frequenz erfolgt am Eingang _Control Voltage_ der Astabile Kippstufen, welche vom _Discharge_ Ausgang der Unterspannungschaltung angesteuert wird. Zusätzlich wurde die Anschaltung des Ödruckschalters nicht wie weiter an der Unterspannungschaltung sondern, mittels einer einfachen Logik bestehend aus Transisor und Dioden (abgek. [DTL](http://de.wikipedia.org/wiki/Diode-Transistor-Logik)) an die nachgelagerten Kippstufe integriert. 

Bleibt noch zu erwähnen, dass beim Ereignis _Öldruckverlust_ die Kontrolllampe schneller blinkt, als beim Ereignis _Störung Generator_ und dass das Ereignis _Störung Generator_ Vorrang hat. In der fertigen Schaltung wird zusätzlich noch ein Stützkondensator C8 von 100nF direkt am TLC556 zwischen Speisespannung und Masse eingesetzt.

## Quellen und weiterführende Literatur

### Links
- Elektronik-Kompendium; [Timer NE555 und NE556](https://www.elektronik-kompendium.de/sites/bau/0206115.htm)
- Burton Lang; [NE555 Low Voltage Battery Disconnect Circuit](http://www.gorum.ca/lvdisc.html)
- [Das CMOS Kochbuch](https://www.amazon.de/Das-CMOS-Kochbuch-Don-Lancaster/dp/3883220027) von Don Lancaster; ISBN 3-88322-002-7

### Nächste Seite
Weiter geht's mit [Bezinstandsanzeige Ducati](benzinstandsanzeige.html).
