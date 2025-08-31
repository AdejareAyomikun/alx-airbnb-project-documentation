# Airbnb Clone Backend Requirements

This document outlines the **technical and functional requirements** for the backend of the Airbnb Clone project.  
It focuses on three key features: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. User Authentication

### Functional Requirements
- Users must be able to **register** with an email and password.
- Users must be able to **log in** and receive a secure session or JWT token.
- Users must be able to **update profile information** (name, phone, profile picture).
- Only authenticated users can access protected routes (e.g., property creation, bookings).

### API Endpoints
- `POST /api/auth/register` → Register a new user.
- `POST /api/auth/login` → Authenticate and return a token.
- `GET /api/users/:id` → Get user profile.
- `PUT /api/users/:id` → Update user profile.

### Input / Output
- **Input**: JSON payload with user details.
```json
{
  "email": "user@example.com",
  "password": "SecurePass123",
  "name": "John Doe"
}
```
- **Output**: JWT token & user info.
```json
{
  "token": "eyJhbGciOi...",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "user@example.com"
  }
}
```

### Validation Rules
- Email must be unique and valid.
- Password must have at least 8 characters, 1 uppercase, 1 number.
- Usernames cannot exceed 50 characters.

### Performance Criteria
- Login response within **<200ms**.
- Support up to **10,000 concurrent sessions**.

---

## 2. Property Management

### Functional Requirements
- Hosts must be able to **create, update, and delete properties**.
- Properties must include **title, description, price, location, amenities, and images**.
- Guests must be able to **search for properties** by filters (location, price range, amenities).

### API Endpoints
- `POST /api/properties` → Create a new property (Host only).
- `PUT /api/properties/:id` → Update property details (Host only).
- `DELETE /api/properties/:id` → Delete a property.
- `GET /api/properties` → List all properties with optional filters.
- `GET /api/properties/:id` → Get property details.

### Input / Output
- **Input**: JSON payload for property creation.
```json
{
  "title": "Modern Apartment in Lagos",
  "description": "2-bedroom apartment near the beach",
  "price": 120,
  "location": "Lagos, Nigeria",
  "amenities": ["WiFi", "Air Conditioning"],
  "images": ["img1.jpg", "img2.jpg"]
}
```
- **Output**: Created property object.
```json
{
  "id": 101,
  "title": "Modern Apartment in Lagos",
  "price": 120,
  "location": "Lagos, Nigeria"
}
```

### Validation Rules
- Title must not exceed 100 characters.
- Price must be a positive number.
- At least one image required per property.

### Performance Criteria
- Search queries must execute within **500ms**.
- Must scale to **1 million property records**.

---

## 3. Booking System

### Functional Requirements
- Guests must be able to **book available properties** for specific dates.
- Hosts must be able to **view bookings** for their properties.
- System must prevent **double bookings**.
- Guests must be able to **cancel bookings** before check-in date.

### API Endpoints
- `POST /api/bookings` → Create a booking.
- `GET /api/bookings/:id` → Get booking details.
- `GET /api/bookings/user/:userId` → Get all bookings for a user.
- `DELETE /api/bookings/:id` → Cancel a booking.

### Input / Output
- **Input**: JSON payload for booking.
```json
{
  "userId": 1,
  "propertyId": 101,
  "checkIn": "2025-09-10",
  "checkOut": "2025-09-15"
}
```
- **Output**: Booking confirmation.
```json
{
  "id": 5001,
  "userId": 1,
  "propertyId": 101,
  "status": "confirmed"
}
```

### Validation Rules
- Check-in date must be before check-out date.
- Booking duration must not exceed 30 days.
- User must be authenticated.

### Performance Criteria
- Booking confirmation response within **<300ms**.
- Handle **5,000 concurrent bookings per minute**.

---

✅ This document ensures the backend is **secure, scalable, and user-friendly**.
