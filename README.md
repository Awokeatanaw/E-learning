# 🎓 BIRUH AMIRO E-Learning Platform

**BIRUH AMIRO E-Learning Platform** is a full-stack Learning Management System (LMS) built with the MERN stack (MongoDB, Express.js, React, Node.js). Designed to connect students, educators, and administrators in a seamless digital learning environment, the platform supports course creation and multimedia streaming, interactive quizzes, progress tracking, local Ethiopian payment integration (**Chapa**), automated **PDF certificate generation**, and role-based access control powered by **Clerk**.

---

## 📖 About the Project

BIRUH AMIRO provides an end-to-end online education ecosystem tailored for modern learners and instructors:
- **Students** can browse and search courses, preview free lectures, purchase courses securely via **Chapa** (in ETB / Birr), stream video lectures (YouTube, Cloudinary authenticated video, or secure range-streamed local uploads), take course quizzes, track lecture-by-lecture progress, rate courses, and generate downloadable PDF certificates upon meeting completion criteria.
- **Educators** can apply to become instructors (with CV upload and duplicate email/phone validation), create and edit rich-text courses with chapters and video lectures, build interactive multiple-choice quizzes, and monitor course revenue and student enrollments from a dedicated dashboard.
- **Administrators** have full oversight of platform analytics, user roles, course publishing/moderation, revenue reports, and educator application approvals/rejections with automated email notifications.

---

## ✨ Features

### 👨‍🎓 Student Features
- **Course Discovery & Search**: Browse published courses, filter by keyword/category, view course curriculum, total duration, lecture counts, ratings, and discounts.
- **Secure Course Enrollment & Payment**: Integrated with **Chapa Payment Gateway** (`ETB` / `Birr`) with automatic transaction verification (`tx_ref`).
- **Interactive Video Player & Secure Streaming**:
  - Watch free preview lectures before enrolling.
  - Stream enrolled course lectures via **YouTube**, **Cloudinary signed URLs** (5-minute expiring authenticated video URLs), or **HTTP Range-supported secure backend streaming**.
- **Progress Tracking**: Mark individual lectures as completed, view real-time completion percentages (`rc-progress`), or reset course progress.
- **Interactive Quizzes & Instant Assessment**: Take course-specific quizzes, check individual answers or submit full quizzes, and track quiz scores.
- **Automated PDF Certificate Generation**:
  - Automatically unlocks when a student achieves **≥ 50% course completion** and **≥ 50% quiz score**.
  - Generates and downloads a custom A4 PDF Certificate of Completion using **Puppeteer**, tracking download counts.
- **Course Ratings**: Rate enrolled courses (1–5 stars) with real-time average rating calculation.
- **Become an Educator Workflow**: Submit an educator application with personal details and CV upload, and track application status.

### 👨‍🏫 Educator Features
- **Educator Dashboard**: View total courses, total enrolled students, total earnings, and recent student enrollment breakdowns.
- **Course Management (Create & Edit)**:
  - Rich-text course descriptions powered by **Quill.js**.
  - Organize curriculum into chapters and lectures with duration, free-preview toggles, video links, or direct video file uploads (`multer` + `Cloudinary`).
  - Set course pricing and percentage discounts.
- **Quiz Builder & Management**: Create, update, and delete multiple-choice quizzes linked to specific courses.
- **Student Enrollment Tracking**: View detailed lists of students enrolled in the educator's courses and purchase dates.

### 🛡️ Admin Features
- **Platform Overview & Analytics**: Real-time statistics on total users, total courses, completed purchases, total enrollments, and total platform revenue.
- **User & Role Management**: Search, paginate, delete users, and update roles (`student`, `educator`, `admin`) synchronized between MongoDB and **Clerk Public Metadata**.
- **Course Moderation**: Search, review, edit, publish/unpublish, or delete courses across the platform.
- **Educator Application Review**: View submitted educator requests, preview uploaded CVs, and approve or reject applications with automated **SMTP email notifications** sent to applicants.

---

## 🛠️ Technology Used

### Frontend (`e-learning/`)
| Technology | Purpose |
| :--- | :--- |
| **React 19** (`vite`) | Component-based UI and fast HMR build tooling |
| **Tailwind CSS v4** (`@tailwindcss/vite`) | Utility-first responsive styling |
| **React Router DOM v7** | Client-side routing for Student, Educator, and Admin portals |
| **Clerk React** (`@clerk/clerk-react`) | Authentication, user sessions, and profile management |
| **Quill** (`quill`) | Rich-text editor for course descriptions |
| **React YouTube** (`react-youtube`) | Embedded video playback for YouTube-hosted lectures |
| **RC Progress** (`rc-progress`) | Visual course progress bars |
| **Lucide React / Heroicons / React Icons** | UI iconography |
| **Humanize Duration** | Formatting lecture and course durations (`h`, `m`) |

