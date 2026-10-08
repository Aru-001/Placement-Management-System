# Placement Management System (PMS) — Frontend(Phase 1)

The frontend of the **Placement Management System (PMS)** is a responsive React-based web application built with **Vite, Redux Toolkit, React Router, Axios, Tailwind CSS, Framer Motion, and Lucide React**.

It provides separate role-based interfaces for Students, Recruiters, and Administrators while integrating AI-powered resume analysis, job recommendations, and candidate ranking.

---

# 1. Project Overview

The PMS frontend provides the user interface for the complete campus placement workflow.

The application supports three primary roles:

* **Student**
* **Recruiter**
* **Administrator**

Each role is redirected to its own dashboard after authentication.


                         PMS Frontend
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
          Student          Recruiter        Admin
          Dashboard        Dashboard       Dashboard


---

# 2. Technology Stack

| Technology       | Purpose                 |
| ---------------- | ----------------------- |
| React            | UI development          |
| Vite             | Frontend tooling        |
| Redux Toolkit    | Global state management |
| React Router DOM | Routing                 |
| Axios            | HTTP requests           |
| Tailwind CSS     | Styling                 |
| Framer Motion    | Animations              |
| Lucide React     | Icons                   |
| JavaScript / JSX | Application development |

---

# 3. Frontend Architecture

The V1 frontend follows a modular architecture.


src/
├── assets/
├── components/
├── pages/
├── features/
├── layouts/
├── routes/
├── services/
├── store/
├── hooks/
├── utils/
└── ...


The application separates:

* Pages
* Reusable components
* API services
* Redux slices
* Async thunks
* Routing
* Utility functions

---

# 4. Application Flow

The general application flow is:


User
 │
 ▼
Landing Page
 │
 ▼
Authentication
 │
 ▼
JWT Authentication
 │
 ▼
Role Detection
 │
 ├─────────────┬─────────────┐
 ▼             ▼             ▼
Student      Recruiter      Admin
Dashboard    Dashboard      Dashboard


---

# 5. User Roles

## Student

The student interface provides:

* Registration
* Email verification
* Login
* Profile management
* Resume upload
* Resume analysis
* Job browsing
* Job recommendations
* Job application
* Application tracking
* Notifications

---

## Recruiter

The recruiter interface provides:

* Recruiter registration
* Recruiter profile
* Job creation
* Job management
* Applicant management
* Candidate shortlisting
* Candidate rejection
* Interview scheduling
* AI candidate ranking
* Notifications

---

## Administrator

The administrator interface provides:

* Recruiter approval
* Recruiter rejection
* User management
* Recruiter management
* Job management
* Placement reports
* System monitoring

---

# 6. Routing

React Router is used for client-side routing.

The application contains role-specific routes.

Conceptually:


/
├── login
├── register
├── verify-email
├── forgot-password
├── reset-password
│
├── /student
│   ├── dashboard
│   ├── profile
│   ├── resume
│   ├── jobs
│   ├── recommendations
│   └── applications
│
├── /recruiter
│   ├── dashboard
│   ├── profile
│   ├── jobs
│   ├── applicants
│   └── rankings
│
└── /admin
    ├── dashboard
    ├── users
    ├── recruiters
    ├── jobs
    └── reports


Protected routes ensure that authenticated users cannot directly access unauthorized application areas.

---

# 7. Authentication

The frontend communicates with the backend authentication APIs.

Authentication features include:

* Registration
* Email verification
* OTP verification
* Login
* Logout
* Forgot password
* Reset password
* Protected routes
* Role-based navigation

The authentication token is maintained on the client and used when communicating with protected APIs.

---

# 8. Redux Toolkit

Redux Toolkit is used for application state management.

The V1 architecture uses Redux slices and asynchronous thunks for API-related operations.

Conceptually:


Component
   │
   ▼
Dispatch AsyncThunk
   │
   ▼
API Service
   │
   ▼
Backend
   │
   ▼
Redux Store
   │
   ▼
Component


Redux is organized around application modules such as:


auth
student
recruiter
jobs
applications
notifications
admin


---

# 9. Axios API Layer

Axios is used for communication with the backend.

The frontend centralizes API configuration such as:

* Base API URL
* Authentication headers
* Request configuration
* Response handling
* Error handling

Typical flow:


