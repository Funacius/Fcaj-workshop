---
title: "What EduCloud Can Learn from Riot Games' Amazon EKS Platform"
menuTitle: "Riot Games and Amazon EKS"
weight: 2
pre: "<b>3.2.</b>"
---

# What EduCloud Can Learn from Riot Games' Amazon EKS Platform

Large online games and learning platforms appear different, but they share an
important infrastructure problem: traffic changes quickly, services must remain
available, and developers should spend more time building product features than
managing servers.

This article studies the AWS case study
[Riot Games Cuts $10M Annual Infrastructure Costs by Migrating to Amazon EKS](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/)
and translates its engineering lessons to EduCloud Lite. The article is an
independent analysis; EduCloud Lite does **not** claim to run Amazon EKS in its
current deployment.

## 1. The infrastructure challenge

Riot Games operates live-service games such as League of Legends and VALORANT.
Its platform must support a global player base, low latency, high availability,
automatic scaling, and repeatable deployments across regions.

EduCloud has a smaller scale, but the underlying questions are similar:

- How can a new course or lesson be released without manually changing a server?
- How can the platform handle a sudden assessment or enrollment spike?
- How can developers use a consistent deployment process?
- How can infrastructure cost stay proportional to actual usage?

## 2. What Riot Games changed

According to the AWS case study, Riot began migrating to Amazon EKS in 2021 after
using a previous container orchestration platform. EKS provided a managed
Kubernetes control plane, while Riot standardized its developer environment
around repeatable infrastructure and automated node management.

The reported outcomes include:

- support for more than 180 million monthly active users;
- approximately 10 million dollars in annual infrastructure savings;
- 90 percent faster infrastructure setup; and
- 12 times faster game infrastructure deployment.

Riot also used Karpenter for node lifecycle management, Terraform for
infrastructure automation, isolated clusters for games or use cases, and AWS
Local Zones/Outposts for latency-sensitive workloads. These details are from the
[official AWS case study](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/),
not measurements from EduCloud.

## 3. Technical principles behind the result

### 3.1 Managed orchestration

EKS removes the need to operate a Kubernetes control plane manually. Developers
can focus on application containers, health checks, resource requests, and
deployment policies while AWS manages the control-plane layer.

### 3.2 Automated capacity

Karpenter observes pending workloads and provisions suitable nodes instead of
requiring a fixed server pool. This is useful for workloads that have quiet and
busy periods, such as a game launch or a large online examination.

### 3.3 Standardized developer platform

Riot created a centrally managed environment so teams could request compute,
networking, and storage using approved patterns. The platform abstracts low-level
infrastructure while preserving governance.

### 3.4 Isolation and blast-radius control

Riot moved toward isolated clusters for individual games or use cases. A problem
in one workload is therefore less likely to consume the capacity or
configuration of another workload.

### 3.5 Location-aware delivery

For strict latency requirements, Riot uses services such as AWS Local Zones and
Outposts to place workloads closer to players. EduCloud does not need this
complexity today, but the principle is relevant if a future release serves
multiple regions.

## 4. A small Kubernetes deployment example

The following example shows the type of declarative configuration that a future
EduCloud container deployment could use. It is not part of the current
Elastic Beanstalk deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: educloud-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: educloud-api
  template:
    metadata:
      labels:
        app: educloud-api
    spec:
      containers:
        - name: api
          image: ACCOUNT_ID.dkr.ecr.ap-southeast-1.amazonaws.com/educloud-api:1.0.0
          ports:
            - containerPort: 8000
          resources:
            requests:
              cpu: "250m"
              memory: "512Mi"
            limits:
              cpu: "1"
              memory: "1Gi"
```

In a production EKS design, this Deployment could be combined with a Horizontal
Pod Autoscaler, an Application Load Balancer, Amazon ECR, CloudWatch Container
Insights, and an automated node provisioner such as Karpenter.

## 5. Applying the lessons to EduCloud Lite

| Riot Games principle | Current EduCloud implementation | Possible future direction |
| --- | --- | --- |
| Managed compute | FastAPI on Elastic Beanstalk | Container image on EKS or ECS |
| Automated scaling | Single-instance environment for demo | Auto Scaling or HPA based on demand |
| Standardized provisioning | Deployment notes and scripts | Terraform or CloudFormation |
| Workload isolation | Separate API, S3, Cognito, and database responsibilities | Namespace or service isolation |
| Global delivery | CloudFront for frontend and course media | Regional API deployments and edge-aware routing |
| Observability | Elastic Beanstalk health and CloudWatch logs | Container Insights, metrics, traces, and alarms |

The correct lesson is not to replace Elastic Beanstalk immediately. For an
internship submission with low traffic, Elastic Beanstalk is simpler and cheaper
to operate. EKS becomes reasonable when the application has multiple container
services, independent scaling requirements, or a team able to operate Kubernetes.

## 6. Cost and complexity trade-offs

EKS can improve portability, standardization, and scaling, but it introduces
cluster operations, networking, IAM for service accounts, observability, image
management, and Kubernetes security responsibilities. A managed control plane
does not make the entire application serverless.

For EduCloud, the practical progression is:

1. keep the current Elastic Beanstalk deployment stable;
2. package FastAPI as a Docker image;
3. add infrastructure as code and health alarms;
4. measure request volume and scaling needs; and
5. migrate to ECS or EKS only when the operational benefit justifies the extra
   complexity.

## 7. Key takeaways

- Scaling is an architectural capability, not only a larger EC2 instance.
- A platform team can make cloud infrastructure repeatable for developers.
- Automated capacity should respond to workload demand and cost limits.
- Isolation reduces the blast radius of failures and simplifies ownership.
- The best service depends on project scale: EKS is powerful, but not mandatory
  for every web application.

## Conclusion

Riot Games demonstrates how a global game company can use Amazon EKS, Karpenter,
Terraform, and location-aware AWS services to simplify infrastructure and
accelerate releases. EduCloud Lite applies the same ideas at a smaller scale:
separate responsibilities, automate repeatable steps, monitor health, and only
introduce Kubernetes when the workload and team are ready for it.

## References

- [Riot Games Cuts $10M Annual Infrastructure Costs by Migrating to Amazon EKS — AWS Case Study](https://aws.amazon.com/solutions/case-studies/riot-games-case-study/)
- [Amazon Elastic Kubernetes Service documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [Karpenter documentation](https://karpenter.sh/)
- [EduCloud Lite source code](https://github.com/Funacius/EduCloud)
