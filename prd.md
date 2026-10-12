# Product Requirements Document (PRD)

## CollegeConnect --- Campus Events & Engagement Platform

**Document status:** MVP specification\
**Version:** 2.0\
**Last updated:** 12 October 2026\
**Product type:** Responsive web application

------------------------------------------------------------------------

## 1. Executive Summary

CollegeConnect is a centralized platform that helps students discover
and register for campus events while giving authorized organizers a
reliable way to publish events and manage attendance.

The MVP focuses on one complete, dependable journey: **an organizer
publishes an event → a student discovers it → the student registers →
the organizer views the attendee list**.

The product should be simple to use on mobile and desktop, visually
polished for a live demonstration, and structured so features such as QR
check-in and calendar integration can be added later without rebuilding
the core.

## 2. Problem Statement

Campus event information is often spread across social media, messaging
groups, posters, and word of mouth. Students can miss relevant events or
struggle to find accurate dates, venues, and registration details.
Organizers may rely on manual spreadsheets to track registrations and
seat limits.

CollegeConnect brings event discovery and registration into one
searchable, consistent experience.

## 3. Product Goals

### 3.1 MVP goals

-   Provide a single, browsable feed of upcoming campus events.
-   Let users view complete event information and register or cancel
    registration.
-   Prevent duplicate registrations and registrations beyond event
    capacity.
-   Let authorized organizers create, edit, publish, and cancel events.
-   Give organizers an attendee list and basic registration counts.
-   Persist event and registration data in MongoDB.
-   Provide clear loading, empty, success, and error states.

### 3.2 Success criteria

-   A visitor can find an event and open its details without creating an
    account.
-   A student can register for an event and see it in their
    registered-events list.
-   A student cannot register twice for the same event.
-   The API rejects registration when an event is full or cancelled.
-   An organizer can create an event and see it appear in the public
    feed.
-   Event and registration records remain available after a server
    restart.
-   The main student flow works on common desktop and mobile screen
    sizes.

### 3.3 Out of scope for the initial MVP

-   Payments and paid tickets.
-   Native mobile apps.
-   Complex university ERP integrations.
-   Multi-university tenancy.
-   AI-based event recommendations.
-   Real-time chat or social feeds.
-   Production-grade email verification and password recovery unless
    time permits.

## 4. Users and Roles

### 4.1 Visitor

-   Browse and search published upcoming events.
-   Filter events by category and date.
-   View event details.
-   Must sign in to register.

### 4.2 Student

-   All visitor capabilities.
-   Register for an event and cancel their registration.
-   View their registrations.
-   See registration status and event capacity information.

### 4.3 Organizer

-   Create, edit, publish, and cancel events they are authorized to
    manage.
-   View registration totals and attendee lists for their events.
-   Export attendee data to CSV if included in the MVP time budget.

### 4.4 Administrator

-   Manage users and organizer permissions.
-   Review or remove inappropriate events.
-   Administrator controls may be deferred from the first demo.

**Authorization rule:** Roles must be checked on the server. Hiding a
button in the frontend is not sufficient authorization.

## 5. Core User Stories

-   As a visitor, I want to browse upcoming events so I can decide what
    interests me.
-   As a student, I want to filter by category and date so I can find
    relevant events quickly.
-   As a student, I want to register and view my registrations so I can
    keep track of events I plan to attend.
-   As a student, I want clear feedback when an event is full or
    registration is closed.
-   As an organizer, I want to create and update event details so
    students see accurate information.
-   As an organizer, I want to view attendees and remaining capacity so
    I can plan the event.
-   As an administrator, I want role-based controls so event management
    remains restricted to authorized users.

## 6. MVP Functional Requirements

### 6.1 Event discovery

-   Show published events, ordered by start date.
-   Search by event title and, where practical, description or venue.
-   Filter by category: Technical, Cultural, Sports, Workshop, Guest
    Lecture, Other.
-   Filter by date range: Today, This Week, Upcoming.
-   Display title, category, date/time, venue, banner or fallback image,
    and available seats.
-   Show loading, no-results, and API-error states.

### 6.2 Event details

-   Display title, description, category, start/end time, venue,
    organizer, capacity, and registration status.
-   Validate that the end time is later than the start time.
-   Clearly label cancelled events and events with closed or full
    registration.
-   Use the event's configured timezone consistently.

### 6.3 Registration

-   Require authentication before registering.
-   Allow one active registration per student per event.
-   Reject registrations for cancelled, unpublished, or full events.
-   Allow cancellation where event policy permits.
-   Keep capacity counts accurate when registrations are created or
    cancelled.
-   Return actionable messages for duplicate, full, unavailable, or
    invalid registrations.

### 6.4 Organizer event management

-   Provide a protected organizer dashboard.
-   Create and edit events with title, description, category, start/end
    time, venue, capacity, and optional banner URL.
-   Publish or cancel an event.
-   Allow organizers to manage only events they own, unless an
    administrator grants broader access.
-   Validate all inputs on the server.
-   Do not permanently delete an event with existing registrations
    unless an explicit, safe policy is implemented; prefer cancellation.