React Component
      ↓
Redux AsyncThunk
      ↓
API Service
      ↓
Axios
      ↓
Backend API


---

# 10. Authentication State

The frontend maintains authentication information required for the application session.

The authenticated user contains information such as:


User ID
Name
Email
Role
Authentication Token


The application uses this information to:

* Determine the logged-in user
* Protect routes
* Redirect users to the correct dashboard
* Attach authentication information to API requests

---

# 11. Student Interface

The student dashboard provides access to:


Dashboard
Profile
Resume
Resume Intelligence
Jobs
Smart Job Match
Applications
Notifications


---

## Student Profile

Students can manage:

* Personal information
* Contact information
* Department
* Passing year
* Skills
* Certifications
* Experience
* Profile picture
* Resume

---

# 12. Resume Intelligence

The frontend provides a dedicated interface for displaying AI resume analysis.

The backend returns structured analysis data which is presented through the UI.

The dashboard can display:


Resume Score
Technical Skills
Projects
Experience
Education
Missing Skills
Suggestions
Domain


The frontend does not perform the AI analysis itself.

It consumes the structured result returned by the backend.

---

# 13. Smart Job Match

Students can view recommended jobs based on their resume information.

The recommendation workflow is:


Student Resume Analysis
          │
          ▼
Backend Recommendation Engine
          │
          ▼
Recommended Jobs
          │
          ▼
Frontend


The frontend displays:

* Match score
* Matched skills
* Missing skills
* Recommendation level
* Explanation
* Job information

---

# 14. Job Browsing

Students can browse available jobs.

Job cards/details can contain:

* Job title
* Company
* Description
* Skills
* Salary
* Location
* Job type
* Eligibility
* Deadline
* Application status

Students can navigate to the job details page and apply.

---

# 15. Applications

Students can:

* Apply for jobs
* View submitted applications
* Track application status
* View interview information
* View recruiter updates

Typical application statuses include:


Applied
Shortlisted
Rejected
Interview Scheduled
Selected


---

# 16. Recruiter Interface

The recruiter dashboard provides:


Dashboard
Profile
Jobs
Applicants
Candidate Ranking
Interviews
Notifications


Recruiters can create and manage job postings.

---

# 17. Job Management

Recruiters can:

* Create jobs
* Edit jobs
* Delete jobs
* Open/close jobs
* View applicants

Job forms collect information such as:


Job Title
Description
Skills
Salary
Location
Job Type
Departments
Passing Year
CGPA
Deadline


---

# 18. Applicant Management

Recruiters can view applicants for their jobs.

Applicant information can include:

* Student profile
* Resume
* Skills
* Application status
* Cover letter
* Interview information
* Recruiter remarks

Recruiters can:


Shortlist
Reject
Schedule Interview


---

# 19. AI Candidate Ranking

Recruiters can view candidates ranked by the backend ranking engine.

The frontend presents information such as:


Rank
Candidate
Overall Score
Skill Match
Project Score
Resume Score
Matched Skills
Missing Skills
Recommendation


The ranking calculation happens on the backend.

The frontend is responsible for presenting the result.

---

# 20. Administrator Interface

The administrator dashboard provides centralized management.

Features include:

* Recruiter approval
* Recruiter rejection
* User management
* Recruiter management
* Job management
* Placement reports

The dashboard provides a high-level overview of placement activity.

---

# 21. Notifications

The frontend displays notifications generated by the backend.

Notifications can be related to:

* New jobs
* Application status
* Interview scheduling
* Recruiter approval

Socket.IO can be used to receive real-time notification events.

---

# 22. Reusable Components

The frontend uses reusable components to reduce duplication.

Examples include:


Buttons
Inputs
Modals
Cards
Tables
Dropdowns
Navbar
Sidebar
Dashboard Cards
Loaders
Empty States
Error States


Feature-specific components are kept close to their respective modules where appropriate.

---

# 23. Styling

Tailwind CSS is used for styling.

The UI follows a component-based design approach with:

* Responsive layouts
* Reusable utility classes
* Dashboard components
* Cards
* Tables
* Forms
* Navigation components
* Modal interfaces

---

# 24. Animations

Framer Motion is used to provide UI animations.

Examples include:

* Page transitions
* Component entrance animations
* Modal animations
* Dashboard interactions
* Hover effects