### Backend (`server/`)
| Technology | Purpose |
| :--- | :--- |
| **Node.js & Express.js** | RESTful API server and middleware architecture |
| **MongoDB & Mongoose** | NoSQL database and schema modeling |
| **Clerk Express & Svix** (`@clerk/express`, `svix`) | JWT authentication middleware, role metadata sync, and webhook verification |
| **Chapa API** (`axios`) | Ethiopian payment gateway initialization and verification |
| **Cloudinary** (`cloudinary`) | Cloud storage for course thumbnails and authenticated lecture videos |
| **Multer** (`multer`) | Multipart form-data handling for images, lecture videos, and CV uploads |
| **Puppeteer** (`puppeteer`) | Headless Chromium HTML-to-PDF rendering for course certificates |
| **Nodemailer** (`nodemailer`) | Automated SMTP emails for enrollments, new courses, and educator request updates |

---

## 📂 Project Structure

```text
E-learning/
├── e-learning/                        # Frontend (React + Vite)
│   ├── public/                        # Static assets
│   ├── src/
│   │   ├── assets/                    # Images, icons, and fallback/mock data
│   │   ├── components/
│   │   │   ├── admin/                 # AdminSidebar, AdminTopbar, StatCard
│   │   │   ├── common/                # Modal, BecomeEducatorModal
│   │   │   ├── educator/              # Educator Navbar, Sidebar, Footer
│   │   │   └── student/               # Hero, Navbar, CourseCard, SearchBar, Rating, Footer, etc.
│   │   ├── context/
│   │   │   └── AppContext.jsx         # Global state (courses, auth sync, enrollments, helpers)
│   │   ├── pages/
│   │   │   ├── admin/                 # AdminDashboard, AdminUsers, AdminCourses, AdminReports, AdminEducatorRequests
│   │   │   ├── educator/              # Dashboard, AddCourse, EditCourse, MyCourses, CreateQuiz, MyQuizzes, StudentEnrolled
│   │   │   └── student/               # Home, CourseList, CourseDetails, Player, QuizList, QuizTake, MyEnrollments, PaymentSuccess, About, ContactUs, PrivacyPolicy
│   │   ├── services/
│   │   │   └── api.js                 # Centralized API client for backend communication
│   │   ├── App.jsx                    # Application routes & layout wrapper
│   │   └── main.jsx                   # Entry point with ClerkProvider & AppContextProvider
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
│
└── server/                            # Backend (Node.js + Express + MongoDB)
    ├── config/
    │   ├── cloudinary.js              # Cloudinary SDK configuration
    │   ├── mongodb.js                 # Mongoose database connection
    │   └── multer.js                  # File upload configuration
    ├── controllers/
    │   ├── adminController.js         # Admin user/course/report management
    │   ├── certificateController.js   # Certificate eligibility, viewing & Puppeteer PDF generation
    │   ├── courseController.js        # Course CRUD, Chapa payment, rating, progress & secure video streaming
    │   ├── educatorController.js      # Educator dashboard, course creation/editing, enrolled students
    │   ├── educatorRequestController.js # Educator CV application & admin approval/rejection
    │   ├── quizController.js          # Quiz creation, grading, and result tracking
    │   ├── userController.js          # User profile & enrolled courses retrieval
    │   └── webhooks.js                # Clerk webhook handler (user.created, user.updated, user.deleted)
    ├── middleware/
    │   ├── adminMiddleware.js         # Admin role authorization guard
    │   └── authMiddleware.js          # Educator role authorization guard
    ├── models/
    │   ├── Certificate.js             # Issued certificates & download counts
    │   ├── Course.js                  # Courses, chapters, lectures, ratings
    │   ├── EducatorRequest.js         # Educator applications & CV metadata
    │   ├── Progress.js                # Per-student lecture completion tracking
    │   ├── Purchase.js                # Chapa payment transactions
    │   ├── Quiz.js                    # Course quizzes & questions
    │   ├── QuizResult.js              # Student quiz submissions & percentage scores
    │   └── user.js                    # Users, roles (student/educator/admin), enrollments
    ├── routes/
    │   ├── adminRoutes.js             # /api/admin/*
    │   ├── certificateRoute.js        # /api/certificate/*
    │   ├── courseRoute.js             # /api/course/*
    │   ├── educatorRoutes.js          # /api/educator/*
    │   ├── quizRoute.js               # /api/quiz/*
    │   └── userRoute.js               # /api/user/*
    ├── utils/
    │   ├── mailer.js                  # Nodemailer SMTP helper
    │   └── syncUser.js                # Syncs Clerk users with local MongoDB User collection
    ├── server.js                      # Express app entry point (Port 5000)
    └── package.json

---

## ⚙️ Installation & Setup

### Prerequisites

Make sure you have the following installed and configured:

- __Node.js__ (v18+ recommended) and __npm__
- __MongoDB__ (Local instance or MongoDB Atlas URI)
- __Clerk Account__ (Publishable Key, Secret Key, and Webhook Secret)
- __Cloudinary Account__ (Cloud Name, API Key, and API Secret)
- __Chapa Account__ (Secret Key for payment processing)
- __SMTP Credentials__ (e.g., Gmail App Password or custom SMTP for email notifications)

---

### 1. Clone the Repository

```bash
git clone https://github.com/Awokeatanaw/E-learning.git
cd E-learning
```

---

### 2. Backend Setup (`server/`)

Navigate to the `server` directory and install dependencies:

```bash
cd server
npm install
```

Create a `.env` file in the `server/` directory with the following environment variables:

```env
# Server & URLs
BACKEND_URL=http://localhost:5000
FRONTEND_URL=http://localhost:5173
VITE_CURRENCY=ETB

