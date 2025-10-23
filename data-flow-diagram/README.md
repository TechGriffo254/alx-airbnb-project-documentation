# Data Flow Diagram - Airbnb Clone Backend

This document describes the Data Flow Diagram (DFD) for the Airbnb Clone backend system, illustrating how data moves through the system from inputs to outputs.

## DFD Overview

A Data Flow Diagram visualizes:
- **External Entities:** Actors that interact with the system (Guest, Host, Payment Gateway)
- **Processes:** Operations that transform or process data
- **Data Stores:** Databases or storage locations for data
- **Data Flows:** Movement of data between entities, processes, and stores

---

## Level 0 DFD (Context Diagram)

### External Entities:
1. **Guest** - Books properties and makes payments
2. **Host** - Lists properties and receives payouts
3. **Payment Gateway** - Processes payments and refunds
4. **Email Service** - Sends notifications

### System Boundary:
- **Airbnb Clone Backend System**

### Data Flows:
- Guest → System: Registration data, search queries, booking requests, review data
- System → Guest: Search results, booking confirmations, property details
- Host → System: Property listings, availability updates, payout requests
- System → Host: Booking notifications, guest information, payout confirmations
- System ↔ Payment Gateway: Payment information, transaction status
- System → Email Service: Notification emails

---

## Level 1 DFD (Main Processes)

### Processes:

#### 1.0 User Management
- **Inputs:** Registration data, login credentials, profile updates
- **Outputs:** User authentication tokens, profile information
- **Data Stores:** User Database
- **Description:** Handles user registration, authentication, and profile management

#### 2.0 Property Management
- **Inputs:** Property details, photos, pricing, availability
- **Outputs:** Property listings, search results
- **Data Stores:** Property Database, Property Images Storage
- **Description:** Manages property listings creation, updates, and retrieval

#### 3.0 Search & Discovery
- **Inputs:** Search criteria (location, dates, guests, filters)
- **Outputs:** Filtered property results
- **Data Stores:** Property Database, Booking Database
- **Description:** Processes search queries and returns available properties

#### 4.0 Booking Management
- **Inputs:** Booking requests (property, dates, guests)
- **Outputs:** Booking confirmations, booking status
- **Data Stores:** Booking Database, Property Database, User Database
- **Description:** Creates, validates, and manages booking reservations

#### 5.0 Payment Processing
- **Inputs:** Payment details, booking information
- **Outputs:** Payment confirmations, receipts, refunds
- **Data Stores:** Payment Database, Booking Database
- **External:** Payment Gateway
- **Description:** Processes payments, refunds, and host payouts

#### 6.0 Review Management
- **Inputs:** Review data (rating, comments)
- **Outputs:** Published reviews, aggregated ratings
- **Data Stores:** Review Database, Booking Database, User Database
- **Description:** Manages guest and host reviews

#### 7.0 Messaging System
- **Inputs:** Messages between guests and hosts
- **Outputs:** Message delivery, notifications
- **Data Stores:** Message Database, User Database
- **Description:** Handles communication between users

#### 8.0 Notification Service
- **Inputs:** Booking events, payment events, message events
- **Outputs:** Email/SMS/push notifications
- **External:** Email Service, SMS Gateway
- **Description:** Sends notifications to users

---

## Data Stores

### 1. User Database
- **Contents:** User profiles, authentication data, roles (guest/host)
- **Accessed By:** User Management, Booking Management, Review Management

### 2. Property Database
- **Contents:** Property details, location, pricing, amenities
- **Accessed By:** Property Management, Search & Discovery, Booking Management

### 3. Booking Database
- **Contents:** Booking records, dates, status, guests
- **Accessed By:** Booking Management, Payment Processing, Review Management

### 4. Payment Database
- **Contents:** Payment transactions, refunds, payout records
- **Accessed By:** Payment Processing

### 5. Review Database
- **Contents:** Reviews, ratings, comments
- **Accessed By:** Review Management, Search & Discovery

### 6. Message Database
- **Contents:** Message threads, conversation history
- **Accessed By:** Messaging System

### 7. Property Images Storage
- **Contents:** Property photos, profile pictures
- **Accessed By:** Property Management, User Management

---

## Key Data Flows

### Guest Booking Flow:
1. Guest → Search & Discovery: Search criteria
2. Search & Discovery → Property Database: Query
3. Property Database → Search & Discovery: Available properties
4. Search & Discovery → Guest: Search results
5. Guest → Booking Management: Booking request
6. Booking Management → Property Database: Check availability
7. Booking Management → Payment Processing: Payment authorization
8. Payment Processing → Payment Gateway: Process payment
9. Payment Gateway → Payment Processing: Payment confirmation
10. Payment Processing → Booking Database: Store transaction
11. Booking Management → Booking Database: Create booking
12. Booking Management → Guest: Booking confirmation
13. Booking Management → Notification Service: Trigger email
14. Notification Service → Email Service: Send confirmation email

### Property Listing Flow:
1. Host → Property Management: Property details + photos
2. Property Management → Property Images Storage: Upload photos
3. Property Management → Property Database: Store property data
4. Property Management → Host: Listing confirmation

### Review Submission Flow:
1. Guest → Review Management: Review data
2. Review Management → Booking Database: Verify completed booking
3. Review Management → Review Database: Store review
4. Review Management → Property Database: Update aggregate rating
5. Review Management → Host: Review notification

---

## Creating the DFD in Draw.io

### Steps:
1. Go to [https://app.diagrams.net/](https://app.diagrams.net/)
2. Create a new blank diagram
3. Use these shapes:
   - **External Entities:** Rectangles (outside system boundary)
   - **Processes:** Circles or rounded rectangles with numbers (e.g., "1.0 User Management")
   - **Data Stores:** Open rectangles (parallel lines) with labels
   - **Data Flows:** Arrows with labels describing the data

4. **For Level 0 (Context Diagram):**
   - Place external entities around the perimeter
   - Draw a large circle/rectangle in the center for "Airbnb Clone System"
   - Connect entities to system with labeled arrows

5. **For Level 1 (Detailed Processes):**
   - Replace the system circle with individual process circles
   - Add data store rectangles
   - Connect processes to data stores and external entities
   - Label all arrows with specific data descriptions

6. Export as PNG: **File → Export as → PNG**
7. Save as `data-flow.png`

### Visual Guidelines:
- Use consistent shapes for each element type
- Ensure all arrows are labeled
- Number processes sequentially (1.0, 2.0, etc.)
- Keep the diagram clean and avoid crossing arrows when possible
- Use colors to distinguish between different types of flows (optional)

---

## Expected Diagram Content

Your DFD should include:
- **Context Diagram (Level 0):** Shows the system as a whole with external entities
- **Level 1 Diagram:** Shows 8 main processes with data stores and flows
- Clear labels on all data flows
- Proper notation (circles for processes, rectangles for entities, parallel lines for data stores)

---

**Export Instructions:**
- Create both Level 0 and Level 1 diagrams (can be combined or separate)
- Export as `data-flow.png`
- Save editable source as `data-flow.drawio`
- Place both files in this directory

---

**Project:** ALX Airbnb Clone - Backend Blueprint  
**Repository:** alx-airbnb-project-documentation  
**Last Updated:** October 23, 2025