### 6.5 Attendee management

-   Show attendee count and remaining capacity.
-   Display the organizer's attendee list with the minimum fields needed
    for event operations.
-   Offer CSV export as a stretch feature.
-   Do not expose attendee lists publicly.

### 6.6 Authentication and authorization

-   Provide sign-up and sign-in for students and approved organizers.
-   Store password hashes, never plaintext passwords.
-   Validate authentication and role permissions on the server.
-   Use secure token handling appropriate to the chosen deployment. For
    a browser app, prefer secure, HttpOnly cookies where feasible; if
    using bearer tokens, document the storage and expiry approach and
    avoid storing sensitive credentials in local storage.
-   Validate email format and optionally restrict accounts to the
    institution's approved email domain once that domain is known.
-   Never hardcode production secrets in source control.

## 7. Non-Functional Requirements

### Security

-   Validate and normalize all user input on the server.
-   Use environment variables for database URLs and secrets.
-   Configure CORS for known frontend origins in deployment.
-   Avoid returning password hashes, tokens, or unnecessary personal
    data in API responses.
-   Add rate limiting and stronger production security headers before
    public production use.
-   Prevent unauthorized event editing and attendee-list access.

### Reliability and data integrity

-   Enforce uniqueness for a student's registration per event at the
    database level.
-   Handle capacity checks safely under concurrent requests.
-   Use appropriate database indexes for event dates, categories, and
    registration lookups.
-   Return consistent HTTP status codes and JSON error formats.
-   Log server errors without logging passwords, tokens, or sensitive
    data.

### Usability and accessibility

-   Responsive layout for mobile, tablet, and desktop.
-   Keyboard-accessible forms and controls with visible focus states.
-   Readable contrast, labels, and useful validation messages.
-   Consistent loading, empty, success, and error states.

### Maintainability

-   Keep frontend and backend in separate folders in one Git repository.
-   Separate routes, controllers/services, models, and middleware as
    complexity grows.
-   Use reusable React components and a shared API helper.
-   Document setup, environment variables, and demo credentials.

## 8. Technical Architecture

### Frontend

-   React with Vite
-   JavaScript for the initial MVP
-   Tailwind CSS for styling
-   Lucide React for icons
-   React Router only if multiple routes are needed
-   Native `fetch` or a small API helper for HTTP requests

### Backend

-   Node.js
-   Express
-   Mongoose
-   REST API under `/api`
-   Zod or equivalent validation may be added for request schemas
-   Authentication middleware and role-based authorization

### Database

-   MongoDB Atlas for hosted development or a local MongoDB instance
-   Suggested collections:
    -   `users`
    -   `events`
    -   `registrations`

### Repository structure

``` text
Project-Seminar/
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── .env.example
│   └── package.json
├── server/
│   ├── src/
│   │   ├── config/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── services/
│   │   └── app.js
│   ├── .env.example
│   └── package.json
├── .gitignore
├── README.md
└── PRD.md
```

The implementation may start with fewer files for speed, then be
refactored into this structure as features are added.

## 9. Data Model (Initial Proposal)

### User

-   `name`: required string
-   `email`: required, normalized and unique
-   `passwordHash`: required for password-based authentication
-   `role`: `student`, `organizer`, or `admin`
-   `createdAt`, `updatedAt`

### Event

-   `title`: required string
-   `description`: required string
-   `category`: enum
-   `startAt`, `endAt`: required dates
-   `venue`: required string
-   `capacity`: positive integer
-   `bannerUrl`: optional URL
-   `status`: `draft`, `published`, or `cancelled`
-   `organizerId`: reference to User
-   `createdAt`, `updatedAt`

### Registration

-   `eventId`: reference to Event
-   `studentId`: reference to User
-   `status`: `registered` or `cancelled`
-   `createdAt`, `updatedAt`
-   Unique active registration policy for each event/student pair

**Capacity integrity:** Registration creation should use a transaction
or another concurrency-safe strategy supported by the selected MongoDB
deployment. If transactions are unavailable, use an atomic capacity
update and rollback-safe registration creation strategy. Do not rely
solely on a frontend seat count.

## 10. REST API Contract (Proposed)

### Health

-   `GET /api/health` --- returns API status.

### Events

-   `GET /api/events` --- list published events; supports `q`,
    `category`, and date filters.
-   `GET /api/events/:id` --- get one published event's details.
-   `POST /api/events` --- create an event; organizer/admin only.
-   `PATCH /api/events/:id` --- update an authorized event;
    organizer/admin only.
-   `PATCH /api/events/:id/cancel` --- cancel an authorized event.
-   `GET /api/events/:id/registrations` --- attendee list; authorized
    organizer/admin only.

### Authentication

-   `POST /api/auth/signup` --- create a student account.
-   `POST /api/auth/login` --- authenticate.
-   `POST /api/auth/logout` --- end the session.
-   `GET /api/auth/me` --- return the current user's safe profile.

### Registrations

-   `POST /api/events/:id/registrations` --- register the current
    student.
-   `DELETE /api/events/:id/registrations/me` --- cancel the current
    student's registration.
