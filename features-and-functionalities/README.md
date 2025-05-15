# Airbnb Clone Backend: Features & Functionalities

This document outlines the **key features and functionalities** of the Airbnb Clone backend system, aligned with the implemented ERD and normalized schema.

---

## 🧑‍💼 1. User Management

### 1.1 User Roles
- **Guest**: Can book properties, write reviews, send messages.
- **Host**: Can create/manage property listings and respond to reviews.
- **Admin**: Can manage users, listings, and platform-wide content.

### 1.2 Features
- **User Registration**
  - Sign up with email and password.
  - Assign role: `guest`, `host`, or `admin`.
  - Store password securely as a hash.
- **Authentication**
  - Login using email/password with JWT tokens.
  - OAuth2 support (Google/Facebook) for convenience.
- **Profile Management**
  - Update personal details (name, email, phone, profile photo).
  - Change password and role-specific preferences.

---

## 🏠 2. Property Management

### 2.1 Listing Management (For Hosts)
- Create new property with:
  - `name`, `description`, `location`, `pricepernight`
  - Amenities (future scope), availability
- Edit and delete listings
- Each property is linked to a `USER` (host)

---

## 🔍 3. Search & Filtering

### 3.1 Filterable Attributes
- **Location**
- **Price range**
- **Number of guests**
- **Amenities** (future scope: Wi-Fi, pets, pool, etc.)

### 3.2 Features
- Full-text search and filtering on `PROPERTY`
- Pagination support for scalability

---

## 📅 4. Booking System

### 4.1 Booking Creation
- Guests book a property by selecting:
  - `start_date`, `end_date`
- System:
  - Prevents overlapping bookings
  - Stores `price_at_booking` and calculates `total_price`

### 4.2 Booking Cancellation
- Guests or hosts can cancel
- Update `status` (pending, confirmed, canceled)

### 4.3 Booking Tracking
- Booking records are tied to:
  - `PROPERTY`, `USER`, `PAYMENT`, and `REVIEW`

---

## 💳 5. Payment Integration

### 5.1 Features
- Secure payment processing via Stripe/PayPal
- Snapshot `amount` taken from `BOOKING.total_price`
- Supports multi-currency (future scope)

### 5.2 Payouts
- Hosts receive payouts after completed bookings (manual or automatic)

---

## 🌟 6. Reviews & Ratings

### 6.1 Review Flow
- Guests submit `rating` (1-5) and `comment` post-booking
- Linked to:
  - `USER`, `PROPERTY`, and `BOOKING`
- Hosts can optionally respond (future enhancement)

### 6.2 Moderation
- Admin interface to monitor/report abuse

---

## 🔔 7. Notification System

### 7.1 Channels
- Email (via SendGrid or Mailgun)
- In-app notifications (message table or pub/sub system)

### 7.2 Events Triggering Notifications
- Booking confirmation
- Booking cancellation
- Payment success/failure

---

## 🛠️ 8. Admin Dashboard

### 8.1 Admin Capabilities
- View & manage:
  - All users (guests, hosts)
  - Property listings
  - All bookings
  - Payments and reviews
- Ban/unban users (future scope)
- Generate platform reports

---

## ⚙️ 9. Technical Architecture Overview

| Layer         | Description |
|---------------|-------------|
| **Database**  | MySQL with normalized schema (3NF), including `USER`, `PROPERTY`, `BOOKING`, `PAYMENT`, `REVIEW`, `MESSAGE` tables. |
| **API**       | RESTful API endpoints for each entity with proper HTTP status codes. |
| **Auth**      | JWT-based sessions, with role-based access control (RBAC). |
| **File Storage** | Local or cloud-based (AWS S3/Cloudinary) for images. |
| **Testing**   | Unit + integration tests (e.g., Pytest or Postman for API tests). |
| **Logging**   | Global error handling and logging middleware. |
| **Security**  | Encrypted data, rate-limiting, and secure credentials handling. |
| **Performance** | Caching with Redis and optimized SQL queries. |

