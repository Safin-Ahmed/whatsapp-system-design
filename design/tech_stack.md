# Tech Stack

The following tools and services are used to build this messaging system:

---

## Backend

- **Node.js** (NestJS)
- **TypeScript**

## Communication

- **WebSockets**
- **REST API** (fallback + media upload)

## Pub/Sub

- **Redis Cluster**

## Database

- **PostgreSQL** (for durable messages, chats, inbox, participants)

## Storage

- **AWS S3** (media storage via pre-signed URLs)

## Others

- **Docker** (local development setup)
- **Github Actions** (CI/CD)
- **AWS** (Infra)
- **PULUMI** (Infra Automation)
