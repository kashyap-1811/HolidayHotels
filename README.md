# HolidayHotels 🏨

A full-stack Airbnb-inspired hotel/vacation rental listing platform built with Node.js, Express, MongoDB, and EJS. Users can browse, create, and manage property listings with image uploads, category filtering, location search, and a review system — all behind authentication and authorization.

---

## 🌟 Features

- **Browse Listings** — View all available rentals on the home/index page
- **Category Filtering** — Filter listings by category (Trending, Mountains, Beach, Desert, Farms, Arctic, Countryside, Iconic Cities, Camping, Lake, Caves, Tropical)
- **Location Search** — Case-insensitive search by location name
- **Create & Manage Listings** — Authenticated users can create, edit, and delete their own listings
- **Image Uploads** — Property images are uploaded to and served from Cloudinary
- **Reviews** — Authenticated users can add, edit, and delete reviews (with 1–5 star ratings)
- **User Authentication** — Sign up, log in, and log out using Passport.js (local strategy)
- **Authorization** — Only the owner of a listing or the author of a review can modify or delete it
- **Flash Messages** — User-friendly success/error notifications
- **Session Persistence** — Sessions stored in MongoDB via connect-mongo

---

## 🛠️ Tech Stack

| Layer        | Technology                                      |
|--------------|-------------------------------------------------|
| Runtime      | Node.js (v22)                                   |
| Framework    | Express.js v5                                   |
| Database     | MongoDB + Mongoose                              |
| Templating   | EJS + ejs-mate (layouts)                        |
| Auth         | Passport.js + passport-local + passport-local-mongoose |
| File Uploads | Multer + Cloudinary (multer-storage-cloudinary) |
| Validation   | Joi                                             |
| Sessions     | express-session + connect-mongo                 |
| CSS          | Custom CSS + Bootstrap (via views)              |

---

## 📁 Project Structure

