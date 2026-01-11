# ARCHITECTURE (IMPLEMENTATION SPECIFICATIONS)

**🔍 ZOOM: PRODUCT_MAP → Technical Implementation Details**

This document contains detailed technical specifications for implementing the Shopping List Aggregator system.

## Table of Contents
1. Tech Stack
2. Database Schema
3. API Contracts
4. Authentication & Authorization
5. State Management
6. Offline Strategy
7. Data Flow
8. Error Handling
9. Performance Considerations
10. Security

---

## 1. Tech Stack

### Frontend
- **Framework**: React 18+ with TypeScript
- **Meta-Framework**: Next.js 14 (App Router)
- **State Management**: Zustand (lightweight, simpler than Redux for this use case)
- **Styling**: Tailwind CSS + shadcn/ui components
- **Forms**: React Hook Form + Zod validation
- **Offline**: Workbox (service worker library)
- **HTTP Client**: TanStack Query (React Query) for server state

### Backend
- **Runtime**: Node.js 20 LTS
- **Framework**: Express.js
- **Language**: TypeScript
- **Validation**: Zod (shared with frontend)
- **Authentication**: Lucia Auth (session-based)
- **Database ORM**: Drizzle ORM

### Database
- **Primary**: PostgreSQL 16
- **Caching**: Redis 7 (session store, rate limiting)
- **File Storage**: AWS S3 (receipt images)

### Infrastructure
- **Hosting**: Vercel (frontend) + Railway (backend + DB)
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry (errors) + PostHog (analytics)

### Development
- **Package Manager**: pnpm
- **Monorepo**: Turborepo (apps/web, apps/api, packages/shared)
- **Linting**: ESLint + Prettier
- **Testing**: Vitest (unit) + Playwright (e2e)

---

## 2. Database Schema

### Complete PostgreSQL Schema

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Users table (authentication)
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email VARCHAR(255) UNIQUE NOT NULL,
  email_verified BOOLEAN DEFAULT FALSE,
  hashed_password TEXT NOT NULL,
  name VARCHAR(255),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);

-- Sessions table (Lucia Auth)
CREATE TABLE sessions (
  id TEXT PRIMARY KEY,
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  expires_at TIMESTAMPTZ NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_sessions_user_id ON sessions(user_id);

-- Items table (core entity)
CREATE TABLE items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  notes TEXT,
  is_deleted BOOLEAN DEFAULT FALSE, -- Soft delete
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  deleted_at TIMESTAMPTZ
);

CREATE INDEX idx_items_user_id ON items(user_id);
CREATE INDEX idx_items_deleted ON items(user_id, is_deleted) WHERE is_deleted = FALSE;

-- Tag types enum
CREATE TYPE tag_type AS ENUM ('store', 'urgency', 'budget', 'person', 'category');