# Database
MONGODB_URI=your_mongodb_connection_string

# Clerk Authentication & Webhooks
CLERK_PUBLISHABLE_KEY=pk_test_xxxxxxxxxxxxxxxxxxxxxxxxx
CLERK_SECRET_KEY=sk_test_xxxxxxxxxxxxxxxxxxxxxxxxx
CLERK_WEBHOOK_SECRET=whsec_xxxxxxxxxxxxxxxxxxxxxxxxx

# Cloudinary Storage
CLOUDINARY_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_SECRET_KEY=your_cloudinary_api_secret

# Chapa Payment Gateway
CHAPA_SECRET_KEY=CHASECK_TEST-xxxxxxxxxxxxxxxxxxxxxxxxx

# SMTP Mailer (Nodemailer)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_email_app_password
SMTP_FROM="BIRUH AMIRO E-Learning <your_email@gmail.com>"
```

Start the backend server:

```bash
# Development mode (with nodemon)
npm run dev

# Production mode
npm start
```

The backend API will run at `http://localhost:5000`.

---

### 3. Frontend Setup (`e-learning/`)

Open a new terminal, navigate to the `e-learning` directory, and install dependencies:

```bash
cd e-learning
npm install
```

Create a `.env` file in the `e-learning/` directory:

```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_xxxxxxxxxxxxxxxxxxxxxxxxx
VITE_BACKEND_URL=http://localhost:5000
VITE_CURRENCY=birr
```

Start the Vite development server:

```bash
npm run dev
```

The frontend application will run at `http://localhost:5173`.

---

## 🚀 Usage

### 1. Student Workflow

1. __Sign Up / Log In__: Authenticate via Clerk from the top navigation bar.
2. __Explore Courses__: Visit `/home` or `/course-list` to search and filter available courses.
3. __Preview & Enroll__: Open a course (`/courses/:id`), watch free preview lectures, and click __Enroll Now__ to pay via __Chapa__.
4. __Learn & Track Progress__: After payment verification (`/payment-success`), go to __My Enrollments__ (`/my-enrollments`) and open the __Course Player__ (`/player/:id`) to stream lectures and mark them complete.
5. __Take Quizzes & Earn Certificates__: Complete course quizzes (`/courses/:id/quizzes`). Once you achieve __≥ 50% lecture progress__ and __≥ 50% quiz score__, generate and download your official PDF Certificate.

### 2. Educator Workflow

1. __Apply as Educator__: Click __Become Educator__ in the navbar, fill in your qualification details, and upload your CV.

2. __Access Educator Portal__: Once approved by an administrator, navigate to `/educator/dashboard`.

3. __Create Courses & Quizzes__:

   - Add courses with thumbnails, chapters, and video lectures at `/educator/add-course`.
   - Build course assessments at `/educator/create-quiz`.
   - Manage existing courses and view enrolled students at `/educator/my-courses` and `/educator/student-enrolled`.

### 3. Admin Workflow

1. __Access Admin Portal__: Users with the `admin` role can navigate to `/admin`.

2. __Manage Platform__:

   - View revenue and enrollment metrics on the dashboard (`/admin` & `/admin/reports`).
   - Promote/demote user roles or remove accounts (`/admin/users`).
   - Publish, unpublish, edit, or delete courses (`/admin/courses`).
   - Review CVs and approve or reject educator applications (`/admin/educator-requests`).

---

## 🤝 Contributing

Contributions are welcome and appreciated! To contribute to __BIRUH AMIRO E-Learning Platform__:

1. __Fork the repository__ on GitHub.

2. __Clone your fork__ locally:

   ```bash
   git clone https://github.com/<your-username>/E-learning.git
   ```

3. __Create a feature branch__:

   ```bash
   git checkout -b feature/your-feature-name
   ```

4. __Make your changes__ and ensure frontend linting passes (`cd e-learning && npm run lint`).

5. __Commit your changes__ with a clear message:

   ```bash
   git commit -m "feat: add your descriptive feature message"
   ```

6. __Push to your branch__:

   ```bash
   git push origin feature/your-feature-name
   ```

7. __Open a Pull Request__ against the `main` branch describing your changes.
