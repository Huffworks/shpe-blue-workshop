# Weekly status, week ending 2026-10-16

From: Sam Ortega, Systems Engineering
To: Elena Ruiz, Dana Whitfield, Priya Nandakumar, Marcus Lee

## Summary

Kestrel passed its retest at 3,550 rpm and the 72-hour endurance run is under way. Harrier revision B is accepted for the 55 C indoor enclosure, but the 70 C outdoor variant is on hold because my 2026-10-13 memo was wrong about it. The Pulse postmortem was reviewed on 2026-10-16 with a corrected summary.

## Kestrel pump test program

Status: on track, with 0.4 mm/s RMS of margin at the pump drive end.

Retest KT-115 ran on 2026-10-14 with the new VC-38 coupling. At 3,550 rpm the pump drive end fell from 6.8 to 4.1 mm/s RMS and the motor drive end from 4.6 to 3.8 mm/s RMS, both under the 4.5 mm/s RMS limit. Hot alignment showed 0.09 mm parallel offset against 0.03 mm cold, so thermal growth is real and the coupling was not the whole cause, contrary to the 2026-10-08 memo. The endurance run started on 2026-10-16 at 09:00; we stop and shim if the pump drive end passes 4.5 mm/s RMS. At hour 6 it read 3.9 mm/s RMS.

Next week: finish the run on 2026-10-19 at 09:00 and, if clean, issue the final vibration memo.

Source: daily notes for 2026-10-14 to 2026-10-16.

## Harrier controller board

Status: at risk for the 70 C outdoor variant.

The 2026-10-13 memo recommended releasing the 70 C outdoor variant with no hardware change, and that was wrong. At 55 C ambient the 1.0 V core buck junction reached 112 C against a 125 C rating, and temperatures rose one for one with ambient, so 70 C ambient puts it at about 127 C. Priya caught this at the 2026-10-14 review, and I sent the correction on 2026-10-16. Nobody had acted on the original recommendation. Board HB-0007 still draws 18.6 W against a 20.0 W budget.

Next week: run the 70 C chamber test in the outdoor enclosure on 2026-10-20.

Source: `docs/harrier-power-budget-thermal-memo.md` and `meetings/2026-10-14-harrier-thermal-review.md`.

## Pulse telemetry service

Status: on track.

The draft postmortem for PULSE-INC-031 said no data was lost, and that was wrong: the full buffer discarded 41,300 messages between 14:31 and 14:50 Central. The corrected summary was accepted at the 2026-10-16 review, and a disk-backed overflow action went to Tomas Reyes, due 2026-10-30. The buffer growth-rate alert is live, the checklist line is merged, and the production pool went to 30 connections on 2026-10-15.

Next week: the Kestrel field trial retries the 2 s reporting rate.

Source: daily notes for 2026-10-15 and 2026-10-16. The copy in `docs/` is the pre-review draft.

## Open questions

KT-115 results at 1,800, 2,400 and 3,000 rpm are not in the source. The notes do not say whether all Pulse action owners and dates were confirmed, or whether 30 connections covers the 2 s reporting rate.
