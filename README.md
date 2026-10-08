# Placement Management System (PMS) — Backend(Phase 1)

The backend of the **Placement Management System (PMS)** is a RESTful API built using **Node.js, Express.js, MongoDB, and Mongoose**.

It provides authentication, role-based authorization, student and recruiter management, job management, applications, notifications, resume processing, AI-powered resume and job analysis, job recommendations, and candidate ranking.

---

## 1. Project Overview

The PMS backend provides the server-side infrastructure for managing the complete campus recruitment workflow.

The system supports three primary roles:

* **Student**
* **Recruiter**
* **Administrator**

The backend follows a modular architecture where authentication, users, students, recruiters, jobs, applications, notifications, and AI functionality are separated into their respective modules.

---

## 2. Technology Stack

| Technology        | Purpose                              |
| ----------------- | ------------------------------------ |
| Node.js           | Backend runtime                      |
| Express.js        | REST API framework                   |
| MongoDB Atlas     | Database                             |
| Mongoose          | MongoDB ODM                          |
| JWT               | Authentication                       |
| bcrypt            | Password hashing                     |
| Multer            | File upload handling                 |
| Cloudinary        | profile storage                      |
| Supabase          | Resume storage                       |
| Google Gemini API | AI analysis                          |
| Socket.IO         | Real-time notifications              |
| Postman           | API testing                          |

---

## 3. Architecture

The backend follows a layered and modular architecture.


Client
   │
   ▼
Routes
   │
   ▼
Middleware
   │
   ▼
Controllers
   │
   ▼
Services / Business Logic
   │
   ▼
Models
   │
   ▼
MongoDB


External services:


                    ┌──────────────┐
                    │ Google Gemini│
                    └──────┬───────┘
                           │
                           ▼
Client → API → Controllers → Services
                           │
                    ┌──────┴──────┐
                    ▼             ▼
                MongoDB       Cloudinary
                                  │
                                  ▼
                             File Storage


---

## 4. User Roles

### Student

Students can:

* Register
* Verify email using OTP
* Login
* Complete their profile
* Upload resumes
* Receive AI resume analysis
* Browse jobs
* Receive AI job recommendations
* Apply for jobs
* Track applications
* Receive notifications

### Recruiter

Recruiters can:

* Register
* Complete recruiter profile
* Wait for administrator approval
* Create jobs
* Update jobs
* Delete jobs
* View applicants
* Shortlist candidates
* Reject candidates
* Schedule interviews
* View AI-powered candidate ranking

### Administrator

Administrators can:

* Approve recruiters
* Reject recruiters
* Manage users
* Manage recruiters
* Manage jobs
* View placement reports
* Monitor system activity

---

# 5. Authentication and Authorization

The backend uses **JWT-based authentication**.

Authentication flow:


Register
   │
   ▼
Email Verification
   │
   ▼
Login
   │
   ▼
JWT Token
   │
   ▼
Protected API
   │
   ▼
Authentication Middleware
   │
   ▼
Role Authorization


### Supported authentication features

* Registration
* Email verification using OTP
* Login
* Logout
* Forgot password
* Reset password
* JWT authentication
* Protected routes
* Role-based authorization

Passwords are hashed using `bcrypt`.

---

# 6. Roles and Authorization

The backend supports:


student
recruiter
admin


Protected routes verify:

1. Whether the user is authenticated.
2. Whether the user's account is active.
3. Whether the user has the required role.
4. Whether the requested resource can be accessed.

---

# 7. Core Backend Modules

## Authentication

Handles:

* Registration
* Login
* Logout
* Email verification
* OTP
* Password reset
* JWT authentication

---

## Student

Handles:

* Student profile
* Personal information
* Academic information
* Skills
* Certifications
* Experience
* Resume
* Resume analysis

---

## Recruiter

Handles:

* Recruiter profile
* Company information
* Company logo
* Recruiter approval
* Recruiter status

---

## Jobs

Recruiters can create and manage jobs.

Job information includes:

* Job title
* Description
* Required skills
* Salary
* Location
* Job type
* Eligibility criteria
* Departments
* Passing year
* CGPA requirements
* Application deadline
* Job status

---

## Applications

Students can apply to jobs.

An application contains:

* Student
* Job
* Resume snapshot
* Cover letter
* Application status
* Recruiter remarks
* Interview information
* Status history

Duplicate applications are prevented using database constraints and application validation.

---

## Notifications

Notifications are generated for events such as:

* New job posting
* Interview scheduling
* Application status changes
* Recruiter approval

Notifications are stored in MongoDB.

Real-time notification delivery is supported using Socket.IO.

---

# 8. Resume Upload

Students can upload their resumes in PDF format.

The processing pipeline is:


Resume PDF
    │
    ▼
Multer
    │
    ▼
Supabse
    │
    ▼
Text Extraction
    │
    ▼
Gemini API
    │
    ▼
Structured Resume Analysis
    │
    ▼
Student Profile


The resume analysis is stored so that Gemini does not need to analyze the same resume repeatedly.

---

# 9. AI Resume Analysis

The backend integrates Google Gemini for resume analysis.

Gemini extracts structured information such as:

* Resume score
* Domain
* Technical skills
* Projects
* Experience
* Education
* Missing skills
* Improvement suggestions

Conceptually:

Resume
   ↓
Text Extraction
   ↓
Gemini
   ↓
Structured JSON
   ↓
Database


---

# 10. AI Job Analysis

When a recruiter creates a job, the backend can analyze the job using Gemini.

The analysis extracts:

* Required skills
* Preferred skills
* Keywords
* Education requirements
* Experience requirements

The analysis is designed to run asynchronously so that job creation does not depend on the Gemini API response.

---

# 11. Job Recommendation System

