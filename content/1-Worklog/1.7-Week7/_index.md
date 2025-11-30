---
title: "Week 7 Worklog"
date: "2025-10-19"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives

- Set up Route53 for DNS management and domain configuration.
- Configure SSL/TLS certificates using AWS Certificate Manager.
- Implement HTTPS traffic routing through the load balancer.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Register a domain on Route53 <br> - Configure Route53 hosted zone | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/">Route53 Documentation</a> |
| Tue | - Request an SSL/TLS certificate via ACM <br> - Validate domain ownership | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/acm/">ACM Documentation</a> |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 09/10/2025 | 12/10/2025 | — |
| Thu | - Configure HTTPS listener on ALB with ACM certificate <br> - Set up HTTP to HTTPS redirect | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-update-rules.html">ALB Listener Rules</a> |
| Fri | - Test HTTPS connectivity and certificate validity <br> - Configure DNS failover (optional) | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html">DNS Failover Guide</a> |
| Sat | - Monitor Route53 query metrics <br> - Document domain and certificate details | 09/10/2025 | 12/10/2025 | — |

### Week 7 Achievements

- Successfully registered and configured a custom domain on Route53.
- Obtained and validated an SSL/TLS certificate via ACM.
- Configured HTTPS traffic on the ALB with proper certificate binding.
- Implemented HTTP to HTTPS redirect for security compliance.
- Tested DNS resolution and HTTPS connectivity from multiple locations.
- Learned Route53 routing policies (simple, weighted, latency-based).

### Challenges and lessons learned

- DNS propagation takes time; TTL settings affect how quickly changes take effect.
- ACM certificate validation requires either DNS or email verification.
- ALB listener rules must properly match hostnames for certificate validation.
