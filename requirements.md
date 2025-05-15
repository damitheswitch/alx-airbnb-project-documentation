🛠️ Backend Requirement Specifications
This document outlines the **technical and functional requirements** for the following core backend features:
1. User Authentication
2. Property Management
3. Booking System

🔐 1. User Authentication
📌 Functional Requirements
* Allow users to register as **guest** or **host**
* Allow login via email/password and optional OAuth (Google/Facebook)
* Support JWT-based session authentication
* Allow users to update profile information

🧪 Validation Rules
* Email must be unique and properly formatted
* Password must be at least 8 characters, with letters and numbers
* Role must be either `guest` or `host`

🔁 API Endpoints

| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login and receive token |
| GET | `/api/users/me` | Get current user info |
| PUT | `/api/users/me` | Update profile |

📥 Sample Input

```json
{ "email": "jane@example.com", "password": "Secure123!", "role": "host" }
```

📤 Sample Output (Login)

```json
{ "token": "eyJhbGciOiJIUzI1NiIsInR...", "user": { "id": 1, "email": "jane@example.com", "role": "host" } }
```

⚙️ Performance & Security
* Token expiration: 24 hours
* Passwords stored as salted hashes (bcrypt)
* Rate limiting on login endpoint to prevent brute-force attacks

🏘️ 2. Property Management
📌 Functional Requirements
* Hosts can create, edit, and delete property listings
* Listings include title, description, location, price, images, and availability
* Properties must be tied to a valid host user

🧪 Validation Rules
* Title and description must not be empty
* Price must be a positive float
* Location is required
* Images must be in JPG/PNG format

🔁 API Endpoints

| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| POST | `/api/properties` | Create a property listing |
| GET | `/api/properties` | Get all listings (with filters) |
| GET | `/api/properties/:id` | Get property details |
| PUT | `/api/properties/:id` | Update property listing |
| DELETE | `/api/properties/:id` | Delete property listing |

📥 Sample Input (Create)

```json
{ "title": "Cozy Loft in Downtown", "description": "A clean and cozy space...", "location": "Shanghai", "price": 85.50, "amenities": ["wifi", "air_conditioning"], "availability": { "start_date": "2025-06-01", "end_date": "2025-06-30" } }
```

📤 Sample Output

```json
{ "id": 101, "host_id": 5, "title": "Cozy Loft in Downtown", "price": 85.50 }
```

⚙️ Performance & Security
* Paginated listing retrieval (default 10 per page)
* Image files stored via Cloudinary or local storage
* Ownership validation: only hosts can edit/delete their own listings

📅 3. Booking System
📌 Functional Requirements
* Guests can book available properties for specific dates
* Prevent double bookings through availability validation
* Guests and hosts can cancel bookings based on rules
* Track booking status: `pending`, `confirmed`, `canceled`, `completed`

🧪 Validation Rules
* Dates must be within property availability range
* Booking duration max: 30 days
* Guests cannot book their own properties

🔁 API Endpoints

| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| POST | `/api/bookings` | Create a booking |
| GET | `/api/bookings/me` | Get current user's bookings |
| PUT | `/api/bookings/:id/cancel` | Cancel a booking |
| GET | `/api/bookings/:id` | View booking details |

📥 Sample Input (Create)

```json
{ "property_id": 101, "start_date": "2025-06-05", "end_date": "2025-06-10" }
```

📤 Sample Output

```json
{ "id": 501, "guest_id": 3, "property_id": 101, "status": "pending" }
```

⚙️ Performance & Security
* All date operations use server timezone (UTC)
* Bookings stored with `created_at` timestamps
* Host notified of new booking via email/in-app notification
