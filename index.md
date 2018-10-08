---
layout: page
title: Tachokombiinstrument
description: Universal LCD Motorrad Tachometer
---

Bei eBay werden diverse "Universal LCD Motorrad Tachometer" mit Drehzahlmesser und Kilometerzähler angeboten.

![Tachokombiinstrument](images/Tachometer.jpg)

Dass die Tachometer häufiger nicht direkt funktionieren bzw. man sich die wichtigen Infos selber besorgen muss, ist leider bei den China Sachen häufiger der Fall. Den Kommentaren nach, haben Einige den Tachometer zum Laufen bekommen.

## Instrumentenseite
### 20pol Buchse Molex 5557
![20pol Buchse Molex 5557](images/20pol_Molex_5557.png)

## Belegungsplan Kabelbaumseite
### 9pin Buchse DJ7091A-2.8-21
![9pin Buchse DJ7091A-2.8-21](images/9pol_DJ7091A-2.8-21.png)

1. hellblau - Blinker rechts, (+) geschaltet
2. blau - Fernlichtkontrolle, (+) geschaltet
3. braun/weiß - Lichtkontrollanzeige, (+) geschaltet
4. orange - Blinker links, (+) geschaltet
5. gelb/schwarz - Drehzahlmesser
6. grün - Masse (-)
7. blau/weiß - Tankanzeige
8. rot - Dauerplus (+) von Batterie
9. schwarz - Plus (+) geschaltet vom Zündschloss

### 3pin Buchse DJ7031A-2.8-21
![3pin Buchse DJ7091A-2.8-21](images/3pol_DJ7091A-2.8-21.png)

1. rot/weiß - Positiver Anschluss (+) für Wegstreckensensor
2. grün - Masse Anschluss (-) für Wegstreckensensor
3. schwarz/weiß - Impulsgeber (Hallsensor) für Wegstrecke

### 6pin Buchse DJ7061A-2.8-21
![6pin Buchse DJ7091A-2.8-21](images/6pol_DJ7091A-2.8-21.png)

1. grün/rot - Leerlaufkontrollanzeige (Neutral), gegen Masse zu schalten
2. rosa - Ganganzeige 1. Gang, gegen Masse zu schalten
3. blau/rot - Ganganzeige 2. Gang, gegen Masse zu schalten
4. grün/schwarz - Ganganzeige 3. Gang, gegen Masse zu schalten
5. gelb/rot - Ganganzeige 4. Gang, gegen Masse zu schalten
6. gelb/weiß - Ganganzeige 5. Gang, gegen Masse zu schalten

### 2pin Buchse DJ7021A-2.8-21

1. n/a - nicht belegt
2. grün/weiß - Ganganzeige 6. Gang, gegen Masse zu schalten

An die Tankanzeige kann auf einen Sensor für 100 Ohm oder 500 Ohm angeschlossen werden. Im Instrument ist per Programmierung der Messsensor für 100 Ohm voreingestellt. Laut Datenblatt erfolgt der Bereich für die 100 Ohm Anzeige zwischen 98 Ohm (Leer) und 8 Ohm (Voll). Das Instrument erwartet eine Anschaltung gegen Masse. Eine Abschaltung der Tankanzeige ist nicht vorgesehen.

Messungen haben ergeben, dass ein offener Anschluss und alle Werte kleiner 10 Ohm und größer 90 Ohm zu einem Blinken der Anzeige führen. Obwohl keine Tankanzeige benötigt, muss der Anschluss für die Tankanzeige passend beschaltet werden, da sonst eine blinkende Tankanzeige am Instrument stört. Die einfachste Methode ist es, dass Instrument auf einen Sensor von 500 Ohm umzuprogrammieren und mittels 470 Ohm Widerstand den Anschluss für die Tankanzeige gegen Masse zu schalten.

Lichtkontrollanzeige soll später für Motorkontrollanzeige genutzt werden. Die Lichtkontrollanzeige wird gegen Plus geschaltet, um sie zur Anzeige zur bringen. Messungen an der Lichtkontrollanzeige zeigen, dass die Lampe bereits bei einer Anschaltung von 3,5V zu glimmen beginnt. Hierzu muss das Instrument nicht eingeschaltet sein. Ab ca. 5V leuchtet die Kontrollleuchte gut sichtbar.

Es wurde folgende Messreiche aufgenommen:

Nr. | Spannung | Strom (Bemerkung)
--- | --- | ---
1 | 3,5V | 0,4mA (Kontrollleuchte glimmt)
2 | 4,9V | 1mA
3 | 10V | 3,4mA (unterer Arbeitsbereich vom Instrument laut Datenblatt)
4 | 11,9V | 4,36mA (unterer Schaltpunkt für Batterieanzeige im Instrument)
5 | 12V | 4,4mA
6 | 13,8V | 5,27mA (Ladeschlussspannung für 12V Blei-Batterie)
7 | 15V | 5,85mA (oberer Schaltpunkt für Batterieanzeige und oberer Arbeitsbereich)

![Lichtkontrollanzeige](images/Lichtkontrollanzeige.png)

Der Innenwiderstand RI der Kontrollanzeige beträgt bei 12V ca. 2,7kOhm

![Innenwiderstand](images/Innenwiderstand.png)

Bei der durchgeführten Messung wurde auch der Arbeitsbereich für Batterieanzeige im Instrument ermittelt: 11,9V bis 15V. Die Werte stimmen durchaus mit der Praxis. Eine fast entladene Batterie hat eine Spannung von 11,89V. Das Gasen einer 12V Blei-Batterie beginnt bei 14,4V.

Das Signal für den Drehzahlmesser kommt von Klemme 1 der Zündspule über einen speziellen Zündsignalwandler. Das Kombiinstrument hat einige Überraschungen und im Netz finden sich einige Hinweise auf missglückte Integrationen. Hauptkritikpunkt am Kombiinstrument ist, dass entgegen der Verkäuferaussagen eine einfache Anschaltung an eine Einzylinder-4-Takt-Maschine nicht möglich ist und somit für die Guzzi eine aufwändige Anpassung für die korrekte Darstellung der Drehzahl erfolgen muss, da jeder Zylinder eine eigene Zündspule hat.

## Quellen und weiterführende Literatur

### Links
- Cafe Racer Forum; [Kennt ihr dieses Instrument aus ebay...?](http://www.caferacer-forum.de/viewtopic.php?f=43&t=15340&p=234461)
- Banggood.com; [12000RMP LCD Digital Speedometer Odometer Motorcycle 1-4 Cylinders](http://www.banggood.com/en/12000-RMP-LCD-Digital-Speedometer-Odometer-Motorcycle-1-4-Cylinders-p-972677.html)

### Nächste Seite
Weiter mit [Lade- und Öldruckkontrollanzeige](pages/kontrollanzeige.html).
