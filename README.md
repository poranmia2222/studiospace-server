
<div align="center">

# ⚙️ StudioSpace — Backend API

### Powering Creative Spaces. Connecting Creators.

A RESTful backend API for StudioSpace, a peer-to-peer creative workspace and production studio booking platform.

<br />

[![Server Repository](https://img.shields.io/badge/GitHub-Server_Repository-181717?style=for-the-badge&logo=github)](https://github.com/poranmia2222/studiospace-server)
[![Client Repository](https://img.shields.io/badge/GitHub-Client_Repository-181717?style=for-the-badge&logo=github)](https://github.com/poranmia2222/studiospace-client)
[![Live Website](https://img.shields.io/badge/🌐_Live_Website-Visit_StudioSpace-477E4D?style=for-the-badge)](https://studiospace-client-flax.vercel.app/)

<br />

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-000000?style=flat-square&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white)

</div>

---

## 📖 About The Project

**StudioSpace Server** is the backend service of the StudioSpace creative workspace marketplace.

It provides RESTful API endpoints to manage creative studios, handle booking operations, retrieve user-specific bookings, and protect private resources through JWT authentication.

Built with Node.js, Express.js, and MongoDB, the server connects the frontend application with the database and handles the platform's core business logic.

## ✨ Key Features

| Feature | Description |
|---|---|
| 🏢 Studio Management | Create, retrieve, update, and delete studio listings. |
| 🔍 Studio Discovery | Retrieve all studios and featured studios. |
| 📄 Studio Details | Fetch individual studio information. |
| 📅 Booking Management | Create and retrieve studio bookings. |
| 👤 User Bookings | Retrieve bookings associated with a specific user. |
| ❌ Booking Cancellation | Delete existing booking records. |
| 🔐 JWT Authentication | Verify user tokens using JOSE and Better Auth JWKS. |
| 🛡️ Protected Routes | Restrict sensitive API operations to authenticated users. |
| 🗄️ MongoDB Integration | Store studio and booking information. |
| 🌍 CORS Configuration | Allow requests from the authorized frontend application. |

## 🛠️ Technologies Used

### ⚙️ Backend Technologies

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)

### 🔐 Authentication & Security

![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)

- JOSE for JWT verification
- Better Auth JWKS integration
- Protected API routes
- Authorization middleware

### 🧰 Tools & Utilities

![NPM](https://img.shields.io/badge/NPM-CB3837?style=for-the-badge&logo=npm&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

---

## 🔗 API Documentation

**Base URL (Local):**

```text
http://localhost:8000
```

### 🏢 Studio Endpoints

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/` | Check server status | Public |
| GET | `/studios` | Get all studios | Public |
| GET | `/featured-studios` | Get featured studios | Public |
| GET | `/studios/:id` | Get studio by ID | Public |
| POST | `/studios` | Create a new studio | Private |
| GET | `/studio/:id` | Get protected studio data | Private |
| PATCH | `/studios/:id` | Update studio information | Private |
| DELETE | `/studios/:id` | Delete a studio | Private |

### 📅 Booking Endpoints

| Method | Endpoint | Description | Access |
|---|---|---|---|
| POST | `/booking` | Create a booking | Private |
| GET | `/booking/:id` | Get booking details | Private |
| GET | `/my-bookings/:emailId` | Get user's bookings | Private |
| DELETE | `/my-bookings/:id` | Cancel/delete a booking | Private |

> 🔒 Private endpoints require a valid Bearer token in the Authorization header.

---

## 🔑 Authentication

StudioSpace uses **Better Auth** on the frontend and JOSE on the backend to verify JWT tokens through the JWKS endpoint.

### Authorization Header

Include the token in your API request:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### JWT Verification Middleware

The backend verifies the token using the Better Auth JWKS endpoint.

```js
import { createRemoteJWKSet, jwtVerify } from "jose";

const JWKS = createRemoteJWKSet(
  new URL("/api/auth/jwks", process.env.CLIENT_URL)
);

const verifyToken = async (req, res, next) => {
  try {
    const authHeader = req.headers.authorization;

    if (!authHeader?.startsWith("Bearer ")) {
      return res.status(401).send({
        message: "Unauthorized: Token missing",
      });
    }

    const token = authHeader.split(" ")[1];

    const { payload } = await jwtVerify(token, JWKS);

    req.user = payload;

    next();
  } catch (error) {
    console.error("JWT Verification Error:", error);

    return res.status(401).send({
      message: "Unauthorized: Invalid or expired token",
    });
  }
};
```

---

## ⚙️ Installation & Setup

Follow these steps to run the backend locally.

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/poranmia2222/studiospace-server.git
```

### 2️⃣ Navigate to the Project Directory

```bash
cd studiospace-server
```

### 3️⃣ Install Dependencies

```bash
npm install
```

### 4️⃣ Configure Environment Variables

Create a `.env` file in the root directory.

```env
# Server Configuration
PORT=8000

# MongoDB Configuration
MONGODB_URI=your_mongodb_connection_string

# Frontend URL for CORS and JWKS
CLIENT_URL=http://localhost:3000
```

Replace the placeholder values with your actual configuration.

### 5️⃣ Start the Development Server

If your project uses `nodemon`:

```bash
npm run dev
```

Otherwise:

```bash
npm start
```

The server should be available at:

```text
http://localhost:8000
```

---

## 📂 Project Structure

```text
studiospace-server/
│
├── index.js
├── package.json
├── package-lock.json
├── vercel.json
├── .env
├── .gitignore
└── README.md
```

*Note: This structure is illustrative. Adjust it to match your actual backend files.*

---

## 🗄️ Database Structure

StudioSpace uses **MongoDB** as its database.

### Database

```text
studiospace
```

### Collections

**1. Studios**

Stores creative studio information, including:

- Studio name and description
- Location and address
- Category and facilities
- Pricing information
- Images and availability
- Owner information

**2. Bookings**

Stores booking information, including:

- Studio ID
- User name and email
- Booking date
- Selected date
- Start and end time
- Booking status

*The fields above describe the intended data model; adjust them to match your actual MongoDB documents.*

---

## 🔒 Environment Variables

| Variable | Description |
|---|---|
| `PORT` | Backend server port |
| `MONGODB_URI` | MongoDB connection string |
| `CLIENT_URL` | Frontend URL used for CORS and JWKS verification |

⚠️ **Security:** Never upload your `.env` file or expose MongoDB credentials and secrets in a public repository.

---

## 🚀 Deployment

The backend can be deployed using **Vercel**.

### Deployment Checklist

1. Push the server code to GitHub.
2. Import the repository into Vercel.
3. Configure the environment variables.
4. Set `CLIENT_URL` to the deployed frontend URL.
5. Verify MongoDB connectivity.
6. Test public and protected API endpoints.

---

## 🔗 Related Project

| Project | Repository |
|---|---|
| 🎨 Frontend | [studiospace-client](https://github.com/poranmia2222/studiospace-client) |
| ⚙️ Backend | [studiospace-server](https://github.com/poranmia2222/studiospace-server) |
| 🌐 Live Application | [StudioSpace](https://studiospace-client-flax.vercel.app/) |

---

## 👨‍💻 Author

<div align="center">

### Poran Mia

**Full Stack Developer | MERN Stack Developer**

Building modern, scalable, and user-friendly web applications.

[![GitHub](https://img.shields.io/badge/GitHub-poranmia2222-181717?style=for-the-badge&logo=github)](https://github.com/poranmia2222)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Poran_Mia-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/poranmia/)

</div>

---

<div align="center">

### ⭐ If you find this project useful, give it a star!

**Made with ❤️ by Poran Mia**

*StudioSpace — Find your space. Create something extraordinary.*

</div>
