# SNIxKWT Hackathon

## BLCK Team

[@Azegele](https://github.com/Azegele) & [@thestephse](https://github.com/thestephse)

### SNI x Kenya Wildlife Trust Hackathon [Participants briefing](https://docs.google.com/presentation/d/1VLqtiZAZAbd1shTh0gsjylLx4jwKxX6t3hGPZLfiCYE/edit#slide=id.gfc8cd0c195_0_129)

1. How can we use technology to support a synergetic relationship between herders and predators?
2. What technologies might we use to incentivize nature stewards for their conservation efforts?
3. Can new, emerging technologies be in true service to indigenous communities' cohabitation with wildlife?

### Technology used by BLCK

- Lora
- Helium Network
- GPS Tracker
- Bluetooth Beacons

## HELIUM - The People's Network

### [Helium explained in under 4 Minutes - Video](https://www.youtube.com/watch?v=dOFWZZ58UYs)
- [Helium documentation](https://docs.helium.com/)
- [Helium Improvement Proposals (HIP)](https://github.com/helium/HIP)
- [Chain Performance](https://dashboard.helium.com/)
- [Chain Statistics](https://etl.dewi.org/)
- [Hotspot Manufacturer Verification](https://docs.google.com/spreadsheets/d/1pOmrMV_oiF0FtR1NOX_pqykKOBsb_QghiNkTlF644DU/edit#gid=440841804)

### LoRaWAN

LoRaWAN is a low-power, wide area networking protocol built on top of the LoRa radio modulation technique. It wirelessly connects devices to the internet and manages communication between end-node devices and network gateways.

### Why Helium?

Crypto incentivized network with almost [1 million gateways worldwide](https://explorer.helium.com/). Return on investment cost of a gateway can made through [Proof of Coverage (PoC)](https://docs.helium.com/blockchain/proof-of-coverage) rewards and Data Credits(DC) consumption. 
LoRaWAN Network Servers (LNS) , managed by [Helium Foundation](https://www.helium.foundation/) can be integrated in minutes.
One could also run their own LNS, instructions [here](https://docs.helium.com/use-the-network/run-a-network-server) 

>Deployed infrastructure can be used permissionlessly by other projects. 



## Concept of GPS Tracker & Beacon

![MOKO LW001 - cow monitoring](https://user-images.githubusercontent.com/42295932/200038420-5bdc0232-f19d-4431-8e6f-d5a258a1752d.png)
_Schema Moko LW001 States & Payloads_

### Working Modes

The Moko LW001 has three different working modes:

_a) Periodic_ 
_b) Intervall_ 
_c) Motion_

Most efficient for battery life is the motion mode, which uses the integrated acceleration sensor to switch between states.

_I) Stationary_ 
_II) Start moving_ 
_III) In movement_ 
_IV) End movement_

### Triggers

Triggers that change states:

_1) Wakeup Threshold_
_2) Wakeup Duration_ 
_3) Motion Threshold_ 
_4) Motion Duration_  

Criteria **1) & 2)** have to be met to change from state **I) to II)**.  
**3) & 4)** has to be met to change state from **II) to III)**. 
If they are not met anymore state will change from **III) to IV)**.

## Payloads

Payloads are small byte-size data packets sent from the tracker.
Helium is at this point only working with Class A sensors. Class A sensors can send Payloads and listen for a short period for an answer. This saves battery life but has the downside that one can not just send commands.
A workaround is **"Heartbeats"**. The sensor sends in defined intervals small payloads to make it possible to receive Downlink commands.

## Location Fix

The LW001 can use three different ways to define its location.

_A) GPS B) WiFi C) BLE_

Localization via GPS should not require further explanation. Here we use it while the Cow is moving.  
The tracker can listen SSIDs of WiFi networks or UUIDs of Bluetooth beacons. If the location of the beacon or WiFi Hotspots is known, one can calculate the tracker's location. (See also: [BLE - Location Fix](#ble-location-fix))


Changing its state from III) to IV) the tracker is "checking for lion presence" three times in an 1000 sec interval. 


## LW001 Configuration (Step 1)




Conf working mode:

> Motion  
> 02020604

Heartbeat Interval:
> 600 sec  
> 02050800000258

Motion Mode Setting:

> 0 = OFF 1 = ON  
> Bit 0 = Notify on start  
> Bit 1 = Fix on start  
> Bit 2 = Notify on trip  
> Bit 3 = Fix on trip  
> Bit 4 = Notify on end  
> Bit 5 = Fix on end  
>
> 110111 -> to decimal
>
> 02022437

