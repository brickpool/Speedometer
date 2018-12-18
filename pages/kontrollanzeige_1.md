---
layout: page
title: Ladekontrollanzeige
description: Universal LCD Motorrad Tachometer
---

Die Guzzi sowie die Ducati Monster hat eine Ladekontrollleuchte, die im Original zwischen Anschluss Kl.61 vom Gleichrichter und der positiven Versorgungsspannung (12V) vom Instrument liegt. Es kann zwar nicht der Ladezustand oder die aktuelle Spannung geprüft werden, aber es gibt zumindest eine Anzeige, dass die Batterie bei laufenden Motor geladen wird.

Da sowohl der Regler und Gleichrichter sowie das Instrument ausgetauscht werden, ist eine Anpassung der Funktion Ladekontrollleuchte notwendig. Das China-Instrument bietet leider keine eigene Ladekontrollleute. Da die Tachobeleuchtung als Kontrolle ob Licht eingeschaltet ist ausreicht, wird zukünftig die Lichtkontrollleuchte als Anzeige genommen.

Die Kontrollleuchte soll immer zur Anzeige kommen, wenn mehr Strom der Batterie entnommen wird als hinein fließt. Hierzu wird der Schaltpunkt für die Anzeige einfach über die Ladeschlussspannung definiert. Bei einer 12V Blei-Batterie beträgt die Ladeschlussspannung bei Schonladung 13,8V. Der Generatorspannung muss aber mindestens 13,4V liefern, damit mindestens eine Erhaltungsladung stattfindet. Eine **vollgeladene** Batterie sollte 12,8 V haben, bei ca. 12,6V ist Sie **normal geladen**, bei ca. 12,4V **schwach geladen**, bei ca. 12,0 V **normal entladen** und bei Werten unterhalb von 11,8 V **tief entladen**.

Die Guzzi bzw. Ducati besitzt darüber hinaus eine Öldruckanzeige. Der zugehörige Sensor wird bei Problemen gegen Masse geschaltet und bringt im Original im Instrument eine Lampe zur Anzeige, die an die Versorgungsspannung angeschlossen ist.

