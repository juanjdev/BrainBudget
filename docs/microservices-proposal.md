# Microservices Architecture Proposal

## Service Breakdown & Specifications

### Overview
This document provides detailed specifications for each microservice, including responsibilities, APIs, data models, dependencies, and implementation details.

---

## Service #1: Auth Service

### Purpose
Handles all authentication and authorization concerns across the platform.

### Responsibilities
- User authentication (login/logout)
- Token generation and validation (JWT)
- OAuth2 integration (Google, GitHub)
- Multi-factor authentication (2FA)
- Session management
- Password reset flows
- API key management

### Technology Stack
- **Runtime**: Node.js with NestJS or Go
- **Database**: PostgreSQL (users, sessions)
- **Cache**: Redis (token blacklist, rate limiting)
- **Security**: bcrypt/argon2, jsonwebtoken

### API Endpoints

```typescript
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
POST   /api/auth/refresh
POST   /api/auth/forgot-password
POST   /api/auth/reset-password
POST   /api/auth/verify-email
POST   /api/auth/resend-verification
POST   /api/auth/mfa/enable
POST   /api/auth/mfa/disable
POST   /api/auth/mfa/verify
GET    /api/auth/oauth/:provider
GET    /api/auth/oauth/:provider/callback
POST   /api/auth/validate-token (Internal)
```

### Data Models
```typescript
interface User {
  id: string;
  email: string;
  passwordHash: string;
  emailVerified: boolean;
  mfaEnabled: boolean;
  mfaSecret?: string;
  createdAt: Date;
  updatedAt: Date;
}

interface Session {
  id: string;
  userId: string;
  tokenHash: string;
  refreshTokenHash: string;
  ipAddress: string;
  userAgent: string;
  expiresAt: Date;
  createdAt: Date;
}
```

### Events Published
- `user.registered`
- `user.login`
- `user.logout`
- `user.password_reset`
- `user.email_verified`

### Dependencies
- Email Service (for verification emails)
- Notification Service (for security alerts)

### Security Considerations
- Rate limiting on login attempts
- Token rotation on refresh
- Secure password hashing (Argon2)
- HTTP-only, secure cookies
- CSRF protection
- Account lockout after failed attempts

### Deployment
- **Scaling**: Horizontal (stateless)
- **Resources**: 0.5 CPU, 512MB RAM per instance
- **Replicas**: Minimum 2

---

## Service #2: User Service

### Purpose
Manages user profiles, preferences, and account settings.

### Responsibilities
- User profile CRUD operations
- Avatar management
- User preferences
- Account settings
- User search and discovery
- Profile completeness tracking

### Technology Stack
- **Runtime**: Node.js with NestJS or Python with FastAPI
- **Database**: PostgreSQL
- **Cache**: Redis (profile cache)
- **Storage**: S3 (avatars)

### API Endpoints

```typescript
GET    /api/users/me
PUT    /api/users/me
DELETE /api/users/me
PATCH  /api/users/me/avatar
GET    /api/users/:id
GET    /api/users/search?q=
GET    /api/users/me/preferences
PUT    /api/users/me/preferences
GET    /api/users/me/settings
PUT    /api/users/me/settings
```

### Data Models
```typescript
interface UserProfile {
  id: string;
  userId: string;
  firstName: string;
  lastName: string;
  avatarUrl?: string;
  bio?: string;
  timezone: string;
  language: string;
  createdAt: Date;
  updatedAt: Date;
}

interface UserPreferences {
  userId: string;
  currency: string;
  dateFormat: string;
  darkMode: boolean;
  notificationSettings: object;
}
```

### Events Published
- `user.profile_updated`
- `user.preferences_updated`
- `user.avatar_changed`
- `user.deleted`

### Events Consumed
- `user.registered` (create profile)

### Dependencies
- Auth Service (for user authentication)
- Storage Service (for avatar uploads)

### Caching Strategy
- Cache user profiles for 15 minutes
- Invalidate on update
- Cache preferences for 1 hour

---

## Service #3: Expense Service

### Purpose
Core service for managing expense records and transactions.

