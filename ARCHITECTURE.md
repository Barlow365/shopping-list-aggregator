# Shopping List Aggregator - Technical Architecture

**Version:** 1.0
**Last Updated:** January 2026
**Status:** Design Specification

---

## Table of Contents

1. [Technology Stack](#technology-stack)
2. [System Architecture](#system-architecture)
3. [Data Models](#data-models)
4. [API Design](#api-design)
5. [Real-Time Synchronization](#real-time-synchronization)
6. [Authentication & Security](#authentication--security)
7. [Third-Party Integrations](#third-party-integrations)
8. [Scalability Plan](#scalability-plan)
9. [Performance Targets](#performance-targets)

---

## Technology Stack

### Frontend

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Framework** | React with TypeScript | Type safety, component reusability, large ecosystem |
| **Build Tool** | Vite | Fast dev server, optimized production builds |
| **UI Library** | Tailwind CSS + shadcn/ui | Rapid development, consistent design system |
| **State Management** | Zustand | Simple, performant, less boilerplate than Redux |
| **Real-Time** | Socket.io client | WebSocket support with fallback to long-polling |
| **Forms** | React Hook Form + Zod | Type-safe form validation |
| **Date/Time** | date-fns | Lightweight, tree-shakeable |

### Backend

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Framework** | Node.js + Express | Fast, JavaScript full-stack, large ecosystem |
| **Alternative** | Python + FastAPI | Type hints, async support, auto-generated docs |
| **Real-Time** | Socket.io server | Bidirectional real-time communication |
| **Authentication** | JWT + bcrypt | Stateless auth, secure password hashing |
| **Validation** | Zod (shared with frontend) | Single source of truth for schemas |
| **Task Queue** | Bull (Redis-backed) | Background jobs (receipts, notifications) |

### Database

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Primary DB** | PostgreSQL | ACID compliance, JSON support, mature ecosystem |
| **Cache Layer** | Redis | Fast in-memory cache for sessions and real-time data |
| **Search** | PostgreSQL Full-Text Search (MVP) | Built-in, no extra service for MVP |
| **Future** | Elasticsearch | Advanced search, analytics (Phase 2+) |

### Infrastructure

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Hosting** | Vercel (frontend) + Railway (backend) | Simple deployment, auto-scaling, affordable |
| **Alternative** | AWS (EC2 + RDS + S3) | More control, scale flexibility |
| **CDN** | Cloudflare | Global edge caching, DDoS protection |
| **File Storage** | AWS S3 / Cloudflare R2 | Receipt images, user uploads |
| **Monitoring** | Sentry (errors) + PostHog (analytics) | Error tracking, user behavior insights |

### External Services

| Service | Purpose | MVP |
|---------|---------|-----|
| **OCR** | Mindee / AWS Textract | Receipt scanning | Phase 2 |
| **NLP** | OpenAI API / Claude | Voice capture parsing | Phase 2 |
| **Email** | Resend / SendGrid | Transactional emails | MVP |
| **SMS** | Twilio | Optional notifications | Phase 2 |
| **Payments** | Stripe | Future monetization | Phase 3+ |

---

## System Architecture

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                            │
├─────────────────────────────────────────────────────────────────┤
│  Web App (React + TypeScript + Tailwind)                       │
│  - Desktop browsers                                              │
│  - Mobile browsers (responsive PWA)                              │
│  - Offline support (Service Worker)                              │
└──────────────────────┬──────────────────────────────────────────┘
                       │ HTTPS + WebSocket
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                        API GATEWAY LAYER                        │
├─────────────────────────────────────────────────────────────────┤
│  Cloudflare (CDN + DDoS protection)                             │
│  - Rate limiting                                                 │
│  - SSL termination                                               │
│  - Caching                                                       │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                          │
├─────────────────────────────────────────────────────────────────┤
│  Node.js API Server (Express)                                   │
│  ├── REST API (CRUD operations)                                 │
│  ├── WebSocket Server (real-time sync)                          │
│  ├── Authentication (JWT)                                        │
│  └── Background Workers (receipt processing, emails)            │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                         DATA LAYER                              │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  PostgreSQL     │  │  Redis          │  │  S3             │ │
│  │  (Primary DB)   │  │  (Cache/Queue)  │  │  (File Storage) │ │
│  │  - Users        │  │  - Sessions     │  │  - Receipts     │ │
│  │  - Lists        │  │  - Real-time    │  │  - Images       │ │
│  │  - Items        │  │  - Bull queue   │  │                 │ │
│  │  - Receipts     │  │                 │  │                 │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Component Interactions

```
User Action: "Add milk to list"
     │
     ├──> Frontend (React)
     │     ├── Optimistic UI update (instant feedback)
     │     └── Send to backend via WebSocket
     │
     ├──> Backend (Node.js)
     │     ├── Validate request
     │     ├── Save to PostgreSQL
     │     ├── Update Redis cache
     │     └── Broadcast to all connected clients
     │
     └──> Other Users' Frontends
           └── Receive update via WebSocket → UI updates
```

---

## Data Models

### Entity Relationship Diagram

```
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│    User      │       │    List      │       │    Item      │
├──────────────┤       ├──────────────┤       ├──────────────┤
│ id           │───┐   │ id           │───┐   │ id           │
│ email        │   │   │ name         │   │   │ list_id      │───┘
│ password_hash│   │   │ budget       │   │   │ name         │
│ name         │   │   │ created_by   │───┘   │ quantity     │
│ created_at   │   │   │ created_at   │       │ store        │
└──────────────┘   │   │ updated_at   │       │ priority     │
                   │   └──────────────┘       │ price_est    │
                   │                          │ tags         │
                   │   ┌──────────────┐       │ notes        │
                   │   │ ListMember   │       │ status       │
                   │   ├──────────────┤       │ added_by     │───┐
                   └───│ user_id      │       │ created_at   │   │
                       │ list_id      │       │ updated_at   │   │
                       │ role         │       └──────────────┘   │
                       └──────────────┘                          │
                                                                 │
       ┌──────────────┐       ┌──────────────┐                  │
       │   Receipt    │       │  PriceHistory│                  │
       ├──────────────┤       ├──────────────┤                  │
       │ id           │       │ id           │                  │
       │ list_id      │       │ item_name    │                  │
       │ image_url    │       │ store        │                  │
       │ total_amount │       │ price        │                  │
       │ uploaded_by  │───────│ date         │                  │
       │ uploaded_at  │       │ user_id      │──────────────────┘
       └──────────────┘       └──────────────┘
```

### Schema Definitions

#### User Table

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  name VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

#### List Table

```sql
CREATE TABLE lists (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  budget DECIMAL(10, 2),
  created_by UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_lists_created_by ON lists(created_by);
```

#### List Member Table

```sql
CREATE TABLE list_members (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  role VARCHAR(20) CHECK (role IN ('owner', 'editor', 'viewer')),
  joined_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(list_id, user_id)
);

CREATE INDEX idx_list_members_list_id ON list_members(list_id);
CREATE INDEX idx_list_members_user_id ON list_members(user_id);
```

#### Item Table

```sql
CREATE TABLE items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  quantity DECIMAL(10, 2) DEFAULT 1,
  store VARCHAR(255),
  priority VARCHAR(20) CHECK (priority IN ('must', 'should', 'nice-to-have')) DEFAULT 'should',
  price_estimate DECIMAL(10, 2),
  tags TEXT[], -- PostgreSQL array type
  notes TEXT,
  status VARCHAR(20) CHECK (status IN ('pending', 'cart', 'purchased')) DEFAULT 'pending',
  added_by UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_items_list_id ON items(list_id);
CREATE INDEX idx_items_status ON items(status);
CREATE INDEX idx_items_store ON items(store);
```

#### Receipt Table

```sql
CREATE TABLE receipts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  image_url VARCHAR(500),
  total_amount DECIMAL(10, 2),
  store VARCHAR(255),
  uploaded_by UUID REFERENCES users(id),
  uploaded_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_receipts_list_id ON receipts(list_id);
```

#### Price History Table

```sql
CREATE TABLE price_history (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  item_name VARCHAR(255) NOT NULL,
  store VARCHAR(255),
  price DECIMAL(10, 2) NOT NULL,
  date DATE NOT NULL,
  user_id UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_price_history_item_store ON price_history(item_name, store);
```

---

## API Design

### REST API Endpoints

#### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Create new user account |
| POST | `/api/auth/login` | Login and receive JWT token |
| POST | `/api/auth/logout` | Invalidate session |
| GET | `/api/auth/me` | Get current user profile |

#### Lists

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/lists` | Get all lists for current user |
| POST | `/api/lists` | Create a new list |
| GET | `/api/lists/:id` | Get list details with items |
| PUT | `/api/lists/:id` | Update list (name, budget) |
| DELETE | `/api/lists/:id` | Delete list |
| POST | `/api/lists/:id/invite` | Invite user to list |
| DELETE | `/api/lists/:id/members/:userId` | Remove member from list |

#### Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/lists/:id/items` | Get all items for a list |
| POST | `/api/lists/:id/items` | Add item(s) to list |
| PUT | `/api/items/:id` | Update item |
| DELETE | `/api/items/:id` | Delete item |
| PATCH | `/api/items/:id/status` | Update item status |

#### Receipts

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/receipts` | Upload receipt image |
| GET | `/api/receipts/:id` | Get receipt details |
| POST | `/api/receipts/:id/process` | Trigger OCR processing |

#### Price History

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/prices/:itemName` | Get price history for item |
| POST | `/api/prices` | Manually add price data |

### WebSocket Events

#### Client → Server

| Event | Payload | Description |
|-------|---------|-------------|
| `join_list` | `{ listId }` | Subscribe to list updates |
| `leave_list` | `{ listId }` | Unsubscribe from list |
| `add_item` | `{ listId, item }` | Add item in real-time |
| `update_item` | `{ itemId, changes }` | Update item |
| `delete_item` | `{ itemId }` | Delete item |

#### Server → Client

| Event | Payload | Description |
|-------|---------|-------------|
| `item_added` | `{ item, addedBy }` | New item added to list |
| `item_updated` | `{ itemId, changes, updatedBy }` | Item updated |
| `item_deleted` | `{ itemId, deletedBy }` | Item deleted |
| `list_updated` | `{ listId, changes }` | List metadata changed |

---

## Real-Time Synchronization

### Strategy

**Optimistic Updates + WebSocket Sync**

```
User Action (Add Item):
  1. Frontend immediately adds item to UI (optimistic)
  2. Send WebSocket event to server
  3. Server validates and persists to DB
  4. Server broadcasts to all connected clients
  5. Clients reconcile (if conflict, server wins)
```

### Conflict Resolution

| Scenario | Resolution |
|----------|------------|
| **Concurrent edits** | Last-write-wins (timestamp-based) |
| **Network partition** | Queue changes, sync when reconnected |
| **Deleted item edited** | Ignore edit, show "item was deleted" message |

### Offline Support

```
Offline Mode:
  ├── Service Worker caches app shell
  ├── IndexedDB stores list data locally
  ├── User can view and edit lists offline
  ├── Changes queued in local storage
  └── On reconnect, sync queued changes to server
```

---

## Authentication & Security

### JWT Token Structure

```json
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "userId": "uuid",
    "email": "user@example.com",
    "iat": 1234567890,
    "exp": 1234654290
  }
}
```

### Security Measures

| Layer | Protection |
|-------|------------|
| **Transport** | HTTPS only, strict TLS |
| **Authentication** | JWT with 7-day expiration, secure cookies |
| **Passwords** | bcrypt hashing (12 rounds) |
| **Input Validation** | Zod schemas on frontend + backend |
| **SQL Injection** | Parameterized queries (never raw SQL) |
| **XSS** | React auto-escaping, CSP headers |
| **CSRF** | SameSite cookies, CSRF tokens |
| **Rate Limiting** | 100 requests/minute per IP |
| **DDoS** | Cloudflare protection |

---

## Third-Party Integrations

### Receipt OCR (Phase 2)

**Provider:** Mindee or AWS Textract

```javascript
// Example OCR workflow
async function processReceipt(imageUrl) {
  const ocrResult = await mindee.parse(imageUrl);

  return {
    total: ocrResult.total_amount,
    items: ocrResult.line_items.map(item => ({
      name: item.description,
      price: item.unit_price,
      quantity: item.quantity
    })),
    store: ocrResult.merchant_name,
    date: ocrResult.date
  };
}
```

### Store APIs (Phase 3)

| Store | API | Purpose |
|-------|-----|---------|
| **Instacart** | Instacart API | Pre-build carts |
| **Walmart** | Walmart Open API | Product search, prices |
| **Target** | Target API | Product search, prices |

---

## Scalability Plan

### Vertical Scaling (MVP → 1K users)

- Single server instance
- PostgreSQL on managed service (Railway/AWS RDS)
- Redis on same server or managed service

### Horizontal Scaling (1K → 100K users)

| Component | Strategy |
|-----------|----------|
| **API Servers** | Load balancer + multiple Node.js instances |
| **Database** | Read replicas for queries, primary for writes |
| **WebSocket** | Redis Pub/Sub for cross-server message broadcasting |
| **File Storage** | S3 / R2 with CDN |
| **Cache** | Redis cluster with sharding |

### Database Optimization

- Indexes on frequently queried columns
- Materialized views for analytics
- Partitioning by date (price_history table)
- Connection pooling

---

## Performance Targets

| Metric | Target |
|--------|--------|
| **API Response Time (p95)** | < 200ms |
| **WebSocket Sync Delay** | < 500ms |
| **Page Load Time (FCP)** | < 1.5s |
| **Offline Mode Availability** | 100% (with service worker) |
| **Database Query Time (p95)** | < 50ms |
| **Concurrent Users (single server)** | 1,000+ |

---

## Deployment Pipeline

```
Code Change:
  ├── Git push to main branch
  ├── GitHub Actions triggers CI/CD
  ├── Run tests (unit + integration)
  ├── Build frontend (Vite)
  ├── Build backend (TypeScript → JS)
  ├── Deploy frontend to Vercel
  ├── Deploy backend to Railway
  └── Run smoke tests on production
```

---

**Simple, scalable, secure. Built to grow from MVP to millions.**
