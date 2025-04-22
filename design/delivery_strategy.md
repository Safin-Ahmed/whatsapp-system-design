# Delivery Strategy

This document outlines how messages are routed, delivered, and persisted across clients and devices.

---

## Send Message

1. Client sends message via WebSocket
2. Backend stores message in DB and creates InboxPending records for offline recipients clients
3. Backend publishes message to Redis topic:
   - `group:<group_id>` (for group chat)
   - `client:<client_id>` (for direct 1:1 message)

---

## Receive Message

1. Other chat servers subscribed to the topic receive the message.
2. Forward to connected Websocket clients.
3. Clients send an `ack` -> backend removes specific InboxPending row.

---

## Offline clients

- On reconnect, client fetches undelivered messages using `InboxPending` table.
- Deletes or marks delivered entries

---

## Media Handling

- Attachments are uploaded to S3 using pre-signed URLs.
- Messages DB stores only the uploaded url.

---

## Notes

- Max 3 devices/client connection per user.
- Redis Pub/Sub is ephemeral -> persistent InboxPending table ensures durability
- We will need to periodically clean up the old messages in the Inbox and messages tables with Cron Job which will delete messages older than 30 days.
