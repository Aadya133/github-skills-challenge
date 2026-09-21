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
