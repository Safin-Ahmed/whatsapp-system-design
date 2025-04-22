# WhatsApp-Scale Messaging System Design

This repository contains the architecture and design of a real-time messaging system inspired
by WhatsApp, capable of supporting 1 billion daily active users.

---

## Features

1. Real time group messaging
2. Offline delivery with durable Inbox
3. Multi-device sync (max 3 clients per user)
4. Redis Cluster Pub/Sub as WebSocket backplane
5. Media sharing via S3 pre-signed URLs
6. Scalable, event-driven architecture

---

## System Diagram

![System Design](./diagrams/whatsapp.png)

---

## Why I Built This

I'm Safin - a full-stack software engineer who loves building at scale. I designed this WhatsApp-scale messaging system from scratch to demonstrate how I think about distributed systems, trade-offs, durability, and real-time infrastructure.

This project reflects how I approach engineering:

- Thoughtfully scoped
- Deeply reasoned
- Pracitcal in trade-offs
- Ready to scale

Feel free to connect or reach out:
safin.ahmed2000@gmail.com
[LinkedIn](https://www.linkedin.com/in/safin-ahmed/)
[X](https://x.com/Ahmed2000Safin)

## Documentation

- [architecture.md](./architecture.md)
- [requirements.md](./requirements.md)
- [delivery_strategy.md](./delivery_strategy.md)
- [tradeoffs.md](./tradeoffs.md)
- [tech_stack.md](./tech_stack.md)

## What's Next

The system design is complete and published. I'm currently working on the implementation, with updates to follow.

Star ⭐️ the repo or follow me to stay updated.
