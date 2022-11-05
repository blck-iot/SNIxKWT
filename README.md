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

### LoRaWAN

LoRaWAN is a low-power, wide area networking protocol built on top of the LoRa radio modulation technique. It wirelessly connects devices to the internet and manages communication between end-node devices and network gateways.

### Why Helium?

Crypto intensivized network with almost 1.Mio Gateways worldwide. Costs of gateway can "can be earned back (missing the word)" through PoC (Proof of Coverage) rewards and DC (Data Credits) consumption. 
LNS managed by Helium Foundation. End-Nodes can be integrated in minutes.

>Deployed infrastructure can be used permissionless by other projects. 



## Concept of GPS Tracker & iBeacon

![MOKO LW001 - cow monitoring](https://user-images.githubusercontent.com/42295932/200038420-5bdc0232-f19d-4431-8e6f-d5a258a1752d.png)
_Schema Moko LW001 States & Payloads_

### Working Modes

The Moko LW001 has three different working modes:

_a) Periodic_ 
_b) Intervall_ 
_c) Motion_

Most economic for battery life is the motion mode, which uses the integrated acceleration sensor to switch between states.

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
Helium is at this point only accepting Class A sensors. Class A sensor can send Payloads and are listening for a short period for an answer. This saves battery life, but has the downside that one can not just send commands.
A workaround are **"Heartbeats"**. The sensor is sending in defined intervalls small payloads and makes it possible to send Downlink commands.

## Location Fix

The LW001 can use three different ways to define its location.

_A) GPS B) WiFi C) BLE_

Lokalization via GPS should not require further explanation. Here we use it while the Cow is moving.  
The tracker can listen SSID's of WiFi networks or UUID's of buethooth beacons. If the location of beacon or Wifi Hotspots is know, one can calculate the Trackers location. 


We turned this concept around. Assuming that:
>**IF** the tracker can *hear* a beacon and this beacon is e.g. attached to a lions collar **THEN** the lion must be within a radius around the tracker.

Changing its state from III) to IV) the tracker is "checking for lion presence" three times in an 1000 sec interval. 




## LW001 Configuration




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

<img width="1391" alt="CleanShot 2022-11-05 at 12 13 57@2x" src="https://user-images.githubusercontent.com/42295932/200117148-e256897f-b222-4881-8362-07b3a7b87e9f.png">


Also unexperienced users can create geofences.

<img width="1343" alt="CleanShot 2022-11-05 at 12 01 26@2x" src="https://user-images.githubusercontent.com/42295932/200117110-e9ea0b5d-4015-44c0-9d9d-e0abc3a77136.png">

<img width="560" alt="CleanShot 2022-11-05 at 12 02 20@2x" src="https://user-images.githubusercontent.com/42295932/200117085-62c26d94-7a6c-4ae6-a8bd-789d83f6ca0c.png">



### Analysis

### Alerts


## Conclusion

### Limitation

### Next Steps

### StreamR (MQTT)

### More sensors

#### Vision AI

#### Audio Sensors

## Vision

### Carbon Credits

#### DMRV

#### Grazing Strategies

#### Weather Data

#### Data Unions