### Responsibilities
- Expense CRUD operations
- Search and filtering
- Soft delete and recovery
- Expense history tracking
- Recurring expense management
- Bulk operations
- Data export

### Technology Stack
- **Runtime**: Node.js with NestJS or Go with Gin
- **Database**: PostgreSQL (primary), ElasticSearch (search)
- **Cache**: Redis (frequent queries)
- **Queue**: RabbitMQ/BullMQ (async processing)

### API Endpoints

```typescript
POST   /api/expenses
GET    /api/expenses
GET    /api/expenses/:id
PUT    /api/expenses/:id
DELETE /api/expenses/:id
PATCH  /api/expenses/:id/restore
GET    /api/expenses/search?q=
POST   /api/expenses/bulk
DELETE /api/expenses/bulk
GET    /api/expenses/export?format=csv|pdf|excel
POST   /api/expenses/:id/duplicate
GET    /api/expenses/stats
```

### Data Models
```typescript
interface Expense {
  id: string;
  userId: string;
  householdId?: string;
  categoryId: string;
  paymentMethodId?: string;
  amount: number;
  currency: string;
  amountInBaseCurrency: number;
  merchantName?: string;
  description?: string;
  transactionDate: Date;
  location?: string;
  isRecurring: boolean;
  recurringTemplateId?: string;
  status: 'pending' | 'completed' | 'cancelled';
  metadata: object;
  createdAt: Date;
  updatedAt: Date;
  deletedAt?: Date;
}
```

### Events Published
- `expense.created`
- `expense.updated`
- `expense.deleted`
- `expense.restored`
- `expense.bulk_imported`

### Events Consumed
- `receipt.processed` (update with OCR data)
- `bank_transaction.matched` (link to transaction)
- `ai.categorized` (update category)

### Business Logic
- Automatic currency conversion
- Transaction date validation
- Duplicate detection
- Budget impact calculation

### Performance Optimization
- Indexed queries on user_id, date, category
- Pagination with cursor-based approach
- ElasticSearch for full-text search
- Background jobs for bulk operations

---

## Service #4: Category Service

### Purpose
Manages expense categories, subcategories, and category-related operations.

### Responsibilities
- Category CRUD operations
- System and custom categories
- Category hierarchy
- Category budgets
- Category analytics
- Icon and color management

### Technology Stack
- **Runtime**: Node.js with Express or Go
- **Database**: PostgreSQL
- **Cache**: Redis (category tree)

### API Endpoints

```typescript
GET    /api/categories
POST   /api/categories
GET    /api/categories/:id
PUT    /api/categories/:id
DELETE /api/categories/:id
GET    /api/categories/tree
GET    /api/categories/:id/expenses
GET    /api/categories/:id/stats
POST   /api/categories/:id/subcategories
```

### Data Models
```typescript
interface Category {
  id: string;
  userId?: string;
  householdId?: string;
  name: string;
  icon: string;
  color: string;
  parentCategoryId?: string;
  isSystem: boolean;
  isIncome: boolean;
  sortOrder: number;
  createdAt: Date;
  updatedAt: Date;
}
```

### Events Published
- `category.created`
- `category.updated`
- `category.deleted`

### Caching Strategy
- Cache entire category tree per user
- Invalidate on any category change
- TTL: 1 hour

---

## Service #5: Storage Service

### Purpose
Handles all file storage operations, particularly receipt images.

### Responsibilities
- File upload to S3/R2
- Image optimization and compression
- Signed URL generation
- File deletion
- Storage quota management
- Thumbnail generation

### Technology Stack
- **Runtime**: Node.js or Go
- **Storage**: AWS S3 or Cloudflare R2
- **Image Processing**: Sharp.js or ImageMagick
- **Queue**: BullMQ (processing jobs)

### API Endpoints

```typescript
POST   /api/storage/upload
GET    /api/storage/files/:id
DELETE /api/storage/files/:id
GET    /api/storage/signed-url/:key
POST   /api/storage/optimize
GET    /api/storage/quota
```

### Data Models
```typescript
interface StoredFile {
  id: string;
  userId: string;
  fileKey: string;
  fileUrl: string;
  fileName: string;
  fileSize: number;
  mimeType: string;
  metadata: object;
  createdAt: Date;
}
```

