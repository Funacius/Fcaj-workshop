---
title: "From Logs to Per-Resource Alerts with Amazon CloudWatch"
menuTitle: "CloudWatch Alerts"
weight: 3
pre: "<b>3.3.</b>"
---

EduCloud Lite currently streams Elastic Beanstalk application logs to Amazon
CloudWatch and exposes recent operational information in the Admin dashboard.
Logs are essential for investigation, but a production operations process also
needs proactive notification: the system should report which resource crossed
a threshold without waiting for an administrator to search through a dashboard.

This article studies the AWS Cloud Operations Blog post
[Getting per-resource alarm notifications with Amazon CloudWatch](https://aws.amazon.com/blogs/mt/getting-per-resource-alarm-notifications-with-amazon-cloudwatch/),
published on July 30, 2026. It explains the behavior of CloudWatch Metrics
Insights alarms with `GROUP BY`, the notifications generated for individual
contributors, and the role of Amazon EventBridge when an organization needs
custom routing or automated remediation.

## 1. The operational problem

Monitoring one resource with one alarm is straightforward. At larger scale, a
fleet can contain many EC2 instances, accounts, or Regions. Creating and
maintaining one alarm per resource adds operational overhead, while a single
aggregate alarm can hide the full scope of an incident.

For example, assume one EC2 instance exceeds a CPU threshold and changes the
alarm to `ALARM`. Several minutes later, three other instances breach the same
threshold. An operations team still needs a notification for every newly
affected instance, even though the overall alarm never returned to `OK` between
those breaches.

Without resource-level detail, responders must manually inspect metrics to
identify affected resources. This increases detection and response time and can
lead to incomplete incident scoping.

## 2. Metrics Insights alarms and contributors

CloudWatch Metrics Insights supports SQL-style metric queries. A query using
`GROUP BY` returns multiple time series, for example one series for each
`InstanceId`. CloudWatch treats each unique dimension combination as a
**contributor** and evaluates it independently.

A simplified query for an EC2 fleet can follow this pattern:

```sql
SELECT AVG(CPUUtilization)
FROM SCHEMA("AWS/EC2", InstanceId)
GROUP BY InstanceId
```

The important behavior is that a new breaching contributor can invoke an alarm
action even when another contributor has already placed the alarm in the
`ALARM` state. Notifications include contributor identifiers and attributes,
such as account and instance information, so responders can identify the
specific resource.

## 3. Native notification paths

CloudWatch provides contributor-level alarm actions for standard operational
needs:

- **Amazon SNS:** sends the notification to an email, chat integration, ticket
  system, or another subscriber.
- **AWS Lambda:** invokes custom code for processing or remediation.

For a team that only needs a standard message identifying every breaching
resource, a native alarm action is normally sufficient. It avoids adding an
unnecessary event-processing layer.

This distinction supports a useful architecture principle: choose the smallest
solution that satisfies the operational requirement. Event-driven extensions
should be added only when their additional control is needed.

## 4. Extending the flow with EventBridge

CloudWatch also emits a Contributor State Change event to Amazon EventBridge
when an individual contributor breaches or recovers. These events are separate
from the normal alarm actions and contain structured resource-level data.

An EventBridge rule can route matching events to supported targets. This is
useful when an organization needs to:

- filter events based on account, resource, state, or other attributes;
- send different resource classes to different teams;
- enrich and reformat notification messages;
- open an incident or ticket automatically;
- invoke a runbook or Lambda remediation function; or
- deliver the same event to multiple operational destinations.

The resulting conceptual flow is:

```text
AWS resource metrics
        -> CloudWatch Metrics Insights alarm
        -> Contributor State Change event
        -> EventBridge rule
        -> SNS / Lambda / incident target
```

EventBridge decouples the alarm from downstream processing. Notification and
remediation logic can then evolve without rebuilding the original monitoring
query.

## 5. Relationship to EduCloud Lite

The current EduCloud deployment uses Elastic Beanstalk for the FastAPI backend.
Application logs are sent to CloudWatch, and administrators can inspect recent
logs and health information through the Admin dashboard.

This is a valid foundation, but it is still mainly reactive. An administrator
usually has to open the dashboard or CloudWatch to discover and investigate a
problem.

If the environment later expands to an Elastic Beanstalk load-balanced and
auto-scaled configuration, the per-resource approach becomes more valuable.
Multiple EC2 instances can serve the same application, and a failure might
affect only one contributor. A fleet-level alarm could detect the condition
while contributor-level data identifies the exact instance.

This AWS blog therefore represents a **future monitoring improvement**, not a
feature claimed as completed in the current EduCloud release.

## 6. Candidate metrics and alarms

The following signals would be useful for an expanded EduCloud environment:

| Signal | Operational meaning | Possible response |
| --- | --- | --- |
| EC2 `CPUUtilization` | Instance is overloaded or processing abnormal traffic | Inspect requests, scale capacity, or analyze code paths |
| EC2 status-check failure | Infrastructure or operating-system problem | Replace or recover the unhealthy instance |
| Load balancer HTTP 5xx | Backend is returning server errors | Inspect FastAPI and database logs |
| Target response time | API latency is increasing | Inspect database connections and application performance |
| Unhealthy host count | Load balancer cannot use one or more targets | Notify the operator and investigate the target health check |
| Disk or memory custom metric | Runtime resource pressure | Clean temporary data, tune workers, or resize the instance |

Metrics must be selected from actual failure modes. Adding many alarms without
clear ownership or action creates alert fatigue rather than observability.

## 7. A staged adoption plan

A cost-conscious student project can adopt the design incrementally:

1. Keep CloudWatch Logs and Elastic Beanstalk health as the operational base.
2. Add one actionable alarm for backend health or HTTP 5xx errors.
3. Route the alarm to a single SNS email subscription.
4. Test both the breach and recovery paths.
5. Add grouped Metrics Insights queries only after multiple resources are in
   service.
6. Introduce EventBridge only when custom filtering, multiple destinations, or
   automated remediation becomes necessary.

This order avoids deploying infrastructure that provides no current value. It
also creates a clear relationship between each alarm, its owner, and its
response procedure.

## 8. Key lessons

- **Logs and alarms solve different problems:** logs provide evidence for
  investigation; alarms provide timely detection.
- **Fleet alarms must preserve resource identity:** responders need to know
  which contributor breached, not only that the fleet has a problem.
- **A persistent `ALARM` state must not suppress new incidents:** contributor-
  level actions preserve visibility as other resources breach.
- **Native actions should be the default:** SNS or Lambda is sufficient for
  standard contributor notifications.
- **EventBridge is an extension layer:** use it for filtering, fan-out,
  enrichment, integration, and remediation.
- **Every alarm needs an action:** an alert without an owner or response
  procedure adds noise instead of resilience.

## Conclusion

CloudWatch Metrics Insights alarms provide a practical balance between fleet-
wide monitoring and per-resource visibility. A grouped alarm reduces the need
to maintain many individual alarms while still notifying operators about every
new breaching contributor.

For EduCloud Lite, the immediate priority remains stable logging and application
health. Per-resource notifications become an appropriate next step when the
backend scales beyond a single instance. EventBridge can then be introduced if
the project requires conditional routing or automated remediation.

**Primary source:** [Getting per-resource alarm notifications with Amazon CloudWatch – AWS Cloud Operations Blog](https://aws.amazon.com/blogs/mt/getting-per-resource-alarm-notifications-with-amazon-cloudwatch/)  
**CloudWatch documentation:** [Create an alarm on a Metrics Insights query](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Metrics_Insights_Alarm.html)  
**EduCloud source code:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)