-- Tags table
CREATE TABLE tags (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  item_id UUID NOT NULL REFERENCES items(id) ON DELETE CASCADE,
  type tag_type NOT NULL,
  value VARCHAR(255) NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_tags_item_id ON tags(item_id);
CREATE INDEX idx_tags_type ON tags(type);
CREATE INDEX idx_tags_value ON tags(type, value); -- For filtering by tag
CREATE UNIQUE INDEX idx_tags_unique_item_type_value ON tags(item_id, type, value);

-- Stores table (reference data)
CREATE TABLE stores (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(255) NOT NULL,
  type VARCHAR(50), -- 'grocery', 'pharmacy', 'hardware', etc.
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_stores_name ON stores(name);

-- Budgets table
CREATE TABLE budgets (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  amount DECIMAL(10, 2) NOT NULL,
  period VARCHAR(50) NOT NULL, -- 'trip', 'week', 'month'
  start_date DATE,
  end_date DATE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_budgets_user_id ON budgets(user_id);

-- Trips table
CREATE TABLE trips (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  store_id UUID REFERENCES stores(id),
  status VARCHAR(50) DEFAULT 'draft', -- 'draft', 'active', 'completed'
  estimated_total DECIMAL(10, 2),
  actual_total DECIMAL(10, 2),
  started_at TIMESTAMPTZ,
  completed_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_trips_user_id ON trips(user_id);
CREATE INDEX idx_trips_status ON trips(user_id, status);

-- Trip items (many-to-many: trips <-> items)
CREATE TABLE trip_items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  item_id UUID NOT NULL REFERENCES items(id) ON DELETE CASCADE,
  status VARCHAR(50) DEFAULT 'pending', -- 'pending', 'confirmed', 'skipped'
  sort_order INTEGER, -- For custom ordering in shop mode
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(trip_id, item_id)
);

CREATE INDEX idx_trip_items_trip_id ON trip_items(trip_id);
CREATE INDEX idx_trip_items_item_id ON trip_items(item_id);

-- Receipts table
CREATE TABLE receipts (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  trip_id UUID NOT NULL REFERENCES trips(id) ON DELETE CASCADE,
  image_url TEXT, -- S3 URL
  total_amount DECIMAL(10, 2),
  uploaded_at TIMESTAMPTZ DEFAULT NOW(),
  processed BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_receipts_trip_id ON receipts(trip_id);

-- Receipt items (extracted from OCR or manual entry)
CREATE TABLE receipt_items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  receipt_id UUID NOT NULL REFERENCES receipts(id) ON DELETE CASCADE,
  item_id UUID REFERENCES items(id) ON DELETE SET NULL, -- Matched item
  line_text TEXT, -- Raw OCR text
  price DECIMAL(10, 2),
  quantity INTEGER DEFAULT 1,
  matched BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_receipt_items_receipt_id ON receipt_items(receipt_id);

-- Price memory (store-specific pricing)
CREATE TABLE price_memory (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  item_id UUID NOT NULL REFERENCES items(id) ON DELETE CASCADE,
  store_id UUID REFERENCES stores(id) ON DELETE CASCADE,
  last_price DECIMAL(10, 2) NOT NULL,
  last_seen TIMESTAMPTZ NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(item_id, store_id)
);

CREATE INDEX idx_price_memory_item_id ON price_memory(item_id);
CREATE INDEX idx_price_memory_store_id ON price_memory(store_id);

-- Groups table (Phase 2)
CREATE TABLE groups (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(255) NOT NULL,
  created_by UUID NOT NULL REFERENCES users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Group members (Phase 2)
CREATE TABLE group_members (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  group_id UUID NOT NULL REFERENCES groups(id) ON DELETE CASCADE,
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  role VARCHAR(50) DEFAULT 'member', -- 'admin', 'member'
  joined_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(group_id, user_id)
);

CREATE INDEX idx_group_members_group_id ON group_members(group_id);
CREATE INDEX idx_group_members_user_id ON group_members(user_id);

-- Triggers for updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_items_updated_at BEFORE UPDATE ON items FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_budgets_updated_at BEFORE UPDATE ON budgets FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_trips_updated_at BEFORE UPDATE ON trips FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_trip_items_updated_at BEFORE UPDATE ON trip_items FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_price_memory_updated_at BEFORE UPDATE ON price_memory FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

---

## 3. API Contracts

### REST API Endpoints

#### Authentication
```typescript
// POST /api/auth/signup
Request: { email: string, password: string, name?: string }
Response: { user: User, sessionId: string }

// POST /api/auth/login
Request: { email: string, password: string }
Response: { user: User, sessionId: string }

// POST /api/auth/logout
Request: {}
Response: { success: boolean }

// GET /api/auth/me
Response: { user: User | null }
```

#### Items
```typescript
// GET /api/items
Query: { includeDeleted?: boolean }
Response: { items: Item[] }

// POST /api/items
Request: {
  name: string,
  notes?: string,
  tags?: { type: TagType, value: string }[]
}
Response: { item: Item }

// PATCH /api/items/:id
Request: { name?: string, notes?: string }
Response: { item: Item }

// DELETE /api/items/:id (soft delete)
Response: { success: boolean }

// POST /api/items/:id/restore
Response: { item: Item }
```

#### Tags
```typescript
// POST /api/items/:itemId/tags
Request: { type: TagType, value: string }
Response: { tag: Tag }

// DELETE /api/tags/:id
Response: { success: boolean }

// GET /api/tags/values?type=store
Response: { values: string[] } // Unique values for dropdown
```

#### Trips
```typescript
// GET /api/trips
Query: { status?: 'draft' | 'active' | 'completed' }
Response: { trips: Trip[] }

// POST /api/trips
Request: { storeId: string, itemIds: string[] }
Response: { trip: Trip }

// PATCH /api/trips/:id
Request: { status?: string, actualTotal?: number }
Response: { trip: Trip }

// PATCH /api/trips/:tripId/items/:itemId
Request: { status: 'pending' | 'confirmed' | 'skipped' }
Response: { tripItem: TripItem }
```

#### Receipts
```typescript
// POST /api/receipts
Request: FormData { tripId: string, image: File }
Response: { receipt: Receipt, uploadUrl: string }

// GET /api/receipts/:id
Response: { receipt: Receipt, items: ReceiptItem[] }

// POST /api/receipts/:id/match
Request: { receiptItemId: string, itemId: string }
Response: { matched: boolean, priceMemoryUpdated: boolean }
```

#### Price Memory
```typescript
// GET /api/price-memory?itemId=xxx
Response: { prices: PriceMemory[] } // All stores

// GET /api/price-memory?itemId=xxx&storeId=yyy
Response: { price: PriceMemory | null } // Specific store
```

#### Budgets
```typescript
// GET /api/budgets
Response: { budgets: Budget[] }

// POST /api/budgets
Request: { amount: number, period: 'trip' | 'week' | 'month', startDate?: string }
Response: { budget: Budget }

// PATCH /api/budgets/:id
Request: { amount?: number }
Response: { budget: Budget }
```

---

## 4. Authentication & Authorization

### Strategy: Session-Based Auth with Lucia

**Why Sessions Over JWT**:
- Better security (revocable)
- Simpler implementation
- Server-side control

**Implementation**:
```typescript
// libs/auth.ts
import { Lucia } from "lucia";
import { DrizzlePostgreSQLAdapter } from "@lucia-auth/adapter-drizzle";

const adapter = new DrizzlePostgreSQLAdapter(db, sessions, users);

export const lucia = new Lucia(adapter, {
  sessionCookie: {
    expires: false, // Session cookies
    attributes: {
      secure: process.env.NODE_ENV === "production"
    }
  },
  getUserAttributes: (attributes) => {
    return {
      email: attributes.email,
      name: attributes.name
    };
  }
});
```

**Authorization Rules**:
- Users can only access their own items, trips, receipts
- Middleware checks `userId` from session matches resource ownership
- Groups (Phase 2): Role-based access (admin can delete, members can view/edit)

**Password Hashing**:
```typescript
import { Argon2id } from "oslo/password";

const hashedPassword = await new Argon2id().hash(password);
const valid = await new Argon2id().verify(hashedPassword, password);
```

---

## 5. State Management

### Zustand Store Structure

```typescript
// stores/itemStore.ts
interface ItemStore {
  items: Item[];
  loading: boolean;
  error: string | null;

  // Actions
  fetchItems: () => Promise<void>;
  addItem: (item: CreateItemInput) => Promise<Item>;
  updateItem: (id: string, updates: Partial<Item>) => Promise<Item>;
  deleteItem: (id: string) => Promise<void>;

  // Derived state
  activeItems: () => Item[];
  itemsByTag: (type: TagType, value: string) => Item[];
}

// stores/tripStore.ts
interface TripStore {
  currentTrip: Trip | null;
  trips: Trip[];

  startTrip: (storeId: string, itemIds: string[]) => Promise<Trip>;
  completeTrip: (tripId: string) => Promise<void>;
  updateTripItem: (tripId: string, itemId: string, status: TripItemStatus) => Promise<void>;
}

// stores/offlineStore.ts
interface OfflineStore {
  isOnline: boolean;
  syncQueue: SyncAction[];

  addToSyncQueue: (action: SyncAction) => void;
  processSyncQueue: () => Promise<void>;
}
```

### Server State with TanStack Query

```typescript
// hooks/useItems.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

export function useItems() {
  return useQuery({
    queryKey: ['items'],
    queryFn: fetchItems,
    staleTime: 5 * 60 * 1000 // 5 minutes
  });
}

export function useAddItem() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: addItem,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['items'] });
    }
  });
}
```

---

## 6. Offline Strategy

### Service Worker with Workbox

**Cache Strategy**:
- **App Shell**: Cache-first (HTML, CSS, JS)
- **API Data**: Network-first with fallback to cache
- **Images**: Cache-first with expiration

```typescript
// service-worker.ts
import { precacheAndRoute } from 'workbox-precaching';
import { registerRoute } from 'workbox-routing';
import { NetworkFirst, CacheFirst } from 'workbox-strategies';
import { ExpirationPlugin } from 'workbox-expiration';

// Precache app shell
precacheAndRoute(self.__WB_MANIFEST);

// API routes: Network-first
registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new NetworkFirst({
    cacheName: 'api-cache',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 50,
        maxAgeSeconds: 5 * 60 // 5 minutes
      })
    ]
  })
);

// Images: Cache-first
registerRoute(
  ({ request }) => request.destination === 'image',
  new CacheFirst({
    cacheName: 'image-cache',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 100,
        maxAgeSeconds: 30 * 24 * 60 * 60 // 30 days
      })
    ]
  })
);
```

### Offline Sync Queue

```typescript
interface SyncAction {
  id: string;
  type: 'CREATE_ITEM' | 'UPDATE_ITEM' | 'DELETE_ITEM' | 'UPDATE_TRIP_ITEM';
  payload: any;
  timestamp: number;
}

// When offline, queue mutations
function queueOfflineMutation(action: SyncAction) {
  const queue = getOfflineQueue();
  queue.push(action);
  localStorage.setItem('offline-queue', JSON.stringify(queue));
}

// When online, process queue
async function processSyncQueue() {
  const queue = getOfflineQueue();

  for (const action of queue) {
    try {
      await executeAction(action);
      removeFromQueue(action.id);
    } catch (error) {
      console.error('Sync failed:', action, error);
      // Keep in queue for retry
    }
  }
}
```

### Conflict Resolution

**Strategy**: Last-Write-Wins (LWW)
- Use `updated_at` timestamp to determine latest version
- Server always wins on conflict
- Show user notification if their offline changes were overwritten
- Optional: Merge UI for manual conflict resolution (Phase 2)

---

## 7. Data Flow

### Item Capture Flow
```
User types item name
  → Frontend validates (min 2 chars)
  → POST /api/items { name, notes }
  → Backend creates item + default tags
  → Response returns item with ID
  → Frontend updates Zustand store
  → TanStack Query invalidates cache
  → UI shows new item in list
```

### Trip Generation Flow
```
User selects store
  → Frontend filters items by store tag
  → User selects items for trip
  → POST /api/trips { storeId, itemIds }
  → Backend creates trip + trip_items
  → Calculates estimated_total from price_memory
  → Returns trip object
  → Frontend navigates to /app/trips/:tripId
```

### Receipt Matching Flow
```
User uploads receipt image
  → POST /api/receipts (FormData)
  → Backend uploads to S3
  → (Phase 1) Manual matching UI
  → (Phase 2) OCR extracts line items
  → User confirms matches
  → POST /api/receipts/:id/match { receiptItemId, itemId }
  → Backend updates price_memory(item_id, store_id, price)
  → Returns updated price_memory
```

---

## 8. Error Handling

### Frontend Error Boundaries

```typescript
// components/ErrorBoundary.tsx
export class ErrorBoundary extends React.Component {
  state = { hasError: false, error: null };

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, info) {
    console.error('Error caught:', error, info);
    Sentry.captureException(error);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback error={this.state.error} />;
    }
    return this.props.children;
  }
}
```

### API Error Responses

```typescript
// Standard error format
{
  error: {
    code: 'VALIDATION_ERROR' | 'NOT_FOUND' | 'UNAUTHORIZED' | 'INTERNAL_ERROR',
    message: string,
    details?: any
  }
}

// Example error handling middleware
app.use((err, req, res, next) => {
  console.error(err);
  Sentry.captureException(err);

  if (err instanceof ValidationError) {
    return res.status(400).json({
      error: {
        code: 'VALIDATION_ERROR',
        message: err.message,
        details: err.errors
      }
    });
  }

  res.status(500).json({
    error: {
      code: 'INTERNAL_ERROR',
      message: 'Something went wrong'
    }
  });
});
```

### Retry Logic

```typescript
// TanStack Query retry configuration
export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: (failureCount, error) => {
        // Don't retry on 4xx errors
        if (error.response?.status >= 400 && error.response?.status < 500) {
          return false;
        }
        // Retry up to 3 times on 5xx or network errors
        return failureCount < 3;
      },
      retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000)
    }
  }
});
```

---

## 9. Performance Considerations

### Database Optimization
- **Indexes**: Created on all foreign keys and frequently queried columns
- **N+1 Queries**: Use JOIN or batch loading (Dataloader pattern)
- **Pagination**: Cursor-based for large lists (items, trips)
- **Caching**: Redis for frequently accessed data (stores, tag values)

### Frontend Optimization
- **Code Splitting**: Route-based with Next.js
- **Image Optimization**: Next.js Image component with WebP
- **Lazy Loading**: React.lazy for heavy components (AID modal)
- **Virtualization**: react-window for long lists (1000+ items)

### API Rate Limiting
```typescript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  standardHeaders: true,
  legacyHeaders: false,
  store: new RedisStore({ client: redis })
});

app.use('/api/', limiter);
```

---

## 10. Security

### Input Validation (Zod)
```typescript
// shared/schemas.ts
import { z } from 'zod';

export const CreateItemSchema = z.object({
  name: z.string().min(2).max(255),
  notes: z.string().max(1000).optional(),
  tags: z.array(z.object({
    type: z.enum(['store', 'urgency', 'budget', 'person', 'category']),
    value: z.string().min(1).max(255)
  })).optional()
});

// API route
app.post('/api/items', async (req, res) => {
  const result = CreateItemSchema.safeParse(req.body);
  if (!result.success) {
    return res.status(400).json({ error: result.error });
  }
  // ... create item
});
```

### SQL Injection Prevention
- **Use Drizzle ORM**: Parameterized queries by default
- **Never interpolate user input**: Use placeholders

### XSS Prevention
- **React escapes by default**: No dangerouslySetInnerHTML without sanitization
- **Content Security Policy**: Set CSP headers

### CSRF Protection
- **SameSite cookies**: `sameSite: 'lax'` on session cookies
- **CORS**: Whitelist frontend domain only

### File Upload Security
```typescript
// Receipt upload validation
const MAX_FILE_SIZE = 10 * 1024 * 1024; // 10MB
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/webp'];

function validateUpload(file: File) {
  if (file.size > MAX_FILE_SIZE) {
    throw new Error('File too large');
  }
  if (!ALLOWED_TYPES.includes(file.type)) {
    throw new Error('Invalid file type');
  }
}
```

---

## Next Steps

1. Review EXECUTION_PLAN.md for phasing details
2. Set up development environment (see README)
3. Run database migrations
4. Implement Phase 0 scaffolding
5. Begin Phase 1 MVP development