Fix on start:

>GPS    
>02022601

Amount fix start:

>Once  
>02022601

Interval on trip:

>1000 sec  
>020527000003E8

Strategy during trip:
>GPS  
>02022801

Amount fix on end
>3 
>02022A03

Interval on end:

>100 sec 
>02032B0064

Strategy on end: 
>BLE 
>02022C02

## Lion Detection

### Bluetooth Beacon

Bluetooth beacons are hardware transmitters — a class of Bluetooth Low Energy (LE) devices that broadcast their identifier to nearby portable electronic devices. The technology enables smartphones, tablets and other devices to perform actions when in close proximity to a beacon.
(Source: [Wikipedia](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy_beacon))


### BLE Location Fix

<img width="250" alt="CleanShot 2022-11-06 at 11 20 11@2x" src="https://user-images.githubusercontent.com/42295932/200165431-96077fb7-9794-48a8-ac2a-9d5805eb7efd.png">

The LW001 can "listen" for Bluetooth beacons. The intended use is to calculate the tracker's location based on the signal strength and known locations of beacons. 

We re-purposed this functionality. The logic behind this is:

>**IF** the tracker can *hear* a beacon and this beacon is attached to a lion's collar **THEN** the lion must be within a radius around the tracker.  

##### Filters

For this, we configured filters so that only beacons with defined _Major_ are considered by the tracker. With _Major_ settings,  groups of beaconing tags can be created. In our case, all lion tags will have the same _Major_
 The payload that is sent from the tracker to TAGO c

- **UUID**
- **RSSI**
- **BLE Fix Status**

 >BLE Fix Success == Lion in range
 BLE Fix Failed == No lion in range

 We can map the UUID to an individual lion and with RSSI estimate the distance of the lion to the cow.


## LW001 Configuration (Step 2) 

Filter condition: 
> OR
> 02023400
> 
Switch Filter A:
> ON	
> 02023501
> 

MAC Filter: 
> OFF  
> 02043700

Major Filter: 

> ON	
> Major 01 - 02
> 026380100010002

Minor Filter:
> OFF	
> 02063900

Raw Data Filter:
> OFF	
> 02083A00
>
UUID Filter:
> OFF 	
> 02053B00
> 
RSSI Filter: 

> -125dBm	
> 02023C83

## Helium Console 

