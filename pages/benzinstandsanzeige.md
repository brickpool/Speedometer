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
  Vbat       Vcc                              Vcc
    |         |                                |
   .-/        |                      ___      .-.
   |/| NTC    = 1N4148           .--|___|--.  | |10k
   /-'        ^                  |   100k  |  '-'    .----o A
    |         |         10k___   |         |   |     |
  E o         |   Vref <--|___|--+---|+\   |   |   |/c
    |   ___   |            ___  LM393|  >--+---+---|  BC547b
    +--|___|--+---------+-|___|------|-/       |   |\e
    |   10k   |         |  1k                  V     |
   .-.        |        _|_              1N4148 =    .-.
   | |195     = 1N4148 --- 1n                  |    | |10
   '-'        ^         |                      V    '-'
    |         |         |                BAT41 =     |
 o--+---------+---------+----------------------+-----'
    |
   ===
```

Der Invertierende [Schmitt-Trigger](http://de.wikipedia.org/wiki/Schmitt-Trigger) wird mit Hilfe eines Komparators (hier LM393) realisiert.

Die Referenzspannung wird mittels einer Referenzspannung von 4,3V über einen Eingangswiderstand am positiven Eingang des Komperators angelegt. Ein zusätzlicher Widerstand (hier 100k) sorgt für die Mitkopplung und damit für die Hysterese welche im Verhältnis zum Einsgangswiderstand berechnet wird.

  R(hys) = Re * (2 * Vcc/(V(high)-V(low)) - 1)

Ist die Referenzspannung (wie hier) die Hälfte von Vcc kann folgende vereinfachte Formel verwendet werden, um die Hysterese zu berechnen:

  V(hys) = Vref * (Re/R(hys) + 1) = V(high) - V(low)

Mit einem Eingangswiderstandswert von 10k und einem Widerstand von 100k für die Hysterese, wird bei Nutzung einer Versorgungsspannung von Vcc = 8,6V und einer Referenzspannung von Vcc/2 folgendes Verhalten erzielt:
- Sobald am Eingang die Spannung von 4,1V unterschritten (entspricht V(low)) wird, geht der Komperator in die positive Sättigung und der NPN Transistor leitet. Am Ausgang liegt ein 10 Ohm Widerstand gegen Masse.
- Sofern 4,5V überschritten werden (entspricht V(high)), geht der Komperator in die negative Sättigung und der Transsitor sperrt. Der Ausgang ist somit unbeschaltet.

Die in Serie geschaltete Schottky Diode BAT41 und Kleinsignaldiode 1N4148 dienen zusammen als Strombegrenzung (ca. 30mA) für den 10 Ohm Widerstand, sprich den über Ausgang _A_ zugeführten Strom.

## Quellen und weiterführende Literatur

### Links
- Duc-Forum.de; [Tankkontrollleuchte LED für alte SS/Monster funzt](http://www.duc-forum.de/thread.php?threadid=71131)
- Wikipedia; [Heißleiter](https://de.wikipedia.org/wiki/Hei%C3%9Fleiter)
- Wikipedia; [Elektrische Eigenschaften einer Glühlampe](https://de.wikipedia.org/wiki/Gl%C3%BChlampe#Elektrische_Eigenschaften)
- ElectronicsTutorials; [OPV-Komparator](https://www.electronics-tutorials.ws/de/operationsverstarker/opamp-komparator.html)

### Nächste Seite
Weiter geht's mit [Zündsignalwandler](zuendsignalwandler_1.html).
