# Backend API Documentation

This document describes the available API endpoints for the backend service.

---

## /users/register

**Description:**  
Registers a new user by creating a user account with the provided information.

**HTTP Method:**  
`POST`

**Request Body:**  
JSON format with the following fields:
- `fullname` (object):
  - `firstname` (string, required): User's first name (minimum 3 characters)
  - `lastname` (string, optional): User's last name (minimum 3 characters)
- `email` (string, required): User's email address (must be a valid email)
- `password` (string, required): User's password (minimum 6 characters)

**Example Request:**
```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john@example.com",
  "password": "secret123"
}
```

**Example Response:**
```json
{
  "token": "<JWT Token>",
  "user": {
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john@example.com"
  }
}
```

**Status Codes:**
- `201 Created`: User registered successfully
- `400 Bad Request`: Validation error or user already exists
- `500 Internal Server Error`: Server error

---

## /users/login

**Description:**  
Authenticates a user using their email and password, returning a JWT token upon successful login.

**HTTP Method:**  
`POST`

**Request Body:**
- `email` (string, required): User's email address (must be a valid email)
- `password` (string, required): User's password (minimum 6 characters)

**Example Request:**
```json
{
  "email": "john@example.com",
  "password": "secret123"
}
```

**Example Response:**
```json
{
  "token": "<JWT Token>",
  "user": {
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john@example.com"
  }
}
```

**Status Codes:**
- `200 OK`: Login successful
- `400 Bad Request`: Validation error
- `401 Unauthorized`: Invalid email or password

---

## /users/profile

**Description:**  
Retrieves the profile information of the currently authenticated user.

**HTTP Method:**  
`GET`

**Authentication:**  
Requires a valid JWT token in the `Authorization` header:  
`Authorization: Bearer <token>`

**Example Response:**
```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john@example.com"
}
```

**Status Codes:**
- `200 OK`: Profile retrieved
- `401 Unauthorized`: Invalid or missing token

---

## /users/logout

**Description:**  
Logout the current user and blacklist the token provided in cookie or headers.

**HTTP Method:**  
`GET`

**Authentication:**  
Requires a valid JWT token in the `Authorization` header or cookie.

**Example Response:**
```json
{
  "message": "Logged out"
}
```

**Status Codes:**
- `200 OK`: Logout successful
- `401 Unauthorized`: Invalid or missing token

---

## /captains/register

**Description:**  
Registers a new captain by creating a captain account with the provided information.

**HTTP Method:**  
`POST`

**Request Body:**  
JSON format with the following fields:
- `fullname` (object):
  - `firstname` (string, required): Captain's first name (minimum 3 characters)
  - `lastname` (string, optional): Captain's last name (minimum 3 characters)
- `email` (string, required): Captain's email address (must be a valid email)
- `password` (string, required): Captain's password (minimum 6 characters)
- `vehicle` (object):
  - `color` (string, required): Vehicle color (minimum 3 characters)
  - `plate` (string, required): Vehicle plate number (minimum 3 characters)
  - `capacity` (number, required): Vehicle passenger capacity (minimum 1)
  - `vehicleType` (string, required): Type of vehicle (`car`, `motorcycle`, or `auto`)

