---
layout: page
title: KFZ-Versorgungsspannung
description: Universal LCD Motorrad Tachometer
---

Im normalen Betrieb liegen die Spannungen im Kfz-Bordnetz bei 12V (10V bis 15V). Kritische Spannungspegel von bis 60V in Folge eines massiven Masseversatzes entstehen vor allem dann, wenn Masseverbindungen wegkorrodiert sind oder die Batteriepolklemmung locker ist. Dem nicht genug, die Versorgungsspannung fällt bis auf 8V beim Anlassen ab, hat ca. 12,6 V sobald das Kfz abgestellt ist, erreicht 14,4 V beim Fahren, 28,8 V beim Jumpstart vom LKW und kennt auch Spannungsspitzen von kurzzeitig -100 V wenn ein Relais abfällt oder +100 V, wenn ein Kabel der Lichtmaschine einen Wackelkontakt hat. Elektrische Kfz-Anlagen/Bauteile werden daher nach einer ISO-Norm (ISO 16750) getestet.

Als Verpolungsschutz sollte eine Diode (z.B. BAV21/250mA, 1N4003/1A, BYV27-200/2A) eingesetzt werden, die den Betriebsstrom und die maximalen Spannungsimpulse (von 100V oder mehr) aushält. Ein Elko 4,7..220μF (je nach Stromstärke) puffert kurze Spannungseinbrüche. Damit der Elko beim Absinken der Versorgungsspannung nicht entladen wird, sollte er in Reihe nach der Diode eingebracht werden. Eine zusätzliche Sicherung oder ein Drahtwiderstand mit hoher Leistung (u.a. auch wegen der Spannungsfestigkeit) begrenzt den Strom.

Für einfache Anwendungen reicht es, nur gegen zerstörerische Spannungen abzublocken:

![KFZ-Versorgungsspannung Abb. 1](../images/Versorgungsspannung_1.png)

Um Störungen mit hohen Frequenzen von der Schaltung fernzuhalten, ist ganz vorne am Eingang ein Keramikkondensator von 1..10nF zwischen Versorgungsspannung und Masse sinnvoll, der diese Frequenzen dämpft. Um es noch _sauberer_ zu haben, könnte man anstatt den Sicherungswiderstand eine Drossel mit 47μH setzen.

Um länger höhere Spannungen von der Schaltung fernhalten, wird eine Überspannungsschutzdiode (TVS-Diode, Transil) eingesetzt, die erst bei einer Spannung von 28,5V (Spannung für den Jumpstart-Test ist 28,8V und der Load Dump-Test erfolgt mit 27V) leitet und bei vollem Ableitstrom nicht mehr als die maximal erlaubte Spannung der Bauteile an die Schaltung lässt (absolutes Maximum bei 41,5V mit einem Stromimpuls von 14,4A siehe [Datenblatt](http://www.diodes.com/assets/Datasheets/ds21502.pdf)).

Eine Schaltung die zwar keine Prüfung nach ISO (7637-1 von 1990 für ein 12V-System) besteht, aber viele Faktoren bereits berücksichtigt, könnte wie folgt aussehen:

![KFZ-Versorgungsspannung Abb. 2](../images/Versorgungsspannung_2.png)

Die TVS-Diode P6KE30A (Unidirektional) hat eine Durchbruchspannung von 28,5V, leitet sicher bei 31,5V und begrenzt die maximale Spannung, wenn die Quelle mehr liefert. Nachfolgende Bauteile sollten bis zu ca. 35V vertragen (der dann abzuleitende Strom ist dann schon so hoch, dass der Schutzwiderstand vorher _auslöst_). Bei Einsatz einer TVS-Diode kann der Keramikkondensator am Eingang ein 1nF/50V und der Elko ein Kondensator mit z.B. 100μF/50V sein. Da höhere Ströme durch die P6KE30A abfließen können, wurde die Diode BYV27-200 ausgewählt, die für ein Strom If bis 2A genutzt werden kann. Zudem hat Sie eine Sperrspannung bis 200V und weist eine Durchlassspannung UF von 1V auf.

Eine Stabilisierung mittles Zenerdiode ist einfach und ausreichend für die angeschlossenen Schaltungseinheiten. Mit Ihr kann eine einfache stabilisierte Spannungsausgang mit geringer Restwelligkeit bei wechselnden Laststrombedingungen erzeugt werden. 

Indem die Zenerdiode einen Teil des Stroms von der Spannungsquelle über einen geeigneten Strombegrenzungswiderstand (R5) leitet, wird der Spannungsabfall an Pin 7.1 aufrechterhalten. Durch Verwendung einer 5,1V Zenerdiode (Datenblatt: [BZX79.pdf](http://www.nxp.com/documents/data_sheet/BZX79.pdf)) wird eine spannungsgeregelte Versorgung von ca. 5V (+/- 10%) zur Verfügung gestellt. Der maximal benötigte Ausgangsstrom an Pin 7.1 beträgt 10mA, somit kann die Leistungsaufnahme der Z-Dioden gering gehalten werden. Am Vorwiderstand können bis zu 30V (siehe oben) abfallen, daher ist ein Vorwiderstand von 2 Watt nötig.

![KFZ-Versorgungsspannung Abb. 3](../images/Versorgungsspannung_3.png)

Da die Z-Diode beim _Versuch_, die Spannung zu stabilisieren, eine elektrische Welligkeit auf der Versorgung erzeugt, kommt ein zusätzlicher Entkopplungskondensator von 22μF/16V zum Einsatz, der kurze Lastspitzen abfängt und die Restwelligkeit der Ausgangsspannung reduziert.

Die Z-Diode ist zudem ein Teil der Schutzmaßnahme gegen Überspannung für alle IC-Eingänge mit entsprechender Dioden-Schutzschaltung gegen Plus:

![KFZ-Versorgungsspannung Abb. 4](../images/Versorgungsspannung_4.png)

Sobald an einem externen Eingang eine Überspannung erzeugt wird, die über eine Schutzdiodenschaltung nach Plus oder gegen Masse geleitet wird (siehe Abschnitt Kontrollanzeige), fließt ggf. ein genügend schädlicher Strom, der dazu führen kann, dass eines der ICs Schaden nimmt. Die Z-Diode sorgt dafür, dass alle Spannungen von externen Eingängen die größer als 5,8V sind, sicher abgeleitet werden (ein [Low-Drop-Spannungsregler](http://de.wikipedia.org/wiki/Spannungsregler) selbst wäre nicht in der Lage einen Rückstrom zu absorbieren). Die Strombegrenzung für die Dioden erfolgt durch den Eingangswiderstand des jeweiligen externen Einganges.

## Quellen und weiterführende Literatur

### Links
- Mikrocontroller.net; [Kfz Spannungsspitzenkiller / Transientenschutz](http://www.mikrocontroller.net/articles/Kfz_Spannungsspitzenkiller_/_Transientenschutz)
- Mikrocontroller.net; [12V KFZ Versorgungs-Schaltung aus de.dse-faq](https://www.mikrocontroller.net/topic/392585)
- de.sci.electronics-FAQ; [F.23. Das KFZ-Bordnetz](http://www.dse-faq.elektronik-kompendium.de/dse-faq.htm#F.23)
- Elektronik-Kompendium; [Spannungsstabilisierung mit Z-Diode](http://www.elektronik-kompendium.de/sites/slt/1012151.htm)

### Startseite
Zum Anfang geht's [hier](../index.html).
