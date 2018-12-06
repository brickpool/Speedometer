---
layout: page
title: Benzinstandsanzeige Ducati
description: Universal LCD Motorrad Tachometer
---

Die Guzzi besitzt keinen Geber und keine Reservelampe. Ein offener Anschluss und Werte größer 90 bzw. 500 Ohm bringen die Füllstandsanzeige des Kombiinstrumentes zum Blinken. Der Anschluss für die Tankanzeige muss daher passend beschaltet werden, da sonst eine blinkende Tankanzeige am Instrument stört. Die einfachste Methode ist es, dass Instrument auf einen Sensor von 500 Ohm umzuprogrammieren und mittels 470 Ohm Widerstand den Anschluss für die Tankanzeige gegen Masse zu schalten. Es wird dann nur der untere Balken angezeigt.

Die Ducati Monster verwendet ein Heißleiter (abk. NTC) als Benzinstandgeber. Der Sensor wird bei ausreichend gefüllten Tank vom Benzin umflossen und ist dadurch kalt. Der innenliegende Heißleiter leitet somit schlecht und verhindert so das Einschalten der zugehörigen Kontrolllampe im Original-Instrument. Sofern der Sensor nicht ausreichend von Benzin umgeben ist, erwärmt sich der Heißleiter (langsam) durch den Stromfluss und verliert (almählich) seinen hohen Anfangswiderstand.

Aufgrund der Kaltleitereigenschaft einer Glühlampe kommt es zu einem zügigen Umschalten, sofern die Glühwendel auf Betriebstemperatur aufgeheizt ist. Mit steigender Temperatur erhöht sich der elektrische Widerstand und somit der Spannungsabfall an der Glühlampe, womit die Glühlampe die volle elektrische Leistung erhält. 

Die Bauteile unterliegen zwar einer Exemplarstreuung, doch durch die venwendete kombination aus Heißleiter-Sensor und Glühlampen-Kaltleiter kann eine sehr steile Kennlinie für einen bestimmten Temperaturbereich erzielt werden. 

Es wurden folgende Messwerte (bei einer Batteriespannung von 12,6V) aufgenommen:

Tank | Strom | Spannung am Geber | Spannung a.d. Glühlampe
---- | ----- | ----------------- | --------------------------
voll | 10mA  | 12,4V             | 0,2V
leer | 78mA  | 2,2V              | 10,4V (Kontrollleuchte an)

## Tanklevelanzeige
Die Tanklevelanzeige vom Kombiinstrument arbeitet auf Widerstandbasis. Zur Bestimmung des Widerstanswerte misst das Instrument die Spannung bei einem vorgegebenen konstanten Strom.

Wie bereits weiter oben beschrieben verhält sich der Benzinstandsgeber der Ducati nicht wie ein _Widerstandsgeber_, sondern eher wie ein _Schalter_. Die folgende Schaltung kann daher nur die beiden Zuständen **leer** (Balkenanzeige blinkt, entspricht Widerstand unendlich) oder **voll** (volle Balkenanzeige, Widerstand 100 Ohm) anzeigen. 

![Benzinstandsanzeige](../images/Benzinstandsanzeige_2.png)

Die Benzinstandsanzeige wird mit Hilfe eines zweistufig invertierenden Schaltverstärker realisiert. Das Eingangssignal wird mittels eines Spannungsteilers an die Basis des Transistors der ersten Schaltstufe angelegt. Die Umschaltspannung Vu liegt bei ca. 3V. In Abhängigkeit von der Eingangsspannung wird folgendes Verhalten erzielt: 
- Sobald am Eingang die Spannung von 3V unterschritten wird (Vin < Vu), sperrt der Transistor und am Ausgang liegt der 100 Ohm Widerstand über den Transistor der zweiten Stufe gegen Masse.
- Sofern die 3V überschritten werden (Vin > Vu) leitet der Transistor der ersten Stufe und der Transitor der zweiten Stufe sperrt. Der Ausgang ist somit unbeschaltet.

Das Verhältnis zwischen Vu und Vbe (= 0,7V) bestimmt das Verhältnis der Widerstandswerte vom Eingangsspannungteiler:

    R1 = R2 * (Vu/Vbe - 1)
       = 3,3k * (3V/0,7V - 1)
       = 10k

Mit einem Eingangswiderstandswert von 10k und einem Widerstand von 3,3k gegen Masse fließt bei Nutzung einer Versorgungsspannung von Vbat = 8..35V und bei Vin > Vu ein ausreichend großer aber nicht zu großer Basisstrom durch den Transistor der ersten Schaltstufe.

Die 3,3V Z-Diode dient als Strombegrenzung (ca. 25-30mA) für den 100 Ohm Widerstand, sprich den über Ausgang _Out_ zugeführten Strom.

## Quellen und weiterführende Literatur

### Links
- Duc-Forum.de; [Tankkontrollleuchte LED für alte SS/Monster funzt](http://www.duc-forum.de/thread.php?threadid=71131)
- Wikipedia; [Heißleiter](https://de.wikipedia.org/wiki/Hei%C3%9Fleiter)
- Wikipedia; [Elektrische Eigenschaften einer Glühlampe](https://de.wikipedia.org/wiki/Gl%C3%BChlampe#Elektrische_Eigenschaften)
- ElectronicsTutorials; [OPV-Komparator](https://www.electronics-tutorials.ws/de/operationsverstarker/opamp-komparator.html)

### Nächste Seite
Weiter geht's mit [Zündsignalwandler](zuendsignalwandler_1.html).
