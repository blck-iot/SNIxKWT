# Moko LW001-BG PRO


### Additional Remarks
**Working Modes**
01 means standby mode
02 means periodic mode
03 means timing mode
04 means motion mode

**Location Fix**
Convert to binary
Bit 0 refers WIFI fix
Bit 1 refers Bluetooth fix
Bit 2 refers GPS fix
*If the value is 0, it means off; if the value is 1, it means on*


### Downlink Commands

| Configure Command                | CMD | Len (Bytes) | ID | Example          | Send                   | Remark                                                      |
| -------------------------------- | --- | ----------- | -- | ---------------- | ---------------------- | ----------------------------------------------------------- |
| Time Zone                        | 02  | 02          | 04 | 09               | 02020409               | signed number 09 = UTC + 8         |
| Set Password                     | 02  | 09          | 05 | 4D6F6B6F34333231 | 0209054D6F6B6F34333231 | Moko4321                           |
| Working mode                     | 02  | 02          | 06 | 04               | 02020604               |                                    |
| Heartbeat Interval               | 02  | 05          | 08 | 00000258         | 02050800000258         | Range: 300 ~ 86400s.               |
| Off by magnet                    | 02  | 02          | 09 | 01               | 02020901               |                                    |
| Switch Offline Fix               | 02  | 02          | 0B | 01               | 02020B01               |                                    |
| Low Power Settings               | 02  | 02          | 0C | 01               | 02020C01               | *Convert to binary. Bit 0 refers low Power Prompt Setting value, 0 means 5%, 1 means 10%; Bit 1 refers function switch of low power payload, 0 means didn’t send low power payload, 1 means send low power payload*                                 |
| Led indicator                    |     |             |    |                  |                        |                                    |
| Strategy for periodic            | 02  | 02          | 20 | 02               | 02022002               | bluetooth, binary                  |
| Interval for periodic            | 02  | 05          | 21 | 000003E3         | 020521000003E3         | 1000s                              |
| Strategy for timing mode         | 02  | 02          | 22 | 04               | 02022204               | 100 -> GPS, binary                 |
| Timing point of timing mode      | 02  | 03          | 23 | 0103             | 0203230103             | 00:15 and 00:45 - *Convert to decimal, the unit is 15mins. 01 means 00:15; 02 means 00:30; 03 means 00:45...; Maximum 10 report timing points can be set*                  |
| Motion mode settings             | 02  | 02          | 24 | 19               | 02022419               |                                    |
| Configure number of fix on start | 02  | 02          | 25 | 02               | 02022502               | 2 times, convert decimals, 1 - 255 |
| Position Strategy on start       | 02  | 02          | 26 | 02               | 02022602               | BLE, binary                        |
| Report interval in trip          | 02  | 05          | 27 | 000003E8         | 020527000003E8         | 1000s, decimals in s               |
| Strategy in trip                 | 02  | 02          | 28 | 02               | 02022802               | BLE,binary                         |
| Trip end timeout                 | 02  | 02          | 29 | 0A               | 0202290A               | 100s, decomal in s                 |
| Number of fix on end             | 02  | 02          | 2A | 03               | 02022A03               | 3, decimals 1 - 255                |
| Report interval trip end         | 02  | 03          | 2B | 0064             | 02032B0064             | 100s, decimals in s                |
| Strategy on trip end             | 02  | 02          | 2C | 02               | 02022C02               | BLE, binary                        |
| Strategy downlink position       | 02  | 02          | 2D | 02               | 02022D02               | BLE, binary                        |
| BLE fix timeout                  | 02  | 02          | 32 | 05               | 02023205               | decimals in s                      |
| Number of devices BLE fix        | 02  | 02          | 33 | 01               | 02023301               | decomals, number                   |
| Relation Filter A & B            | 02  | 02          | 34 | 01               | 02023401               | 00 OR 01 AND                       |
| Switch state Filter A            | 02  | 02          | 35 | 00               | 02023500               | 00 off 01                          |
| ADV Filter A                     | 02  | 02-31       | 36 | 016D6F6B6F       | 0202-3136016D6F6B6F    | *Send: 02 06 36 01 6D 6F 6B 6F (The 1st byte of Command DATA is the filter flag, 00 means off, 01 means Positive filter, 02 means Reverse filter. The rest content of Command DATA is the filter content, convert to ASCII. The ADV name filter content of Filter Condition A is moko)* moko                               |
| Major Filter A                   | 02  | 6           | 38 | 010001000A       | 02638010001000A        |                                    |
| Minor Filter A                   | 02  | 6           | 39 | 010001000A       | 02639010001000A        |                                    |
| Raw data filter                  |     |             |    |                  |                        |                                    |
| UUID Filter A                    | 02  | 2-18        | 3B | 000500           |022-183B000500         | *(The 1st byte of Command DATA is the filter flag, 00 means off, 01 means Positive filter, 02 means Reverse filter. The rest content of Command DATA is the filter content. The UUID filter content of Filter*                                   |
| RSSI Filter A                    | 02  | 02          | 3C | C0               | 02023CC0               | \-64dBm                            |
| Switch Filter B                  | 02  | 02          | 3E | 00               | 02023E00               |                                    |
| ADV Filter B                     | 02  | 02-31       | 3F | 016D6F6B6F       | 0202-313F016D6F6B6F    |                                    |
| Major Filter B                   | 02  | 2/6         | 41 | 010001000A       | 022/641010001000A      |                                    |
| Minor Filter B                   | 02  | 2/6         | 42 |                  | 022/642                |                                    |
| Raw data filter                  |     |             |    |                  |                        |                                    |
| UUID Filter B                    | 02  | 2-18        | 44 |                  | 022-1844               |                                    |
| RSSI Filter B                    | 02  | 02          | 45 |                  | 020245                 |                                    |
| Coarse Accuracy                  | 02  | 02          | 48 | 64               | 02024864               | 100m                               |
| Fine accuracy                    | 02  | 02          | 49 |                  | 020249                 | 20m                                |
| Course timout                    | 02  | 03          | 4A | 00FF             | 02034A00FF             | 256s in s                          |
| Fine timeout                     | 02  | 05          | 4B | 0005             | 02054B0005             | 5s, decimals in s                  |
| PDOP limit                       | 02  | 02          | 4C | 64               | 02024C64               | 10, unit is 0.1                    |
| GPS Model                        | 02  | 02          | 4E | 01               | 02024E01               | Stationary                         |
| GPS fix mode                     | 02  | 02          | 4D | 01               | 02024D01               |                                    |
| GPS time budget                  | 02  | 05          | 4F | 00000708         | 02054F00000708         | decomals, sec, 1800s               |
| Switch autonomous aiding         | 02  | 02          | 50 | 00               | 02025000               | 00 off 01 on                       |
| Aiding accuracy                  | 02  | 03          | 51 | 03E8             | 02035103E8             | in m , 1000m                       |
| Aiding timeout                   | 02  | 03          | 52 | 03E8             | 02035203E8             | ins, 1000s                         |
| Switch GPS extreme               | 02  | 02          | 53 | 01               | 02025301               |                                    |
| Region device                    | 02  | 02          | 60 | 05               | 02026005               |                                    |
| LoraMode                         | 02  | 02          | 61 |                  | 020261                 |                                    |
| DEVEUI                           | 02  | 09          | 63 |                  | 020963                 |                                    |
| APPEUI                           | 02  | 09          | 64 |                  | 020964                 |                                    |
| APPKEY                           | 02  | 17          | 65 |                  | 021765                 |                                    |
| DEVADDR                          | 02  | 05          | 66 |                  | 020566                 |                                    |
| APPSKEY                          | 02  | 17          | 67 |                  | 021767                 |                                    |
| Switch Beacon Mode               | 02  | 02          | 70 | 00               | 02027000               |                                    |
| ADV intervall                    | 02  | 02          | 71 | 0A               | 0202710A               | 1000ms, 100ms units                |
| Connectable state                | 02  | 02          | 72 | 01               | 02027201               | 00 unconnectable                   |
| Broadcast timeout                | 02  | 02          | 73 | 03               | 02027303               | decimals in min                    |
| UUID of Beacon                   | 02  | 17          | 74 |                  | 021774                 |                                    |
| Major of Beacon                  | 02  | 03          | 75 | 0001             | 0203750001             |                                    |
| Minor Beacon                     | 02  | 03          | 76 | 0002             | 0203760002             |                                    |
| Power of Beacons                 | 02  | 02          | 77 | BF               | 020277BF               | \-65 dbm                           |
| BT TX Power                      | 02  | 02          | 78 | 04               | 02027804               |                                    |
| ADV Name                         | 02  | 1-14        | 79 |                  | 021-1479               | Convert to ASCII                   |
| Scanning Type                    | 02  | 02          | 7A | 03               | 02027A03               |                                    |
| 3Axis Wakeup                     | 02  | 03          | 80 | 0305             | 0203800305             |                                    |
| 3Axis motion threshold           | 02  | 03          | 81 | 0A08             | 0203810A08             |                                    |
| Switch shock detection           | 02  | 02          | 82 | 01               | 02028201               |                                    |
| shock threshold                  | 02  | 02          | 83 | 0A               | 0202830A               | 100mg                              |
| report interval shock threshold  |     |             |    |                  |                        |                                    |
| shock timeout                    |     |             |    |                  |                        |                                    |
| switch man down                  | 02  | 02          | 86 | 01               | 02028601               |                                    |
| idle detection timeout           | 02  | 03          | 87 | 0064             | 0203870064             | decimals, in h                     |
| switch tamper alarm              | 02  | 02          | 88 | 01               | 02028801               |                                    |
| switch active state count        | 02  | 02          | 89 | 01               | 02028901               |                                    |
| Active state timeout             | 02  | 05          | 8A | 00000064         | 02058A00000064         | decimals in s                      |
|                                  |     |             |    |                  |                        |
