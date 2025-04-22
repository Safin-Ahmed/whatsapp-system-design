# Deep Dives: Architecture Decisions

As I designed this messaging system, I knew solving the functional requirements wasn't enough - I needed to address the **non-functional realities** that show up in production systems at scale.

This section walks through some of the deeper architectural challenges I anticipated and tackled, with a focus on **scale, reliability, multi-device delivery, and architectural trade-offs**.

These are the areas I'd expect any senior engineer or solutions architect to reason about - especially when building systems for millions or billions of users.

---

## 1. Handling Billions of Simultaneous Users

The moment I moved beyond a single-host prototype, it was obvious that I'd need to support **hundreds or even thousands of chat servers**. But horizontal scaling isn't enough - **routing becomes the real bottlenck**.

Consider this: if User A is connected to one server and sends a message to User B and C, how do we guarantee delivery if each of them is connected to different servers?

> This is the **host confusion problem** - and it's a killer for naive horizontal scaling.

---

### Approaches I Rejected

- **Naive Horizontal Scaling**
  Just load balance across chat servers and hope. It's easy to implement, but breaks routing unless all users in a conversation are connected to the same server. It doesn't scale geographically.

- **Kafka Topic Per User**
  This approach guarantees delivery, but introduces massive operational overhead - too many partitions, billions of kafka topics, coordination challenges, and significant latency for real-time use cases.

---

### My Solution: Redis Pub/Sub as a Real-Time Backplane

After evaluationg the trade-offs, I settled on a **clustered Redis Pub/Sub model** to fan out messages across chat servers.

- Each user or group is assigned a topic (`client+user:<id>` or `group:<id>`).
- Chat servers **subscribe to the topics** they need.
- When a message is published, Redis fans it out to the right servers - regardless of who is connected where.

This lets me:

- Route messages **Without inter-server coordination**
- Maintain stateless chat servers
- Scale horizontally without bottlenecks

It's simple, fast and production-proven

---

## Supporting Multiple Devices Per User (The "Multi-Client Sync" Problem)

When you design messaging systems today, **supporting multi-device sync is table stakes**.

I asked myself:

> What happens if a user reads a message on their phone, but later opens their laptop and expects the same sync state?

This led me to rethink delivery strategy.

---

### My Approach

#### Per-Client Subscriptions

Each device is assigned a `clientId`, and chat servers subscribe to `client+user:<id>` topics. This lets me target devices independently - ensuring each one receives every message.

#### Per-Client Inbox Table

I introduced a dedicated `InboxPending` table that tracks message delivery **per client**, not per user. This avoids false assumptions like "once one device gets the message, all are synced".

#### Client Registry

Each `clientId` is stored with metadata - device type, last seen, and user linkage - making it easy to manage active sessions and expire stale ones.

---

### Constraints I Implemented

- **Limit of 3 clients per user** - to prevent resource exhaustion and control delivery fanout.
- **Stale client cleanup** - background workers clear out inactive sessions

## Why These Deep Dives Matter

Building backend systems at scale isn't just about choosing Redis or WebSockets. It's about understanding **the pressure points of scale** - routing, delivery semantics, client fanout, storage and building in resilience _before_ you hit the bottleneck.

These are the decisions that separate good systems from great ones - and building this from scratch gave me an opportunity to interalize those lessons deeply.
