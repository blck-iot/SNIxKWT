# SNIxKWT Hackathon

## BLCK Team

@Azegel & @thestephse

### SNI x Kenya Wildlife Trust Hackathon [Participants briefing](https://docs.google.com/presentation/d/1VLqtiZAZAbd1shTh0gsjylLx4jwKxX6t3hGPZLfiCYE/edit#slide=id.gfc8cd0c195_0_129)

1. How can we use technology to support a synergetic relationship between herders and predators?
2. What technologies might we use to incentivize nature stewards for their conservation efforts?
3. Can new, emerging technologies be in true service to indigenous communities' cohabitation with wildlife?

### Technology used by BLCK

- LoraWan (needs to be defined)
- Helium Network
- GPS Tracker, connected to LoraWan
- Bluetooth Beacons (iBeacon)

## HELIUM - The People's Network

### [Helium explained in under 4 Minutes - Video](https://www.youtube.com/watch?v=dOFWZZ58UYs)

### LoRaWAN

LoRaWAN is a low-power, wide area networking protocol built on top of the LoRa radio modulation technique. It wirelessly connects devices to the internet and manages communication between end-node devices and network gateways.

### Why Helium?

Crypto intensivized network with almost 1.Mio Gateways worldwide. Costs of gateway can "can be earned back (missing the workd" through PoC rewards. LNS managed by Helium Foundation.

Deployed infrastructure can be used by other projects. 



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



