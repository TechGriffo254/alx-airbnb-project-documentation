# Flowcharts - Airbnb Clone Backend

This directory contains flowcharts that detail specific backend processes for the Airbnb Clone system.

## Flowchart 1: User Registration Process

### Process Steps:

1. **Start**
2. User navigates to registration page
3. User enters registration details (first name, last name, email, password, phone)
4. **Decision:** Is email format valid?
   - **No** → Display error "Invalid email format" → Return to step 3
   - **Yes** → Continue
5. **Decision:** Does password meet requirements (min 8 chars, uppercase, lowercase, number)?
   - **No** → Display error "Password must meet requirements" → Return to step 3
   - **Yes** → Continue
6. **Decision:** Does email already exist in database?
   - **Yes** → Display error "Email already registered" → Return to step 3
   - **No** → Continue
7. Hash password using bcrypt
8. Create user record in database
9. Generate email verification token
10. Send verification email to user
11. Display success message "Registration successful! Please check your email."
12. **End**

---

## Flowchart 2: Property Booking Process

### Process Steps:

1. **Start**
2. User selects property and desired dates
3. System retrieves property details from database
4. **Decision:** Is user logged in?
   - **No** → Redirect to login page → Return to step 1 after login
   - **Yes** → Continue
5. User enters number of guests
6. **Decision:** Does guest count exceed property max capacity?
   - **Yes** → Display error "Too many guests" → Return to step 5
   - **No** → Continue
7. System checks property availability for selected dates
8. **Decision:** Is property available?
   - **No** → Display error "Property not available for these dates" → Return to step 2
   - **Yes** → Continue
9. Calculate total price:
   - Nightly rate × number of nights
   - Add cleaning fees
   - Add service fees
   - Add taxes
10. Display price breakdown to user
11. User reviews and confirms booking
12. **Decision:** User confirms booking?
    - **No** → Return to step 2 or **End**
    - **Yes** → Continue
13. Create booking record with status "pending"
14. Redirect to payment page
15. User enters payment details (card info)
16. Send payment request to Payment Gateway API
17. **Decision:** Payment successful?
    - **No** → Display error "Payment failed" → Return to step 15 (with retry limit)
    - **Yes** → Continue
18. Update booking status to "confirmed"
19. Store payment transaction in database
20. Send booking confirmation email to guest
21. Send booking notification email to host
22. Display success message "Booking confirmed!"
23. **End**

---

## Flowchart 3: Property Search Process

### Process Steps:

1. **Start**
2. User enters search criteria:
   - Location (city, country)
   - Check-in date
   - Check-out date
   - Number of guests
3. **Decision:** Are dates valid (check-in before check-out, not in past)?
   - **No** → Display error "Invalid dates" → Return to step 2
   - **Yes** → Continue
4. Query database for properties matching location
5. **Decision:** Any properties found?
   - **No** → Display "No properties found in this location" → **End**
   - **Yes** → Continue
6. Filter properties by guest capacity (>= number of guests)
7. Check availability: exclude properties with conflicting bookings for selected dates
8. User applies optional filters (price range, amenities, property type)
9. Filter results based on optional criteria
10. Sort results by selected option (price, rating, distance)
11. Paginate results (20 per page)
12. Display property cards with:
    - Property photo
    - Title
    - Price per night
    - Rating
    - Location
13. **Decision:** User wants to view property details?
    - **Yes** → Navigate to property details page
    - **No** → **Decision:** User wants to modify search?
      - **Yes** → Return to step 2
      - **No** → **End**

---

## Creating Flowcharts in Draw.io

### Steps:

1. Go to [https://app.diagrams.net/](https://app.diagrams.net/)
2. Create a new blank diagram
3. Select **Flowchart** shapes from the left panel
4. Use standard flowchart symbols:
   - **Oval:** Start/End
   - **Rectangle:** Process/Action step
   - **Diamond:** Decision point (Yes/No)
   - **Parallelogram:** Input/Output
   - **Arrow:** Flow direction

### Visual Guidelines:

- Start at the top, flow downward
- Use clear, concise labels for each step
- Decision diamonds should have two or more outgoing arrows labeled with conditions
- Ensure all paths lead to an end point
- Use consistent colors (optional: green for success, red for errors)
- Keep the diagram clean and avoid overlapping lines

### Export Instructions:

1. Select the flowchart you want to create (User Registration, Property Booking, or Property Search)
2. Build the complete flowchart following the steps above
3. Export as PNG: **File → Export as → PNG**
4. Save the file with appropriate name:
   - `user-registration-flowchart.png`
   - `property-booking-flowchart.png`
   - `property-search-flowchart.png`
5. Save editable source as `.drawio` file with same name
6. Place files in this directory

---

## Required Deliverable

**Per project requirements, create at least ONE flowchart** from the options above. The property booking process is recommended as it demonstrates the most complex workflow including payment integration.

---

**Export as:** `data-flow-diagram.png` (Note: Despite the name in requirements, this is actually a flowchart file)

---

**Project:** ALX Airbnb Clone - Backend Blueprint  
**Repository:** alx-airbnb-project-documentation  
**Last Updated:** October 23, 2025
