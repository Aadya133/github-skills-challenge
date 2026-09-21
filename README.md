# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)





Task 1: Set Up the Project

Service being monitored: The payment-service is being monitered which processes payment requests and relies on a database connection.

Operational problem: Detecting payment timeouts and degraded performance, including high response times, CPU and memory usage, and database connection errors.

Purpose of AIOps: To analyze service telemetry, identify anomalies automatically, and publish them as events so operational teams can detect and respond to incidents quickly.

Task 2: Analyse Logs and Metrics

1. Metric fields
   - `response_time_ms`
   - `cpu_percent1`
   - `memory_percent`

2. Log information
   - `log_level` identifies the severity, such as `INFO` or `ERROR`.
   - `message` describes the event, such as successful processing or a timeout.
   - `service` identifies the source service.

3. Timestamp usage
   - `timestamp` records when each observation occurred.
   - Observations are recorded at one-minute intervals from `10:00` to `10:09`, allowing metric and log changes to be tracked over time.

4. Normal behaviour
   - `10:00`–`10:04` and `10:07`–`10:09`.
   - Response times are approximately `120–150 ms`, CPU is `42–50%`, memory is `51–57%`, and logs report successful processing at `INFO` level.

5. Unusual behaviour
   - `10:05`: Response time rises to `610 ms` and an `ERROR` reports a payment service timeout.
   - `10:06`: Response time rises to `640 ms`, CPU reaches `94%`, memory reaches `91%`, and an `ERROR` reports a database connection timeout. This is the strongest anomaly.


TASK 3 : Identify Anomalies

Total Records: 10 r
Anomalies detected: 2
Events consumed and displayed: 2

Detection report

- `10:05`: Anomaly detected for `payment-service`.
  - Response time: `610 ms`, above the `500 ms` threshold.
  - Log level: `ERROR`.
  - Message: `Payment service timeout`.
- `10:06`: Anomaly detected for `payment-service`.
  - Response time: `640 ms`.
  - CPU: `94%`, above the `80%` threshold.
  - Memory: `91%`, above the `80%` threshold.
  - Log level: `ERROR`.
  - Message: `Database connection timeout`.

Analysis

- The normal observations from `10:00`–`10:04` and `10:07`–`10:09` were not flagged.
- No expected anomaly appears to have been missed in the supplied data.
- No normal event was incorrectly flagged.
- The event reasons and retained source record provide sufficient context for understanding each flag.

Limitation

The detector uses fixed thresholds and only checks `ERROR` logs. It could be improved with ml based anomaly detection for adaptive baseline.


Task 4: Verify the AIOps Event Flow

Component roles

- Event/message: The anomaly dictionary created by `AnomalyDetector`.
- Producer: Publishes each detected event.
- Topic: In-memory `anomaly-events` storing published events.
- Consumer: Reads events from the shared topic.
- Downstream AIOps component: `run_pipeline`, which returns and displays the consumed events.

Verified flow

1. The detector identified 2 anomalies.
2. Each anomaly became an event.
3. The producer published both events.
4. The shared topic stored both events.
5. The consumer received both events.
6. `run_pipeline` returned and displayed them downstream.

Validation: 9 tests passe.


Task 5: Investigate and Correct the Workflow

Problems identified and corrected:

1. `AnomalyDetector` was checking for `WARNING` logs, but the operational data uses
   `ERROR` for payment and database failures. The detector now flags `ERROR` logs,
   including an error-only event when metrics are normal.
2. `aiops_pipeline` published to `service-events` while the consumer listened to
   `anomaly-events`. The producer and consumer need to share the same `anomaly-events`
   topic.

Changed Components: `AnomalyDetector`, `EventTopic`, `EventProducer`,
`EventConsumer`, and `run_pipeline`.

Verification:The test  passed with 10 tests.

Task 6: Execute the End-to-End Pipeline

Command:

```
PYTHONPATH=/workspaces/github-skills-challenge python -m pytest -q && \
PYTHONPATH=/workspaces/github-skills-challenge/src \
python /workspaces/github-skills-challenge/src/aiops_pipeline.py
```

Result: 10 tests passed; 10 records processed; 2 anomalies detected and consumed.


Task 7: Demonstrate the Complete AIOps Flow

Flow:

`Operational Data -> Anomaly Detection -> Event -> Producer -> Topic -> Consumer -> AIOps`

1.Operational data processed: `service_data.json` supplied 10 observations to
   `run_pipeline`.
2. Anomalous behaviour detected: `AnomalyDetector` identified the observations
   at `10:05` and `10:06` because of high response time, resource utilization, and
   `ERROR` logs.
3. Anomaly event generated: Each flagged observation became an event containing
   its timestamp, service, type, reasons, and original source record.
4. Event published: `EventProducer` published both events to the in-memory
   `anomaly-events` topic.
5. Event consumed: `EventConsumer` read both events from that same topic.
6. Event processed successfully: `run_pipeline` returned both consumed events
   as `events_consumed`.
7. Final AIOps output: The report identified `payment-service` timeouts and
   showed the metric and log reasons for each anomaly.

Observed result:

Records processed: 10
Anomalies detected: 2
Events consumed: 2
10:05: High response time, Error log detected
10:06: High response time, High CPU utilization, High memory utilization, Error log detected



