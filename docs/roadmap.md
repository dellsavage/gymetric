# Gymetric Development Roadmap

This roadmap summarizes Gymetric's current state and planned direction.

It is intentionally organized as `Current`, `Next`, and `Later` so planned work is not presented as completed functionality.

Priorities may change as the product is tested with real gym workflows.

## Current

The current phase is focused on strengthening the foundation of the web and mobile applications.

### Product capabilities

- Class scheduling
- Member management
- Booking workflows
- Attendance tracking
- Coach workflows
- Announcements and member communication
- Member-facing mobile experience
- Tenant-specific branding and data
- Owner, coach, and member-oriented interfaces

### Architecture

- Next.js web application
- Expo and React Native mobile application
- Firebase Authentication
- Cloud Firestore
- Cloud Functions
- Tenant-scoped records using `gymId`
- Shared backend data across web and mobile

### Current engineering work

- Improving tenant consistency
- Centralizing shared business rules
- Refining status and date handling
- Migrating remaining workflows to Firestore
- Reducing duplicated logic
- Improving web and mobile synchronization
- Stabilizing role-based experiences
- Documenting system architecture
- Reviewing data and security boundaries

### Current limitations

- Some workflows still rely on earlier data assumptions
- Automated test coverage is not yet comprehensive
- Monitoring and observability are limited
- Membership billing is not complete
- Multi-gym administration is still being expanded
- Some permissions and tenant checks need further centralization
- Import workflows are not yet fully developed

## Next

The next phase will focus on making the existing product more reliable, connected, and usable for day-to-day gym operations.

### Data and architecture

- Complete Firestore migration
- Strengthen tenant isolation
- Centralize role and permission checks
- Standardize shared data models
- Improve backend service boundaries
- Reduce duplicated web and mobile logic
- Add clearer error handling
- Add structured audit and activity records

### Quality and delivery

- Add automated tests for core workflows
- Add tenant-isolation tests
- Add booking and attendance tests
- Introduce CI checks through GitHub Actions
- Add linting and type-checking gates
- Improve deployment verification
- Add monitoring and error reporting

### Product workflows

- Improve class and roster management
- Expand coach control-center workflows
- Improve member booking management
- Improve attendance history
- Expand announcements and communication tools
- Improve owner dashboards and reporting
- Refine settings and tenant configuration
- Improve mobile and web consistency

### Data migration and onboarding

- Create structured member imports
- Create class and schedule imports
- Support migration from spreadsheets
- Define import validation and error reporting
- Prepare import pathways for existing gym platforms

## Later

The later phase represents Gymetric's broader operating-system vision.

These items are planned directions rather than completed features.

### Memberships and finance

- Membership plan management
- Subscription billing
- Payment processing
- Failed-payment workflows
- Discounts and promotional offers
- Invoices and receipts
- Revenue reporting
- Membership status automation

### Staff and operations

- Staff availability
- Coach assignments
- Pay-rate configuration
- Payroll support
- Task management
- Internal operational notes
- Expanded permissions and staff roles

### CRM and growth

- Lead tracking
- Trial-member workflows
- Follow-up tasks
- Forms and waivers
- Lead-to-member conversion tracking
- Automated communication workflows
- Retention and engagement reporting

### Reporting and analytics

- Attendance trends
- Membership trends
- Revenue dashboards
- Class utilization
- Coach activity
- Member engagement
- Churn indicators
- Multi-location reporting

### Platform expansion

- Multi-location management
- Gym-level feature configuration
- Expanded tenant administration
- Public APIs
- Integration support
- Webhooks
- Import tools for existing platforms
- Broader mobile capabilities

### Infrastructure and security

- Expanded audit logging
- Improved observability
- Backup and recovery procedures
- Stronger service isolation
- More comprehensive security testing
- Performance monitoring
- Scalable background processing
- Formalized incident and recovery workflows

### AI-assisted workflows

Potential AI-assisted features may eventually support:

- Operational summaries
- Member communication drafts
- Schedule analysis
- Reporting explanations
- Data-import assistance
- Owner-facing recommendations

AI features will only be added where they improve a real workflow. They are not a substitute for reliable core operations.

## Product Direction

Gymetric began with scheduling and member workflows, but the long-term goal is broader.

The platform is being designed as a connected operating system where:

```text
Classes
→ Bookings
→ Attendance
→ Member History

Members
→ Memberships
→ Payments
→ Communications
→ Reports

Coaches
→ Availability
→ Assignments
→ Attendance
→ Payroll

Leads
→ Forms
→ Follow-ups
→ Trials
→ Memberships
```

Each module should support other parts of the business rather than existing as an isolated feature.

The roadmap will continue to evolve as current workflows are completed, tested, and validated.
