---
title: "Week 11 Worklog"
date: "2025-11-16"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 11 Objectives

- Implement comprehensive monitoring using CloudWatch.
- Create dashboards and alarms for application health.
- Set up logging aggregation and analysis.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Create CloudWatch dashboards for key metrics <br> - Set up custom metrics from application logs | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/">CloudWatch Documentation</a> |
| Tue | - Configure CloudWatch alarms for CPU, memory, and network <br> - Set up SNS topics for alarm notifications | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/">Events Guide</a> |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 06/11/2025 | 09/11/2025 | — |
| Thu | - Set up CloudWatch Insights for log analysis <br> - Create queries for common troubleshooting scenarios | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html">CloudWatch Insights</a> |
| Fri | - Configure application to send custom metrics to CloudWatch <br> - Test alarm triggering and notification flow | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html">Metrics Guide</a> |
| Sat | - Review and optimize dashboard layout <br> - Document monitoring architecture and alert thresholds | 06/11/2025 | 09/11/2025 | — |

### Week 11 Achievements

- Created comprehensive CloudWatch dashboards showing system health.
- Configured proactive alarms for key performance indicators (CPU, memory, error rates).
- Implemented centralized logging for all ECS tasks and services.
- Set up CloudWatch Insights queries for rapid troubleshooting.
- Verified SNS notifications delivery for critical alerts.
- Achieved real-time visibility into application and infrastructure performance.

### Challenges and lessons learned

- Custom metrics incur additional CloudWatch costs; be selective about what to monitor.
- Log retention policies should balance storage costs with compliance requirements.
- Alarm thresholds require tuning to avoid false positives and alert fatigue.
