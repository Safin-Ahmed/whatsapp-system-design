# Architecture Overview

This document breaks down the major components and responsibilities in the system.

---

## Components

### 1. **Client**

- Establishes persistent Websocket connection
- Subscribes to group or client-specific channels.
- Upload media to S3 via pre-signed urls

### 2. **Backend Servers**

- Handle core chat logic and WebSocket sessions.
- Maintain persistent connections to Redis and PostgreSQL
- Act as message producers and consumers.

### 3. **Redis Cluster (Pub/Sub)**

- Acts as the WebSocket backplane
- Topics are created per `userId + clientId`
- Scales horizontally using redis cluster mode.

### 4. **PostgreSQL**

- Stores persistent data:
  - Messages
  - Clients
  - InboxPendings
  - Chat
  - ChatParticipants

### 5. AWS S3 (Media Storage)

- Used to upload and retrieve attachments using signed URLs.

---

### Flow Highlights

- Message delivery uses a Redis pub/sub fanout model
- Media is uploaded outside of WebSocket using signed URLs.
- Multiple devices per user handled using client-specific topics and inbox tracking.
