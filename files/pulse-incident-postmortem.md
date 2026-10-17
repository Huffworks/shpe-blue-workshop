# Pulse telemetry service incident postmortem

Incident: PULSE-INC-031
Owner: Sam Ortega, Systems Engineering
Date of incident: 2026-10-02
Status: draft for review, 2026-10-12
Severity: 2 (degraded service, no safety impact)

## Summary

On 2026-10-02 between 14:06 and 15:12 Central time, the Pulse telemetry service fell behind on ingesting sensor data from field pump skids. Dashboard data was delayed by up to 19 minutes, and the vibration alarm for two skids fired 17 minutes late. The cause was a database connection pool that was too small for the write load after a configuration change raised the skid reporting rate. We restored service by raising the pool size and rolling back the rate change. No telemetry data was lost during the incident.

## What Pulse does

Pulse collects telemetry from every Tarrant pump skid in the field. Each skid's Harrier controller sends vibration, flow, pressure, temperature and power readings every few seconds. Messages pass through an ingest gateway and an in-memory buffer, and writer workers store them in a time-series database. Dashboards and the alarm engine read from that database. On 2026-10-02 there were 412 skids reporting.

Normal performance in the week before the incident:

| Measure | Value |
|---|---|
| Messages per second, ingest | 82 |
| Write latency p50 | 14 ms |
| Write latency p95 | 41 ms |
| Write latency p99 | 88 ms |
| End-to-end delay p99, skid to dashboard | 2.1 s |
| Buffer capacity | 60,000 messages |
| Database connection pool size | 16 |

## Timeline (Central time, 2026-10-02)

| Time | Event |
|---|---|
| 13:58 | Configuration change PCR-2207 deployed. It raised the skid reporting rate from one message every 5 s to one every 2 s for the Kestrel field trial. |
| 14:06 | Ingest rate reached 206 messages per second. Write latency p99 rose to 640 ms. |
| 14:19 | Buffer depth passed 25,000 messages. No alert fired, because the buffer alert threshold was set at 90 percent. |
| 14:27 | Dashboard delay passed 12 minutes. A field technician reported stale data to the support line. |
| 14:31 | Buffer reached 60,000 messages, its capacity. The buffer's overflow policy is to discard the oldest message when full. |
| 14:34 | Alarm engine raised a vibration alarm for skids K-031 and K-044, based on readings taken at 14:17. |
| 14:37 | On-call engineer Tomas Reyes acknowledged the support ticket and opened the incident. |
| 14:44 | All 16 pool connections were busy, with 190 writers waiting. |
| 14:52 | Pool size raised from 16 to 48 by a live configuration change. Write latency p99 fell to 120 ms. |
| 14:58 | PCR-2207 rolled back. Ingest rate returned to 84 messages per second. |
| 15:12 | Buffer drained below 1,000 messages. Dashboard delay back under 3 s. Incident closed. |

Peak measured values during the incident:

| Measure | Peak value | Time |
|---|---|---|
| Ingest rate | 211 messages per second | 14:09 |
| Write latency p99 | 2,300 ms | 14:41 |
| Dashboard delay | 19 min | 14:48 |
| Buffer depth | 60,000 messages (full) | 14:31 to 14:50 |

## Root cause

PCR-2207 multiplied the message rate by 2.5. Each writer holds a database connection for the length of one write, so at 206 messages per second and a p50 write time that had risen to 95 ms under contention, the service needed about 20 connections continuously and more during bursts. With 16 connections, writers queued for connections, latency rose, and rising latency held connections longer, which made the queue grow faster. The pool size of 16 dates from 2024, when 140 skids were reporting.

The PCR-2207 review checked bandwidth and storage but not write concurrency, and the load test environment uses a pool of 64, so the change looked healthy there.

## Contributing factors

1. The buffer alert threshold at 90 percent fired only 3 minutes before the buffer was full. A rate-of-growth alert would have fired around 14:08.
2. The incident was detected by a customer-facing report, not by monitoring. Time from first impact to acknowledgement was 31 minutes.
3. The alarm engine has no check on data age. It raised the K-031 and K-044 alarms 17 minutes after the readings, with no warning that the data was stale.

## What went well

The pool size changed live within a minute, and the PCR-2207 rollback was a single command. The Kestrel field team confirmed the K-031 and K-044 alarms were a known startup transient.

## Action items

| Item | Owner | Due |
|---|---|---|
| Size the connection pool from measured write concurrency, with 50 percent headroom, and document the formula | Tomas Reyes | 2026-10-23 |
| Add a buffer growth-rate alert at 1,000 messages per minute | Sam Ortega | 2026-10-16 |
| Add a data-age check to the alarm engine that flags alarms on readings older than 60 s | Lena Kowalczyk | 2026-10-30 |
| Add write concurrency to the change review checklist for any reporting-rate change | Sam Ortega | 2026-10-16 |
| Match the load test pool size to production | Tomas Reyes | 2026-10-23 |

The Kestrel field trial will retry the 2 s reporting rate after the first two action items are closed.
