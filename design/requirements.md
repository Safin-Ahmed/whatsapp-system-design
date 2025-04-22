# Requirements

This document outlines the core requirements for building a WhatsApp-scale messaging system.

---

## Functional Requirements

1. **Group Messaging**

   - Users can create and join group chats (limit: 100 participants)
   - Each group maintains roles (Admin, Member)

2. **Real-time Messaging**

   - Users can send and receive messages instantly

3. **Offline Message Sync**

   - Users receive messages sent while they were offline (up to 30 days later).
   - Messages are synced across all devices

4. **Media Sharing**

   - Users can upload and send attachments (images, videos, documents)

5. **Multi-Device Support**
   - A user can connect with up to 3 active devices.
   - Each device must indepedently receive and acknowledge messages.

---

## Non-Functional Requirements

1. **Scalability**

   - Support 1 billion daily active users (DAU).
   - Horizontal scaling via Redis cluster and distributed message delivery.

2. **Latency**

   - < 100ms delivery time for real-time message fanout
   - Fast reconnect and message catch-up for offline clients.

3. **Durability**

   - Messages must persist in storage for 30 days.
   - No message loss in case of client or server failure

4. **Availability**

   - Highly available services with failover support
   - Clients must be able to connect to any regionally available chat server.

5. **Security**
   - Enforce role-based group permissions
   - Secure message delivery and attachment access via pre-signed urls.
