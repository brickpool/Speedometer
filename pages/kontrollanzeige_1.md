---
layout: page
title: Lade- und Öldruckkontrollanzeige
description: Universal LCD Motorrad Tachometer
---

Die Guzzi sowie die Ducati Monster hat eine Ladekontrollleuchte, die im Original zwischen Anschluss Kl.61 vom Gleichrichter und der positiven Versorgungsspannung (12V) vom Instrument liegt. Es kann zwar nicht der Ladezustand oder die aktuelle Spannung geprüft werden, aber es gibt zumindest eine Anzeige, dass die Batterie bei laufenden Motor geladen wird.

Da sowohl der Regler und Gleichrichter sowie das Instrument ausgetauscht werden, ist eine Anpassung der Funktion Ladekontrollleuchte notwendig. Das China-Instrument bietet leider keine eigene Ladekontrollleute. Da die Tachobeleuchtung als Kontrolle ob Licht eingeschaltet ist ausreicht, wird zukünftig die Lichtkontrollleuchte als Anzeige genommen.

Die Kontrollleuchte soll immer zur Anzeige kommen, wenn mehr Strom der Batterie entnommen wird als hinein fließt. Hierzu wird der Schaltpunkt für die Anzeige einfach über die Ladeschlussspannung definiert. Bei einer 12V Blei-Batterie beträgt die Ladeschlussspannung bei Schonladung 13,8V. Der Generatorspannung muss aber mindestens 13,4V liefern, damit mindestens eine Erhaltungsladung stattfindet. Eine **vollgeladene** Batterie sollte 12,8 V haben, bei ca. 12,6V ist Sie **normal geladen**, bei ca. 12,4V **schwach geladen**, bei ca. 12,0 V **normal entladen** und bei Werten unterhalb von 11,8 V **tief entladen**.

Die Guzzi bzw. Ducati besitzt darüber hinaus eine Öldruckanzeige. Der zugehörige Sensor wird bei Problemen gegen Masse geschaltet und bringt im Original im Instrument eine Lampe zur Anzeige, die an die Versorgungsspannung angeschlossen ist.

