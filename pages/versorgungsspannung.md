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

Um länger höhere Spannungen von der Schaltung fernzuhalten, wird eine Überspannungsschutzdiode (TVS-Diode, Transil) eingesetzt, die erst bei einer Spannung von 28,5V (Spannung für den [Jumpstart](https://en.wikipedia.org/wiki/Jump_start_(vehicle)) ist 26V und der [Load Dump](http://de.wikipedia.org/wiki/Load_Dump) wird mit 27V angesetzt) leitet und bei vollem Ableitstrom nicht mehr als die maximal erlaubte Spannung der Bauteile an die Schaltung lässt (das Maximum liegt bei 41,5V mit einem Stromimpuls von 14,4A siehe [Datenblatt](http://www.diodes.com/assets/Datasheets/ds21502.pdf)).

Eine Schaltung die zwar keine Prüfung nach ISO (7637-1 von 1990 für ein 12V-System) besteht, aber viele Faktoren bereits berücksichtigt, könnte wie folgt aussehen:

![KFZ-Versorgungsspannung (gesamt)](../images/Versorgungsspannung.png)

Die TVS-Diode P6KE30A (Unidirektional) hat eine Durchbruchspannung von 28,5V, leitet sicher bei 31,5V und begrenzt die maximale Spannung, wenn die Quelle mehr liefert. Nachfolgende Bauteile sollten bis zu ca. 35V vertragen (der dann abzuleitende Strom ist dann schon so hoch, dass der Sicherungswiderstand vorher _auslöst_). Bei Einsatz einer TVS-Diode kann der Keramikkondensator am Eingang ein 1nF/50V und der Elko ein Kondensator mit z.B. 100μF/50V sein. Da höhere Ströme durch die P6KE30A abfließen können, wurde die Diode 1N4003 ausgewählt, die für ein Strom If bis 1A genutzt werden kann. Zudem hat Sie eine Sperrspannung bis 200V und weist eine Durchlassspannung UF von maximal 1V auf.

![KFZ-Versorgungsspannung Abb. 2](../images/Versorgungsspannung_2.png)

Eine Stabilisierung mittles Z-Diode (Datenblatt: [BZX79.pdf](http://www.nxp.com/documents/data_sheet/BZX79.pdf)) im Regelkreis und Transistor in Kollektorschaltung (Datenblatt: [BD139.pdf](http://www.onsemi.com/pub/Collateral/BD139-D.PDF)) ist einfach und ausreichend für die angeschlossenen Schaltungseinheiten. Der Spannungsausgang an Pin 9 stellt eine stabile Spannung bei wechselnden Laststrombedingungen zur Verfügung. Der maximal benötigte Ausgangsstrom an Pin 9 soll 200mA betragen. 

Indem ein Teil der Ausgangsspannung über eine Regelschaltung bestehend aus Spannungsteiler R6 und R7, Transistor T1 und Z-Diode D34 geleitet wird, werden Spannungen größer 12,1V heruntergeregelt. Der Spannungsteiler dient zum Einstellen des Arbeitspunkts am Transistor T1 und gleichzeitig als Grundlast damit ein mindest Basis-Strom bzw. eine mindes Spannung von 0,7V an der  Basis-Emmiter-Strecke vom Kleinleistungstransistor T2 vorhanden ist. Die Leistungsaufnahme der Z-Diode und T1 kann gering gehalten werden. Am Transistor T2 können jedoch bis zu 23V (siehe oben) abfallen, daher ist bei einer maximalen Strombelastung von 200mA ein Transistor mit einer Leistung von mind. 1 Watt nötig.

![KFZ-Versorgungsspannung Abb. 3](../images/Versorgungsspannung_3.png)

Da die Regelschaltung beim _Versuch_, die Spannung zu stabilisieren, eine elektrische Welligkeit auf der Versorgung erzeugt, kommt ein zusätzlicher Entkopplungskondensator von 22μF/16V zum Einsatz, der kurze Lastspitzen abfängt und die Restwelligkeit der Ausgangsspannung reduziert.

Die Reglerschaltung ist zudem ein Teil der Schutzmaßnahme gegen Überspannung für alle Eingänge mit entsprechender Dioden-Schutzschaltung gegen Plus. Sobald an einem externen Eingang eine Überspannung erzeugt wird, die über eine Schutzdiodenschaltung nach Plus oder gegen Masse geleitet wird (siehe Abschnitt Kontrollanzeige), fließt ggf. ein genügend schädlicher Strom, der dazu führen kann, dass eines der Halbleiter Schaden nimmt. Die Reglerschaltung sorgt dafür, dass alle Spannungen von externen Eingängen die größer als die Betriebsspannung +0,7V sind (12,1V + 0,7V = 12,8V), sicher abgeleitet werden. Die Strombegrenzung erfolgt durch den Eingangswiderstand des jeweiligen externen Einganges.

## Quellen und weiterführende Literatur

### Links
- Mikrocontroller.net; [Kfz Spannungsspitzenkiller / Transientenschutz](http://www.mikrocontroller.net/articles/Kfz_Spannungsspitzenkiller_/_Transientenschutz)
- Mikrocontroller.net; [12V KFZ Versorgungs-Schaltung aus de.dse-faq](https://www.mikrocontroller.net/topic/392585)
- de.sci.electronics-FAQ; [F.23. Das KFZ-Bordnetz](http://www.dse-faq.elektronik-kompendium.de/dse-faq.htm#F.23)
- Elektronik-Kompendium; [PROTECTION STANDARDS APPLICABLE TO AUTOMOBILES](http://www.elektronik-kompendium.de/public/schaerer/FILES/sgs_pulsetest_auto.pdf)
- Elektronik-Kompendium; [Spannungsstabilisierung mit Z-Diode und Transistor (Kollektorschaltung)](https://www.elektronik-kompendium.de/sites/slt/0204131.htm)
- Das InterNetzteil- und Konverter-Handbuch von Dipl.-Ing Jörg Rehrmann; [3.2.3 Der geregelte Laengsregler](http://www.joretronik.de/Web_NT_Buch/Kap3/Kapitel3.html#3.2.3)

### Startseite
Zum Anfang geht's [hier](../index.html).
