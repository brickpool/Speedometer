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
---- | ----- | ----------------- | --------------------------
voll | 10mA  | 12,4V             | 0,2V
leer | 78mA  | 2,2V              | 10,4V (Kontrollleuchte an)

```
  Vbat        .---------+-------------------+---------->> 7V5
    |         |         |                   |
   .-/        |        .-.        ___      .-.
   |/| NTC    = 1N4148 | |22k .--|___|--.  | |10k
   /-'        ^        '-'    |   33k   |  '-'    .-----o A
    |         |         |     |         |   |     |
  E o         |         +-----+---|+\   |   |   |/c
    |   ___   |   ___   |    LM393|  >--+---+---|  BC547b
    +--|___|--+--|___|--)---------|-/       |   |\e
    |   10k   |    2k   |                   V     |
   .-.        |        .-.           1N4148 =    .-.
   | |195     = 1N4148 | |27k               |    | |10
   '-'        ^        '-'                  V    '-'
    |         |         |             BAT41 =     |
 o--+---------+---------+-------------------+-----'
    |
   ===
```

Der Invertierende [Schmitt-Trigger](http://de.wikipedia.org/wiki/Schmitt-Trigger) wird mit Hilfe eines Komparators (hier LM393) realisiert.

Die Referenzspannung wird mittels der beiden Widerstände 22k und 27k am positiven Eingang des Komperators angelegt. Der Widerstand mit dem Wert von 33k sorgt für die Mitkopplung und damit für die Hysterese welche im Verhältnis zum Einsgangswiderstand (ca. 12k = `22k||27k`) berechnet ist.

Die drei oben genannten Widerstandswerte sind so ausgelegt, dass bei Nutzung einer Versorgungsspannung von 7,5V folgendes Verhalten erzielt wird:
- Wird am Eingang die Spannung von 3V unterschritten, geht der Komperator in die positive Sättigung und der NPN Transistor leitet. Am Ausgang liegt der 10 Ohm Widerstand gegen Masse.
- Sofern 5V überschritten werden, geht der Komperator in die negative Sättigung und der Transsitor sperrt. Der Ausgang ist somit unbeschaltet. 

## Quellen und weiterführende Literatur

### Links
- Wikipedia; [Heißleiter](https://de.wikipedia.org/wiki/Hei%C3%9Fleiter)
- Wikipedia; [Elektrische Eigenschaften einer Glühlampe](https://de.wikipedia.org/wiki/Gl%C3%BChlampe#Elektrische_Eigenschaften)
- Duc-Forum.de; [Tankkontrollleuchte LED für alte SS/Monster funzt](http://www.duc-forum.de/thread.php?threadid=71131)

### Nächste Seite
Weiter geht's mit [Zündsignalwandler](zuendsignalwandler_1.html).
