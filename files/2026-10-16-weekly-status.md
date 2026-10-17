# Weekly status, week ending 2026-10-16

From: Sam Ortega, Systems Engineering
To: Elena Ruiz, Dana Whitfield, Priya Nandakumar, Marcus Lee

## Summary

Kestrel passed its vibration retest with the new coupling and the 72-hour endurance run is under way, with little margin at top speed. Harrier revision B is accepted for the 55 C indoor enclosure, but my memo was wrong about the 70 C outdoor variant, which is now on hold until a chamber run on 2026-10-20. The Pulse postmortem was reviewed and corrected, because messages were lost during the incident after all.

## Kestrel pump test program

Status: on track, with a thin margin at 3,550 rpm.

The replacement VC-38 coupling was installed on 2026-10-13 and realigned cold to 0.03 mm parallel and 0.02 mm per 100 mm angular. Retest KT-115 on 2026-10-14 brought the pump drive end at 3,550 rpm down from 6.8 to 4.1 mm/s RMS and the motor drive end from 4.6 to 3.8 mm/s RMS, both under the 4.5 mm/s RMS limit, and the 2x peak dropped to 3.2 mm/s. The hot alignment reading after the hold showed 0.09 mm parallel offset, up from 0.03 mm cold, so thermal growth is real and the coupling was not the whole story. The margin at the pump drive end is 0.4 mm/s RMS. Dana agreed to start the 72-hour endurance run on 2026-10-16 at 09:00 with a stop rule: if the pump drive end passes 4.5 mm/s RMS at any point, the crew stops and shims the motor feet. At hour 6 the pump drive end read 3.9 mm/s RMS and the motor drive end 3.6 mm/s RMS, both steady.

Next week: the endurance run is due to finish on 2026-10-19 at 09:00. If it finishes clean, the vibration qualification is done and I write the final memo.

Source: daily notes for 2026-10-13 to 2026-10-16, and `docs/kestrel-pump-vibration-test-memo.md` for the KT-114 values and the limit.

## Harrier controller board

Status: at risk for the 70 C outdoor variant.

The 55 C chamber soak on 2026-10-12 put the core buck junction at 112 C against its 125 C rating and the Ethernet PHY at 86 C against 105 C, with total input still 18.6 W. I issued the power budget and thermal memo on 2026-10-13, and it said the board could go into the 70 C outdoor enclosure as is. That was wrong. At the thermal review on 2026-10-14, Priya showed that temperatures rose one for one with ambient, which puts the core buck near 127 C at a 70 C ambient, over its 125 C rating. I sent the correction to Priya and the Harrier team on 2026-10-16. Revision B is accepted for the 55 C indoor enclosure, and nobody had acted on the original recommendation, so there is nothing to undo. Revision C will look at running the core buck at 500 kHz.

Next week: the 70 C chamber run on 2026-10-20 decides whether the outdoor variant can go ahead.

Source: daily notes for 2026-10-13 to 2026-10-16, `meetings/2026-10-14-harrier-thermal-review.md` and `docs/harrier-power-budget-thermal-memo.md`.

## Pulse telemetry service

Status: on track.

While checking the timeline for Marcus, I found that the postmortem summary was wrong. The buffer was full from 14:31 to 14:50 and its overflow policy discards the oldest message, and Lena's search of the gateway log found 41,300 messages discarded in that window. The postmortem said no data was lost. The corrected summary now gives the 41,300 figure, and the review on 2026-10-16 with Marcus, Tomas and Lena added an action item for a disk-backed overflow, owned by Tomas and due 2026-10-30. My buffer growth-rate alert, set at 1,000 messages per minute, passed code review and is live. The change review checklist line for reporting-rate changes is merged. Tomas's pool sizing formula, measured write concurrency times 1.5, took the pool to 30 connections in production on 2026-10-15.

Next week: the Kestrel field trial can retry the 2 s reporting rate.

Source: daily notes for 2026-10-13 to 2026-10-16 and `docs/pulse-incident-postmortem.md`.

## Open questions

Kestrel has 0.4 mm/s RMS of margin at 3,550 rpm and a known hot offset, and the notes do not say whether the motor feet will be shimmed after the endurance run if it passes. The notes give no result yet for the Harrier 70 C chamber run, so the outdoor variant has no answer. The notes do not say who runs the Pulse retry of the 2 s reporting rate on the Kestrel field trial, or on which day.
