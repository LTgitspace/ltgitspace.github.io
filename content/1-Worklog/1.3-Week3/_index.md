---
title: "Week 3 Worklog"
date: "2025-09-21"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives
\* Stand up Angular app with local HTTPS.
\* Integrate AWS Cognito, S3, API Gateway, Lambda, CloudWatch.
\* Establish baseline logging, metrics, and auth flows.

### Tasks
| Day  | Task                                                                                                  | Planned Date | Done Date | References |
|------|-------------------------------------------------------------------------------------------------------|--------------|-----------|------------|
| Mon  | Initialize Angular project. <br> add self-signed cert <br> document HTTPS setup.                      | 2025-09-09   | 2025-09-09 |  |
| Tue  | Configure AWS Cognito user pool<br> implement login/logout & route guards<br> token refresh handling. | 2025-09-10   | 2025-09-10 |  |
| Wed  | Build S3 service with presigned URL upload/download<br> add retry + error notifications.              | 2025-09-11   | 2025-09-11 |  |
| Thu  | Create Lambda + API Gateway endpoints                                                                 | 2025-09-12   | 2025-09-12 |  |
| Fri  | Work on proposal                                                                                      | 2025-09-13   | 2025-09-13 |  |
| Sat  | Work on proposal                                                                                      | 2025-09-14   | 2025-09-14 |  |

### Achievements
\* Local Angular dev over HTTPS.
\* Cognito auth (login, refresh, protected routes).
\* S3 secure file upload/download workflow.
\* Lambda + API Gateway backend consumed by frontend.
\* Added logging + initial metrics (invocation counts, basic latency).
\* Reduced overly broad IAM policies.
\* Increased frontend test coverage from ~15% to ~40%.
\* Captured actionable AWS event insights.

### Blockers / Mitigations
\* CORS misconfiguration on API Gateway \-> adjusted allowed origins/headers.
\* Self-signed cert trust prompts \-> added README setup section.
\* Large file upload latency \-> added chunked upload to Week 4 backlog.