```
HolidayHotels/
├── app.js                  # Express app entry point
├── cloudConfig.js          # Cloudinary & Multer storage configuration
├── middleware.js           # Auth & authorization middleware
├── package.json
├── init/
│   ├── data.js             # Seed data
│   └── index.js            # Database seeder script
├── models/
│   ├── listing.js          # Listing Mongoose model & schema
│   ├── reviews.js          # Review Mongoose model & schema
│   └── user.js             # User Mongoose model (passport-local-mongoose)
├── schemas/
│   ├── schema.js           # Joi validation schema for listings
│   └── review.js           # Joi validation schema for reviews
├── routes/
│   ├── listing.js          # Listing routes
│   ├── reviews.js          # Review routes
│   └── user.js             # Auth routes (signup/login/logout)
├── controller/
│   ├── listing.js          # Listing route handlers
│   ├── review.js           # Review route handlers
│   └── user.js             # User/auth route handlers
├── views/
│   ├── layouts/            # EJS boilerplate layout
│   ├── includes/           # Navbar, footer, flash partials
│   ├── listings/           # index, show, new, edit views
│   ├── reviews/            # all reviews, edit review views
│   ├── users/              # signup, login views
│   ├── home.ejs            # Landing page
│   └── error.ejs           # Error page
├── public/
│   ├── css/                # Custom stylesheets
│   └── js/                 # Client-side scripts
└── utils/
    ├── ExpressError.js     # Custom error class
    └── wrapAsync.js        # Async error wrapper
```

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v22+
- [MongoDB Atlas](https://www.mongodb.com/atlas) account (or local MongoDB)
- [Cloudinary](https://cloudinary.com/) account for image hosting

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/kashyap-1811/HolidayHotels.git
   cd HolidayHotels
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Create a `.env` file** in the project root with the following variables:

   ```env
   # MongoDB
   password=<your_mongodb_atlas_password>
   secret=<your_session_secret>

   # Cloudinary
   CLOUD_NAME=<your_cloudinary_cloud_name>
   CLOUD_API_KEY=<your_cloudinary_api_key>
   CLOUD_API_SECRET=<your_cloudinary_api_secret>
   ```

4. **(Optional) Seed the database**

   ```bash
   node init/index.js
   ```

5. **Start the server**

   ```bash
   node app.js
   ```

6. Open your browser and navigate to `http://localhost:8080`

---

## 🗺️ API Routes

### Listings (`/listings`)

| Method | Path                  | Auth Required | Description                              |
|--------|-----------------------|---------------|------------------------------------------|
| GET    | `/listings`           | No            | List all listings (supports `?category=`) |
| GET    | `/listings/search`    | No            | Search listings by `?location=`          |
| GET    | `/listings/new`       | Yes           | Show new listing form                    |
| POST   | `/listings`           | Yes           | Create a new listing                     |
| GET    | `/listings/:id`       | No            | Show a specific listing                  |
| GET    | `/listings/:id/edit`  | Yes (owner)   | Show edit form for a listing             |
| PATCH  | `/listings/:id`       | Yes (owner)   | Update a listing                         |
| DELETE | `/listings/:id`       | Yes (owner)   | Delete a listing                         |

### Reviews (`/listings/:id/reviews`)

| Method | Path                                    | Auth Required  | Description                  |
|--------|-----------------------------------------|----------------|------------------------------|
| GET    | `/listings/:id/reviews`                 | No             | Show all reviews for listing |
| POST   | `/listings/:id/reviews`                 | Yes            | Add a new review             |
| GET    | `/listings/:id/reviews/:reviewId/edit`  | Yes (author)   | Show edit form for a review  |
| PUT    | `/listings/:id/reviews/:reviewId`       | Yes (author)   | Update a review              |
| DELETE | `/listings/:id/reviews/:reviewId`       | Yes (author)   | Delete a review              |

### Users (`/`)

| Method | Path        | Description          |
|--------|-------------|----------------------|
| GET    | `/signup`   | Show signup form     |
| POST   | `/signup`   | Register a new user  |
| GET    | `/login`    | Show login form      |
| POST   | `/login`    | Log in               |
| GET    | `/logout`   | Log out              |

---

## 🔐 Environment Variables

| Variable          | Description                                    |
|-------------------|------------------------------------------------|
| `password`        | MongoDB Atlas cluster password                 |
| `secret`          | Secret key for session signing and encryption  |
| `CLOUD_NAME`      | Cloudinary cloud name                          |
| `CLOUD_API_KEY`   | Cloudinary API key                             |
| `CLOUD_API_SECRET`| Cloudinary API secret                          |

> **Note:** The app runs in development mode by default. Set `NODE_ENV=PRODUCTION` in your environment to skip loading `.env` via dotenv.

---

## 📦 Data Models

### Listing
| Field         | Type       | Notes                                      |
|---------------|------------|--------------------------------------------|
| `title`       | String     | Required                                   |
| `description` | String     |                                            |
| `image`       | Object     | `{ filename, url }` — stored on Cloudinary |
| `price`       | Number     |                                            |
| `location`    | String     |                                            |
| `country`     | String     |                                            |
| `categories`  | [String]   | Enum of 12 categories, defaults to `trending` |
| `reviews`     | [ObjectId] | References Review documents                |
| `owner`       | ObjectId   | References User document                   |

### Review
| Field        | Type     | Notes                      |
|--------------|----------|----------------------------|
| `comment`    | String   |                            |
| `rating`     | Number   | 1–5                        |
| `created_at` | Date     | Defaults to creation time  |
| `author`     | ObjectId | References User document   |

### User
| Field      | Type   | Notes                                          |
|------------|--------|------------------------------------------------|
| `email`    | String | Required                                       |
| `username` | String | Managed by passport-local-mongoose             |
| `password` | String | Hashed & salted by passport-local-mongoose     |

---

## 📄 License

ISC © [Kashyap Rupareliya](https://github.com/kashyap-1811)
