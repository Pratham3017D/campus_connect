# Product Requirements Document (PRD): CollegeConnect

## 1. Product Overview
CollegeConnect is a centralized web platform designed to streamline the discovery, registration, and management of university events (Technical, Cultural, Sports, and Workshops). It bridges the communication gap between student organizations and the general student body, fostering better campus engagement.

## 2. Target Audience
* **Students:** Looking to easily discover events on campus, track their schedules, and register.
* **Club Organizers / Committees:** Needing an organized platform to promote upcoming events, manage capacities, and track registrations.
* **University Administration:** Overseeing campus activities and monitoring overall student engagement.

## 3. Core Features (MVP Scope)
### Authentication & Profiles
* JWT-based secure login/signup.
* University email (`.edu` or specific domain) verification to keep the platform exclusive.
* User dashboard showing "My Tickets" and "My Saved Events".

### Event Feed / Discovery Dashboard
* Search bar and category filters (Tech, Sports, Cultural, Guest Lectures).
* Chronological sorting (Today, This Week, Upcoming).
* Visual event cards utilizing **Lucide Icons** for indicating categories, time, and location.

### Event Details Page
* Comprehensive view including: Event banner image, rich-text description, exact date/time, venue, organizer contact info.
* Dynamic "Register/Unregister" button.
* "Add to Calendar" integration (1-click sync with Google/Apple Calendar).

### Registration & Capacity Management
* Enforce attendee limits (e.g., "Only 50 seats left").
* QR Code Ticketing: Generate unique QR codes upon registration for easy check-ins.
* Organizers can view a real-time list of registered attendees and export it to a CSV file.

### Organizer Dashboard
* Role-Based Access Control (RBAC) ensuring only authorized users can create events.
* CRUD operations: Create, Read, Update, and Delete events.
* Image upload functionality for event banners (e.g., via Cloudinary).
* Post-event gallery and feedback management.

## 4. Technical Stack
| Component | Technology | Role |
| :--- | :--- | :--- |
| **Frontend UI** | React + Vite | Fast, modular, and component-driven user interface. |
| **Styling** | Tailwind CSS | Utility-first, responsive design with minimal CSS bloat. |
| **Iconography** | Lucide React | Clean, consistent SVG icon system. |
| **Backend API** | Node.js + Express | RESTful API creation and business logic routing. |
| **Database** | MongoDB (Mongoose) | Flexible NoSQL document storage for events and users. |
| **Version Control**| Git + GitHub | Collaborative tracking and source code management. |
| **Hosting** | Vercel | CI/CD deployment for both the React frontend and Node API. |

## 5. User Flows
* **Student Flow:** Land on Home -> Browse/Filter Events -> Click Event -> Log In -> Click Register -> Receive QR Ticket -> See Event in Dashboard.
* **Organizer Flow:** Log In -> Access Organizer Dashboard -> Click "Create Event" -> Fill Form (Title, Date, Category, Image) -> Publish -> Monitor Live Registrations -> Scan QR Codes at Event.

## 6. Technical Architecture & Deployment Strategy
The project will use a monorepo approach, keeping frontend and backend logic separated but housed within the same Git repository.

### Repository Structure
```text
CollegeConnect/
├── client/          # React + Vite + Tailwind + Lucide
├── server/          # Node.js + Express + MongoDB
├── .gitignore
└── README.md
```

### Deployment (Vercel)
* **Frontend:** Deployed by targeting the `client` directory. Vercel will auto-detect Vite.
* **Backend:** Deployed by targeting the `server` directory, utilizing a `vercel.json` configuration file to run the Express API in Vercel's serverless environment. Environment variables (`MONGO_URI`, `JWT_SECRET`) must be configured in the Vercel dashboard prior to deployment.