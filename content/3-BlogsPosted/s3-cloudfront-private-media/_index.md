---
title: "How Supercell Uses Amazon Aurora for Scalable Gaming"
menuTitle: "Supercell and Amazon Aurora"
weight: 2
pre: "<b>3.2.</b>"
---

# How Supercell Uses Amazon Aurora for Scalable Gaming

Mobile games operate continuously. Players expect their progress, purchases,
social interactions, and events to remain available even when a game becomes
globally popular. This makes database availability a product requirement, not
only an infrastructure concern.

This article studies the AWS customer story
[Supercell Leverages AWS for Seamless and Scalable Gaming Experience](https://aws.amazon.com/solutions/case-studies/supercell-aurora-case-study/)
and extracts general lessons about managed relational databases, high
availability, and operational focus. It is an independent analysis of the case
study and does not claim access to Supercell's private implementation details.

## 1. The problem faced by a global game studio

Supercell develops mobile games including Clash of Clans. According to AWS,
Supercell supports approximately 200 million monthly active users across its
globally launched games. At that scale, a database outage can affect player
progress, purchases, matchmaking state, and live events at the same time.

The important requirements are:

- high availability during normal play and peak events;
- predictable performance as the player population grows;
- backups and recovery without long operational interruptions; and
- less time spent troubleshooting database hardware.

## 2. Why Amazon Aurora is relevant

The AWS case study explains that Supercell adopted Amazon Aurora, a managed
relational database compatible with MySQL. AWS states that the move
significantly reduced downtime and allowed Supercell's engineers to focus more
on game features instead of hardware troubleshooting.

Aurora separates database compute from a distributed storage layer. The storage
volume is replicated across Availability Zones, and Aurora can use reader
instances for read scaling and failover. These characteristics are described in
the [Aurora high-availability documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html).

Aurora is not simply “a faster database.” It is a managed operating model that
automates parts of provisioning, replication, backup, failure detection, and
recovery while leaving schema design and query tuning to the application team.

## 3. A conceptual Aurora request flow

The following example is an illustrative application pattern, not Supercell's
private source code:

```python
import os
import psycopg2

writer = os.environ["AURORA_WRITER_ENDPOINT"]
reader = os.environ["AURORA_READER_ENDPOINT"]

def read_player_profile(player_id: str):
    with psycopg2.connect(host=reader, dbname="game") as conn:
        with conn.cursor() as cursor:
            cursor.execute(
                "SELECT level, inventory FROM player_profiles WHERE id = %s",
                (player_id,),
            )
            return cursor.fetchone()

def save_purchase(player_id: str, item_id: str):
    with psycopg2.connect(host=writer, dbname="game") as conn:
        with conn.cursor() as cursor:
            cursor.execute(
                "INSERT INTO purchases(player_id, item_id) VALUES (%s, %s)",
                (player_id, item_id),
            )
        conn.commit()
```

The principle is simple: route writes to the writer endpoint and distribute
read-heavy operations to reader capacity. The exact topology, connection pool,
transaction strategy, and consistency requirements must be designed for each
game.

## 4. Lessons from the case study

### 4.1 Reliability is part of player experience

A player does not distinguish between an application bug and a database outage.
If progress is lost or a purchase is delayed, the product feels unreliable.
Database availability should therefore be measured as a player-facing quality
attribute.

### 4.2 Managed services reduce undifferentiated work

The value of Aurora is not only its engine compatibility. A managed service lets
the team spend less effort on hardware maintenance, storage repair, backups, and
failover procedures, while still retaining control over relational schemas and
queries.

### 4.3 Read and write workloads should be separated

Game backends often perform many reads: player profiles, inventories,
leaderboards, configuration, and event data. Separating read capacity from the
writer can protect transactional operations during traffic spikes.

### 4.4 Plan for events, not only average traffic

Live games have launches, seasonal events, promotions, and content updates. A
database design that works during a quiet week may fail during an event. Capacity
tests should model peak concurrency and write bursts.

### 4.5 Operations must remain visible

Managed infrastructure does not eliminate the need for monitoring. Teams still
need alarms for connection saturation, query latency, replication lag, storage,
failed transactions, and unusual write volume.

## 5. What should be evaluated before adopting Aurora?

Aurora is a strong candidate when an application needs a managed relational
engine, high availability across Availability Zones, read replicas, automated
backups, or global database options. AWS documents Aurora as compatible with
MySQL and PostgreSQL, with a distributed storage subsystem that grows as data
increases. [Aurora overview](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

It is not automatically the best option for every project. A small prototype
may be better served by a simpler managed PostgreSQL provider. The decision
should consider:

- expected traffic and write volume;
- required recovery point and recovery time objectives;
- number of Availability Zones and Regions;
- read replica and failover requirements;
- connection pooling and query behavior; and
- total cost of instances, storage, I/O, backups, and data transfer.

## 6. Limitations and responsible interpretation

The AWS customer story provides a high-level description rather than a complete
Supercell architecture diagram. Therefore, this article does not infer the
company's exact schema, cluster size, query design, or internal failover runbook.
The technical examples are general patterns that illustrate the lessons, not a
reconstruction of proprietary systems.

## Conclusion

Supercell's AWS customer story demonstrates an important principle for online
games: database reliability directly affects player trust. Amazon Aurora can
reduce operational database work through managed replication, distributed
storage, backups, and failover capabilities. The broader lesson is to choose a
database architecture based on player-facing reliability requirements and peak
workload behavior, not only on average traffic or benchmark numbers.

## References

- [Supercell Leverages AWS for Seamless and Scalable Gaming Experience — AWS Customer Story](https://aws.amazon.com/solutions/case-studies/supercell-aurora-case-study/)
- [What is Amazon Aurora? — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)
- [High availability for Amazon Aurora — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)
- [Amazon Aurora storage reliability — AWS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.StorageReliability.html)
