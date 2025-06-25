# Backend Feature Requirement Specifications

This document outlines the functional and technical requirements for three core backend features in the Airbnb Clone project: **User Authentication**, **Property Management**, and the **Booking System**.

---

## 1. User Authentication

### API Endpoints
- `POST /api/register` – Register a new user
- `POST /api/login` – Authenticate a user
- `GET /api/profile` – Retrieve current user profile
- `PUT /api/profile` – Update user profile

### 📥 Input/Output
**Registration Input:**
- name: string
- email: string (unique)
- password: string (min 8 characters)
- role: string (guest or host)

**Login Input:**
- email: string
- password: string

**Output:**
- Success: User info + JWT token
- Failure: Error message (e.g., invalid credentials)

### Validation Rules
- All fields are required.
- Email must be valid and unique.
- Password must meet minimum length and complexity requirements.
- Role must be either `guest` or `host`.

### ⚙️ Performance Criteria
- Login/registration should complete within 1 second.
- JWT should expire in 24 hours.
- Rate limit login attempts to max 5 per minute/IP.

---

## 2. Property Management

### API Endpoints
- `POST /api/properties` – Create a new listing (Host only)
- `GET /api/properties` – List all properties
- `GET /api/properties/:id` – Retrieve single property
- `PUT /api/properties/:id` – Update listing (Host only)
- `DELETE /api/properties/:id` – Delete listing (Host only)

### 📥Input / Output
**Create/Edit Input:**
- title: string
- description: string
- location: string
- price: number
- amenities: array of strings
- available_dates: array of dates

**Output:**
- Success: Confirmation + property ID
- Failure: Validation error or unauthorized access

### Validation Rules
- Price must be a positive number.
- Title, location, and availability are mandatory.
- User must be authenticated and authorized as host.
- No duplicate listings (same host + title + location).

### ⚙️ Performance Criteria
- Listings must be searchable in under 2 seconds.
- Pagination support for queries with >50 records.
- Support image uploads (limit: 5 images per listing, max 2MB each).

---

## 3. Booking System

### API Endpoints
- `POST /api/bookings` – Create a booking
- `GET /api/bookings` – List user’s bookings
- `DELETE /api/bookings/:id` – Cancel booking
- `GET /api/bookings/:id` – View booking details

### 📥 Input / Output
**Booking Input:**
- property_id: string
- start_date: date
- end_date: date

**Output:**
- Success: Booking confirmation + ID + status
- Failure: Date unavailable or policy violation

### Validation Rules
- Start and end dates must not overlap existing bookings.
- User cannot book their own listing.
- Date format must follow ISO 8601 (YYYY-MM-DD).
- Start date must be before end date.

### ⚙️ Performance Criteria
- Booking confirmation within 3 seconds.
- Prevent double-booking using database-level constraints.
- Support high concurrency during peak demand (e.g., 100+ bookings per minute).

---

## 🔒 Security & Compliance (Applies to All Features)
- All endpoints require secure HTTPS communication.
- Sensitive fields (passwords, tokens) must never be exposed.
- Data access controlled via JWT + role-based access.
- All errors returned in consistent JSON format.

---

## 📌 Future Feature Integration (for planning)
- Payments (Stripe/PayPal)
- Review & Rating System
- Notifications (email )
- Admin Dashboard