-   `GET /api/me/registrations` --- list the current student's
    registrations.

All protected endpoints require server-verified authentication. Request
and response schemas should be documented in the README as they are
implemented.

## 11. Main User Flows

### Student

1.  Open CollegeConnect.
2.  Browse or filter published events.
3.  Open an event details page.
4.  Sign in or create an account if not authenticated.
5.  Register if seats are available.
6.  See confirmation and find the event under My Registrations.
7.  Cancel registration if permitted.

### Organizer

1.  Sign in with an approved organizer account.
2.  Open the organizer dashboard.
3.  Create an event as a draft or publish it.
4.  Review the event in the public feed.
5.  View attendee totals and the attendee list.
6.  Update event details or cancel the event when necessary.

## 12. UX and Visual Direction

-   Contemporary, clean campus-product aesthetic.
-   Strong page hierarchy and readable typography.
-   Event cards with category labels, dates, venue, and capacity.
-   Responsive navigation with clear access to Explore, My
    Registrations, and Organizer Dashboard based on role.
-   Use Lucide icons consistently and provide text labels where an icon
    alone may be unclear.
-   Use a consistent spacing, color, border, and radius system.
-   Use meaningful loading skeletons and friendly empty states.
-   Avoid decorative complexity that slows down the live MVP.

## 13. MVP Prioritization

### P0 --- Required for the first working full-stack demo

1.  Project setup and environment configuration.
2.  API health endpoint.
3.  MongoDB connection.
4.  Event model and event-list endpoint.
5.  Event feed and event details.
6.  Student registration endpoint with duplicate and capacity checks.
7.  Organizer event creation and attendee list.
8.  Basic authentication and server-side role checks.
9.  End-to-end testing of the core flow.

### P1 --- Add after P0 is reliable

-   Search and category/date filters.
-   Registration cancellation.
-   Responsive polish and robust empty/error states.
-   CSV export.
-   Event banner uploads.
-   Deployment and environment configuration.

### P2 --- Future enhancements

-   QR ticket generation and scanning/check-in.
-   Google/Apple Calendar integration.
-   Email notifications and institutional email verification.
-   Post-event feedback and galleries.
-   Admin analytics and event engagement metrics.
-   Saved events and personalized recommendations.

## 14. Testing and Acceptance Checklist

-   [ ] API health endpoint returns a successful response.
-   [ ] Published events load from MongoDB in the frontend.
-   [ ] Draft and cancelled events do not appear in the public feed.
-   [ ] Invalid event data is rejected with clear validation errors.
-   [ ] Unauthenticated users cannot register or create events.
-   [ ] Students cannot access organizer-only endpoints.
-   [ ] Organizers cannot edit events they do not own.
-   [ ] Duplicate registrations are rejected.
-   [ ] Registrations cannot exceed capacity, including under concurrent
    requests.
-   [ ] Cancellation updates available capacity correctly.
-   [ ] Attendee data is visible only to authorized users.
-   [ ] Data persists after restarting the backend.
-   [ ] The main flow works on desktop and mobile.
-   [ ] `.env` files and secrets are excluded from Git.

## 15. Risks and Mitigations

  -----------------------------------------------------------------------
  Risk                                Mitigation
  ----------------------------------- -----------------------------------
  Scope is too large for the seminar  Build the core
                                      event-to-registration journey
                                      first; defer P2 features.

  MongoDB setup delays progress       Test the API and UI with seed data
                                      temporarily, then connect MongoDB.

  Capacity race conditions            Enforce capacity safely on the
                                      server and test concurrent
                                      registrations.

  Unauthorized event edits            Verify identity, role, and event
                                      ownership on every protected
                                      endpoint.

  Deployment environment differs from Document environment variables and
  local                               test production build before
                                      deployment.

  QR codes or email verification      Keep them out of the initial MVP
  consume time                        and add only after core acceptance
                                      tests pass.
  -----------------------------------------------------------------------

## 16. Demo Plan

The live demonstration should follow this sequence:

1.  Open the public event feed.
2.  Search or filter for an event.
3.  Open its details and show capacity.
4.  Sign in as a student and register.
5.  Show the registration in My Registrations.
6.  Sign in as an organizer and create or publish an event.
7.  Show the event in the public feed and inspect its attendee list.
8.  Demonstrate that a duplicate registration or full event is rejected.

Use clearly identified demo accounts and sample events. Do not use real
student data in a public demonstration.

## 17. Assumptions and Open Questions

-   The first release serves one college or university.
-   The approved university email domain is not yet specified.
-   Organizers are approved by an administrator or seeded for the demo;
    public self-assignment of the organizer role is not allowed.
-   Event times use the institution's local timezone.
-   Payments are not required.
-   The initial release is a responsive web application, not a native
    app.
-   Deployment target is Vercel, subject to validating the Express API
    deployment approach and MongoDB connectivity.

## 18. Definition of Done

The MVP is complete when a student can discover a published event,
register once, and see the registration; an authorized organizer can
create and publish an event and view its attendees; data persists in
MongoDB; and the main journey passes the acceptance checklist without
exposing protected operations or attendee information.
