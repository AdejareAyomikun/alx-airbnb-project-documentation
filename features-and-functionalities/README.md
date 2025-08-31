# Airbnb Clone – Backend Features & Functionalities

This document enumerates the features and backend capabilities required for a scalable, secure, and robust Airbnb-style application. It aligns with three groups of requirements: **Core Functionalities**, **Technical Requirements**, and **Non-Functional Requirements**.

---

## 🔑 Core Functionalities

### 1) User Management
- **Registration (Guest/Host)** with email verification
- **Login / Logout** (JWT-based sessions)
- **OAuth** (Google, Facebook) – optional but recommended
- **Profile Management**: name, photo, phone, preferences
- **RBAC** roles: `guest`, `host`, `admin`

### 2) Property Listings Management (Hosts)
- **Create / Update / Delete** listings
- Fields: title, description, location, price, amenities, images
- **Availability management** (date ranges / calendar)
- **Host dashboard**: list & filter own properties

### 3) Search & Filtering
- Search by **location, price range, guests, amenities**
- **Pagination** for large datasets
- Optional: sort by price, rating, recency

### 4) Booking Management
- **Create booking** for date range; validate availability (prevent double bookings)
- **Booking status**: `pending`, `confirmed`, `canceled`, `completed`
- **Cancel bookings** (policy-driven)
- **View booking history** (guest & host perspectives)

### 5) Payments
- Integrate **Stripe/PayPal** for secure payments
- **Upfront payments** by guests
- **Payouts to hosts** after completion
- **Multi-currency** support
- Handle **refunds** and **payment status** updates

### 6) Reviews & Ratings
- Guests **review** properties after completed booking
- **Rating (1–5)** and comment
- Hosts can **respond** to reviews
- Prevent abuse: reviews must be tied to a **completed booking**

### 7) Notifications
- **Email + in-app** notifications for:
  - booking confirmations / cancellations
  - payment updates / refunds
- Templates + delivery logs

### 8) Admin Dashboard
- Manage **users, listings, bookings, payments**
- **Moderate** reviews, handle disputes/refunds
- **Reports & analytics** (bookings, revenue, active users)

---

## 🛠️ Technical Requirements

### Database (PostgreSQL/MySQL)
Required tables: **Users, Properties, Bookings, Reviews, Payments** (+ Messages/Notifications if used).  
Normalization: up to **3NF**; selective denormalization where justified (e.g., `Booking.total_price` for pricing history).

### API Development
- **RESTful** endpoints with proper HTTP methods & status codes
- Error payloads with standardized structure
- Optional **GraphQL** for complex aggregations/joins

### Authentication & Authorization
- **JWT** access & refresh tokens
- **RBAC** gatekeeping for routes (guest/host/admin)
- Security controls: password hashing, token blacklist/rotation

### File Storage (Scenario-based)
- Store images (properties, profile photos) via **file storage for implementation**
- Cloud-ready (S3/Cloudinary) adapters for production

### Third-Party Services
- **Email** (SendGrid/Mailgun)
- **Payments** (Stripe/PayPal)
- Optional **SMS**/push notifications

### Error Handling & Logging
- Global API error middleware
- Structured logging (request/response, correlation IDs)
- Audit trails for critical actions

---

## 🚀 Non-Functional Requirements

### Scalability
- Modular service design
- Horizontal scaling (stateless API + load balancer)
- DB read replicas when needed

### Security
- Encrypt sensitive data at rest & in transit (TLS)
- Rate limiting, input validation, WAF
- Secrets management; OWASP best practices

### Performance
- **Caching** (Redis) for hot reads (search results, lookups)
- N+1 query avoidance; indexes on FK/search fields
- Background jobs for webhooks/notifications

### Testing
- **Unit & integration tests** (e.g., pytest/Jest)
- Automated API tests (happy/edge/negative paths)
- CI coverage reports

---

## 👥 Roles & Permissions (RBAC)

| Capability                              | Guest | Host | Admin |
|-----------------------------------------|:-----:|:----:|:-----:|
| Register / Login                        |  ✅   |  ✅  |  ✅   |
| Manage own profile                      |  ✅   |  ✅  |  ✅   |
| Create / manage properties              |       |  ✅  |  ✅   |
| Search & view properties                |  ✅   |  ✅  |  ✅   |
| Create bookings                         |  ✅   |      |  ✅   |
| Cancel own bookings                     |  ✅   |      |  ✅   |
| Accept/decline bookings (own property)  |       |  ✅  |  ✅   |
| Payments for bookings                   |  ✅   |      |  ✅   |
| Payouts                                 |       |  ✅  |  ✅   |
| Reviews on completed bookings           |  ✅   |      |  ✅   |
| Respond to reviews (on own property)    |       |  ✅  |  ✅   |
| Manage users/listings/bookings/payments |       |      |  ✅   |

---

## ✅ Acceptance Criteria (per module)

- **Auth**: JWT login/refresh; protected routes reject invalid tokens; role guards enforced.
- **Properties**: CRUD with validation; only host/admin can mutate; images stored; indexed queries.
- **Search**: Filters & pagination; response times acceptable with dataset ≥ 10k rows (with indexes/caching).
- **Bookings**: No overlaps; status lifecycle enforced; audit logs; cancellation policy enforced.
- **Payments**: Charge + webhook handling; idempotency keys; refunds supported; secure vault for secrets.
- **Reviews**: Only by guests with completed bookings; one review per booking; host reply optional.
- **Notifications**: Emails sent on booking/payment events; persisted delivery status; retries for failures.
- **Admin**: Can view/moderate all; export basic reports.

---

## 🗺️ System Diagram

See **`airbnb-backend-features.png`** (exported from Draw.io). It depicts:
- API Gateway / Backend API
- Services: Auth/User, Property, Booking, Payment, Review, Notification, Admin
- Database (core tables), Cache (Redis)
- File storage, Email provider, Payment gateway
- Observability & logging

---

## 📄 Files in this folder

- `README.md` – this document  
- `airbnb-backend-features.drawio` – source diagram (editable)  
- `airbnb-backend-features.png` – exported PNG diagram

---

## 🚢 How to update the diagram

1. Open `airbnb-backend-features.drawio` in https://app.diagrams.net/  
2. Edit modules/arrows as needed.  
3. Export → **PNG** (keep the filename above).  
4. Commit both `.drawio` and `.png`.