### Events Published
- `file.uploaded`
- `file.deleted`
- `file.optimized`

### Processing Pipeline
1. Receive upload request
2. Validate file type and size
3. Generate unique filename
4. Upload to S3
5. Create thumbnails (if image)
6. Optimize and compress
7. Store metadata
8. Return URLs

### Security
- Signed URLs with expiration
- File type validation
- Virus scanning (optional)
- Access control checks

---

## Service #6: Analytics Service

### Purpose
Provides data aggregation, analytics, and insights generation.

### Responsibilities
- Expense aggregations
- Trend analysis
- Budget tracking calculations
- Custom report data
- Time-series analytics
- Comparative analysis

### Technology Stack
- **Runtime**: Python with FastAPI or Node.js
- **Database**: PostgreSQL (read replicas)
- **Cache**: Redis (computed analytics)
- **Processing**: Pandas (Python) or data processing libraries

### API Endpoints

```typescript
GET    /api/analytics/overview
GET    /api/analytics/spending-by-category
GET    /api/analytics/trends?period=
GET    /api/analytics/top-merchants
GET    /api/analytics/budget-performance
GET    /api/analytics/spending-patterns
GET    /api/analytics/compare?period1=&period2=
POST   /api/analytics/custom-query
```

### Data Models
```typescript
interface AnalyticsQuery {
  userId: string;
  dateRange: { start: Date; end: Date };
  groupBy: 'category' | 'merchant' | 'day' | 'week' | 'month';
  filters: object;
  aggregations: string[];
}

interface AnalyticsResult {
  data: any[];
  summary: {
    totalExpenses: number;
    totalAmount: number;
    avgAmount: number;
  };
  period: { start: Date; end: Date };
  generatedAt: Date;
}
```

### Events Consumed
- `expense.created` (update aggregates)
- `expense.updated` (recalculate)
- `expense.deleted` (adjust totals)

### Performance Optimization
- Materialized views for common queries
- Pre-computed aggregates
- Incremental updates
- Caching with smart invalidation

---

## Service #7: Report Service

### Purpose
Generates formatted reports in various formats (PDF, CSV, Excel).

### Responsibilities
- PDF report generation
- CSV export
- Excel file creation
- Report templates
- Scheduled reports
- Email delivery

### Technology Stack
- **Runtime**: Node.js
- **PDF**: Puppeteer or PDFKit
- **Excel**: ExcelJS
- **Queue**: BullMQ (async generation)
- **Storage**: S3 (generated reports)

### API Endpoints

```typescript
POST   /api/reports/generate
GET    /api/reports/:id
GET    /api/reports/:id/download
DELETE /api/reports/:id
POST   /api/reports/schedule
GET    /api/reports/scheduled
DELETE /api/reports/scheduled/:id
GET    /api/reports/templates
```

### Data Models
```typescript
interface Report {
  id: string;
  userId: string;
  type: 'expense_summary' | 'budget_report' | 'tax_report' | 'custom';
  format: 'pdf' | 'csv' | 'excel';
  parameters: object;
  status: 'pending' | 'generating' | 'completed' | 'failed';
  fileUrl?: string;
  createdAt: Date;
  completedAt?: Date;
}

interface ScheduledReport {
  id: string;
  userId: string;
  reportType: string;
  frequency: 'daily' | 'weekly' | 'monthly';
  parameters: object;
  recipients: string[];
  nextRun: Date;
  isActive: boolean;
}
```

### Events Published
- `report.generated`
- `report.failed`
- `report.scheduled`

### Processing
- Queue-based generation
- Template rendering
- Data formatting
- File compression
- Storage upload

---

## Service #8: AI Service

### Purpose
Provides AI-powered features including categorization, insights, and chatbot.

### Responsibilities
- Expense categorization
- Merchant recognition
- Anomaly detection
- Spending pattern analysis
- Budget recommendations
- Natural language processing
- Chatbot responses