Die folgende Beispielschaltung soll an einem Fahrzeug mit 12V-Netz eine Kontrollleuchte im Instrument ansteuern. Die Kontrollleuchte La1 wird bei einer Spannung von unter 12,6V (+/- 5%) an [Klemme 61](http://de.wikipedia.org/wiki/Klemmenbezeichnung#Beleuchtung) eingeschaltet. 

![Ladekontrollanzeige](../images/Kontrollanzeige.png)

## Unterspannungserkennung der Spannung an Kl.61
Die Schwellwertmessung auf Basis eines Differenzverstärkers. Die Schaltung arbeitet im Prinzip als Komperator. Die zu messende Spannung wird mit einem Spannungsteiler an den positiven Eingang des Differenzverstärkers angelegt. Die Referenzspannung Uref wird unter Nutzung einer Z-Dode von 6,2V und eines Vorwiderstand R5 an den negativen Eingang des Differenzverstärkers gelegt.

![Messen der Spannung an Kl.61](../images/Messung_Spannung_Kl.61.png)

Die Z-Diode soll bei allen Spannungen (Arbeitsbereich vom Instrument 10-15V) zuverlässig arbeitet. Um einen stabilen Temperaturkoeffizient für die Z-Diode zu erhalten (siehe Datenblatt) soll R5 so dimensioniert sein, dass der Strom durch die Z-Diode ca. 1 mA beträgt. 

    R3(max) = (Umin-Ube-Uz)/Iz = 10V-0,2V-2,7V / 1mA = 7,1kOhm
    R3(min) = (Umax-Ube-Uz)/Iz = 15V-0,2V-2,7V / 1mA = 12,1kOhm

Bei einer Versorgungsspannung von 10-15V und einem Widerstand R3 von ca. 10kOhm beträgt der Kollektorstrom ca. 0,7mA (bei 10V) bis 1,2mA (bei 15V).

## Schalten der Versorgungsspannung
Die Kontrollleuchte ist im Instrument gegen Masse geschaltet. Sie erfordert also das schalten Versorgungsspannung. Da dies weiterhin elektronisch passieren soll, nutzen wir hierzu ein PNP-Transistor. Hier liegt der Emitter an der Versorgungsspannung vom Fahrzeug (Kl. 30).

![Schalten der Versorgungsspannung Abb. 1](../images/Schalten_der_Versorgungsspannung_1.png)

Zur Anwendung kommt ein Transistor Typ BC557B (Datenblatt: [BC560.pdf](https://www.fairchildsemi.com/ds/BC/BC560.pdf)) der die Kontrollleuchte La1 bei 12V mit einer gemessenen Last von 2,7kOhm schalten soll. Vorteil des Transistors hier ist, dass der Spannungsabfall Uce(sat) am Transistor bei _Durchschaltung_ nur ca. 0,3V bis 0,65V beträgt. 

Die Steuerspannungen Ube am Transistor bezieht sich bei einem PNP-Transistor auf die positive Versorgungsspannung und nicht auf Masse. Ein PNP immer schaltet dann durch, wenn Vbe die Schaltschwelle unterschreiten, also Uin um Ube kleiner ist als die Spannung Ue am Emitter.

Der Basiswiderstand R6 berechnet sich aus `(Ubat-Ube) / Ib`. Im Datenblatt findet sich für den BC557 Typ **B** eine Gleichspannungsverstärkung von 200. Mit einem Überbuchungsfaktor ü = 3 erhält man die gewünschte Stromverstärkung `Ic/Ib` von `200/3 = 66`. Da nur kleine Ströme fließen (Ic < 10 mA und somit Ib < 0,5 mA) wird Uce(sat) mit dem im Datenblatt zugehörigen angegeben Wert von 0,3V angesetzt.

Die Versorgungsspannung vom Instrument beträgt 10V bis 15V. Etwa 0,7V fallen an der Basis-Emitter-Strecke Ube ab (bei Ic = 10mA), also wird bei einer niedrigen Versorgungsspannung von 10V für eine vernünftige Durchschaltung ein passender Widerstand R6 benötigt.

    Ib(min) = Ic(min) / 66
            = 10 mA / 66
            = 0,15mA

    R6(max) = Ubat(min) – Ube / Ib(min)
            = (10V – 0,7V) / 0,15mA
            = 62kOhm

Der höchst zulässige Kollektorstrom Ic für den Transistortyp beträgt max. 100mA mit einem zugehörigen Basisstrom Ib von 5mA (siehe Datenblatt). In dem Sättigungsbereich fallen bei hohen Strömen an der Basis-Emitter-Strecke 0,9V ab. Bei Versorgungsspannung von 15V ist der hierzu notwendige Basiswiderstand R6 ebenfalls zu berechnen.

    R6(min) = Umax – Ube / Ib(max)
            = (15V – 0,9V) / 5mA
            = 2,8kOhm

Für unseren Anwendungsfall verwenden wir für R6 ein Widerstand von 10kOhm der einen Basisstrom von ca. 0,9mA (bei 10V) bis 1,4mA (bei 15V) hervorruft.

Damit die Transistorstufe zuverlässig arbeitet, kommt zusätzlich der Widerstand R7 zu Anwendung. Er vermeidet, dass der Transistor bei offenenem Eingang durch Störeinstrahlung teilweise leitet, indem er den Basisanschluss auf die Versorgungsspannung vorspannt, so dass der Transistor sperrt. In unserem im Anwendungsfall verwenden wir für R7 ein Widerstand von 6,8kOhm. Der Inverter erhält noch eine zusätzliche D3 um die Schaltschwelle von 0,7V anzuheben. Die Differenz zur Schaltschwelle der Messstufe beträgt somit ca. 1V.

### Strombegrenzung am Ausgang
Da die Anschaltung von La1 erfolgt extern und der PNP-Transistor bei einer Fehlbeschaltung vom Kollektor nicht zerstört wird, wird eine Strombegrenzung eingebracht.

![Schalten der Versorgungsspannung Abb. 2](../images/Schalten_der_Versorgungsspannung_2.png)

Widerstand R8 und Z-Diode begrenzen den maximalen Kollektorstrom Ic(max) von La1 z.B bei _Kurzschluss_. Die Z-Diode bewirkt, dass an der Basis von T2 nur eine maximale Spannung von Uf = 3,3V erreicht werden kann. Die zugehörige Basis-Emitter-Spannung Vbe von T2 wird mit 0,7V angesetzt. Der maximale Strom wird erreicht, wenn der Spannungsabfall an R8 den Wert von 2,6V (Uf - Ube) erreicht. Der Widerstand R8, für einen vorgebenen maximalen Strom von 25mA, lässt sich wie folgt berechnen:

    R8(min) = (Uf - Ube) / Ic(max) = 3,3V – 0,7V / 25mA = 104 Ohm

Für R8 wird ein Wert von 100 Ohm gewählt, der den Strom Ic(max) auf ca. 26 mA begrenzt. Die maximale Verlustleistung für R8 unter _Last_ beträgt weniger als 1/2 Watt:

    Pmax = (Ubat(max) - Uce(sat)) * Ic
         = (15V – 0,3V) * 10mA
         = 382mW

Die Verlustleistung am Transistor beträgt bei _Kurzschluss_ ebenfalls nur ca. 0,3W. Die Berechnung erfolgt über die Kollektor-Emitter-Spannung Uce = Ubat(max) - U(R8) bei maximalen Strom Ic(max):

    Pmax = (Ubat(max) - U(R8)) * Ic
         = (15V – 2,6V) * 26A
         = 322mW

## Einbringen einer Hysterese
Der Differenzverstärker wird bei minimalen Über- oder Unterschreiten der Eingangsspannung hin und her kippen. Durch Einbringung definierter Schaltschwellen, die sich voneinander durch eine entsprechende Spannungsdifferenz unterscheiden, kann jedoch das Gesamtverhalten gegenüber Rauschen oder Störsignale verbessert werden.

Die Erzeugung dieser Schalthysterese kann mit Hilfe einer Mitkopplung erreicht werden. In unserem Fall indem ein Teil der Ausgangsspannung an den positiven Eingang des Differenzverstärkers zurückgeführt wird. Dazu wird lediglich ein Widerstand (R9) benötigt.

Eine solche Schaltung wird als (nicht-invertierender) [Schmitt-Trigger](https://de.wikipedia.org/wiki/Schmitt-Trigger) bezeichnet. Berechnet wird die Hysterese durch Nutzung folgender Formeln (bei Uin ca. Ubat und unter Vernachlässigung von R6, bei R9 viel größer R6):

    U(low) = Uref * (R1||R9 + R2) / R2
           = Uz * (R1/R2 * R9/(R1+R9) + 1)
           = 6,2V * (10k/10k * 100k/(10k+100k) + 1)
           = 11,8V

    U(high) = Uref * (R1 + R2||R9) / (R2||R9)
            = Uz * (R1/R2 * (R2+R9)/R9 + 1)
            = 6,2V * (10k/10k * (10k+100k)/100k + 1)
            = 13,0V

Wird am Eingang die Spannung von 13V überschritten, geht der Differenzverstärker in die positive Sättigung und der Ausgang ist logisch _High_, bei unterschreiten der Spannung von 11,8V, geht er in die negative Sättigung und der Ausgang ist logisch _Low_.

Zum Schutz vor positiven und negativen Spannungsspitzen sind zusätzlich Dioden vom Typ 1N4148 ampositiven Eingang des Differenzverstärkers eingesetzt. Der Kondensator C1 bildet zusammen mit R1 ein Tiefpass, damit keine Störsignale an der Basis vom Transistor T1 gelangen.

## Quellen und weiterführende Literatur

### Links
- Elektroniktutor.de; [Regelverstärker zur Spannungsstabilisierung](https://elektroniktutor.de/analogtechnik/regelvst.html)
- Elektronik-Kompendium; [Schalten und Steuern mit Transistoren I](http://www.elektronik-kompendium.de/public/schaerer/powsw1.htm)
- DL6GL von Georg Latzel; [Schalten der Versorgungsspannung](http://dl6gl.de/grundlagen/schalten-mit-transistoren)
- Wikipedia; [Starterbatterie](http://de.wikipedia.org/wiki/Starterbatterie#Wartung,_Pflege_und_Prüfung)
- Wikipedia; [Konstantstromquelle](http://de.wikipedia.org/wiki/Konstantstromquelle#Mit_Bipolartransistor)
- Mikrocontroller.net; [Unterspannungsabschaltung gesucht](http://www.mikrocontroller.net/topic/340319#3744991)

### Nächste Seite
Weiter geht's mit [Öldruckkontrollanzeige](kontrollanzeige_2.html).
