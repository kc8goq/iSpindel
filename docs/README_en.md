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
  - [Circuit Diagram](#circuit-diagram)
  - [Sled](#sled)
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

Powered by the thread [Alternative to the Spindle](http://hobbybrauer.de/forum/viewtopic.php?f=7&t=11157&view=unread#p170499), the idea was born to reproduce the commercially available electronic tildting spindle using low-cost components.

The system is based around the use of a heeling (or tilting) cylinder, an ingenious and easy concept dervied from ships - you do not need any external reference (except for gravity) and the cylinder is extremely easy to keep clean. The inclination angle changes in relation to the buoyancy and thus directly in relation to the sugar content. That is, there is an angle formed between the center of mass and the center of bouyancy depending on the density of the fluid.


![Kränung](kraengung.jpg)

Therefore the idea  is to place a Wifi-enable IoT device with an accelerometer and temperature sensor in a floating cylinder. The system will measure the sensors and every 5 minutes it will connect to the Wlan and sends its tilt angle, temperature and battery voltage to a cloud service to store the data.

### *Metacentric Height*

Actually, this is the "metacentre", the cylinder will tilt as the liquid density changes in relation to its center of mass and center of bouyancy. The angle of tilt can then be measured. 

It is possible to trim the cylinder by adding a few grams on the bottom so that the cylinder is more upright, or on the lid, so that it is more tilted.

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
### Circuit Diagram

***Please see (German) [Circuit Diagram](Schaltplan.md)***

***

### Sled
- A 3D Printed sled is used to secure the electronics and battery inside the plastic housing as shown below. The 3D model can be found in the repository.
![Sled](Schlitten_cad.jpg)
![Assembled](assembled2.jpg)
![Assembled](assembled.jpg)

<a href="http://www.youtube.com/watch?feature=player_embedded&v=gpVarh8BxhQ" target="_blank"><img src="http://img.youtube.com/vi/gpVarh8BxhQ/0.jpg" 
alt="Druck" width="240" height="180" border="10" /></a>



***

## Configuration

### Ubidots

- To start, you must create a free account at [Ubidots.com](https://ubidots.com)
- Next, you must go to the menu  ```API Credentials``` to get a ```Token``` to be used by the iSpindle to authorize writing data to the Ubidots account.
***Write this down.***  
![Token](UbiToken.jpg)  

> Update 28.11.16: This process was simplified, removing several steps. They have been omitted from translation.


### Portal

By pressing the  ```Reset Button``` the Wemos creates an access point, which allows you to make the necessary settings to configure the device.

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

