# Design Trade-Offs

This system was designed with performance, durability, and scalability in mind. Below are the key trade-offs and design choices.

---

## Redis Pub/Sub vs Kafka

- **Chosen**: Redis Pub/Sub
- Low Latency (single digit ms)
- Built-in backplane for WebSocket
- Scalable using redis cluster mode
- Tradeoff -> At-most-once delivery (can be handled via postgres InboxPending table)

---

## Per-User Topics vs Per-Client Topics

- **Chosen**: Per-Client Topics
- Enables multi-device sync
- Supports granular delivery
- Tradeoff -> Increases topic count, slightly more Redis memory usage (can be handled via a constraint of 3 devices per user)

---

## InboxPending Table Design

- **Chosen**: Per-Client Inbox
- Allows tracking delivery per device
- Works well with Redis non durable nature
- Tradeoff -> Requires periodic cleanup for more than 30 days old messages

---

## WebSockets vs Polling

- **Chosen**: WebSockets
- Instant message push
- Scalable with redis cluster
- Tradeoff -> Requires persistent connection infrastructure

---

## AWS S3 Upload vs Postgres Upload

- **Chosen**: AWS S3 Upload
- Highly scalable and available managed service for blob storage
- Offload pressure from core backend to AWS managed service
- High security using pre-signed urls
- Tradeoff -> Additional latency (single digit ms)
