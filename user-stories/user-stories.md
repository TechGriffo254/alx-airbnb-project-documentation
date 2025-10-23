# User Stories - Airbnb Clone Backend

This document contains user stories derived from the use case diagram. Each story follows the format: **"As a [user type], I want to [action], so that [benefit]."**

---

## Guest User Stories

### 1. User Registration
**As a** guest,  
**I want to** register an account with my email and password,  
**So that** I can access the platform and book properties.

**Acceptance Criteria:**
- User can provide first name, last name, email, phone, and password
- Email must be unique and validated
- Password must meet security requirements (min 8 characters, uppercase, lowercase, number)
- Verification email is sent after registration
- User receives a success message upon successful registration

---

### 2. Property Search
**As a** guest,  
**I want to** search for properties by location, dates, and number of guests,  
**So that** I can find suitable accommodations for my trip.

**Acceptance Criteria:**
- User can enter destination (city, country, or coordinates)
- User can select check-in and check-out dates
- User can specify number of guests
- System displays available properties matching search criteria
- Results can be filtered by price, property type, amenities, and ratings
- Search results are paginated (e.g., 20 properties per page)

---

### 3. View Property Details
**As a** guest,  
**I want to** view detailed information about a property,  
**So that** I can make an informed decision before booking.

**Acceptance Criteria:**
- User can see property title, description, and location
- Property photos are displayed in a gallery
- Amenities list is visible
- Pricing breakdown shows nightly rate, cleaning fees, and service fees
- User can view host profile and ratings
- Past guest reviews and ratings are displayed
- Availability calendar shows blocked and available dates

---

### 4. Create a Booking
**As a** guest,  
**I want to** book a property for specific dates,  
**So that** I can secure my accommodation for my trip.

**Acceptance Criteria:**
- User can select check-in and check-out dates
- User can specify number of guests (must not exceed max capacity)
- System calculates and displays total price (nightly rate × nights + fees)
- User can add optional special requests
- System validates property availability before confirming
- Payment is processed securely
- User receives booking confirmation email
- Booking status is set to "confirmed" or "pending" based on host settings

---

### 5. Cancel a Booking
**As a** guest,  
**I want to** cancel my booking,  
**So that** I can receive a refund according to the cancellation policy if my plans change.

**Acceptance Criteria:**
- User can view their upcoming bookings
- User can cancel a booking with confirmation prompt
- System calculates refund amount based on cancellation policy
- Refund is processed to original payment method
- Host is notified of cancellation
- Booking status is updated to "cancelled"
- User receives cancellation confirmation email

---

### 6. Write a Review
**As a** guest,  
**I want to** leave a review and rating for a property after my stay,  
**So that** I can share my experience with future guests.

**Acceptance Criteria:**
- User can only review properties they have booked and stayed at
- Review can be submitted within 14 days of checkout
- User provides rating (1-5 stars) and written feedback
- User can rate specific categories (cleanliness, accuracy, communication, etc.)
- Review is published after host also submits their review or deadline expires
- User receives confirmation after submitting review

---

### 7. Manage Profile
**As a** guest,  
**I want to** update my profile information,  
**So that** my account details are current and accurate.

**Acceptance Criteria:**
- User can edit first name, last name, phone, and bio
- User can upload/change profile photo
- User can update notification preferences
- Changes are saved and reflected immediately
- User receives confirmation message after successful update

---

### 8. Add Properties to Wishlist
**As a** guest,  
**I want to** save properties to my wishlist,  
**So that** I can easily find and compare them later.

**Acceptance Criteria:**
- User can click "Add to Wishlist" on any property
- Property is added to user's wishlist
- User can create multiple wishlists with custom names
- User can view all saved properties in wishlist
- User can remove properties from wishlist
- User can share wishlist link with others

---

### 9. Message the Host
**As a** guest,  
**I want to** send messages to a host,  
**So that** I can ask questions about the property before or during my booking.

**Acceptance Criteria:**
- User can send messages to property hosts
- Messages are organized by conversation thread
- User receives notifications when host replies
- Message history is preserved
- User can attach photos or documents (optional)

---

