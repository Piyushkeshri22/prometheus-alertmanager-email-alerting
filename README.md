# Prometheus & Alertmanager Email Alerting

## Overview

Implemented email-based alert notifications using Prometheus and Alertmanager in Kubernetes.

This implementation demonstrates how Prometheus detects CPU threshold violations and sends alerts to Alertmanager, which delivers email notifications through Gmail SMTP.

Both firing and resolved email notifications are demonstrated.

## Technologies Used

- Kubernetes
- Prometheus
- Prometheus Operator
- Alertmanager
- Gmail SMTP
- YAML


## Prerequisites

Before implementing this setup, ensure the following components are available:

- A running Kubernetes cluster.
- Prometheus Operator installed in the cluster.
- Prometheus and Alertmanager deployed and configured.
- Node Exporter metrics available for CPU monitoring.
- A Gmail account with 2-Step Verification enabled.
- A Gmail App Password for SMTP authentication.
- A Kubernetes Secret named `gmail-smtp-secret` in the `monitoring` namespace, containing the SMTP password under the key `password`.

**Note:** The CPU alert threshold is set to 5% for demonstration purposes to make alert triggering easier during testing.


## Configuration Files

| File | Description |
|---|---|
| `cpu-alert.yml` | Defines the HighCPUUsage alert using PrometheusRule |
| `alertmanager-email.yml` | Configures email notifications using AlertmanagerConfig |

## Alert Configuration

- Alert Name: `HighCPUUsage`
- Condition: CPU utilization above 5%
- Evaluation Duration: 1 minute
- Severity: Warning

## Email Notification Configuration

- SMTP Server: `smtp.gmail.com:587`
- TLS: Enabled
- Receiver: Gmail
- Repeat Interval: 1 hour
- Resolved Notifications: Enabled

## Alerting Workflow

1. Prometheus continuously collects CPU metrics.
2. Prometheus evaluates the configured alert rule.
3. When CPU utilization exceeds the configured threshold, the alert enters the firing state.
4. Prometheus forwards the alert to Alertmanager.
5. Alertmanager sends an email notification through Gmail SMTP.
6. When the alert condition clears, Alertmanager sends a resolved notification.

### Architecture

```text
CPU Metrics
     |
     v
 Prometheus
     |
     v
 Alert Rule
     |
     v
 Alertmanager
     |
     v
 Gmail SMTP
     |
     v
 Email Notification
 (Firing / Resolved)
```

## Screenshots

### 1. Prometheus Alert Firing

Shows the HighCPUUsage alert in the firing state.


![Prometheus Alert](screenshots/01-prometheus-alert-firing.png)

### 2. Alertmanager

Shows the alert received and grouped by Alertmanager.


![Alertmanager](screenshots/02-alertmanager-received-alert.png)

### 3. Firing Email Notification

Demonstrates the email notification received when the CPU alert fires.


![Firing Email](screenshots/03-email-alert-firing.png)


### 4. Resolved Email Notification

Demonstrates the recovery notification received after the alert condition clears.


![Resolved Email](screenshots/04-email-alert-resolved.png)


## Security

- SMTP credentials are stored in a Kubernetes Secret.
- Gmail App Passwords are not stored directly in configuration files.
- Sensitive information is excluded from this repository.

## Outcome

Successfully demonstrated end-to-end email alerting using Prometheus and Alertmanager, including both firing and resolved notifications.