### Technology Stack
- **Runtime**: Python with FastAPI
- **ML**: scikit-learn, TensorFlow/PyTorch
- **NLP**: OpenAI API, Transformers
- **Database**: PostgreSQL (training data)
- **Cache**: Redis (model predictions)

### API Endpoints

```typescript
POST   /api/ai/categorize
POST   /api/ai/analyze-spending
GET    /api/ai/insights
POST   /api/ai/chat
POST   /api/ai/detect-anomalies
GET    /api/ai/recommendations
POST   /api/ai/train (Admin only)
```

### Data Models
```typescript
interface CategorizationRequest {
  merchantName: string;
  amount: number;
  description?: string;
}

interface CategorizationResult {
  suggestedCategoryId: string;
  confidence: number;
  reasoning: string;
  alternativeCategories: Array<{
    categoryId: string;
    confidence: number;
  }>;
}

interface Insight {
  type: 'pattern' | 'anomaly' | 'recommendation' | 'trend';
  title: string;
  description: string;
  severity: 'low' | 'medium' | 'high';
  data: object;
  actionable: boolean;
}
```

### Events Published
- `ai.categorized`
- `ai.insight_generated`
- `ai.anomaly_detected`

### Events Consumed
- `expense.created` (categorize new expense)
- `user.preferences_updated` (retrain model)

### ML Pipeline
1. Data collection
2. Feature engineering
3. Model training
4. Model evaluation
5. Deployment
6. Monitoring & retraining

### Models
- **Categorization**: Random Forest or Neural Network
- **Anomaly Detection**: Isolation Forest
- **NLP**: Fine-tuned GPT model or similar

---

## Service #9: OCR Service

### Purpose
Processes receipt images and extracts structured data.

### Responsibilities
- Image preprocessing
- Text extraction
- Data parsing (amount, date, merchant, items)
- Confidence scoring
- Error handling and retry

### Technology Stack
- **Runtime**: Python with FastAPI
- **OCR**: Google Cloud Vision API or Tesseract
- **Image Processing**: OpenCV, PIL
- **Queue**: BullMQ (processing jobs)

### API Endpoints

```typescript
POST   /api/ocr/process
GET    /api/ocr/results/:id
POST   /api/ocr/retry/:id
GET    /api/ocr/status/:id
```

### Data Models
```typescript
interface OCRJob {
  id: string;
  expenseId: string;
  receiptId: string;
  imageUrl: string;
  status: 'pending' | 'processing' | 'completed' | 'failed';
  result?: OCRResult;
  error?: string;
  attempts: number;
  createdAt: Date;
  completedAt?: Date;
}

interface OCRResult {
  rawText: string;
  extractedData: {
    amount?: number;
    date?: string;
    merchant?: string;
    items?: Array<{
      name: string;
      quantity: number;
      price: number;
    }>;
  };
  confidence: number;
}
```

### Events Published
- `receipt.processing_started`
- `receipt.processed`
- `receipt.processing_failed`

### Events Consumed
- `file.uploaded` (start OCR)

### Processing Pipeline
1. Receive image URL
2. Download image
3. Preprocess (rotate, crop, enhance)
4. Extract text
5. Parse structured data
6. Validate extracted data
7. Calculate confidence score
8. Store results
9. Publish event

---

## Service #10: Budget Service

### Purpose
Manages budgets, tracks spending against budgets, and generates alerts.

### Responsibilities
- Budget CRUD operations
- Budget tracking and calculations
- Alert threshold management
- Budget recommendations
- Rollover budget handling

### Technology Stack
- **Runtime**: Node.js with NestJS
- **Database**: PostgreSQL
- **Cache**: Redis (budget calculations)
- **Queue**: BullMQ (alert jobs)

### API Endpoints

```typescript
POST   /api/budgets
GET    /api/budgets
GET    /api/budgets/:id
PUT    /api/budgets/:id
DELETE /api/budgets/:id
GET    /api/budgets/:id/progress
GET    /api/budgets/:id/forecast
POST   /api/budgets/:id/alerts
```

