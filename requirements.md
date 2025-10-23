# Technical Requirements Specification - Airbnb Clone Backend

This document provides detailed technical and functional requirements for key backend features of the Airbnb Clone application.

---

## Table of Contents

1. [User Authentication](#1-user-authentication)
2. [Property Management](#2-property-management)
3. [Booking System](#3-booking-system)
4. [Payment Processing](#4-payment-processing)

---

## 1. User Authentication

### 1.1 Overview

The User Authentication system handles user registration, login, profile management, and authorization using JWT (JSON Web Tokens) for stateless authentication.

### 1.2 Functional Requirements

#### FR-1.1: User Registration
- Users must be able to register as either Guests or Hosts
- Registration requires: first name, last name, email, phone, password
- Email verification must be sent upon registration
- Users cannot register with an existing email

#### FR-1.2: User Login
- Users must be able to log in using email and password
- Upon successful login, system generates JWT access token and refresh token
- Failed login attempts are tracked (rate limiting after 5 failed attempts)

#### FR-1.3: Password Management
- Users can request password reset via email
- Password reset tokens expire after 1 hour
- Users must be able to change password when logged in

#### FR-1.4: Profile Management
- Users can view and update profile information
- Users can upload profile photos (max 5MB, formats: JPG, PNG)
- Profile updates are validated before saving

### 1.3 API Endpoints

#### POST /api/v1/auth/register
Register a new user account.

**Request Body:**
```json
{
  "firstName": "string (required, 2-50 chars)",
  "lastName": "string (required, 2-50 chars)",
  "email": "string (required, valid email format)",
  "password": "string (required, min 8 chars, 1 uppercase, 1 lowercase, 1 number)",
  "phone": "string (optional, E.164 format)",
  "role": "string (optional, enum: ['guest', 'host'], default: 'guest')"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Registration successful. Please check your email for verification.",
  "data": {
    "userId": "uuid",
    "email": "string",
    "firstName": "string",
    "lastName": "string"
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid input data
- 409 Conflict: Email already exists
- 500 Internal Server Error: Server error

---

#### POST /api/v1/auth/login
Authenticate user and generate tokens.

**Request Body:**
```json
{
  "email": "string (required)",
  "password": "string (required)"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "userId": "uuid",
      "email": "string",
      "firstName": "string",
      "lastName": "string",
      "role": "string"
    },
    "accessToken": "string (JWT, expires in 15min)",
    "refreshToken": "string (JWT, expires in 7 days)"
  }
}
```

**Error Responses:**
- 400 Bad Request: Missing credentials
- 401 Unauthorized: Invalid credentials
- 429 Too Many Requests: Too many failed attempts
- 500 Internal Server Error

---

#### GET /api/v1/auth/profile
Get current user's profile (requires authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "userId": "uuid",
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "phone": "string",
    "profilePhoto": "string (URL)",
    "role": "string",
    "createdAt": "ISO 8601 timestamp",
    "isVerified": "boolean"
  }
}
```

**Error Responses:**
- 401 Unauthorized: Invalid or missing token
- 404 Not Found: User not found

---

#### PUT /api/v1/auth/profile
Update user profile (requires authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Request Body:**
```json
{
  "firstName": "string (optional, 2-50 chars)",
  "lastName": "string (optional, 2-50 chars)",
  "phone": "string (optional, E.164 format)",
  "bio": "string (optional, max 500 chars)"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "userId": "uuid",
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "bio": "string"
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid input
- 401 Unauthorized: Not authenticated

---

### 1.4 Validation Rules

| Field | Rules |
|-------|-------|
| firstName | Required, 2-50 characters, alphabetic |
| lastName | Required, 2-50 characters, alphabetic |
| email | Required, valid email format, unique |
| password | Required, min 8 chars, 1 uppercase, 1 lowercase, 1 number, 1 special char |
| phone | Optional, E.164 format (e.g., +1234567890) |
| bio | Optional, max 500 characters |

### 1.5 Security Requirements

- Passwords must be hashed using bcrypt with salt rounds = 10
- JWT tokens must be signed with HS256 or RS256 algorithm
- Access tokens expire after 15 minutes
- Refresh tokens expire after 7 days
- Implement rate limiting: max 5 login attempts per email per 15 minutes
- Store refresh tokens in HTTP-only cookies or secure database
- Validate JWT on all protected routes

### 1.6 Performance Criteria

- Registration endpoint: Response time < 500ms (95th percentile)
- Login endpoint: Response time < 300ms (95th percentile)
- Profile retrieval: Response time < 200ms (95th percentile)
- Support 1000 concurrent authentication requests

---

## 2. Property Management

### 2.1 Overview

The Property Management system allows hosts to create, update, and manage property listings with details, photos, pricing, and availability.

### 2.2 Functional Requirements

#### FR-2.1: Create Property Listing
- Hosts can create new property listings
- Minimum required fields: title, description, location, price, max guests
- Hosts can upload multiple property photos (min 3, max 20)
- Properties are set to "active" by default

#### FR-2.2: Update Property
- Hosts can update any property details
- Hosts can add/remove photos
- Hosts can activate/deactivate listings

#### FR-2.3: Search Properties
- Guests can search properties by location, dates, guests, price range
- Search results are paginated (20 per page)
- Results can be sorted by price, rating, distance

#### FR-2.4: View Property Details
- Anyone can view property details
- Display includes photos, amenities, reviews, availability

### 2.3 API Endpoints

#### POST /api/v1/properties
Create a new property listing (requires host authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
Content-Type: application/json
```

**Request Body:**
```json
{
  "title": "string (required, 10-100 chars)",
  "description": "string (required, 50-2000 chars)",
  "propertyType": "string (required, enum: ['apartment', 'house', 'villa', 'condo', 'other'])",
  "location": {
    "addressLine1": "string (required)",
    "addressLine2": "string (optional)",
    "city": "string (required)",
    "state": "string (optional)",
    "country": "string (required, ISO 3166-1 alpha-2)",
    "postalCode": "string (optional)",
    "latitude": "number (required, -90 to 90)",
    "longitude": "number (required, -180 to 180)"
  },
  "pricing": {
    "pricePerNight": "number (required, > 0)",
    "cleaningFee": "number (optional, >= 0, default: 0)",
    "currency": "string (optional, ISO 4217, default: 'USD')"
  },
  "capacity": {
    "maxGuests": "integer (required, 1-50)",
    "bedrooms": "integer (required, >= 1)",
    "beds": "integer (required, >= 1)",
    "bathrooms": "number (required, >= 0.5)"
  },
  "amenities": ["string (array of amenity IDs)"],
  "houseRules": "string (optional, max 1000 chars)",
  "cancellationPolicy": "string (optional, enum: ['flexible', 'moderate', 'strict'], default: 'moderate')"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Property created successfully",
  "data": {
    "propertyId": "uuid",
    "title": "string",
    "status": "active",
    "createdAt": "ISO 8601 timestamp"
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid input
- 401 Unauthorized: Not authenticated as host
- 500 Internal Server Error

---

#### GET /api/v1/properties/search
Search for properties.

**Query Parameters:**
```
location: string (required, city or country)
checkIn: date (required, ISO 8601 format, YYYY-MM-DD)
checkOut: date (required, ISO 8601 format)
guests: integer (required, min 1)
minPrice: number (optional, >= 0)
maxPrice: number (optional, >= minPrice)
propertyType: string (optional, enum)
amenities: string (optional, comma-separated amenity IDs)
sortBy: string (optional, enum: ['price_asc', 'price_desc', 'rating', 'distance'], default: 'rating')
page: integer (optional, min 1, default: 1)
limit: integer (optional, 1-50, default: 20)
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "properties": [
      {
        "propertyId": "uuid",
        "title": "string",
        "propertyType": "string",
        "location": {
          "city": "string",
          "country": "string",
          "latitude": "number",
          "longitude": "number"
        },
        "pricing": {
          "pricePerNight": "number",
          "currency": "string"
        },
        "capacity": {
          "maxGuests": "integer",
          "bedrooms": "integer",
          "bathrooms": "number"
        },
        "images": ["string (URLs)"],
        "rating": "number (1-5, or null)",
        "reviewCount": "integer"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "totalResults": "integer",
      "totalPages": "integer"
    }
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid query parameters

---

#### GET /api/v1/properties/:propertyId
Get detailed information about a specific property.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "propertyId": "uuid",
    "title": "string",
    "description": "string",
    "propertyType": "string",
    "location": { /* full location object */ },
    "pricing": { /* full pricing object */ },
    "capacity": { /* full capacity object */ },
    "amenities": [
      {
        "amenityId": "uuid",
        "name": "string",
        "icon": "string"
      }
    ],
    "images": [
      {
        "imageId": "uuid",
        "url": "string",
        "caption": "string",
        "isPrimary": "boolean"
      }
    ],
    "host": {
      "hostId": "uuid",
      "firstName": "string",
      "profilePhoto": "string",
      "memberSince": "date",
      "responseRate": "number (0-100)",
      "rating": "number (1-5)"
    },
    "rating": "number (1-5, average)",
    "reviewCount": "integer",
    "reviews": [ /* array of recent reviews */ ],
    "houseRules": "string",
    "cancellationPolicy": "string",
    "availability": { /* calendar data */ }
  }
}
```

**Error Responses:**
- 404 Not Found: Property does not exist
- 500 Internal Server Error

---

### 2.4 Validation Rules

| Field | Rules |
|-------|-------|
| title | Required, 10-100 characters |
| description | Required, 50-2000 characters |
| pricePerNight | Required, numeric, > 0 |
| maxGuests | Required, integer, 1-50 |
| latitude | Required, -90 to 90 |
| longitude | Required, -180 to 180 |
| checkIn/checkOut | Required, ISO 8601 date, checkOut > checkIn, not in past |

### 2.5 Performance Criteria

- Property creation: Response time < 1s
- Search endpoint: Response time < 500ms (with database indexing on location, price, dates)
- Property details: Response time < 300ms
- Support 5000+ concurrent search requests

---

## 3. Booking System

### 3.1 Overview

The Booking System handles reservation creation, validation, status management, and cancellations.

### 3.2 Functional Requirements

#### FR-3.1: Create Booking
- Guests can book available properties
- System validates property availability before confirming
- Booking cannot exceed property's max guest capacity
- Prevents double-booking of same property for overlapping dates

#### FR-3.2: View Bookings
- Guests can view their booking history
- Hosts can view bookings for their properties
- Bookings display status: pending, confirmed, cancelled, completed

#### FR-3.3: Cancel Booking
- Guests and hosts can cancel bookings
- Refund calculated based on cancellation policy
- Host notified of guest cancellation, vice versa

### 3.3 API Endpoints

#### POST /api/v1/bookings
Create a new booking (requires guest authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Request Body:**
```json
{
  "propertyId": "uuid (required)",
  "checkIn": "date (required, ISO 8601, YYYY-MM-DD)",
  "checkOut": "date (required, ISO 8601)",
  "guests": "integer (required, >= 1)",
  "specialRequests": "string (optional, max 500 chars)"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "uuid",
    "propertyId": "uuid",
    "checkIn": "date",
    "checkOut": "date",
    "guests": "integer",
    "pricing": {
      "pricePerNight": "number",
      "nights": "integer",
      "subtotal": "number",
      "cleaningFee": "number",
      "serviceFee": "number",
      "taxes": "number",
      "total": "number",
      "currency": "string"
    },
    "status": "confirmed",
    "paymentStatus": "paid",
    "createdAt": "ISO 8601 timestamp"
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid input or validation error
- 401 Unauthorized: Not authenticated
- 404 Not Found: Property does not exist
- 409 Conflict: Property not available for selected dates
- 422 Unprocessable Entity: Guest count exceeds max capacity
- 500 Internal Server Error

---

#### GET /api/v1/bookings
Get user's bookings (requires authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Query Parameters:**
```
status: string (optional, enum: ['pending', 'confirmed', 'cancelled', 'completed'])
page: integer (optional, default: 1)
limit: integer (optional, default: 10)
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "bookings": [
      {
        "bookingId": "uuid",
        "property": {
          "propertyId": "uuid",
          "title": "string",
          "image": "string (URL)"
        },
        "checkIn": "date",
        "checkOut": "date",
        "guests": "integer",
        "total": "number",
        "status": "string",
        "createdAt": "ISO 8601 timestamp"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "totalResults": "integer",
      "totalPages": "integer"
    }
  }
}
```

---

#### DELETE /api/v1/bookings/:bookingId
Cancel a booking (requires authentication).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Request Body:**
```json
{
  "reason": "string (optional, max 500 chars)"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Booking cancelled successfully",
  "data": {
    "bookingId": "uuid",
    "status": "cancelled",
    "refund": {
      "amount": "number",
      "currency": "string",
      "processingTime": "string (e.g., '5-10 business days')"
    }
  }
}
```

**Error Responses:**
- 401 Unauthorized: Not authenticated
- 403 Forbidden: User not authorized to cancel this booking
- 404 Not Found: Booking does not exist
- 422 Unprocessable Entity: Booking cannot be cancelled (e.g., already completed)

---

### 3.4 Validation Rules

| Field | Rules |
|-------|-------|
| propertyId | Required, valid UUID, property must exist and be active |
| checkIn | Required, ISO 8601 date, must be >= today |
| checkOut | Required, ISO 8601 date, must be > checkIn |
| guests | Required, integer, >= 1, <= property.maxGuests |
| specialRequests | Optional, max 500 characters |

### 3.5 Business Logic

**Booking Validation:**
1. Verify property exists and is active
2. Verify check-in date is not in the past
3. Verify check-out > check-in (minimum 1 night)
4. Verify guest count <= property max capacity
5. Verify property is available (no conflicting bookings)
6. Calculate total price based on nightly rate, cleaning fees, service fees (typically 10-15%), and taxes (location-dependent)

**Cancellation Policies:**
- **Flexible:** Full refund if cancelled 24+ hours before check-in
- **Moderate:** Full refund if cancelled 5+ days before check-in, 50% refund if 2-5 days before
- **Strict:** 50% refund if cancelled 7+ days before check-in, no refund within 7 days

### 3.6 Performance Criteria

- Booking creation: Response time < 1s (includes payment processing)
- Availability check: < 200ms
- Booking retrieval: < 300ms
- Support 500+ concurrent booking requests

---

## 4. Payment Processing

### 4.1 Overview

The Payment Processing system integrates with third-party payment gateways (Stripe or PayPal) to handle secure payment transactions, refunds, and host payouts.

### 4.2 Functional Requirements

#### FR-4.1: Process Payment
- Capture payment from guest when booking is confirmed
- Store payment transaction details
- Handle payment failures and retries

#### FR-4.2: Process Refunds
- Calculate refund amount based on cancellation policy
- Initiate refund to guest's original payment method
- Update booking and payment status

#### FR-4.3: Host Payouts
- Transfer funds to host after guest check-in (typically 24 hours after)
- Support multiple payout methods (bank transfer, PayPal)
- Track payout status and history

### 4.3 API Endpoints

#### POST /api/v1/payments
Process a payment for a booking (internal, triggered during booking creation).

**Request Body:**
```json
{
  "bookingId": "uuid (required)",
  "amount": "number (required, > 0)",
  "currency": "string (required, ISO 4217)",
  "paymentMethod": {
    "type": "string (required, enum: ['card', 'paypal'])",
    "cardNumber": "string (required if type=card, encrypted)",
    "expiryMonth": "integer (required if type=card, 1-12)",
    "expiryYear": "integer (required if type=card, YYYY)",
    "cvv": "string (required if type=card, 3-4 digits)",
    "paypalEmail": "string (required if type=paypal)"
  }
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Payment processed successfully",
  "data": {
    "paymentId": "uuid",
    "bookingId": "uuid",
    "amount": "number",
    "currency": "string",
    "status": "paid",
    "transactionId": "string (external gateway ID)",
    "paidAt": "ISO 8601 timestamp"
  }
}
```

**Error Responses:**
- 400 Bad Request: Invalid payment details
- 402 Payment Required: Payment declined by gateway
- 404 Not Found: Booking not found
- 500 Internal Server Error

---

#### POST /api/v1/payments/:paymentId/refund
Process a refund for a cancelled booking (internal or admin).

**Request Body:**
```json
{
  "amount": "number (optional, defaults to full refund)",
  "reason": "string (optional)"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Refund processed successfully",
  "data": {
    "refundId": "uuid",
    "paymentId": "uuid",
    "amount": "number",
    "currency": "string",
    "status": "refunded",
    "refundedAt": "ISO 8601 timestamp"
  }
}
```

---

#### GET /api/v1/payments/:paymentId
Get payment details (requires authentication, user must be guest or host of booking).

**Headers:**
```
Authorization: Bearer <accessToken>
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "paymentId": "uuid",
    "bookingId": "uuid",
    "amount": "number",
    "currency": "string",
    "method": "string",
    "status": "string (paid, refunded, failed)",
    "transactionId": "string",
    "paidAt": "ISO 8601 timestamp",
    "refundedAt": "ISO 8601 timestamp (if applicable)"
  }
}
```

**Error Responses:**
- 401 Unauthorized: Not authenticated
- 403 Forbidden: Not authorized to view this payment
- 404 Not Found: Payment not found

---

### 4.4 Security Requirements

- **PCI DSS Compliance:** Use payment gateway's tokenization; never store raw card details
- **Encryption:** All payment data transmitted over HTTPS (TLS 1.2+)
- **Validation:** Validate card numbers using Luhn algorithm before sending to gateway
- **Idempotency:** Implement idempotency keys to prevent duplicate charges
- **Webhooks:** Handle payment gateway webhooks for async status updates
- **Audit Logging:** Log all payment transactions for compliance and dispute resolution

### 4.5 Integration Requirements

**Stripe Integration:**
- Use Stripe API v2023-10 or later
- Implement Stripe Elements for secure card input (frontend)
- Use Payment Intents API for 3D Secure (SCA) support
- Handle webhook events: `payment_intent.succeeded`, `payment_intent.failed`, `refund.created`

**PayPal Integration:**
- Use PayPal REST API v2
- Implement OAuth 2.0 for authentication
- Support Express Checkout flow
- Handle IPN (Instant Payment Notification) for transaction status

### 4.6 Validation Rules

| Field | Rules |
|-------|-------|
| amount | Required, numeric, > 0, max 2 decimal places |
| currency | Required, ISO 4217 code (e.g., USD, EUR) |
| cardNumber | Required (if card), 13-19 digits, valid Luhn checksum |
| expiryMonth | Required (if card), 1-12 |
| expiryYear | Required (if card), YYYY, >= current year |
| cvv | Required (if card), 3-4 digits |

### 4.7 Performance Criteria

- Payment processing: Response time < 3s (depends on gateway)
- Payment retrieval: Response time < 200ms
- Refund processing: Response time < 2s
- Webhook processing: < 500ms (to acknowledge receipt)
- Support 100+ concurrent payment requests

---

## General System Requirements

### Technology Stack (Recommended)

- **Runtime:** Node.js 18+ or Python 3.10+
- **Framework:** Express.js (Node) or FastAPI (Python)
- **Database:** PostgreSQL 14+ (primary), Redis (caching)
- **Authentication:** JWT (jsonwebtoken library)
- **Password Hashing:** bcrypt
- **File Storage:** AWS S3 or Cloudinary (for property images)
- **Payment Gateway:** Stripe API
- **Email Service:** SendGrid or AWS SES

### Cross-Cutting Concerns

#### Error Handling
- All endpoints return standardized error format:
```json
{
  "success": false,
  "error": {
    "code": "string (e.g., VALIDATION_ERROR, NOT_FOUND)",
    "message": "string (human-readable)",
    "details": [ /* array of specific errors */ ]
  }
}
```

#### Logging
- Log all API requests (method, path, user, timestamp, response time)
- Log errors with stack traces
- Use structured logging (JSON format)

#### Rate Limiting
- Apply rate limits per user and per IP:
  - Authentication endpoints: 5 requests per 15 minutes
  - Search endpoints: 60 requests per minute
  - Booking endpoints: 10 requests per minute

#### Monitoring
- Track API response times (P50, P95, P99)
- Monitor database query performance
- Alert on error rate > 1%
- Monitor payment gateway uptime

---

## Appendix

### HTTP Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST (resource created) |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Authenticated but not authorized |
| 404 | Not Found | Resource does not exist |
| 409 | Conflict | Resource conflict (e.g., duplicate email) |
| 422 | Unprocessable Entity | Valid syntax but business logic error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error |

### Glossary

- **JWT (JSON Web Token):** A compact, URL-safe token format for authentication
- **Idempotency:** Ability to execute operation multiple times with same result
- **PCI DSS:** Payment Card Industry Data Security Standard
- **SCA (Strong Customer Authentication):** EU requirement for payment authentication
- **E.164:** International phone number format (+[country][number])
- **ISO 8601:** International date/time format standard

---

**Project:** ALX Airbnb Clone - Backend Blueprint  
**Repository:** alx-airbnb-project-documentation  
**Version:** 1.0  
**Last Updated:** October 23, 2025
