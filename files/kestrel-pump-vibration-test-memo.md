# Kestrel pump skid vibration test memo

From: Sam Ortega, Systems Engineering
To: Kestrel program team
Date: 2026-10-08
Test: KT-114, Bay 3 test stand, run on 2026-10-06 and 2026-10-07

## Summary

The Kestrel KP-200 pump skid passed the vibration limit at three of four test speeds and failed at the top speed. At 3,550 rpm the pump drive-end bearing measured 6.8 mm/s RMS in the horizontal direction, against the program limit of 4.5 mm/s RMS. The spectrum at that point is dominated by the second harmonic of running speed, which usually points to a coupling or shaft alignment problem rather than a pump hydraulic problem. The replacement flexible coupling ordered on 2026-10-08 will bring vibration below the limit at every test speed, so the skid can be released for the 72-hour endurance run once it is installed.

## System under test

The test article is a single Kestrel skid made up of five blocks connected in series.

1. Drive motor. A 75 kW, 4-pole induction motor (Tarrant part VM-75-4) fed by a variable frequency drive, rated 1,780 rpm at 60 Hz and run up to 3,550 rpm at 120 Hz for this test.
2. Coupling. A flexible disc coupling (part VC-38) transfers torque from the motor shaft to the pump shaft across a 6 mm gap.
3. Pump. The KP-200 single-stage centrifugal pump with a 6-vane impeller, rated 180 m3/h at 42 m head at 3,550 rpm. Water circulates from the stand reservoir through the pump and a throttling valve and back.
4. Sensors. Four triaxial accelerometers (100 mV/g) are stud-mounted at the motor non-drive end, motor drive end, pump drive end and pump non-drive end bearing housings. An optical tachometer reads shaft speed from a reflective strip on the coupling hub. A magnetic flow meter on the discharge line and two pressure transducers (suction and discharge) record the operating point.
5. Data acquisition. All sensor signals run to a 16-channel data acquisition unit sampling at 25.6 kHz with 16-bit resolution. Each test point is a 10-second record. The stand PC computes overall RMS velocity over 10 Hz to 1,000 Hz and a spectrum at 0.25 Hz resolution.

The signal flow is therefore: motor drives coupling, coupling drives pump, pump moves water around the loop, sensors on all four bearing housings and the loop feed the data acquisition unit, and the data acquisition unit feeds the stand PC.

## Method

The skid was bolted to the Bay 3 isolation slab and laser aligned on 2026-10-05. The recorded alignment was 0.04 mm parallel offset and 0.03 mm per 100 mm angular, both inside the coupling maker's tolerance of 0.10 mm and 0.05 mm per 100 mm.

The pump was run at four speeds, 1,800, 2,400, 3,000 and 3,550 rpm, with the discharge valve set for the best efficiency flow at each speed. Each speed was held for 15 minutes to reach thermal steady state, and the reported value is the mean of three 10-second records.

## Results

Overall vibration, mm/s RMS, highest of the three axes at each location:

| Speed (rpm) | Motor NDE | Motor DE | Pump DE | Pump NDE |
|---|---|---|---|---|
| 1,800 | 1.1 | 1.4 | 1.6 | 1.2 |
| 2,400 | 1.5 | 2.0 | 2.4 | 1.7 |
| 3,000 | 2.2 | 3.1 | 3.9 | 2.6 |
| 3,550 | 2.9 | 4.6 | 6.8 | 3.4 |

The program limit is 4.5 mm/s RMS at any bearing housing. Two values at 3,550 rpm exceed it: pump drive end at 6.8 mm/s and motor drive end at 4.6 mm/s. They sit on either side of the coupling, while both non-drive-end readings stay well under the limit.

The 3,550 rpm operating point was 176 m3/h at 41.2 m head, within 3 percent of rating.

## Spectrum at 3,550 rpm, pump drive end, horizontal

| Peak | Frequency (Hz) | Amplitude (mm/s RMS) |
|---|---|---|
| 1x running speed | 59.2 | 2.1 |
| 2x running speed | 118.3 | 5.9 |
| 3x running speed | 177.5 | 1.0 |
| Vane pass (6x) | 355.0 | 0.8 |

The 2x component carries most of the energy and is almost three times the 1x component. A dominant 2x peak at the pump drive end, with both drive ends elevated, is the classic sign of misalignment or a coupling that is not taking up misalignment as designed. The small vane pass peak argues against a hydraulic cause, and the moderate 1x peak argues against simple imbalance.

The rise is not linear with speed: pump drive-end vibration grows 2.9 mm/s from 3,000 to 3,550 rpm, more than the 0.8 mm/s gained from 1,800 to 2,400 rpm. The cold alignment was inside tolerance, so the likely explanation is thermal growth of the motor frame at high speed shifting the shafts, or wear in the VC-38 disc pack. We did not take a hot alignment reading.

## Next steps

1. Install the replacement VC-38 coupling when it arrives from stores, expected 2026-10-12.
2. Repeat the four-speed test with the new coupling.
3. Take a hot alignment reading immediately after the 3,550 rpm hold.
4. If both drive-end readings are under 4.5 mm/s RMS at all speeds, start the 72-hour endurance run.

Raw data is on the test stand share under KT-114. Questions to Sam Ortega.
