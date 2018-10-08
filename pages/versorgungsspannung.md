---
layout: page
title: KFZ-Versorgungsspannung
description: Universal LCD Motorrad Tachometer
---

Im normalen Betrieb liegen die Spannungen im Kfz-Bordnetz bei 12V (10V bis 15V). Kritische Spannungspegel von bis 60V in Folge eines massiven Masseversatzes entstehen vor allem dann, wenn Masseverbindungen wegkorrodiert sind oder die Batteriepolklemmung locker ist. Dem nicht genug, die Versorgungsspannung fällt bis auf 8V beim Anlassen ab, hat ca. 12,6 V sobald das Kfz abgestellt ist, erreicht 14,4 V beim Fahren, 28,8 V beim Jumpstart vom LKW und kennt auch Spannungsspitzen von kurzzeitig -100 V wenn ein Relais abfällt oder +100 V, wenn ein Kabel der Lichtmaschine einen Wackelkontakt hat. Elektrische Kfz-Anlagen/Bauteile werden daher nach einer ISO-Norm (ISO 16750) getestet.

Als Verpolungsschutz sollte eine Diode (z.B. BAV21/250mA, 1N4003/1A, BYV27-200/2A) eingesetzt werden, die den Betriebsstrom und die maximalen Spannungsimpulse (von 100V oder mehr) aushält. Ein Elko 4,7..220μF (je nach Stromstärke) puffert kurze Spannungseinbrüche. Damit der Elko beim Absinken der Versorgungsspannung nicht entladen wird, sollte er in Reihe nach der Diode eingebracht werden. Eine zusätzliche Sicherung oder ein 10 Ohm Widerstand mit hoher Leistung (u.a. auch wegen der Spannungsfestigkeit) dient als Leitungsschutz.

Für einfache Anwendungen reicht es, nur gegen zerstörerische Spannungen abzublocken:

![KFZ-Versorgungsspannung Abb. 1](../images/Versorgungsspannung_1.png)

Um Störungen mit hohen Frequenzen von der Schaltung fernzuhalten, ist eine Drossel mit 47μH sinnvoll, die diese Frequenzen dämpft. Um es noch „sauberer“ zu haben, sollte man ganz vorne am Eingang einen Keramikkondensator von 1..10nF zwischen Versorgungsspannung und Masse setzen.

Um länger höhere Spannungen von der Schaltung fernhalten, wird eine Überspannungsschutzdiode (TVS-Diode, Transil) eingesetzt, die bei einer Spannung von 28,8V (Spannung für den Jumpstart-Test, Load Dump-Test wird mit 27V angenommen) noch nicht leitet und bei vollem Ableitstrom nicht mehr als die maximal erlaubte Spannung der Bauteile an die Schaltung lässt.

Eine Schaltung die die Prüfung nach ISO (7637-1 von 1990 für ein 12V-System) besteht sollte, könnte wie folgt aussehen:

![KFZ-Versorgungsspannung Abb. 2](../images/Versorgungsspannung_2.png)

Eine TVS-Diode P6KE33A (Unidirektional) hat eine Durchbruchspannung von 31,4V, leitet sicher bei 34,7V und begrenzt auf 45,7V wenn die Quelle mehr liefert. Nachfolgende Bauteile müssen somit 35V vertragen. Bei Einsatz einer TVS-Diode kann der Keramikkondensator am Eingang ein 1nF/50V und der Elko ein Kondensator mit z.B. 22μF/35V sein. Passend zu den möglichen Strömen, die durch P6KE33A abfließen können, wurde die Diode UF4003 ausgewählt, die für ein Strom IF bis 1A genutzt werden kann, eine Sperrspannung bis 200V hat und eine geringe Durchlassspannung UF von 1V (im Vergleich zu einer 1N4003) aufweist.

Integrierte Spannungsregler sind kurzschluss- und überlastfest. Ein Low-Drop-Spannungsregler (abgekürzt LDO für Low Drop-Out) ist ein Längsregler mit einer geringeren minimal erforderlichen Differenz zwischen Ein- und Ausgangsspannung (Aussetzspannung ca. 1,7V statt z.B. 3V bei Standard-Spannungsreglern). Mit einem LM78L12A (Datenblatt: http://www.fairchildsemi.com/datasheets/LM/LM78L12A.pdf) liegen bereits ab 13,7V spannungsgeregelte 12V an. Der maximale Ausgangsstrom beträgt 100mA und die maximale Eingangsspannung darf 35V betragen.

![KFZ-Versorgungsspannung Abb. 3](../images/Versorgungsspannung_3.png)

Leider neigt ein Low-Drop-Längsregler zu Schwingungen. Das ist nicht nur für die Spannungsregler gefährlich, es können auch unerwünschte hochfrequente Schwingungen und Transienten in der Last erzeugt werden. Zur Vermeidung unerwünschten Schwingens ist ein Kondensator am Eingang einzusetzen. Im Datenblatt ist Cin mit 330nF angegeben. Zusätzlich ist ein Kondensator Cout am Ausgang mit 100nF angegeben. Er verbessert das Regelverhalten bei schnellen Lastwechseln. Cin und Cout sollten so nahe wie möglich am Regler liegen.

Der Glättungskondensator von 22μF/16V fängt kurze Lastspitzen ab und reduziert die Restwelligkeit auf der Ausgangsspannung. Eine Freilaufdiode, die den Regler vor der Zerstörung schütz, ist trotz Einsatz eines Glättungskondensator bei einem LDO nicht notwendig.

Die Beiden Z-Dioden bilden eine Schutzmaßnahme gegen Überspannung für alle CMOS-ICs:

![KFZ-Versorgungsspannung Abb. 4](../images/Versorgungsspannung_4.png)

Sobald an einem externen Eingang eine Überspannung erzeugt wird, die über eine Schutzdiodenschaltung nach 12V oder gegen Masse geleitet wird (siehe Abschnitt Kontrollanzeige), fließt ggf. ein genügend schädlicher Strom, der dazu führen kann, dass eines der ICs Schaden nimmt. Da der LDO selbst nicht in der Lage ist, einen Rückstrom zu absorbieren, werden zusätzlich zwei Z-Dioden in Serie zwischen 12V und Masse eingebracht. Die Z-Dioden D3 und D4 mit einer Z-Spannung von 6,8V sorgen dafür, dass alle Spannungen von externen Eingängen die größer als 13,6V sind, sicher abgeleitet werden. Die Strombegrenzung für die Z-Dioden erfolgt durch den jeweiligen Eingangswiderstand des jeweiligen externen Einganges.

## Quellen und weiterführende Literatur

### Links
- Mikrocontroller.net; [12V KFZ Versorgungs-Schaltung aus de.dse-faq](https://www.mikrocontroller.net/topic/392585)
- de.sci.electronics-FAQ; [F.23. Das KFZ-Bordnetz](http://www.dse-faq.elektronik-kompendium.de/dse-faq.htm#F.23)

### Startseite
Zum Anfang geht's [hier](../index.html).
