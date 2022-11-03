| Payload             |Port| Byte       | Byte                          |  Byte              |  Byte              |  Byte          | Byte          |
| ------------------- | - | ---------- | ------------------------------ | ------------------ | ------------------ | -------------- | ------------- |
|                     | 1 | 0 2        | 3                              | 4                  | 5 8                |                |               |
| Heartbeat              |   | Header     | Last Reboot                    | FW Version         | Acive State count  |                |               |
|                     |   |            | 00 Power Failure               |                    | Convert to decimal |                |               |
|                     |   |            | 01 Bluetooth command           |                    |                    |                |               |
|                     |   |            | 02 LoRaWAN command             |                    |                    |                |               |
|                     |   |            | 03 Normal Power Off            |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
|                     |   | 0 2        | 3                              | 4 10               | 11                 | 12             | 13 XX         |
| Location            | 2 | Header     | Success Type                   | Timestamp          | Time Zone          | Length of data | Location Data |
|                     |   |            | 00 Wifi                        | convert to decimal | standart is UTC    |                |               |
|                     |   |            | 01 BLE                         |                    |                    |                |               |
|                     |   |            | 02 GPS                         |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Location Data       |   | 0 5        | 6                              | 7 12               | 13                 |                |               |
|                     |   | MAC Wifi 1 | RSSI Wifi 1                    | MAC Wifi 2         | RSSI Wifi 2        |                |               |
|                     |   | MAC BLE 1  | RSSI BLE1                      | MAC BLE 2          | RSSI BLE 2         |                |               |
|                     |   |            |                                |                    |                    |                |               |
|                     |   | 0 3        | 4 7                            | 8                  |                    |                |               |
|                     |   | Lat        | Long                           | PDOP               |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Location Failure    | 3 | 0 2        | 3                              | 4                  | 5 XX               |                |               |
|                     |   | Header     | Reason                         | Length of data     | Failure data       |                |               |
|                     |   |            |                                |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Shutdown Payload    | 4 | 0 2        | 1                              |                    |                    |                |               |
|                     |   | Header     | Shutdown                       |                    |                    |                |               |
|                     |   |            | 01 Lora                        |                    |                    |                |               |
|                     |   |            | 00 BLE                         |                    |                    |                |               |
|                     |   |            | 02 Magnetic                    |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Vibration Payload   | 5 | 0 2        | 3 4                            |                    |                    |                |               |
|                     |   | Header     | Vibrations since last payload  |                    |                    |                |               |
|                     |   |            | convert to decimal             |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Man Down            | 6 | 0 2        | 3 4                            |                    |                    |                |               |
|                     |   | Header     | Time idle                      |                    |                    |                |               |
|                     |   |            | Convert decimals, unit Hour    |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Tamper Alert        | 7 | 0 2        | 3 9                            | 10                 |                    |                |               |
|                     |   | Header     | Timestamp                      | Time zone          |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Event message       | 8 | 0 2        | 3                              |                    |                    |                |               |
|                     |   | Header     | Event Type                     |                    |                    |                |               |
|                     |   |            | 00 Start movement              |                    |                    |                |               |
|                     |   |            | 01 in movement                 |                    |                    |                |               |
|                     |   |            | 02 end movement                |                    |                    |                |               |
|                     |   |            | 03 Uplink triggerd by downlink |                    |                    |                |               |
|                     |   |            |                                |                    |                    |                |               |
| Battery Consumption | 9 | 0 2        | 3 6                            | 7 10               | 11 14              | 15 18          | 19 22         |
|                     |   | Header     | GPS working time               | Wifi time          | BLE time           | BLE broadcast  | LoRa time     |
|                     |   |            | Decimals in seconds            | ...                | ...                | ...            | ...           |
|                     |   |            |                                |                    |                    |                |