### Data Models
```typescript
interface Budget {
  id: string;
  userId: string;
  householdId?: string;
  categoryId?: string;
  name: string;
  amount: number;
  currency: string;
  periodType: 'weekly' | 'monthly' | 'quarterly' | 'yearly';
  startDate: Date;
  endDate?: Date;
  rollover: boolean;
  alertThresholds: {
    warning: number;
    critical: number;
  };
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
}

interface BudgetProgress {
  budgetId: string;
  spent: number;
  remaining: number;
  percentageUsed: number;
  daysRemaining: number;
  averageDailySpending: number;
  projectedTotal: number;
  status: 'on_track' | 'warning' | 'exceeded';
}
```

### Events Published
- `budget.created`
- `budget.updated`
- `budget.threshold_reached`
- `budget.exceeded`

### Events Consumed
- `expense.created` (update budget tracking)
- `expense.updated` (recalculate)
- `expense.deleted` (adjust budget)

### Business Logic
- Real-time budget calculations
- Threshold monitoring
- Forecasting based on trends
- Rollover calculations

---

## Service #11: Notification Service

### Purpose
Centralized notification delivery across multiple channels.

### Responsibilities
- Email notifications
- Push notifications
- SMS alerts (optional)
- In-app notifications
- Notification preferences
- Delivery tracking
- Retry logic

### Technology Stack
- **Runtime**: Node.js
- **Email**: Resend or SendGrid
- **Push**: Firebase Cloud Messaging
- **SMS**: Twilio (optional)
- **Queue**: BullMQ (delivery jobs)
- **Database**: PostgreSQL (notification history)

### API Endpoints

```typescript
POST   /api/notifications/send
GET    /api/notifications
GET    /api/notifications/:id
PATCH  /api/notifications/:id/read
DELETE /api/notifications/:id
GET    /api/notifications/preferences
PUT    /api/notifications/preferences
POST   /api/notifications/test
```

### Data Models
```typescript
interface Notification {
  id: string;
  userId: string;
  type: string;
  channel: 'email' | 'push' | 'sms' | 'in_app';
  title: string;
  message: string;
  data?: object;
  status: 'pending' | 'sent' | 'failed';
  isRead: boolean;
  sentAt?: Date;
  readAt?: Date;
  createdAt: Date;
}

interface NotificationPreferences {
  userId: string;
  emailEnabled: boolean;
  pushEnabled: boolean;
  smsEnabled: boolean;
  preferences: {
    budgetAlerts: boolean;
    expenseAdded: boolean;
    splitRequests: boolean;
    insights: boolean;
    marketing: boolean;
  };
}
```

### Events Consumed
- All service events (for notification triggers)

### Notification Types
- Budget alerts
- Expense confirmation
- Split requests
- Receipt processed
- AI insights
- Security alerts
- Marketing (opt-in)

### Delivery Pipeline
1. Receive notification request
2. Check user preferences
3. Format message per channel
4. Queue for delivery
5. Send via appropriate service
6. Track delivery status
7. Retry on failure (max 3 attempts)
8. Store notification history

---

## Service #12: Sharing Service

### Purpose
Manages household accounts, expense sharing, and collaboration features.

### Responsibilities
- Household CRUD operations
- Member management
- Expense sharing
- Split calculations
- Permission management
- Activity feed

### Technology Stack
- **Runtime**: Node.js with NestJS
- **Database**: PostgreSQL
- **Cache**: Redis (household data)
- **Real-time**: WebSocket (activity feed)

### API Endpoints

```typescript
POST   /api/households
GET    /api/households
GET    /api/households/:id
PUT    /api/households/:id
DELETE /api/households/:id
POST   /api/households/:id/members
DELETE /api/households/:id/members/:userId
PATCH  /api/households/:id/members/:userId/role
GET    /api/households/:id/expenses
GET    /api/households/:id/activity
POST   /api/expenses/:id/split
GET    /api/splits/pending
PUT    /api/splits/:id/accept
PUT    /api/splits/:id/reject
```

### Data Models
```typescript
interface Household {
  id: string;
  name: string;
  currency: string;
  ownerId: string;
  createdAt: Date;
  updatedAt: Date;
}

interface HouseholdMember {
  householdId: string;
  userId: string;
  role: 'owner' | 'admin' | 'member' | 'viewer';
  joinedAt: Date;
}

interface ExpenseSplit {
  id: string;
  expenseId: string;
  userId: string;
  amount: number;
  percentage: number;
  status: 'pending' | 'accepted' | 'paid' | 'rejected';
  createdAt: Date;
}
```