**Example Request:**
```json
{
  "fullname": {
    "firstname": "Alice",
    "lastname": "Smith"
  },
  "email": "alice@example.com",
  "password": "mypassword",
  "vehicle": {
    "color": "Red",
    "plate": "XYZ123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

**Example Response:**
```json
{
  "token": "<JWT Token>",
  "captain": {
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

---

## /captains/login

**Description:**  
Authenticates a captain using their email and password, returning a JWT token upon successful login.

**HTTP Method:**  
`POST`

**Request Body:**
- `email` (string, required): Captain's email address (must be a valid email)
- `password` (string, required): Captain's password (minimum 6 characters)

**Example Response:**
```json
{
  "token": "<JWT Token>",
  "captain": {
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

---

## /captains/profile

**Description:**  
Retrieves the profile information of the currently authenticated captain.

**HTTP Method:**  
`GET`

**Authentication:**  
Requires a valid JWT token in the `Authorization` header:  
`Authorization: Bearer <token>`

**Example Response:**
```json
{
  "fullname": {
    "firstname": "Alice",
    "lastname": "Smith"
  },
  "email": "alice@example.com",
  "vehicle": {
    "color": "Red",
    "plate": "XYZ123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

---

## /captains/logout

**Description:**  
Logout the current captain and blacklist the token provided in cookie or headers.

**HTTP Method:**  
`GET`

**Authentication:**  
Requires a valid JWT token in the Authorization header or cookie.

**Example Response:**
```json
{
  "message": "Logout successfully."
}
```

---

## /maps/get-coordinates

**Description:**  
Retrieves the coordinates (latitude and longitude) for a given address.

**HTTP Method:**  
`GET`

**Request Parameters:**
- `address` (string, required): The address for which to retrieve coordinates.

**Example Request:**  
`GET /maps/get-coordinates?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA`

**Example Response:**
```json
{
  "ltd": 37.4224764,
  "lng": -122.0842499
}
```

**Error Responses:**
- `400 Bad Request`: If the address parameter is missing or invalid.
- `404 Not Found`: If the coordinates for the given address could not be found.
  ```json
  { "message": "Coordinates not found" }
  ```

---

## /maps/get-distance-time

**Description:**  
Retrieves the distance and estimated travel time between two locations.

**HTTP Method:**  
`GET`

**Request Parameters:**
- `origin` (string, required): The starting address or location.
- `destination` (string, required): The destination address or location.

**Example Request:**  
`GET /maps/get-distance-time?origin=New+York,NY&destination=Los+Angeles,CA`

**Example Response:**
```json
{
  "distance": {
    "text": "2,789 miles",
    "value": 4486540
  },
  "duration": {
    "text": "1 day 18 hours",
    "value": 154800
  }
}
```

**Error Responses:**
- `400 Bad Request`: If the origin or destination parameter is missing or invalid.
- `404 Not Found`: If the distance and time for the given locations could not be found.
  ```json
  { "message": "No routes found" }
  ```

---

## /maps/get-suggestions

**Description:**  
Retrieves autocomplete suggestions for a given input string.

**HTTP Method:**  
`GET`

**Request Parameters:**
- `input` (string, required): The input string for which to retrieve suggestions.

**Example Request:**  
`GET /maps/get-suggestions?input=1600+Amphitheatre`

**Example Response:**
```json
[
  "1600 Amphitheatre Parkway, Mountain View, CA, USA",
  "1600 Amphitheatre Pkwy, Mountain View, CA 94043, USA"
]
```

**Error Responses:**
- `400 Bad Request`: If the input parameter is missing or invalid.
- `500 Internal Server Error`: If there is an error retrieving suggestions.
  ```json
  { "message": "Unable to fetch suggestions" }
  ```

---

## /rides/create

**Description:**  
Creates a new ride with the provided information.

**HTTP Method:**  
`POST`

**Authentication:**  
Requires a valid JWT token in the `Authorization` header:  
`Authorization: Bearer <token>`

**Request Body:**
- `pickup` (string, required): The pickup address (minimum 3 characters)
- `destination` (string, required): The destination address (minimum 3 characters)
- `vehicleType` (string, required): The type of vehicle (`auto`, `car`, or `moto`)

**Example Request:**
```json
{
  "pickup": "1600 Amphitheatre Parkway, Mountain View, CA",
  "destination": "1 Infinite Loop, Cupertino, CA",
  "vehicleType": "car"
}
```

**Example Response:**
```json
{
  "ride": {
    "user": "user_id",
    "pickup": "1600 Amphitheatre Parkway, Mountain View, CA",
    "destination": "1 Infinite Loop, Cupertino, CA",
    "fare": 75.0,
    "status": "pending",
    "duration": 154800,
    "distance": 4486540,
    "otp": "123456"
  }
}
```

**Error Responses:**
- `400 Bad Request`: If any required field is missing or invalid.
- `500 Internal Server Error`: If there is an error creating the ride.
  ```json
  { "message": "Error message" }
  ```

---

## /rides/get-fare

**Description:**  
Retrieves the fare estimate for a ride between the provided pickup and destination addresses.

**HTTP Method:**  
`GET`

**Authentication:**  
Requires a valid JWT token in the `Authorization` header:  
`Authorization: Bearer <token>`

**Request Parameters:**
- `pickup` (string, required): The pickup address (minimum 3 characters)
- `destination` (string, required): The destination address (minimum 3 characters)

**Example Request:**  
`GET /rides/get-fare?pickup=1600+Amphitheatre+Parkway,+Mountain+View,+CA&destination=1+Infinite+Loop,+Cupertino,+CA`

**Example Response:**
```json
{
  "auto": 50.0,
  "car": 75.0,
  "moto": 40.0
}
```

**Error Responses:**
- `400 Bad Request`: If any required parameter is missing or invalid.
- `500 Internal Server Error`: If there is an error calculating the fare.
  ```json
  { "message": "Error message" }
  ```

---