# Gymetric Multi-Tenancy

This document explains the high-level multi-tenant model used by Gymetric.

Gymetric is designed as one shared platform that can support multiple fitness businesses while keeping each gym's users, records, branding, and workflows logically separated.

Sensitive production configuration, exact security rules, and internal identifiers are intentionally excluded.

## Why Gymetric Is Multi-Tenant

Gymetric is intended to support more than one fitness business without requiring a completely separate application and backend for every gym.

A multi-tenant architecture allows Gymetric to provide:

- One shared web platform
- One shared mobile application
- Shared infrastructure and backend services
- Gym-specific users and data
- Gym-specific branding and settings
- Independent operational workflows for each business

This approach makes it possible to improve the platform once while delivering those improvements across every supported gym.

The goal is not to make every gym operate identically. Gymetric provides a shared foundation while allowing each tenant to maintain its own identity, members, schedules, staff, and settings.

## What `gymId` Represents

A `gymId` identifies the tenant that owns or controls a record.

A tenant is typically one fitness business using Gymetric.

Examples of tenant-scoped records may include:

- Users
- Members
- Classes
- Bookings
- Attendance records
- Announcements
- Feed posts
- Settings
- Branding information

A simplified record may look like:

```json
{
  "gymId": "ignite-fitness-la",
  "memberId": "member-example",
  "status": "active"
}
```

The `gymId` allows the application to determine which gym a record belongs to and which users should be able to access it.

## How Users Belong to Gyms

Authenticated users are associated with tenant-specific application data.

A user record may include information such as:

- Authentication user ID
- Gym ID
- Role
- Member or staff profile ID
- Account status

A simplified example may look like:

```json
{
  "userId": "user-example",
  "gymId": "ignite-fitness-la",
  "role": "member",
  "status": "active"
}
```

The user's `gymId` determines which tenant experience and data should be loaded.

The user's role determines which actions and interfaces should be available within that tenant.

Current role-based experiences include:

- Owner
- Administrator
- Coach
- Member

A member and an owner may belong to the same gym while receiving very different interfaces and permissions.

## How Bookings Are Scoped

Bookings are associated with a specific gym.

A booking may include:

- Gym ID
- Member ID
- Class ID
- Booking status
- Creation timestamp

A simplified booking record may look like:

```json
{
  "gymId": "ignite-fitness-la",
  "memberId": "member-example",
  "classId": "class-example",
  "status": "booked"
}
```

When the application loads bookings, it should only retrieve records associated with the active tenant.

This prevents a booking created for one gym from appearing in another gym's roster, schedule, or member history.

## How Attendance Is Scoped

Attendance records are also associated with a specific gym.

A simplified attendance record may include:

```json
{
  "gymId": "ignite-fitness-la",
  "memberId": "member-example",
  "classId": "class-example",
  "status": "present"
}
```

Tenant scoping ensures that:

- Coaches only manage attendance for their gym
- Members only see their own attendance within the correct gym
- Owners only view reports belonging to their business
- Attendance summaries do not combine unrelated tenant data

Bookings and attendance may reference the same class and member records, but each record still includes tenant context.

## How Branding Changes by Tenant

Gymetric supports tenant-specific presentation so each gym can maintain its own identity within the shared platform.

Tenant branding may include:

- Gym name
- Logo
- Primary color
- Accent color
- Contact information
- Location details
- Feature settings
- Member-facing text

When the active gym changes, the application can load the appropriate tenant configuration and apply it across the web or mobile experience.

This allows one Gymetric application to serve different businesses without making every tenant look identical.

Branding does not replace tenant isolation. It is a presentation layer built on top of the tenant data model.

## Preventing Cross-Gym Data Access

Cross-gym data access should be prevented at multiple layers.

### Authentication

Firebase Authentication identifies the current user.

Authentication alone does not determine which tenant records the user should access.

### Tenant association

The application associates the authenticated user with a `gymId`.

That tenant association is used when loading records and evaluating permitted actions.

### Query scoping

Application queries should include the active tenant identifier where appropriate.

For example, a class query should retrieve classes belonging only to the current gym.

### Backend validation

Trusted backend operations should verify that the requesting user is authorized to access the tenant referenced by the request.

Client-provided tenant identifiers should not be trusted without validation.

### Security rules

Firestore security rules should restrict access based on authenticated identity, tenant membership, role, and record ownership where appropriate.

Exact production rules are intentionally excluded from this public repository.

### Shared service boundaries

Tenant and role checks should be centralized where possible rather than recreated independently throughout the application.

This reduces the risk of inconsistent access behavior between web and mobile workflows.

## Current Migration Progress

Gymetric originally included workflows that were not fully tenant-aware.

The platform is being migrated toward a consistent multi-tenant model.

Current progress includes:

- Adding `gymId` to core user records
- Adding tenant context to bookings
- Adding tenant context to attendance
- Adding tenant context to feed and communication records
- Introducing tenant-aware hooks and services
- Applying tenant-specific branding
- Updating member and coach workflows to use the active gym
- Moving toward Firestore as the primary source of truth

Work still in progress includes:

- Centralizing tenant checks
- Reviewing all remaining legacy queries
- Strengthening role-based authorization
- Expanding tenant-aware administrative workflows
- Improving automated testing for tenant isolation
- Completing migration away from older data sources
- Auditing web and mobile behavior for inconsistent scoping

## Current Limitations

The multi-tenant model is still evolving.

Current limitations may include:

- Some older workflows requiring migration
- Tenant checks existing in more than one layer
- Incomplete automated coverage for isolation behavior
- Administrative workflows that still need broader multi-gym support
- Data models that may change as membership and billing features expand

These limitations are documented to distinguish the current implementation from the intended final architecture.

## Long-Term Direction

The long-term goal is for tenant context to be a consistent part of every Gymetric workflow.

A user should only be able to access:

- Gyms they belong to
- Records owned by those gyms
- Actions allowed by their role
- Member-specific records they are authorized to view

Gymetric should support many fitness businesses through one connected platform without allowing one tenant's operational data to leak into another tenant's experience.
