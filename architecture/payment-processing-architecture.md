# Payment Processing Architecture

## Service Dependencies

**Implements**: Resume-Account-API (backend payment logic)
**Depends On**: Resume-SSO (user authentication), Stripe API (payment processor)
**Used By**: Resume-Account-UI (billing dashboard), Resume-Admin-API (payment administration)

## Core Components

```
┌─────────────────────────────────────────────────────────┐
│              Payment Processing Stack                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Client → Resume-Account-API → Stripe API              │
│              │                                           │
│              ├── Webhook Processor (Durable Object)     │
│              ├── Payment Queue (async operations)       │
│              ├── Idempotency Store (KV - dedup)         │
│              ├── D1 Database (subscription data)        │
│              └── R2 Storage (invoice PDFs)              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

## Subscription Management

**Subscription Tiers**:
- Free: Limited features, no payment required
- Standard ($14.99/mo): Core features, 14-day trial
- Premium ($29.99/mo): Advanced features
- Enterprise ($49.99/mo): Custom features

**Subscription Lifecycle**:
1. **Trial Start** → 14-day free trial with Stripe Customer creation
2. **Trial Active** → User can access paid features, no charge
3. **Trial Converting** → Payment method added, subscription activated
4. **Active** → Regular billing cycle, payment collected
5. **Past Due** → Payment failed, 3-day grace period with retries
6. **Canceled** → User initiated, access until period end
7. **Unpaid** → Auto-downgrade to Free tier after grace period

**Plan Changes**:
- **Upgrade**: Immediate access, prorated charge
- **Downgrade**: Scheduled for next billing cycle
- **Cancel**: Access until period end

## Database Schema

**Core Tables** (8 total):
- `subscriptions` - Active subscriptions with billing cycles
- `payment_methods` - Stripe payment methods per user
- `invoices` - Generated invoices with line items
- `usage_records` - Usage-based billing metrics
- `billing_events` - Complete audit trail
- `webhook_events` - Stripe webhook processing log
- `idempotency_keys` - Duplicate payment prevention (KV backed)
- `payment_attempts` - Failed payment tracking

**Key Indexes**:
```sql
CREATE INDEX idx_subscriptions_user_status ON subscriptions(user_id, status);
CREATE INDEX idx_subscriptions_stripe_id ON subscriptions(stripe_subscription_id);
CREATE INDEX idx_invoices_due_date ON invoices(due_date) WHERE status = 'open';
CREATE INDEX idx_billing_events_idempotency ON billing_events(idempotency_key);
```

## Stripe Webhook Integration

**Webhook Processor** (Durable Object):
- Ensures webhook event ordering
- Prevents race conditions
- Handles idempotency
- Sub-100ms processing

**Critical Webhooks Handled**:
- `customer.subscription.created` - New subscription
- `customer.subscription.updated` - Plan change, status change
- `customer.subscription.deleted` - Cancellation
- `invoice.payment_succeeded` - Successful payment
- `invoice.payment_failed` - Failed payment (trigger retry)
- `payment_method.attached` - New payment method
- `checkout.session.completed` - Checkout success

**Webhook Processing Flow**:
```
Stripe → Webhook Processor DO
           │
           ├─ Verify signature
           ├─ Check idempotency (KV)
           ├─ Order events (timestamp)
           ├─ Update D1 database
           ├─ Enqueue follow-ups (Queue)
           └─ Return 200 OK (<100ms)
```

## Payment Security

**PCI DSS Compliance**:
- ✅ No card data stored (Stripe handles)
- ✅ Webhook signature verification
- ✅ Idempotency keys for all payments
- ✅ Complete audit logging
- ✅ Encrypted data at rest (D1, R2)
- ✅ TLS 1.3 for all connections

**Idempotency Strategy**:
```typescript
// KV-based idempotency with 24-hour TTL
const idempotencyKey = `payment:${userId}:${operationType}:${timestamp}`;
const existing = await env.KV.get(idempotencyKey);
if (existing) return JSON.parse(existing); // Return cached result

// Process payment...
await env.KV.put(idempotencyKey, JSON.stringify(result), { expirationTtl: 86400 });
```

## Failed Payment Handling

**Retry Strategy** (Exponential Backoff):
1. **Immediate**: Try primary payment method
2. **+1 hour**: Retry with same method
3. **+6 hours**: Try alternate payment method
4. **+24 hours**: Final retry, send urgent email
5. **+72 hours**: Grace period ends, downgrade to Free

**User Communications**:
- Payment failed notification (immediate)
- Retry notification (each attempt)
- Grace period warning (24 hours before)
- Downgrade notification (when actioned)

## API Endpoints (Resume-Account-API)

**Subscription Management**:
```
POST   /api/subscriptions              Create subscription
GET    /api/subscriptions/:id          Get subscription details
PATCH  /api/subscriptions/:id          Update subscription
DELETE /api/subscriptions/:id          Cancel subscription
POST   /api/subscriptions/:id/reactivate  Reactivate canceled subscription
```

**Payment Methods**:
```
POST   /api/payment-methods            Add payment method
GET    /api/payment-methods            List payment methods
PATCH  /api/payment-methods/:id        Set default
DELETE /api/payment-methods/:id        Remove payment method
```

**Invoices**:
```
GET    /api/invoices                   List user invoices
GET    /api/invoices/:id               Get invoice details
GET    /api/invoices/:id/pdf           Download invoice PDF
POST   /api/invoices/:id/pay           Retry payment
```

**Usage Tracking**:
```
POST   /api/usage                      Record usage event
GET    /api/usage/current              Get current period usage
```

## Implementation Dependencies

**Foundation** (Required First):
- D1 database with payment tables
- KV namespace for idempotency store
- R2 bucket for invoice storage
- Stripe account with Products and Prices configured

**Core Services** (Build Next):
- Webhook processor Durable Object
- Payment queue consumer
- Subscription management API
- Payment method management API

**Advanced Features** (Add Later):
- Usage-based billing
- Proration calculations
- Invoice PDF generation
- Payment analytics dashboard