### Events Published
- `household.created`
- `household.member_added`
- `household.member_removed`
- `split.created`
- `split.accepted`
- `split.paid`

### Permission System
- **Owner**: Full control
- **Admin**: Manage members, expenses
- **Member**: Add/edit own expenses, view all
- **Viewer**: Read-only access

---

## Service #13: Integration Service

### Purpose
Handles third-party integrations (banks, payment providers, accounting software).

### Responsibilities
- Bank account linking (Plaid)
- Transaction syncing
- Payment provider webhooks
- Calendar integration
- Accounting software export
- API key management

### Technology Stack
- **Runtime**: Node.js with NestJS
- **Database**: PostgreSQL
- **Queue**: BullMQ (sync jobs)
- **Scheduler**: Node-cron (periodic sync)

### API Endpoints

```typescript
POST   /api/integrations/connect
GET    /api/integrations
DELETE /api/integrations/:id
POST   /api/integrations/:id/sync
GET    /api/integrations/:id/status
POST   /api/integrations/webhooks/:provider
GET    /api/integrations/banks/accounts
GET    /api/integrations/banks/transactions
POST   /api/integrations/banks/match
```

### Data Models
```typescript
interface Integration {
  id: string;
  userId: string;
  providerType: 'plaid' | 'stripe' | 'quickbooks';
  providerId: string;
  credentials: object; // encrypted
  settings: object;
  status: 'active' | 'inactive' | 'error';
  lastSyncAt?: Date;
  createdAt: Date;
}

interface BankTransaction {
  id: string;
  bankAccountId: string;
  transactionId: string;
  amount: number;
  description: string;
  merchantName: string;
  transactionDate: Date;
  isMatched: boolean;
  matchedExpenseId?: string;
}
```

### Events Published
- `integration.connected`
- `integration.sync_started`
- `integration.sync_completed`
- `bank_transaction.imported`
- `bank_transaction.matched`

### Sync Strategy
- Initial sync: Last 90 days
- Incremental sync: Daily at midnight
- Manual sync: On-demand
- Webhook updates: Real-time

---

## Inter-Service Communication

### Communication Patterns

#### Synchronous (HTTP/REST)
- Auth validation
- User profile retrieval
- Category lookup
- Direct queries

#### Asynchronous (Events)
- Expense created
- Budget alerts
- Notifications
- Analytics updates

#### Message Queue
- File processing
- Report generation
- Bulk operations
- Email delivery

### Event Schema

```typescript
interface Event {
  id: string;
  type: string;
  source: string;
  timestamp: Date;
  data: object;
  metadata: {
    userId?: string;
    traceId: string;
    version: string;
  };
}
```

---

## Cross-Cutting Concerns

### Logging
- Structured JSON logging
- Correlation IDs
- Log levels (debug, info, warn, error)
- Centralized log aggregation

### Monitoring
- Health checks (`/health`)
- Metrics endpoint (`/metrics`)
- Custom business metrics
- Performance tracking

### Security
- JWT validation middleware
- Rate limiting per service
- Input validation
- SQL injection prevention
- XSS protection

### Error Handling
- Consistent error format
- Error codes
- Retry logic
- Circuit breakers
- Graceful degradation

---

## Deployment Strategy

### Container Configuration
```yaml
# Example docker-compose.yml structure
services:
  auth-service:
    image: expense-tracker/auth:latest
    environment:
      - DATABASE_URL
      - REDIS_URL
      - JWT_SECRET
    resources:
      limits:
        cpus: '0.5'
        memory: 512M
    replicas: 2
```

### Service Mesh (Optional)
- Istio or Linkerd for advanced routing
- Traffic splitting for canary deployments
- Automatic retries and timeouts
- Service-to-service encryption

---

This microservices architecture provides a scalable, maintainable foundation for the expense tracker while showcasing enterprise-level software engineering practices.
