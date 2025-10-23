# Use Case Diagram - Airbnb Clone Backend

This document describes the use case diagram for the Airbnb Clone backend system, illustrating the interactions between actors (users) and the system functionalities.

## Actors

### 1. Guest
A user who searches for and books properties.

### 2. Host
A user who lists and manages properties for rental.

### 3. Admin (Optional)
A system administrator who manages users, properties, and resolves disputes.

### 4. Payment Gateway
An external system that processes payments (e.g., Stripe, PayPal).

---

## Use Cases

### Guest Use Cases

1. **Register Account**
   - **Actor:** Guest
   - **Description:** Guest creates a new account with email and password
   - **Preconditions:** None
   - **Postconditions:** Account is created and verified

2. **Login**
   - **Actor:** Guest
   - **Description:** Guest logs into the system with credentials
   - **Preconditions:** Account must exist
   - **Postconditions:** User is authenticated and receives JWT token

3. **Search Properties**
   - **Actor:** Guest
   - **Description:** Guest searches for available properties by location, dates, price, amenities
   - **Preconditions:** User is logged in (optional for browsing)
   - **Postconditions:** List of matching properties is displayed

4. **View Property Details**
   - **Actor:** Guest
   - **Description:** Guest views complete information about a specific property
   - **Preconditions:** Property must exist
   - **Postconditions:** Property details, photos, reviews, and availability are shown

5. **Create Booking**
   - **Actor:** Guest
   - **Description:** Guest books a property for specific dates
   - **Preconditions:** User is logged in, property is available
   - **Postconditions:** Booking is created with pending status, payment is processed

6. **Make Payment**
   - **Actor:** Guest, Payment Gateway
   - **Description:** Guest provides payment information and completes transaction
   - **Preconditions:** Booking is created
   - **Postconditions:** Payment is authorized, booking is confirmed

7. **Cancel Booking**
   - **Actor:** Guest
   - **Description:** Guest cancels an existing booking
   - **Preconditions:** Booking exists and is cancellable
   - **Postconditions:** Booking is cancelled, refund is processed based on policy

8. **Write Review**
   - **Actor:** Guest
   - **Description:** Guest reviews a property and host after checkout
   - **Preconditions:** Booking is completed
   - **Postconditions:** Review is published after deadline or mutual submission

9. **Manage Profile**
   - **Actor:** Guest
   - **Description:** Guest updates personal information, profile photo, preferences
   - **Preconditions:** User is logged in
   - **Postconditions:** Profile is updated

10. **View Booking History**
    - **Actor:** Guest
    - **Description:** Guest views past and upcoming bookings
    - **Preconditions:** User is logged in
    - **Postconditions:** List of bookings is displayed

11. **Add to Wishlist**
    - **Actor:** Guest
    - **Description:** Guest saves properties to wishlist for later viewing
    - **Preconditions:** User is logged in
    - **Postconditions:** Property is added to wishlist

12. **Send Message to Host**
    - **Actor:** Guest
    - **Description:** Guest communicates with host about a property or booking
    - **Preconditions:** User is logged in
    - **Postconditions:** Message is sent to host

### Host Use Cases

1. **Register as Host**
   - **Actor:** Host
   - **Description:** User registers account with host privileges
   - **Preconditions:** None
   - **Postconditions:** Host account is created

2. **Create Property Listing**
   - **Actor:** Host
   - **Description:** Host adds a new property with details, photos, pricing, amenities
   - **Preconditions:** User is logged in as host
   - **Postconditions:** Property is created and listed

3. **Edit Property**
   - **Actor:** Host
   - **Description:** Host updates property details, photos, pricing, availability
   - **Preconditions:** Property exists and belongs to host
   - **Postconditions:** Property is updated

4. **Manage Property Availability**
   - **Actor:** Host
   - **Description:** Host sets available/blocked dates on calendar
   - **Preconditions:** Property exists
   - **Postconditions:** Property availability is updated

