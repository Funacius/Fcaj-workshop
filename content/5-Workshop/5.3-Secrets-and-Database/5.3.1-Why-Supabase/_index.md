---
title: "Why Supabase"
weight: 1
chapter: false
pre: "<b>5.3.1.</b>"
---

# Why Supabase instead of Aurora PostgreSQL?

EduCloud Lite is an internship project that prioritizes fast setup, low cost,
and reproducibility for other students. For this version, Supabase PostgreSQL is
a better fit than provisioning Aurora PostgreSQL.

| Criteria | Supabase PostgreSQL | Amazon Aurora PostgreSQL |
|---|---|---|
| Best fit | Demo, MVP, student project, fast delivery | Production workload requiring AWS-native scaling, HA, replicas, and deeper operational control |
| Initial cost | Free plan is available for small projects | Usually involves compute, storage, and I/O or ACU-based charges depending on configuration |
| Setup time | Create a project, copy the connection string, and connect | Requires cluster setup, subnet groups, security groups, VPC/networking, and access configuration |
| Workshop complexity | Lower; learners can focus on the application and AWS deployment | Higher; beginners can get blocked by networking and database operations |
| When to migrate | When the project outgrows the demo/MVP stage | Suitable for a more production-grade AWS-native database layer |

For the submission scope, Supabase reduces database setup time and avoids
introducing more infrastructure cost than necessary. Aurora PostgreSQL remains a
stronger production option, but it is beyond the minimum requirement for a
working independent website and a reproducible deployment guide.

References:

- [Supabase Pricing](https://supabase.com/pricing)
- [Amazon Aurora Pricing](https://aws.amazon.com/rds/aurora/pricing/)