Animations are used to improve the user experience without interfering with core functionality.

---

# 25. Icons

Lucide React is used for application icons.

Icons are used throughout:

* Navigation
* Buttons
* Dashboard cards
* Forms
* Notifications
* Actions

---

# 26. Assets

Static assets are stored in the frontend assets directory.

Examples include:


logo.png
college.png
graduation.png


Company/recruiter assets may also be used for UI presentation.

---

# 27. API Communication

The frontend communicates with the backend through REST APIs.

Example flow:


Student
   │
   ▼
Job Page
   │
   ▼
Redux AsyncThunk
   │
   ▼
Axios
   │
   ▼
GET /api/jobs
   │
   ▼
Backend
   │
   ▼
MongoDB


For mutations:


Component
   ↓
AsyncThunk
   ↓
Axios POST/PUT/DELETE
   ↓
Backend
   ↓
Redux State Update
   ↓
UI Update


---

# 28. Environment Variables

The frontend requires environment configuration for the backend API.

Example:

env
VITE_API_BASE_URL=


Environment files containing sensitive configuration should not be committed to Git.

---

# 29. Local Development

### Clone the repository

bash
git clone <repository-url>
cd frontend


### Install dependencies

bash
npm install


### Configure environment variables

Create:


.env


and configure the backend API URL.

### Start development server

bash
npm run dev


The Vite development server will start the frontend locally.

---

# 30. Production Build

Create a production build using:

bash
npm run build


Preview the production build locally using the configured Vite preview command.

---

# 31. Deployment

The frontend is deployed using **Vercel**.

The production frontend communicates with the deployed backend through the configured API base URL.

Deployment architecture:


User
 │
 ▼
Vercel
 │
 ▼
React Application
 │
 ▼
Backend API
 │
 ▼
Render
 │
 ▼
MongoDB Atlas / Cloudinary


---

# 32. Frontend Security

The frontend implements:

* Protected routes
* Role-based navigation
* Authentication state management
* API authentication
* Form validation
* Controlled access to role-specific pages

The frontend should never be treated as the final authorization layer. Backend authorization remains responsible for enforcing access control.

---

# 33. Performance

Current frontend performance considerations include:

* Vite-based build system
* Reusable components
* Modular feature structure
* Efficient Redux state management
* API-based data loading
* Animation management
* Responsive layouts

---

# 34. Current V1 Architecture

The complete V1 frontend flow can be summarized as:


                    React + Vite
                         │
              ┌──────────┼──────────┐
              │          │          │
              ▼          ▼          ▼
           Student    Recruiter    Admin
              │          │          │
              └──────────┼──────────┘
                         ▼
                   React Router
                         │
                         ▼
                    Redux Store
                         │
                         ▼
                   Async Thunks
                         │
                         ▼
                       Axios
                         │
                         ▼
                    Backend API


---

# 35. AI Feature Architecture

The frontend does not directly communicate with Gemini.

Instead:


Frontend
   │
   ▼
Backend API
   │
   ▼
AI Service
   │
   ▼
Gemini
   │
   ▼
Structured Result
   │
   ▼
Backend
   │
   ▼
Frontend


This keeps AI credentials and business logic on the server.

---

# 36. V1 Project Goals

The V1 frontend focuses on providing:

* A complete campus placement workflow
* Role-specific dashboards
* Secure authentication
* Resume management
* AI resume insights
* Job recommendations
* Candidate ranking
* Application management
* Recruiter management
* Notifications
* Responsive web UI

---

# 37. Planned V2 Improvements

The current frontend represents **PMS V1**.

Planned V2 improvements include:

* JSX → TSX migration
* TanStack Query
* Improved API abstraction/HBA
* Removal of unnecessary AsyncThunks
* `useDebounce` for search
* Refined feature-based architecture
* Improved responsive/mobile UI
* Light/Dark/System theme
* Multi-institution UI
* Google OAuth
* Institution onboarding
* System Administrator dashboard
* CGPA/SGPA eligibility UI
* Online aptitude and coding tests
* Virtual interview integration
* Improved AI recommendation UI
* Improved candidate ranking UI
* System analytics
* Institution subscription management

V2 will be implemented on top of the existing V1 functionality rather than changing the fundamental purpose of the application.