The [Helium Console](http://console.helium.com/) is a web-based device management tool hosted by the Helium Foundation that allows developers to register, authenticate, and manage their devices on the Helium network. In addition to device management, Console provides prebuilt connections called Integrations to route device data via HTTPs or MQTT; or directly to cloud services like AWS IoT.

### Register Sensor
_Source: [https://www.aeq-web.com/lht65-sensor-registrieren-im-helium-lorawan-netzwerk/?lang=en](https://www.aeq-web.com/lht65-sensor-registrieren-im-helium-lorawan-netzwerk/?lang=en)_


A new sensor can be inserted under the Devices menu item. All keys necessary for OTTA are already generated by Helium. You can adopt these keys by overwriting the manufacturer keys on the sensor. Of course you can also use the preconfigured keys from the manufacturer. This saves the reprogramming of the sensor. The following screenshot shows the registration process in the Helium console:

![Helium_Console_Add_Device_Keys-102653](https://user-images.githubusercontent.com/42295932/200115406-592a5074-f43d-46ad-97dd-cbaf7f1de3fd.jpeg)



After the device has been created, it appears in the device list. It can take up to 20 minutes until the sensor is fully registered. You should therefore wait a while before switching on the sensor for the first time. The following screenshot shows the previously set up sensor:

![Helium_Console_Device_after_Setup-103013](https://user-images.githubusercontent.com/42295932/200115417-2e8d6cdd-60e8-499d-b349-a724eb203e68.jpeg)

### Activate Sensor
About 20 minutes after registration, the respective sensor can be switched on. The sensor sends out a JOIN request. Each hotspot that has received the JOIN request forwards this packet to the network server. If the keys can be assigned to a device, a hotspot sends a JOIN accept packet with the session keys and a device address back to the sensor. The sensor accepts these keys and can send data into the Helium LoRaWAN from now on. To activate a LHT65, press the user button on the back for a few seconds:

![Helium_Console_JOIN_success-185429](https://user-images.githubusercontent.com/42295932/200115431-6645120c-9741-45cd-8c09-ce038a864153.jpeg)


### Payload Decoder

LoRaWAN tries to send as few bytes as possible over the air. Therefore, sensor data is not transmitted in plain text, but compressed. An example: The temperature value "-12.50Â°C" would require around eight bytes in plain text. An encoder in the sensor can multiply the temperature value by 100 and provide an offset so that the value can be accommodated in a 16-bit integer (whole number value) without decimal places and negative signs. This means that only two bytes are required for the same information. The decoder in turn performs opposite operations to the encoder and converts the temperature value back to the original formatting. There is more information about the compression of the payload data in this article: The LoRaWAN Payload. A decoder is usually stored in the LoRaWAN network, but can often be integrated into the application.

For our integration with TAGO.io, we did not need a decoder, it's done by Tago.
However to use a MQTT, it is necessary to run a decoder function in Console. ([lw001-decoder.js](lw001-decoder.js))



### Console Flow

<img width="750" alt="CleanShot 2022-11-05 at 11 44 33@2x" src="https://user-images.githubusercontent.com/42295932/200116186-36515a0a-1898-4e12-ad60-638f17508d31.png">


## TAGO.io

>TagoIO IoT Cloud Platform provides easy connection of electronic devices with external data to driver smarter decisions using contextual analysis.

### Dashbaord

Individual dashboard can be created and accessed via a WebApp or MobileApp.

<img width="500" alt="CleanShot 2022-11-05 at 12 13 57@2x" src="https://user-images.githubusercontent.com/42295932/200117148-e256897f-b222-4881-8362-07b3a7b87e9f.png">


Also unexperienced users can create geofences.

<img width="500" alt="CleanShot 2022-11-05 at 12 01 26@2x" src="https://user-images.githubusercontent.com/42295932/200117110-e9ea0b5d-4015-44c0-9d9d-e0abc3a77136.png">

<img width="500" alt="CleanShot 2022-11-05 at 12 02 20@2x" src="https://user-images.githubusercontent.com/42295932/200117085-62c26d94-7a6c-4ae6-a8bd-789d83f6ca0c.png">



### Analysis

[Geofence Analysis](tago_geofence.js)

### Alerts

Events such as a cow entering a previously defined area or leaving it can trigger alerts, but also lion-beacons registered by the tracker.
The alert can be an e-mail or SMS.
However, we learned that often children help their parents with herding the cows or simply a herder would not possess a mobile phone. 

We can use a LoRaWan-Based panic button to close this gap. If an alert is a trigger it would send a signal to the panic button which would in response start to vibrate.

Additional logic could be based on how often a button is pressed in response to vibration.

This is an inexpensive solution with a low entry barrier. 
Such panic buttons have a battery life of a couple of months and are re-chargeable. 

<img width="250" alt="Moko LW004" src="https://user-images.githubusercontent.com/42295932/200124235-3e8cecdc-cd10-4222-82a4-174c36f142fb.png">



## Conclusion

![Hackathon Flow (Detail)](https://user-images.githubusercontent.com/42295932/200173941-e17f4730-b5d0-4a92-8e9f-8ed6a5610437.png)


We can use Helium to build a LoRaWan Network in the Masaai Mara National Reserve and its neighbouring conservacies.After meeting with SNI, KWT and our fellow hackathon paarticipants, we realised that there is a multitude of other possible applicatonin addition to our idea of tracking lions with GPS.

It was not necessary to reinvent the wheel as most of the things we need existed already, some slight adjustments and we'd be ready to implement it. 

Tracking lions and/or cows with GPS could be just the first step in our journey in bringing IoT to the Wild. 

### Limitation

We learned that it is not as simple as we sometimes prototype solutions on our screen. It is not feasibly possible to collar every single lion and conduct any further maaintenance on the trackers. 

Prediction models could be one solution, if we collect enough data from lion movements which could be supplimented by or substituted with Vision or Audio Sensors.

### Next Steps

### StreamR (MQTT)

### More sensors

#### Vision AI



#### Audio Sensors

## Vision

![Hackathon Brainstorm Prep](https://user-images.githubusercontent.com/42295932/200174110-eed8d8c1-d84b-4487-b9d9-89094e00f5d6.png)


### Carbon Credits

#### DMRV

#### Grazing Strategies

#### Weather Data

#### Data Unions