The recommendation system does not call Gemini for every recommendation.

Instead, it compares:

Student Resume Analysis
          VS
Job Analysis


The current scoring model considers:


Skill Match       → 70%
Experience Match  → 20%
Education Match   → 10%


The system generates:

* Match score
* Matched skills
* Missing skills
* Explanation
* Recommendation level

Jobs can then be sorted according to their calculated match score.

---

# 12. Candidate Ranking

Recruiters can rank applicants for a particular job.

The ranking system compares:


Applicant Resume Analysis
          VS
Job Analysis


The current ranking model considers:


Skill Match       → 70%
Project Relevance → 20%
Resume Score      → 10%


The result contains:

* Candidate rank
* Overall score
* Matched skills
* Missing skills
* Resume score
* Project score
* Recommendation

Candidates are sorted by their calculated score.

---

# 13. Database

The application uses **MongoDB Atlas**.

Major collections include:

```text
Users
StudentProfiles
RecruiterProfiles
Jobs
Applications
Notifications
```

Relationships are primarily maintained using MongoDB references.

Conceptually:

```text
User
 │
 ├── StudentProfile
 │       │
 │       └── Resume Analysis
 │
 └── RecruiterProfile
         │
         └── Jobs
                 │
                 └── Applications
```

---

# 14. File Storage

Cloudinary is used for storing uploaded media.

Examples include:

* Student resumes
* Student profile pictures
* Recruiter/company logos

The database stores the relevant Cloudinary information rather than storing large files directly inside MongoDB.

---

# 15. API Structure

The backend exposes REST APIs grouped by functionality.

Conceptual structure:

/api/auth
/api/students
/api/recruiters
/api/jobs
/api/applications
/api/notifications
/api/admin


Example authentication endpoints:

```text
POST /api/auth/register
POST /api/auth/verify-email
POST /api/auth/resend-otp
POST /api/auth/login
POST /api/auth/logout
POST /api/auth/forgot-password
POST /api/auth/reset-password
```

---

# 16. Request Flow

A typical protected request follows:

```text
Frontend
   │
   ▼
HTTP Request
   │
   ▼
Authentication Middleware
   │
   ▼
Role Authorization
   │
   ▼
Route
   │
   ▼
Controller
   │
   ▼
Business Logic
   │
   ▼
Mongoose Model
   │
   ▼
MongoDB
   │
   ▼
Response
```

---

# 17. Error Handling

The backend validates incoming requests and handles:

* Authentication errors
* Authorization errors
* Validation errors
* Duplicate records
* Missing resources
* File upload errors
* Database errors
* External API errors

API responses follow a consistent JSON-based response structure wherever applicable.

---

# 18. Environment Variables

The backend requires environment variables for sensitive configuration.

Typical variables include:

```env
PORT=
MONGODB_URI=

JWT_SECRET=
JWT_EXPIRES_IN=

EMAIL_USER=
EMAIL_PASSWORD=

CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=

GEMINI_API_KEY=
```

Do not commit `.env` files or API keys to Git.

---

# 19. Local Development

### Clone the repository

```bash
git clone <repository-url>
cd backend
```

### Install dependencies

```bash
npm install
```

### Configure environment variables

Create:

```text
.env
```

and add the required configuration.

### Start development server

```bash
npm run dev
```

### Production build

If the project configuration provides a production build script:

```bash
npm run build
```

Then start the production server using the configured start script.

---

# 20. API Testing

Postman can be used to test the backend APIs.

Recommended testing order:

```text
1. Register
2. Verify Email
3. Login
4. Create/Complete Profile
5. Upload Resume
6. Create Recruiter
7. Approve Recruiter
8. Create Job
9. Browse Jobs
10. Apply for Job
11. Update Application
12. Test Notifications
13. Test AI Features
```

---

# 21. Deployment

The backend is deployed on **Render**.

Database:

```text
MongoDB Atlas
```

File storage:

```text
Cloudinary
```

AI service:

```text
Google Gemini API
```

The backend communicates with the deployed frontend through the configured production API URL and CORS configuration.

---

# 22. Security

The backend implements:

* JWT authentication
* Password hashing with bcrypt
* Role-based authorization
* Protected routes
* Email verification
* Input validation
* Duplicate application prevention
* Secure file upload handling
* Environment-based secrets
* CORS configuration

---

# 23. Performance

Current performance-oriented implementations include:

* MongoDB indexes
* Lean database queries where applicable
* Asynchronous AI processing
* Stored AI analysis
* Avoiding repeated Gemini calls for recommendations
* Avoiding repeated Gemini calls for candidate ranking
* Background processing for pending AI analysis
* Retry handling for AI failures

---

# 24. AI Architecture Principle

A key design principle of PMS is separating **AI extraction** from **business logic**.

Instead of:

```text
Every recommendation
       ↓
Gemini
```

the system follows:

```text
Resume
   ↓
Gemini
   ↓
Structured Resume Data
            │
            ├──────────────┐
            ▼              ▼
    Recommendation      Ranking
```

This reduces:

* Gemini API calls
* API cost
* Response latency
* Dependency on AI availability

and makes recommendation/ranking more deterministic.

---

# 25. Future V2 Direction

The current backend represents **PMS V1**.

Planned V2 improvements include:

* JavaScript → TypeScript
* Improved modular architecture
* Multi-institution support
* Google OAuth
* Institution/corporate domain validation
* System administration role
* Pagination
* Search
* Sorting
* CGPA/SGPA eligibility filtering
* Online aptitude/coding tests
* Virtual interview integration
* Improved recommendation model
* Improved candidate ranking model
* System-wide statistics
* Institution subscriptions
* Redis and background workers
* Improved scalability and observability

These features are intentionally kept outside the V1 implementation.
