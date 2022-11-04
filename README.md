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

## Concept of GPS Tracker & iBeacon

![MOKO LW001 - cow monitoring (2)](https://user-images.githubusercontent.com/42295932/200037171-02ec999c-e195-4639-ac03-208fa5ff527d.jpg)
_Schema Moko LW001 States & Payloads_




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