5. **View Bookings**
   - **Actor:** Host
   - **Description:** Host views all bookings for their properties
   - **Preconditions:** User is logged in as host
   - **Postconditions:** List of bookings is displayed

6. **Confirm/Reject Booking**
   - **Actor:** Host
   - **Description:** Host approves or declines a booking request
   - **Preconditions:** Booking exists with pending status
   - **Postconditions:** Booking status is updated, guest is notified

7. **Review Guest**
   - **Actor:** Host
   - **Description:** Host reviews guest after checkout
   - **Preconditions:** Booking is completed
   - **Postconditions:** Review is published

8. **Send Message to Guest**
   - **Actor:** Host
   - **Description:** Host communicates with guest about booking
   - **Preconditions:** User is logged in
   - **Postconditions:** Message is sent to guest

9. **Receive Payout**
   - **Actor:** Host, Payment Gateway
   - **Description:** Host receives payment for completed bookings
   - **Preconditions:** Booking is completed, guest has checked in
   - **Postconditions:** Payment is transferred to host account

### Admin Use Cases (Optional)

1. **Manage Users**
   - **Actor:** Admin
   - **Description:** Admin views, suspends, or removes user accounts
   - **Preconditions:** Admin is logged in
   - **Postconditions:** User account status is updated

2. **Manage Properties**
   - **Actor:** Admin
   - **Description:** Admin reviews, approves, or removes property listings
   - **Preconditions:** Admin is logged in
   - **Postconditions:** Property status is updated

3. **Resolve Disputes**
   - **Actor:** Admin
   - **Description:** Admin mediates conflicts between guests and hosts
   - **Preconditions:** Dispute is reported
   - **Postconditions:** Dispute is resolved, actions are taken

4. **View Analytics**
   - **Actor:** Admin
   - **Description:** Admin monitors platform metrics, revenue, bookings
   - **Preconditions:** Admin is logged in
   - **Postconditions:** Reports and dashboards are displayed

---

## Relationships

### Include Relationships
- **Create Booking** includes **Make Payment**
- **Register Account** includes **Send Verification Email**
- **Cancel Booking** includes **Process Refund**

### Extend Relationships
- **Search Properties** extends **Apply Filters** (optional)
- **Create Property Listing** extends **Upload Photos** (optional but recommended)
- **Login** extends **Password Reset** (when user forgets password)

---

## Creating the Diagram in Draw.io

### Steps:
1. Go to [https://app.diagrams.net/](https://app.diagrams.net/)
2. Create a new blank diagram
3. From the left panel, select **UML** → **Use Case**
4. Add actors using stick figure icons:
   - Guest
   - Host
   - Admin
   - Payment Gateway (as external system)
5. Add use case ovals for each functionality
6. Connect actors to use cases with solid lines (associations)
7. Use dashed arrows with `<<include>>` or `<<extend>>` for special relationships
8. Draw a system boundary box around all use cases
9. Export as PNG: **File → Export as → PNG**
10. Save the file as `use-case-diagram.png`

### Visual Elements:
- **Actors:** Stick figures outside the system boundary
- **Use Cases:** Ovals inside the system boundary
- **System Boundary:** Rectangle enclosing all use cases
- **Associations:** Solid lines between actors and use cases
- **Include/Extend:** Dashed arrows with stereotypes

---

## Expected Diagram Content

Your use case diagram should include:
- All actors listed above
- Minimum 15-20 use cases covering core functionalities
- Clear system boundary
- Proper relationships (include/extend where applicable)
- Readable labels and layout

---

**Export Instructions:**
- Export the completed diagram as `use-case-diagram.png`
- Save the editable source as `use-case-diagram.drawio`
- Place both files in this directory

---

**Project:** ALX Airbnb Clone - Backend Blueprint  
**Repository:** alx-airbnb-project-documentation  
**Last Updated:** October 23, 2025