Die folgende Beispielschaltung soll an einem Fahrzeug mit 12V-Netz eine Kontrollleuchte im Instrument ansteuern. Die Kontrollleuchte La1 wird bei einer Spannung von unter 12,6V (+/- 5%) an [Klemme 61](http://de.wikipedia.org/wiki/Klemmenbezeichnung#Beleuchtung) eingeschaltet. Zudem schaltet der Schalter S1 (geschaltet gegen Masse) ebenfalls die Kontrollleuchte.

![Lade-/Öldruckkontrollanzeige](../images/Kontrollanzeige.png)

## Messen der Spannung an Kl.61
Im Internet gibt es hierzu Beispielkonfiguration auf Basis eines TL431AC. Der TL431 ist laut Datenblatt [tl431.pdf](http://www.ti.com/lit/ds/symlink/tl431.pdf) eine "Programmable Voltage Reference", arbeitet im Prinzip als einstellbare Z-Diode. Der Baustein ist klein genug um kommt im TO92 Gehäuse daher. Mit einem
Spannungsteiler an Vref lässt sich stufenlos eine Spannung zwischen circa 2,5V und 36V einstellen. Da intern ein Operationsverstärker genutzt wird, schaltet der TL431 mit hoher Genauigkeit.

Der Spannungsteiler aus R1 und R2 lässt sich mit folgender Formel bestimmen.

    Vin = (1+(R1/R2))*Vref

Durch die eng tolerierte Referenzspannung Vref von typischerweise 2,495V (siehe Datenblatt) und den schwachen Eingangsstrom an Vref (Iref) von 2 bis 4 μA (siehe Datenblatt) lässt sich der Spannungsteiler ziemlich genau berechnen. Mit den Normwerten 33k und 8,2k schaltet der TL431 bei Vin = 12,5V zwischen Katode und Anode durch.

![Messen der Spannung an Kl.61 Abb. 1](../images/Messung_Spannung_Kl.61_1.png)

Zum Schutz vor negativen Spannungsspitzen (bis zu -100V) wird zusätzlich eine Diode vom Typ BAV20 eingesetzt. Ein Kondensator (4,7μF / 160V) puffert bei kurzen Spannungseinbrüchen. An der Diode fällt in Durchlassrichtung eine Spannung von Uf = 0,8V ab (Vin ist somit 11,8V). Für eine Feinanpassung kann ein Potenziometer mit 2k eingesetzt werden. Mit den Normwerten von 18k und 4,7k und unter Berücksichtigung von Uf inkl. Toleranzen (E24-Normreihe mit 5% Toleranz) kann mit dem Potentiometer P1 in jedem Fall die gewünschte Spannungsschaltwerte von 12,6V eingestellt werden.

![Messen der Spannung an Kl.61 Abb. 2](../images/Messung_Spannung_Kl.61_2.png)

Der TL431 soll bei allen Spannungen (Arbeitsbereich vom Instrument 10-15V) zuverlässig arbeitet. R4 muss so Dimensioniert sein, dass der Strom zwischen Katode und Anode Ika mindestens 1 mA und maximal 100 mA beträgt (siehe Datenblatt). Zu beachten ist, dass eine Anwendung des TL431 als Komparator die unpraktische Eigenschaft hat, dass die Spannung Vout im durchgeschalteten Zustand nicht unter Vka sinken kann.

    R3(max) = (Vmin-Vka)/Ika(min) = 10V-2,5V / 1mA = 7,5kOhm
    R3(min) = (Vmax-Vka)/Ika(max) = 15V-2,5V / 100mA = 125Ohm

Bei einer Versorgungsspannung von 10-15V und einem Widerstand R3 von ca. 4,7kOhm beträgt der Strom zwischen Katode und Anode ca. 1,6 (bei 10V) bis 2,7mA (bei 15V).

## Schalten der Versorgungsspannung
Die Kontrollleuchte ist im Instrument gegen Masse geschaltet. Sie erfordert also das schalten Versorgungsspannung. Da dies weiterhin elektronisch passieren soll, nutzen wir hierzu ein PNP-Transistor. Hier liegt der Emitter an der Versorgungsspannung vom Fahrzeug (Kl. 30).

Zur Anwendung kommt ein Transistor Typ BC557B (Datenblatt: [BC560.pdf](https://www.fairchildsemi.com/ds/BC/BC560.pdf)) der die Kontrollleuchte La1 bei 12V mit einer gemessenen Last von 2,7kOhm schalten soll. Vorteil des Transistors hier ist, dass der Spannungsabfall am Transistor bei _Durchschaltung_ nur ca. 0,1V bis 0,6V beträgt. 

![Schalten der Versorgungsspannung Abb. 1](../images/Schalten_der_Versorgungsspannung_1.png)

Die Steuerspannungen Vbe am Transistor bezieht sich bei einem PNP-Transistor auf die positive Versorgungsspannung und nicht auf Masse. Ein PNP immer schaltet dann durch, wenn Vbe die Schaltschwelle unterschreiten, also Vin um Vbe kleiner ist als die Spannung Ve am Emitter.

Der Basiswiderstand R7 berechnet sich aus (Vbat-Vbe) / Ib. Etwas versteckt bei den Daten und der Kennlinie zu Vce(sat) (der Kollektor-Emitter-Sättigungsspannung) findet man eine Referenzwert für die Sättigung von Ic = 20*Ib. D.h. gerechnet wird mit einer Stromverstärkung in Sättigung von 20. Da nur kleine Ströme fließen (Ic < 10 mA und somit Ib < 0,5 mA) wird Vce(sat) mit dem im Datenblatt angegeben Wert von 0,3V angesetzt.

Die Versorgungsspannung vom Instrument beträgt 10V bis 15V. Etwa 0,7V fallen an der Basis-Emitter-Strecke ab (Vbe aus Kennlinie des Datenblatt für den BC557B geschätzt), also wird bei einer niedrigen Versorgungsspannung von 10V für eine vernünftige Durchschaltung ein passender Widerstand R7 benötigt.

    Ib(min) = Ic(min) / 20 = 3,4 mA / 20 = 0,17mA
    R7(max) = Vbat(min) – Vbe / Ib(min) = (10V – 0,7V) / 0,17mA = 31,8kOhm

Der höchst zulässige Kollektorstrom IC für den Transistortyp beträgt max. 100mA mit einem zugehörigen Basisstrom Ib von 5mA (siehe Datenblatt). In dem Sättigungsbereich fallen bei hohen Strömen an der Basis-Emitter-Strecke 0,9V ab. Bei Versorgungsspannung von 15V ist der hierzu notwendige Basiswiderstand R7 ebenfalls zu berechnen.

    Ib(max) = Ic(max) / 20 = 5,85 mA / 20 = 0,3mA
    R7(min) = Vmax – Vbe / Ib(max) = (15V – 0,9V) / 5mA = 2,8kOhm

Für unseren Anwendungsfall verwenden wir für R7 ein Widerstand von 10kOhm der einen Basisstrom von ca. 0,9mA (bei 10V) bis 1,4mA (bei 15V) hervorruft.

Damit die Transistorstufe zuverlässig arbeitet, kommt zusätzlich der Widerstand R6 zu Anwendung. Er vermeidet, dass der Transistor bei Störeinstrahlungen teilweise leitet, indem er den Basisanschluss auf die Versorgungsspannung vorspannt, so dass der Transistor sperrt. Erst mit Schalten von Vin gegen Masse wird die Spannung herabgesetzt, so dass der Transistor leitet. Der dabei fließende _Querstrom_ Iq soll ca. 3- bis 10-mal höher als der Basisstrom Ib sein.

    R6(max) = (Vbat(min) - Vin(min)) / Iq(min) = 10V - 90mV / (Ib(min) * 3) = 9,9V / (0,9mA * 3) = 3,5k
    R6(min) = (Vbat(max) - Vin(min)) / Iq(max) = 15V - 90mV / (Ib(max) * 10) = 14,9V / (1,4mA * 10) = 1,1k

In unserem im Anwendungsfall verwenden wir für R6 ein Widerstand von 3,3kOhm.

![Schalten der Versorgungsspannung Abb. 2](../images/Schalten_der_Versorgungsspannung_2.png)

Da die Anschaltung von La1 erfolgt extern und der PNP-Transistor bei einer Fehlbeschaltung vom Kollektor nicht zerstört wird, wird eine Strombegrenzung eingebracht. Widerstand R8 und LED begrenzen den maximalen Kollektorstrom Ic(max) von La1 z.B bei _Kurzschluss_. Warum überhaupt eine LED? Durch Einsatz einer LED anstatt einer Z-Diode kann R8 klein gehalten werden. Zudem hat eine Konstantstromquelle mit LED einen kleineren Temperaturdrift und somit eine bessere Konstanz von Ic als z.B. zwei Dioden, die in Flussrichtung betrieben werden. 

Die rote LED bewirkt, dass an der Basis von T2 nur eine maximale Spannung von Vf = 1,7V (bis 2V, je nach LED) erreicht werden kann. Die zugehörige Basis-Emitter-Spannung Vbe von T2 wird mit 0,7V angesetzt. Der maximale Strom wird erreicht, wenn der Spannungsabfall an R8 den Wert von 1V (Vf - Vbe) erreicht. Der Widerstand R8, für einen vorgebenen maximalen Strom von 10mA, lässt sich wie folgt berechnen:

    R8(min) = (Vf - Vbe) / Ic(max) = 1,7V – 0,7V / 10mA = 100 Ohm

Für R8 wird ein Wert von 100 Ohm gewählt, der den Strom Ic(max) auf ca. 10..11 mA begrenzt (Vbe ca. 0,6..0,7 V). Die maximale Verlustleistung für R8 unter _Last_ beträgt weniger als 1/4 Watt:

    Pmax = (Vbat(max) - Vce(sat)) * Ic = (15V – 0,3V) * 10mA = 0,15W

Die Verlustleistung am Transistor beträgt bei _Kurzschluss_ ebenfalls nur ca. 0,15W. Die Berechnung erfolgt über die Kollektor-Emitter-Spannung Vce = Vbat(max) - V(R8) bei maximalen Strom Ic(max):

    Pmax = (Vbat(max) - V(R8)) * Ic = (15V – 1V) * 10mA = 0,15W

Die Kontrollanzeige La1 soll bei Verlust des Öldrucks zur Anzeige kommen. Der Öldrucksensor S1 schaltet gegen Masse. Die Integration des Sensors erfolgt unter Nutzung einer Schutzdiode.

## Invertieren der Messstufe
Es soll mit _positiver_ Logik geschaltet werden, also fügen wir eine NPN-Schaltstufe hinzu, die den Eingang der PNP-Schaltstufe versorgt. Zur Anwendung kommt der passende NPN-Transistor Typ BC547B (Datenblatt: [BC547.pdf](https://www.fairchildsemi.com/datasheets/BC/BC547.pdf)).

![Invertieren der Messstufe](../images/Invertieren_der_Messstufe.png)

Die Beschaltung von R4 und R5 zu R3 muss im passenden Verhältnis zu den anliegenden Pegeln ausgesucht werden. Die Ansteuerung Vin erfolgt in Abhängigkeit vom TL431 die _Messstufe_. Im Schaltzustand _High_ liegt die Versorgungsspannung Vbat über den Widerstand R3 an. Im _Low_-Zustand liegt ein Potenzial an Vin von konstanten 2,5V hat.

Der notwenige Basisstrom Ib(min) für das Durchschalten des Transistors lässt sich mit der oben genannten Formel Ic = 20*Ib errechnen. Vce(sat) wird hier (wie oben) mit 90mV angesetzt.

    Ic(max) = I(R6) + Iout = (Vbat(max) – Vce(sat)) / R6 + Iout(max) = (15V - 90mV) / 3.3k + 1,4mA = 6mA
    Ib(min) = Ic(max) / 20 = 6mA / 20 = 0,3mA

Der daraus resultierende maximale Widerstand für R3 und R4 lässt sich über folgende Formel berechnen.

    (R3+R4)max = (Vbat(max) – Vbe) / Ib(min) = (15V – 0,7V) / 0,3mA = 48k

Der Widerstandswert R3 beträgt 4,7kOhm (siehe Messstufe). Für R4 wählen wir den nächst passenden Widerstandswert von 33kOhm aus der E12 Reihe, der den oben genannten Maximalwert nicht übersteigt.

R5 hat zusammen mit R3 und R4 den Zweck die Schaltschwelle über den Transistor zu erhöhen, was hier bedeutet, dass die Schaltschwelle an Vin Richtung Versorgungsspannung verschoben wird. Der Widerstand R5 vermeidet auch, dass der Transistor bei starken Störeinstrahlungen teilweise leitet. Die zugehörige Schaltschwelle Ves lässt wie folgt errechnen:

    Ves = ((R3 + R4) / R5 + 1) * Vbe = ((4,7k + 33k) / 10k + 1) * 0,7V = 4,5V

Die Differenz zur Schaltschwelle der Messstufe (bei _Low_ definiert über Vka = 2,5V) liegt also bei ca. +2V.

## Einbringen einer Hysterese
Der TL431 wird bei minimalen Über- oder Unterschreiten der Referenzspannung bzw. der definierten Eingangsspannung hin und her kippen. Durch Einbringung definierter Schaltschwellen, die sich voneinander durch eine entsprechende Spannungsdifferenz unterscheiden, kann jedoch das Gesamtverhalten gegenüber Rauschen oder Störsignale verbessert werden.

Die Erzeugung dieser Schalthysterese kann wie bei einem [Komperator](https://de.wikipedia.org/wiki/Komparator_(Analogtechnik)) mit Hilfe einer Mitkopplung erreicht werden. In unserem Fall indem ein Teil der Ausgangsspannung Vout von T1 (nicht-invertierende Spannung zum Eingang) an den Eingang Ref (Komperator +) des TL431 zurückgeführt wird. Dazu wird lediglich ein Widerstand (R9) benötigt.

![Schmitt-Trigger](../images/Schmitt-Trigger.png)

Eine solche Schaltung wird als (nicht-invertierender) [Schmitt-Trigger](https://de.wikipedia.org/wiki/Schmitt-Trigger) bezeichnet. Berechnet wird die Hysterese durch Nutzung folgender Formeln (bei Vin = Vbat und unter Vernachlässigung von R6 weil R9 >> R6):

    V(low) = Vref * (R1||R9 + R2) / R2 + Vf
           = 2,5V * (R1/R2 * R9/(R1+R9) + 1) + 0,8V
           = 2,5V * (18k/4,7k * 150k/(18k+150k) + 1) + 0,8V
           = 11,8V

    V(high) = Vref * (R1 + R2||R9) / (R2||R9) + Vf
            = 2,5V * (R1/R2 * (R2+R9)/R9 + 1) + 0,8V
            = 2,5V * (18k/4,7k * (4,7k+150k)/150k + 1) + 0,8V
            = 13,2V

## Quellen und weiterführende Literatur

### Links
- Wikipedia; [Starterbatterie](http://de.wikipedia.org/wiki/Starterbatterie#Wartung,_Pflege_und_Prüfung)
- Pauls Werkstatt von Paul; [Ladekontrollleuchte mit TL431](http://pauls-werkstatt.blogspot.de/2015/06/ladekontrollleuchte-mit-tl431.html)
- Netzmafia von Prof. Jürgen Plate; [Präzisions-Shunt-Regler TL431](http://www.netzmafia.de/skripten/hardware/TL431/index.html)
- Mikrocontroller.net; [Unterspannungsabschaltung gesucht](http://www.mikrocontroller.net/topic/340319#3744991)
- Mikrocontroller.net; [LTspice Schaltung mit TL431 so richtig?](http://www.mikrocontroller.net/topic/380034#4323523)
- DL6GL von Georg Latzel; [Schalten mit Transistoren](http://dl6gl.de/grundlagen/schalten-mit-transistoren)
- Elektronik-Kompendium; [Schalten und Steuern mit Transistoren I](http://www.elektronik-kompendium.de/public/schaerer/powsw1.htm)
- Elektronik-Kompendium; [Schmitt-Trigger (nicht-invertierender)](http://www.elektronik-kompendium.de/sites/bau/0209241.htm)
- Elektronik-Kompendium; [Transistor-LED-Konstantstromquelle](http://www.elektronik-kompendium.de/public/schaerer/currled.htm)

### Nächste Seite
Weiter geht's mit [Lade- und Öldruckkontrollanzeige mit CMOS](kontrollanzeige_2.html).
