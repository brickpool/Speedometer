---
layout: page
title: Benzinstandsanzeige Ducati
description: Universal LCD Motorrad Tachometer
---

Die Guzzi besitzt keinen Geber und keine Reservelampe. Ein offener Anschluss und Werte größer 90 bzw. 500 Ohm bringen die Füllstandsanzeige des Kombiinstrumentes zum Blinken. Der Anschluss für die Tankanzeige muss daher passend beschaltet werden, da sonst eine blinkende Tankanzeige am Instrument stört. Die einfachste Methode ist es, dass Instrument auf einen Sensor von 500 Ohm umzuprogrammieren und mittels 470 Ohm Widerstand den Anschluss für die Tankanzeige gegen Masse zu schalten. Es wird dann nur der untere Balken angezeigt.

Die Ducati Monster verwendet ein Heißleiter (abk. NTC) als Benzinstandgeber. Der Sensor wird bei ausreichend gefüllten Tank vom Benzin umflossen und ist dadurch kalt. Der innenliegende Heißleiter leitet somit schlecht und verhindert so das Einschalten der zugehörigen Kontrolllampe im Instrument. Sofern der Sensor nicht ausreichend von Benzin umgeben ist, erwärmt sich der Heißleiter (langsam) durch den Stromfluss und verliert (almählich) seinen hohen Anfangswiderstand.

Aufgrund der Kaltleitereigenschaft einer Glühlampe kommt es zu einem zügigen Umschalten, sofern die Glühwendel auf Betriebstemperatur aufgeheizt ist. Mit steigender Temperatur erhöht sich der elektrische Widerstand und somit der Spannungsabfall an der Glühlampe, womit die Glühlampe die volle elektrische Leistung erhält. 

Die Bauteile unterleigen zwar einer Exemplarstreuung, doch durch die venwendete kombination aus Heißleiter-Sensor und Glühlampen-Kaltleiter kann eine sehr steile Kennlinie für einen bestimmten Temperaturbereich erzielt werden. 

Es wurde folgende Messwerte (bei einer Batteriespannung von 12,6V) aufgenommen:

Tank | Strom | Spannung am Geber | Spannung a.d. Glühlampe
--- | --- | --- | ---
voll | 10mA | 12,4V | 0,2V
leer | 78mA | 2,2V | 10,4V (Kontrollleuchte an)

## Quellen und weiterführende Literatur

### Links
- Wikipedia; [Heißleiter](https://de.wikipedia.org/wiki/Hei%C3%9Fleiter)
- Wikipedia; [Elektrische Eigenschaften einer Glühlampe](https://de.wikipedia.org/wiki/Gl%C3%BChlampe#Elektrische_Eigenschaften)
- Duc-Forum.de; [Tankkontrollleuchte LED für alte SS/Monster funzt](http://www.duc-forum.de/thread.php?threadid=71131)

### Nächste Seite
Weiter geht's mit [Zündsignalwandler](zuendsignalwandler_1.html).
