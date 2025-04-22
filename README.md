# WhatsApp-Scale Messaging System Design

This document outlines the architecture and components of a distributed messaging system designed to scale to **1 billion daily active users**, support **real-time chat**, **media messaging**, and **multi-device delivery** with durable offline sync.

---

## Design Modules

1. **Create Chat**
2. **Send & Receive Message**
3. **Send & Receive Attachments**
4. **Handle Multiple Clients per User**
5. **Redis Cluster as Pub/Sub Backplane with Socket**
6. **Durable Offline Sync**

---

## Diagram

![System Design](./design/diagrams/whatsapp.png)

---

## Next Sections

- [architecture.md](./design/architecture.md)
- [requirements.md](./design/requirements.md)
- [delivery_strategy.md](./design/delivery_strategy.md)
- [tradeoffs.md](./design/tradeoffs.md)
- [tech_stack.md](./design/tech_stack.md)
