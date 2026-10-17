# Harrier controller board power budget and thermal memo

From: Sam Ortega, Systems Engineering
To: Harrier program team, Priya Nandakumar (electrical lead)
Date: 2026-10-13
Board: Harrier controller, revision B, serial HB-0007

## Summary

The Harrier revision B controller board draws 18.6 W from its 24 V input at full load, which is 7 percent under the 20.0 W budget in the system specification. At a 55 C enclosure ambient, the hottest part is the 1.0 V core buck regulator at a junction temperature of 112 C, against a rated maximum of 125 C. Every other part has at least 19 C of margin. The design has enough thermal margin to ship in the 70 C outdoor enclosure variant without adding a heatsink.

## Board and power tree

The Harrier board controls the Kestrel pump skid. It takes 24 V DC from the skid supply and produces four rails.

1. 12 V rail, from a synchronous buck regulator, feeding the three isolated gate driver supplies.
2. 5 V rail, from a second buck regulator, feeding the sensor excitation and the analog front end.
3. 3.3 V rail, from a buck regulator off the 5 V rail, feeding the microcontroller, the Ethernet PHY and the logic.
4. 1.0 V core rail, from a high-current buck regulator off the 5 V rail, feeding the FPGA core.

## Power budget at full load

Full load means the FPGA running the complete motor control image, all three gate drivers switching at 16 kHz, Ethernet linked at 100 Mb/s and all sensor channels excited. Values were measured on HB-0007 with a power analyzer at 25 C ambient.

| Rail | Voltage (V) | Current (A) | Load power (W) | Regulator efficiency (%) | Regulator loss (W) |
|---|---|---|---|---|---|
| 12 V gate drive | 12.0 | 0.42 | 5.04 | 93 | 0.38 |
| 5 V analog | 5.0 | 0.36 | 1.80 | 91 | 0.18 |
| 3.3 V logic | 3.3 | 0.95 | 3.14 | 90 | 0.35 |
| 1.0 V core | 1.0 | 4.80 | 4.80 | 82 | 1.05 |

The 3.3 V and 1.0 V regulators run from the 5 V rail, so their input power is drawn through the 5 V regulator as well. The cascaded loss of the 5 V regulator carrying those loads adds a further 0.92 W. Fixed overhead for the input protection, the bulk capacitors and the status LEDs is 0.42 W, and the remaining 0.52 W is conduction loss in the board copper and connectors.

Total input power measured at the 24 V connector is 18.6 W, or 0.775 A. The budget in the Harrier system specification, section 4.2, is 20.0 W, leaving 1.4 W of margin.

## Thermal test

The board was mounted in the production enclosure with its standard thermal pad to the aluminum lid and soaked in the environmental chamber for 2 hours at each of two ambients, 25 C and 55 C, at full load. Case temperatures came from thermocouples bonded to the part tops, and junction temperatures were calculated from case temperature, dissipation and the datasheet junction-to-case resistance.

| Part | Dissipation (W) | Theta JC (C/W) | Case at 25 C (C) | Junction at 25 C (C) | Case at 55 C (C) | Junction at 55 C (C) | Rated max junction (C) |
|---|---|---|---|---|---|---|---|
| 1.0 V core buck | 1.05 | 12.0 | 69 | 82 | 99 | 112 | 125 |
| FPGA | 4.80 | 1.6 | 61 | 69 | 91 | 99 | 125 |
| Microcontroller | 0.62 | 9.5 | 54 | 60 | 84 | 90 | 125 |
| Gate driver (hottest of 3) | 0.55 | 14.0 | 57 | 65 | 86 | 94 | 150 |
| Ethernet PHY | 0.31 | 18.0 | 50 | 56 | 80 | 86 | 105 |

Temperatures rose one for one with ambient, about 30 C for a 30 C change.

The 1.0 V core buck is the limiting part, 13 C below its rating at 55 C ambient. The Ethernet PHY is next at 19 C, because its rating is only 105 C.

## Observations

The core buck runs at 82 percent efficiency, which is low for a regulator at this current. The datasheet curve gives 87 percent at 4.8 A when the switching frequency is 500 kHz, but revision B runs it at 1.2 MHz to keep the inductor small. Dropping to 500 kHz would cut its loss to about 0.72 W and lower its junction by about 4 C, at the cost of a larger inductor footprint.

The 12 V gate drive load will rise by about 0.6 W if the switching frequency moves from 16 kHz to 20 kHz, as the motor team has proposed for acoustic reasons. That would still leave 0.8 W of budget margin.

## Recommendations

1. Accept revision B for the standard 55 C indoor enclosure.
2. Release the 70 C outdoor enclosure variant on the same board with no hardware change.
3. Open a revision C study on the core buck switching frequency.
4. Repeat the power measurement if the gate drive frequency changes.

Test data is in the Harrier share under HB-0007/thermal. Questions to Sam Ortega.
