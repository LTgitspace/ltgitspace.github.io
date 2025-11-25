---
title: "Week 9 Worklog"
date: "2025-11-02"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 9 Objectives

- Set up DynamoDB for NoSQL data storage and caching.
- Implement DynamoDB tables with proper indexing strategies.
- Integrate DynamoDB with the application for session management.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Create DynamoDB tables for application data <br> - Define primary keys and attributes | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/">DynamoDB Documentation</a> |
| Tue | - Configure Global Secondary Indexes (GSI) for query flexibility <br> - Set up TTL for session data expiration | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html">DynamoDB Core Concepts</a> |
| Wed | - Configure on-demand billing mode vs provisioned capacity <br> - Monitor DynamoDB throttling and capacity | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BillingMode.html">Billing Mode Guide</a> |
| Thu | - Update application code to integrate DynamoDB operations <br> - Implement session storage using DynamoDB | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html">Query Operations</a> |
| Fri | - Test CRUD operations (Create, Read, Update, Delete) <br> - Load test to verify performance under concurrent requests | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html">Best Practices</a> |
| Sat | - Enable point-in-time recovery <br> - Monitor and optimize query patterns | 23/10/2025 | 26/10/2025 | — |

### Week 9 Achievements

- Successfully created DynamoDB tables with optimized key design.
- Implemented Global Secondary Indexes for flexible querying.
- Configured TTL for automatic session data cleanup.
- Integrated DynamoDB session storage into the application.
- Tested CRUD operations and verified performance metrics.
- Learned DynamoDB partition key design and hot partition mitigation.

### Challenges and lessons learned

- Hot partitions can cause throttling; distribute write load across partition keys.
- Query costs vary based on index utilization; optimize attribute selection in queries.
- On-demand vs provisioned capacity trade-off depends on workload predictability.
