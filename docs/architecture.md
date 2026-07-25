# Gymetric Architecture

This document provides a high-level overview of Gymetric's current application
architecture.

Gymetric is actively being developed, and some implementation details may
change as the platform grows. Production credentials, internal identifiers,
and sensitive configuration are intentionally excluded.

## 1. System Overview

Gymetric is a multi-tenant fitness business platform with separate web and
mobile experiences connected to a shared Firebase backend.

The main components are:

- A Next.js web application for owners, administrators, and coaches
- An Expo and React Native mobile application for members and coaches
- Firebase Authentication for user identity and access
- Cloud Firestore for application data
- Cloud Functions for trusted backend operations and integrations
- Tenant-scoped records using `gymId`

The web and mobile applications use the same backend data while presenting
different interfaces based on the user's role and gym.

![Gymetric system overview](../diagrams/system-overview.png)

## 2. Web Application Responsibilities

The Next.js web application acts as Gymetric's main control center.

Its responsibilities currently include:

- Displaying operational summaries
- Managing classes and schedules
- Viewing and managing members
- Managing bookings and class rosters
- Tracking attendance
- Supporting coach workflows
- Publishing announcements
- Managing gym-level settings
- Switching between owner, coach, and member views during development

The web application is designed around operational workflows rather than
independent pages. Information from members, classes, attendance, programming,
and communications is intended to work together.

## 3. Mobile Application Responsibilities

The Expo and React Native application provides the member-facing and
coach-facing mobile experience.

Its responsibilities include:

- Viewing the member dashboard
- Browsing available classes
- Booking and cancelling reservations
- Viewing upcoming bookings
- Viewing attendance history
- Reading gym announcements and feed posts
- Accessing training information
- Supporting coach tools
- Viewing and updating account information

The mobile application uses tenant-specific branding and content so each gym
can provide its own member experience through the shared Gymetric platform.

## 4. Firebase Authentication

Firebase Authentication is used to identify users and support authenticated
access across the web and mobile applications.

A user's account is associated with application data that may include:

- User ID
- Gym ID
- Role
- Member profile
- Account status

Roles currently include experiences such as:

- Owner
- Administrator
- Coach
- Member

Authentication confirms who the user is. Application-level role and tenant
checks determine what data and workflows the user should be able to access.

Exact production security rules and authentication configuration are not
included in this public repository.

## 5. Firestore Collections

Cloud Firestore is used as the primary application database.

Current or planned collections include:

- `users`
- `members`
- `classes`
- `bookings`
- `attendance`
- `feedPosts`
- `announcements`
- `chatRooms`

Most operational records include a `gymId` field.

A simplified record may look like:

```json
{
  "gymId": "example-gym",
  "memberId": "example-member",
  "status": "active"
}
```

The `gymId` allows the application to separate records belonging to different fitness businesses while using one shared platform.

The exact production schema is still evolving as workflows are refined.

## 6. Cloud Functions

Cloud Functions are used for operations that should not rely entirely on client-side code.

Their responsibilities may include:

- Validating trusted operations
- Processing backend workflows
- Connecting external services
- Handling data imports
- Supporting scheduled operations
- Enforcing business rules
- Performing administrative tasks

As Gymetric grows, more business logic will move into shared backend services to reduce duplication between the web and mobile applications.

## 7. Data Synchronization

The web and mobile applications read from and write to the same Firestore backend.

This allows updates such as the following to appear across experiences:

- A class created in the control center becomes available to members
- A member booking updates the class roster
- A coach attendance action updates the member's history
- An announcement published by the gym appears in the member experience
- Tenant branding and settings affect the appropriate gym experience

Shared services and data models are used where possible to keep behavior consistent across the platform.

## 8. Current Architectural Limitations

Gymetric is still under active development.

Current limitations include:

- Some business rules still exist in multiple parts of the application
- Some workflows are still transitioning from earlier data sources
- The Firestore schema is continuing to mature
- Role and permission handling still needs further centralization
- Automated testing is not yet comprehensive
- Monitoring and observability are still limited
- Some mobile and web workflows are not yet fully synchronized
- Payment and membership billing systems are not yet complete
- Multi-gym administration is still being expanded

These limitations are documented intentionally to distinguish current functionality from planned architecture.

## 9. Planned Improvements

Planned architectural improvements include:

- Centralizing shared business rules
- Strengthening tenant isolation
- Expanding role-based access control
- Completing the migration to Firestore as the primary source of truth
- Adding automated tests
- Adding CI/CD checks
- Improving monitoring and error reporting
- Introducing membership and payment workflows
- Supporting structured imports from existing gym platforms
- Expanding multi-gym administration
- Improving backend service boundaries
- Reducing duplicated web and mobile logic
- Adding more detailed audit and activity tracking

The long-term goal is for Gymetric to function as a connected operating system for fitness businesses rather than a collection of isolated management tools.
