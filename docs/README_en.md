iSpindle (iSpindel) Documentation
===================

**DIY electronic Hydrometer**
***https://universam1.github.io/iSpindel***

***

***Translation is work in progress, feel free to contribute***
***This page was translated on 9 December 2016***

## Documentation in other languages
### [German Documentation (primary)](README.md)
### [Nederlandse Vertaling (lopende werkzaamheden)](docs/README_nl.md) [](#lang-nl)

***

***Please consider supporting this project***  

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/universam)

> Update 07.12.16: Circuit Diagram 
> Update 03.12.16: Firmware 2.1.2 Wlan Improvements  
> Update 29.11.16: Calibration and Excel table 
> Update 28.11.16: Firmware v.2.0 - Ubidots Auto-Configuration  
> Update 28.11.16: Ubidots Einrichtung  
> Update 23.11.16: Circuit diagram and firmware


> The ```iSpindle``` is currently under active development, please see [this Hobbybrauer.de Thread](http://hobbybrauer.de/forum/viewtopic.php?f=7&t=11235)(German). Assistance is welcome and supplements should be submitted by pull request. Many thanks to all who provide support on the basis of basic development work or to find suitable hardware.

![iSpindle in clean water](floating.jpg)
![Dashboard](Dashboard.jpg)


## Table of Contents

- [License](#license)
- [Principle](#principle)
  - [Metacentric Height](#metacentric-height)
- [Construction](#construction)
  - [Components](#components)
  - [Circuit Diagram](#schaltplan)
  - [Sled](#schlitten)
- [Configuration](#configuration)
  - [Ubitdots](#ubitdots)
  - [Portal](#portal)
- [Graphical User Interface](#graphical-user-interface)
  - [Calibrating the Spindle](#calibration)
  - [Ubidots Graphen](#ubidots-graphen)
  - [CraftBeerPi](#craftbeerpi)
- [Software](#software)


***

## License


> All rights reserverd, any commercial use is hereby prohibited and will violate applicable patents.

***

## Principle

Powered by the thread [Alternative to the Spindle](http://hobbybrauer.de/forum/viewtopic.php?f=7&t=11157&view=unread#p170499), the idea was born to reproduce the commercially available electronic spinning spindle.

The idea of the heeling (or tilting) cylinder is ingenious and easy - you do not need any external reference (except for gravity) and the cylinder is extremely easy to keep clean. The inclination angle changes in relation to the buoyancy and thus directly in relation to the sugar content.

No unnecessary opening to spindles and possibly contamination!

![Kränung](kraengung.jpg)

Therefore the idea to put an IoT device with Wifi together with an acceleration sensor and temperature sensor in a floating cylinder. There he watches all bsp. 5min, connects to my Wlan and sends its tilt angle, temperature and battery voltage to a cloud service.

### *Metacentric Height*

Actually, this is the "metacentre", the cylinder will rotate until the metacentrum in the solder is the buoyancy point. We measure this value.

It is possible to trim by adding a few grams on the ground, so that the cylinder is more upright, or on the lid, so that it is more crowded.

The software calculates the Euler angle for X and Y from the XYZ acceleration values and forms the absolute angle. We compute these with the calibrated parameters for ° Plato.


***

## Construction

>***ATTENTION: This is accurate as of 20.11.2016***

### Components

- [Wemos D1 mini](https://www.wemos.cc/product/d1-mini.html)
- ```GY-521``` Gyro & Accelerometer Board (Invensense MPU-6050 Breakout Board)
- [DS18B20 Temperature Sensor](https://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html)
- Protoboard 3x4cm
- Resistors
  - 4k7 Ohm
  - 220k Ohm
  - 470 Ohm
- Micro switches
- ```18650 LiIon Battery``` (e.g. ```Panasonic NCR18650B``` **protected** or without **PCB**) ***UNTESTED***
- Lipo Battery Charger ```TP4056``` ***UNTESTED***
- Plastic Slide (3D printable model in repository)
  - Alternatively: Protoboard PCB ***UNTESTED***)

- Plastic Cylinder ```PET```

> ## Info

> Der Anbieter [cachers-world.de](http://cachers-world.de/de/Petling-XL) unsterstützt dieses Projekt indem er nachhaltig den passenden Petling liefern möchte und über den Gutschein-Code "```HOBBYBRAUER```"  (Großschreibung!) 20% Rabatt gewährt. 
Dieser [Petling-XL](http://cachers-world.de/de/Petling-XL) passt zu dem 3D gedruckten Schlitten.
>
>Info: *"Ist im Moment dann nur 1,44 EUR ab 2017 werden es dann 1,52 EUR sein, weil der Artikel 10ct hoch geht."*

>Info 2: Zur Zeit ausverkauft, nachbestellt.

***
### Schaltplan

***siehe [Schaltplan](Schaltplan.md)***

***

### Schlitten

![Schlitten](Schlitten_cad.jpg)
![Zusammenbau](assembled2.jpg)
![Zusammenbau](assembled.jpg)

<a href="http://www.youtube.com/watch?feature=player_embedded&v=gpVarh8BxhQ" target="_blank"><img src="http://img.youtube.com/vi/gpVarh8BxhQ/0.jpg" 
alt="Druck" width="240" height="180" border="10" /></a>



***

## Configuration

### Ubitdots

- Zu Beginn muss ein kostenloser Account bei [Ubidots.com](https://ubidots.com) erstellt werden
- Im Menü  ```API Credentials``` erhält man seinen ```Token``` durch das die iSpindel die Berechtigung zum Schreiben der Daten erhält.  
***Diesen notieren.***  
![Token](UbiToken.jpg)  

> Update 28.11.16: Durch Auto - Konfiguration sind folgende Schritte nicht mehr nötig

- ~~In diesem Account erstellt man nun ein neues ```Data Source``` und benennt seine iSpindel bsp. "iSpindel001"~~ 

  ![UbiDS](UbiDS.jpg)

- ~~In diesem Device erstellt man nun 3  ```Variable``` das den 3 Datenquellen entspricht die geliefert werden~~
  - ~~Neigung (wird später zu °Plato umgerechnet)~~
  - ~~Temperatur (fließt auch in °Plato ein)~~
  - ~~Batterie Spannung~~ 

  ~~***Notieren dieser 3 ```ID's``` die man über das  ```i``` Icon erhält***~~

  ![IDs](UbiIDs.jpg)~~


### Portal

Durch mehrmaliges Drücken der ```Reset Taste``` erstellt der Wemos einen AccessPoint, mit dem verbunden man die nötigen Einstellugen vornehmen kann.

> Die ```iSpindel``` signalisiert dass sie sich im *Konfiguration-Modus* druch permanentes Blinken im Sekundentakt.  
Man verlässt den *Konfiguration-Modus* durch speichern seiner Einstellungen, durch betätigen des Menüpunkts ```Start iSpindel``` oder durch warten von 5 Minuten. Danach befindet sie sich im *Betriebsmodus* d.h. sie sendet ihre Daten und geht daraufhin direkt in den "Deep Sleep" Standby Modus. Daher ist sie im normalen Modus nicht erreibar.

- Der Ubidots  ```Token``` ~~und die  ```IDs```~~, welche man oben notiert hat, werden nun an dieser Stelle eingetragen.  

- Ebenfalls stellt man hier den ```Intervall``` ein in dem sie Daten liefert. Dies hat direkt mit der Akku Lebensdauer zu tun. Es empfiehlt sich in der Praxis etwa ```1800``` Sekunden (= 30 Minuten) Takt zu wählen.

   ![Setup](setup.jpg)


- Man erreicht es über

   ![AccessPoint](AP.png)![Portal](Portal.png)


- Eine Übersicht der Daten kann man über den ```Info``` Menüpunkt einsehen

  ![Info](info.png)

> Nach dem man obige Daten eingetragen und gespeichert hat, wird die Spindel sich mit dem Wlan und Ubidots verbinden und die Daten übertragen.  
Auf der Ubidots Weboberfläche wird man nun unter ```Sources``` sehen dass die Daten aktualisiert werden.  
Nun kann man im ```Dashboard``` sich seine Graphen nach Belieben zusammenstellen.

***
## Graphical User Interface

### Calibration

> In order to convert the measured iSpindle angle to degrees Plato (°Plato), density (SG), or other units, it is necessary to first calibrate the sensor by making several reference measurements of sugar water of known gravities. These reference measurements can then be converted to a mathematical function which is stored for later measurements and display. Since each self-assembled iSpindle will  have different measured values, each device must be individually calibrated after assembly or reassembly to yield accurate measurements. A detailed procedure is below.

[Calibration Procedure](Calibration_en.md)

### Ubidots Graphen

- [Plato Formula](Calibration_en.md#forumla)

### CraftBeerPi

[Work in Progress](https://github.com/universam1/iSpindel/issues/3)
***

## Software 

### Firmware flashing

[Firmware flashing](Firmware.md)

### Benutzte Bibliotheken

- https://github.com/tzapu/WiFiManager zum Herstellen der Verbindung (verändert)
- https://github.com/bblanchon/ArduinoJson

***Gefällt es dir, na dann spende mir halt ein Bier***  :beers:

[![Donate](https://www.paypalobjects.com/de_DE/DE/i/btn/btn_donate_LG.gif)](https://www.paypal.me/universam)