### 10. View Booking History
**As a** guest,  
**I want to** view my past and upcoming bookings,  
**So that** I can track my reservations and access booking details.

**Acceptance Criteria:**
- User can see list of all bookings (past, current, upcoming, cancelled)
- Each booking shows property name, dates, status, and total cost
- User can filter bookings by status
- User can click on a booking to view full details
- User can download booking confirmation/invoice

---

## Host User Stories

### 11. Create Property Listing
**As a** host,  
**I want to** create a new property listing,  
**So that** I can rent out my property to guests.

**Acceptance Criteria:**
- Host can enter property details (title, description, type, location)
- Host can set pricing (nightly rate, cleaning fees, minimum stay)
- Host can upload multiple photos (minimum 3 required)
- Host can select amenities from predefined list
- Host can set maximum guest capacity
- Host can define house rules and cancellation policy
- Property is listed as "active" after creation
- Host receives confirmation of successful listing

---

### 12. Edit Property Listing
**As a** host,  
**I want to** update my property listing details,  
**So that** I can keep information accurate and competitive.

**Acceptance Criteria:**
- Host can edit all property fields (title, description, pricing, photos, etc.)
- Changes are saved immediately
- Host can activate or deactivate the listing
- Updated information is reflected in search results
- Host receives confirmation after successful update

---

### 13. Manage Availability Calendar
**As a** host,  
**I want to** manage my property's availability calendar,  
**So that** I can control when guests can book my property.

**Acceptance Criteria:**
- Host can view monthly calendar for their property
- Host can block specific dates for personal use
- Host can set custom pricing for specific dates (e.g., holidays, peak season)
- Host can bulk-block multiple dates at once
- Calendar updates are reflected in real-time for guest searches
- Host can sync with external calendars (iCal format)

---

### 14. Review Booking Requests
**As a** host,  
**I want to** review and approve or decline booking requests,  
**So that** I can choose who stays at my property.

**Acceptance Criteria:**
- Host receives notification when new booking request is made
- Host can view guest profile and reviews
- Host can approve or decline request within 24 hours
- Guest is notified of host's decision
- If approved, booking status changes to "confirmed" and payment is captured
- If declined, guest is not charged and booking is cancelled

---

### 15. Review Guests
**As a** host,  
**I want to** leave reviews for guests after their stay,  
**So that** future hosts can make informed decisions.

**Acceptance Criteria:**
- Host can review guests within 14 days of checkout
- Host provides rating (1-5 stars) and written feedback
- Review is published after guest also submits or deadline expires
- Review is visible on guest's profile
- Host receives confirmation after submitting review

---

### 16. Receive Payouts
**As a** host,  
**I want to** receive payments for completed bookings,  
**So that** I can earn income from my property.

**Acceptance Criteria:**
- Payment is automatically transferred to host after guest checks in
- Host can view payout schedule and history
- Host can set up payment method (bank account, PayPal)
- Host receives payout confirmation email
- Host can download payout reports for tax purposes

---

## Admin User Stories

### 17. Manage Users
**As an** admin,  
**I want to** view and manage all user accounts,  
**So that** I can ensure platform safety and compliance.

**Acceptance Criteria:**
- Admin can view list of all users (guests and hosts)
- Admin can search users by email, name, or ID
- Admin can view user details and activity history
- Admin can suspend or deactivate accounts for policy violations
- Admin can reactivate accounts after review
- Actions are logged for audit purposes

---

### 18. Monitor Platform Analytics
**As an** admin,  
**I want to** view platform analytics and reports,  
**So that** I can track business performance and make data-driven decisions.

**Acceptance Criteria:**
- Admin can view dashboard with key metrics (total bookings, revenue, active users)
- Admin can filter data by date range
- Admin can generate reports for bookings, revenue, and user growth
- Admin can export reports as CSV or PDF
- Charts and graphs visualize trends over time

---

## Summary

- **Total User Stories:** 18
- **Guest Stories:** 10
- **Host Stories:** 6
- **Admin Stories:** 2

Each story includes clear acceptance criteria to guide development and testing.

---

**Project:** ALX Airbnb Clone - Backend Blueprint  
**Repository:** alx-airbnb-project-documentation  
**Last Updated:** October 23, 2025
