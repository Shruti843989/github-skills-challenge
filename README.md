# AIOps Monitoring and Event Processing

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

## Scenario

This project simulates a basic AIOps monitoring pipeline for a `payment-service`.

The operational data contains timestamps, response time, CPU utilization, memory utilization, log level, and log messages.

The pipeline detects anomalous service behavior and sends detected anomaly events through an in-memory event topic from producer to consumer.

## Data Analysis

The dataset contains 10 records from `10:00` to `10:09`.

Normal observations generally have:

- Response time around 120–150 ms
- CPU utilization around 42–50%
- Memory utilization around 51–57%
- `INFO` log level
- Successful payment processing messages

Two anomalous observations were identified:

| Timestamp | Response Time | CPU | Memory | Log Level | Observation |
|---|---:|---:|---:|---|---|
| 10:05 | 610 ms | 75% | 70% | ERROR | Payment service timeout |
| 10:06 | 640 ms | 94% | 91% | ERROR | Database connection timeout |

## Anomaly Detection

The detector uses fixed threshold rules:

- Response time > 500 ms → High response time
- CPU > 80% → High CPU utilization
- Memory > 80% → High memory utilization
- `ERROR` log level → Error log detected

The detector identified 2 anomalous records.

### Detection Results

**10:05**

- High response time
- Error log detected

**10:06**

- High response time
- High CPU utilization
- High memory utilization
- Error log detected

## Event Flow

```text
Operational Data
       |
       v
Anomaly Detector
       |
       v
Event Producer
       |
       v
Event Topic
       |
       v
Event Consumer
       |
       v
AIOps Output