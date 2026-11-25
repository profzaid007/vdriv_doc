# RydCare Platform

## Software Requirements Specification (SRS)

**Version:** 1.0
**Status:** Draft / Development Baseline
**Primary Application:** RydCare
**Backend:** Django Ninja
**Database:** PostgreSQL
**Web Application:** Next.js
**Driver Application:** Flutter
**File Storage:** S3-compatible object storage
**Accounting:** TallyPrime via REST/JSON integration layer / Zoho Books
**Ticketing:** Zoho Desk
**Maps / Tracking:** GPS + mapping provider / existing tracking infrastructure

---

# 1. Introduction

## 1.1 Purpose

RydCare is a business operations platform for managing driver-service operations across multiple companies, clients, branches and service locations.

The system will centralize:

* Client management
* Client onboarding
* Client branches and contacts
* Quotations and agreements
* Driver onboarding
* Driver documents
* Driver availability and attendance
* Bookings
* Driver assignment
* Trip execution
* GPS tracking
* Tariff/rate management
* Driver earnings and settlements
* Client billing information
* Invoice synchronization with Tally
* Receivables and payment follow-ups
* Client and driver support tickets through Zoho Desk
* Driver self-service through the mobile application

RydCare will **not attempt to replace accounting software**. Financial accounting, GST, payroll accounting and statutory financial reporting will remain in TallyPrime.

RydCare will also **not attempt to become a full CRM or helpdesk platform**. Zoho Desk will provide the support/ticketing backend.

---

# 2. Product Vision

The system should provide one operational platform for the business while integrating specialized external systems for accounting and support.

```text
                         RYDCare
                    Django + PostgreSQL
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
    CLIENTS             DRIVERS              JOBS
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
        TALLY / CONNECTOR          ZOHO DESK
        Finance / Payroll          Support / Tickets
              │                         │
       GST / Invoices / AR         Client Support
       Payments / Payroll         Driver Support
```

RydCare should be the **operational source of truth**.

Tally should be the **financial/accounting source of truth**.

Zoho Desk should be the **support/ticketing source of truth**.

---

# 3. Goals

## 3.1 Primary Goals

1. Replace manual operational processes with a centralized platform.
2. Manage multiple companies and GST registrations.
3. Manage clients and their branches.
4. Manage both salaried and job-based drivers.
5. Automate booking and driver assignment workflows.
6. Track trips and driver activity.
7. Calculate driver earnings based on completed jobs.
8. Generate invoice-ready billing data.
9. Synchronize financial transactions with Tally.
10. Display outstanding receivables inside RydCare.
11. Provide payment follow-up workflows.
12. Provide client and driver support through Zoho Desk.
13. Allow drivers to create and track support tickets from the Driver App.
14. Store documents securely using object storage.
15. Provide dashboards and operational reports.

---

# 4. Out of Scope

The following should not be recreated unnecessarily inside RydCare:

### Accounting

* General ledger
* GST accounting
* Bank reconciliation
* Financial statements
* Statutory accounting
* Complete payroll accounting

These remain in Tally.

### Helpdesk

* Ticket engine
* SLA engine
* Support-agent management
* Knowledge base

These remain in Zoho Desk.

RydCare will integrate with Zoho Desk rather than duplicate its functionality.

---

# 5. Users and Roles

## 5.1 System Administrator

Can:

* Configure companies
* Manage users
* Configure permissions
* Manage system settings
* Manage integrations
* View all data
* Manage audit logs

## 5.2 Operations Manager

Can:

* Manage clients
* Manage branches
* Manage drivers
* Create bookings
* Assign drivers
* Monitor trips
* Manage operational incidents
* View operational reports

## 5.3 Accounts User

Can:

* View billing data
* View invoices
* View receivables
* View payment status
* Trigger payment follow-ups
* View Tally synchronization status

## 5.4 HR / Driver Management User

Can:

* Onboard drivers
* Manage driver documents
* Manage employee/contractor classification
* Manage attendance
* View driver records
* View driver earnings

## 5.5 Support User

Can:

* View support tickets
* Respond to client tickets
* Respond to driver tickets
* Assign tickets
* Escalate tickets
* Monitor SLA

Support operations will primarily occur in Zoho Desk.

## 5.6 Driver

Can use the Driver App to:

* Login
* View profile
* View assigned jobs
* Accept/confirm jobs
* Start trips
* End trips
* Upload required photos/documents
* View earnings
* View ticket history
* Create support tickets
* Reply to support tickets
* Receive notifications

## 5.7 Client User

Future client portal users may:

* View bookings
* View trip history
* View invoices
* View payment status
* Raise support tickets
* Track ticket status
* Manage contacts

---

# 6. High-Level System Architecture

```text
                         USERS
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
     Admin Web App      Driver App      Client Portal
       Next.js           Flutter            Web
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                   RydCare API
                     Django
                           │
                    ┌──────┴──────┐
                    │             │
                    ▼             ▼
                PostgreSQL    Object Storage
                              S3 / Wasabi /
                              Backblaze
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
    Tally Integration      Zoho Desk
       Connector            Integration
          │                   │
          ▼                   ▼
      TallyPrime          Zoho Desk
```

---

# 7. Core Modules

## 7.1 Company Management

The system must support multiple operating companies.

Example:

```text
Company
├── RydCare
├── Nine Star Call Drivers
└── Just Call Drivers
```

Each company may have:

* Legal name
* Trade name
* GSTIN
* PAN
* Address
* Contact information
* Bank information
* Tally company mapping
* Status

The system must maintain company-level data isolation where required.

---

# 8. Client Management

## 8.1 Client Master

A client record must contain:

* Client ID
* Company
* Legal name
* Display name
* GSTIN
* PAN
* Billing address
* Registered address
* Phone
* Email
* Payment terms
* Status
* Notes
* Documents

## 8.2 Client Branches

A client may have multiple branches.

Example:

```text
SVM
├── Kattupakkam
├── Guduvanchery
└── Ekkatuthangal
```

Branch fields:

* Branch ID
* Client
* Branch name
* Address
* Contact person
* Phone
* Email
* GPS coordinates
* Operating instructions
* Active/inactive status

## 8.3 Client Contacts

Each client can have multiple contacts.

Contact types may include:

* Service Manager
* Accounts
* HR
* Purchase
* Management
* Operations

---

# 9. Client Onboarding

The system must support a structured onboarding process.

### Workflow

```text
New Client
    ↓
Client Details
    ↓
GST Details
    ↓
Branch Creation
    ↓
Contacts
    ↓
Documents
    ↓
Quotation
    ↓
Agreement
    ↓
Approval
    ↓
Active Client
```

Required documents may include:

* GST certificate
* PAN
* Agreement
* Purchase order
* Rate agreement
* Other supporting documents

---

# 10. Client Follow-Up Management

RydCare must provide a simple task/follow-up system.

A follow-up must contain:

* Client
* Contact
* Subject
* Assigned employee
* Due date
* Status
* Notes
* Follow-up type
* Next follow-up date

Example:

```text
Quotation Sent
      ↓
Follow-up in 3 days
      ↓
Employee receives task
      ↓
Email / call
      ↓
Update status
      ↓
Schedule next follow-up
```

The system should support:

* Pending follow-ups
* Today's follow-ups
* Overdue follow-ups
* Completed follow-ups
* Follow-up history

---

# 11. Quotation Management

RydCare must support:

* Quotation creation
* Quotation numbering
* Client selection
* Branch selection
* Service selection
* Tariff/rate selection
* Terms and conditions
* GST calculation
* PDF generation
* Document attachment
* Email sending
* Quotation status

Quotation statuses:

```text
Draft
Sent
Under Review
Approved
Rejected
Expired
Converted
```

---

# 12. Agreement and Document Management

Documents must be stored outside PostgreSQL in S3-compatible object storage.

PostgreSQL should store metadata only.

Example:

```text
Document
├── id
├── company_id
├── client_id
├── driver_id
├── document_type
├── file_name
├── object_key
├── uploaded_by
├── uploaded_at
├── expiry_date
└── status
```

Supported document categories:

### Client

* GST certificate
* Agreements
* Quotations
* Purchase orders
* Rate agreements
* Other documents

### Driver

* Driving licence
* ID documents
* PAN
* Bank documents
* Employment documents
* Agreements
* Certificates
* Photos

---

# 13. Driver Management

RydCare must support two primary driver types.

## 13.1 Salaried Employee

```text
Driver
Type = Employee
```

Operational data remains in RydCare.

Payroll accounting remains in Tally.

## 13.2 Job-Based Driver

```text
Driver
Type = Job-Based
```

Driver earnings are calculated from completed jobs.

Example:

```text
Job 1    ₹220
Job 2    ₹220
Job 3    ₹250
Job 4    ₹300
----------------
Total    ₹990
```

---

# 14. Driver Master

Driver record should contain:

* Driver ID
* Name
* Phone
* Email
* Date of birth
* Address
* Company
* Driver type
* Employee/contractor status
* Joining date
* Status
* Driving licence details
* Licence expiry
* Bank details
* Emergency contact
* Documents
* Profile photo
* FCM token
* Notes

Driver statuses:

```text
Active
Inactive
Suspended
On Leave
Blocked
Pending Verification
```

---

# 15. Driver Onboarding

Workflow:

```text
Application
    ↓
Personal Information
    ↓
Documents
    ↓
Licence Verification
    ↓
Bank Details
    ↓
Driver Classification
    ↓
Approval
    ↓
Driver Account
    ↓
Active
```

The system should support document expiry tracking.

---

# 16. Driver Availability

Drivers should have availability information.

Possible states:

```text
Available
Assigned
On Trip
Unavailable
On Leave
Offline
```

Operations staff should be able to view available drivers before assignment.

---

# 17. Booking Management

A booking must contain:

* Booking ID
* Company
* Client
* Branch
* Customer
* Pickup location
* Drop location
* Vehicle details
* Vehicle number
* Requested date/time
* Job type
* Assigned driver
* Tariff
* Driver payable
* Client billable amount
* Status
* Notes
* Attachments

Booking statuses should support the existing operational workflow:

```text
Pending
Assigned
Confirmed
Customer No Response
Ongoing
Customer Cancelled
Customer Postponed
Vehicle Serviced
Company Driver
Completed
```

---

# 18. Driver Assignment

Operations users must be able to:

* View available drivers
* Assign a driver
* Reassign a driver
* Cancel assignment
* View driver workload
* View driver location where available

The system should prevent conflicting assignments.

---

# 19. Trip Management

The driver app must support:

```text
Assigned
   ↓
Confirmed
   ↓
Start Trip
   ↓
Ongoing
   ↓
End Trip
   ↓
Completed
```

The system should record:

* Start timestamp
* End timestamp
* Start GPS
* End GPS
* Distance
* Photos
* Driver
* Vehicle
* Booking
* Trip status

---

# 20. GPS and Location Tracking

The system should support driver location tracking.

Requirements:

* Driver location ingestion
* Last known location
* Trip location
* GPS timestamp
* Map visualization
* Driver status
* Location history where required

Existing Traccar infrastructure may be integrated where appropriate.

---

# 21. Tariff Management

RydCare must maintain configurable tariff rules.

Example:

```text
Distance < 15 km
₹250

15–20 km
₹300

>20 km
₹6/km
```

Tariffs may be configured by:

* Company
* Client
* Branch
* Job type
* Distance
* Route
* Time
* Vehicle type

Tariff calculations must be versioned so historical jobs retain their original rates.

---

# 22. Driver Earnings

For job-based drivers:

```text
Completed Job
       ↓
Driver Rate
       ↓
Driver Earnings
       ↓
Settlement
```

The system must support:

* Job earnings
* Advances
* Deductions
* Adjustments
* Settlement batches
* Payment status

Example:

```text
Driver: Ravi

Completed Jobs: 42
Gross Earnings: ₹9,360
Advance: ₹2,000
Adjustment: ₹300

Net Payable: ₹7,060
```

RydCare is responsible for calculating the operational payable amount.

Accounting/payment recording can be synchronized with Tally.

---

# 23. Billing

RydCare should calculate billable amounts from completed jobs.

Example:

```text
Completed Jobs
      ↓
Tariff calculation
      ↓
Billable records
      ↓
Invoice batch
      ↓
Tally
```

The system should maintain a link between:

```text
Booking
    ↓
Billing Record
    ↓
Tally Invoice
```

---

# 24. Tally Integration

Tally will be the financial/accounting system.

RydCare should communicate with Tally through a dedicated integration layer.

```text
RydCare
    │
    ▼
Tally Connector / REST Wrapper
    │
    ▼
TallyPrime
```

The connector may use:

* TallyPrime JSON integration
* REST wrapper such as `tally-api`
* Custom integration layer if required

The integration layer must not expose Tally directly to the public internet.

---

# 25. Tally Data Synchronization

## RydCare → Tally

The system should be able to synchronize:

* Customers/ledgers
* Sales invoices
* Driver-related financial transactions where applicable
* Other required accounting vouchers

## Tally → RydCare

The system should synchronize:

* Invoice status
* Payment receipts
* Outstanding balances
* Outstanding invoices
* Relevant financial transaction references

Each synchronized record should retain external IDs.

Example:

```text
RydCare Invoice
    │
    ├── rydcare_invoice_id
    ├── tally_company_id
    └── tally_voucher_id
```

---

# 26. Tally Sync Architecture

A local connector/agent should be preferred.

```text
RydCare VPS
     │
     │ HTTPS
     ▼
Tally Integration Service
     │
     │ Local connection
     ▼
TallyPrime
```

The Tally machine should initiate or maintain the secure connection where possible.

Tally's local HTTP port should not be exposed directly to the public internet.

---

# 27. Synchronization Strategy

The integration must support:

### Initial synchronization

Import:

* Companies
* Ledgers
* Customers
* Existing invoices
* Outstanding balances

### Incremental synchronization

Only synchronize changes after the previous successful synchronization.

### Retry

Failed operations must be retried.

### Idempotency

Repeated synchronization must not create duplicate invoices, customers or payments.

### Audit

Every integration operation should have:

* Request ID
* External ID
* Operation
* Status
* Timestamp
* Error message
* Retry count

---

# 28. Receivables Dashboard

RydCare should provide a client receivables view using synchronized Tally data.

Example:

```text
CLIENT RECEIVABLES

KUN Hyundai
₹2,14,779
42 days overdue

ITC Grand Chola
₹1,25,000
18 days overdue

Renault
₹78,500
12 days overdue
```

Filters:

* Company
* Client
* Branch
* Invoice date
* Due date
* Overdue days
* Payment status

---

# 29. Payment Follow-Ups

RydCare should allow accounts users to manage follow-ups around outstanding invoices.

Workflow:

```text
Outstanding Invoice
        ↓
Follow-Up Created
        ↓
Email / WhatsApp / Call
        ↓
Follow-Up Completed
        ↓
Next Follow-Up
        ↓
Payment Received
        ↓
Tally Sync
        ↓
RydCare Outstanding Updated
```

The system should maintain follow-up history.

---

# 30. Zoho Desk Integration

Zoho Desk will be the support/ticketing backend.

RydCare will provide the interfaces.

```text
Driver App
    │
    ▼
RydCare API
    │
    ▼
Zoho Desk
```

and:

```text
Client Portal / Email
          │
          ▼
      Zoho Desk
          │
          ▼
       RydCare
```

---

# 31. Ticket Categories

Initial categories should include:

### Driver

* Payment issue
* Job issue
* Booking issue
* Driver account
* Document issue
* App issue
* Attendance issue
* Technical issue
* Other

### Client

* Driver complaint
* Booking issue
* Billing issue
* Invoice issue
* Payment issue
* Accident/incident
* Service complaint
* Driver requirement
* Other

---

# 32. Driver Ticketing

The Driver App must provide:

```text
Help & Support
      │
      ├── Create Ticket
      ├── My Tickets
      ├── Ticket Details
      └── Reply
```

A driver should be able to create a ticket with:

* Category
* Subject
* Description
* Booking/job reference
* Photos
* Documents
* Location where appropriate

The RydCare backend will create the corresponding Zoho Desk ticket.

---

# 33. Ticket Synchronization

Example:

```text
Driver App
    ↓
RydCare API
    ↓
Zoho Desk API
    ↓
Ticket Created
```

Responses:

```text
Zoho Desk
    ↓
Webhook
    ↓
RydCare
    ↓
PostgreSQL
    ↓
Push Notification
    ↓
Driver App
```

The driver should never need direct access to the Zoho Desk API.

---

# 34. Ticket Linking

Tickets should be linked to RydCare entities where applicable.

Example:

```text
Zoho Ticket
│
├── Driver ID
├── Client ID
├── Booking ID
├── Trip ID
└── Invoice ID
```

This allows support staff to understand the operational context of the issue.

---

# 35. SLA and Escalation

Zoho Desk should manage:

* Ticket priority
* SLA
* Response deadline
* Resolution deadline
* Escalation
* Assignment
* Status

RydCare should synchronize important ticket status information back to the relevant operational records.

---

# 36. Notifications

RydCare should support:

* Push notifications
* Email
* WhatsApp integration where required
* SMS where required
* In-app notifications

Examples:

```text
Driver assigned
Trip reminder
Booking changed
Ticket reply
Ticket resolved
Payment update
Document expiry
```

---

# 37. Driver Mobile Application

The Driver App should provide:

### Authentication

* Phone number
* OTP
* Session/token management

### Dashboard

* Today's jobs
* Current status
* Earnings summary
* Notifications

### Jobs

* Assigned jobs
* Job details
* Navigation
* Start trip
* End trip
* Photos
* Completion

### Earnings

* Job earnings
* Settlement history
* Payment status

### Documents

* View documents
* Upload documents
* Expiry information

### Support

* Create ticket
* View tickets
* Reply
* View ticket status

---

# 38. Admin Web Application

The Admin Web App should provide:

## Dashboard

* Active drivers
* Available drivers
* Today's bookings
* Ongoing trips
* Completed trips
* Pending bookings
* Billing
* Outstanding receivables
* Open tickets

## Client Management

* Clients
* Branches
* Contacts
* Documents
* Quotations
* Agreements
* Follow-ups

## Driver Management

* Drivers
* Documents
* Attendance
* Availability
* Job history
* Earnings
* Settlements

## Operations

* Bookings
* Assignment
* Trips
* GPS
* Tariffs

## Finance

* Billing
* Invoices
* Outstanding
* Payments
* Tally synchronization

## Support

* Ticket references
* Ticket status
* Support dashboard

---

# 39. Client Portal

The client portal should initially be considered optional/future scope.

Potential functionality:

* Login
* View bookings
* View trip history
* View invoices
* View payment status
* Raise support tickets
* View ticket status
* Download documents

---

# 40. API Architecture

RydCare should expose a versioned API.

Example:

```text
/api/v1/auth/
/api/v1/companies/
/api/v1/clients/
/api/v1/branches/
/api/v1/contacts/
/api/v1/drivers/
/api/v1/bookings/
/api/v1/trips/
/api/v1/tariffs/
/api/v1/billing/
/api/v1/invoices/
/api/v1/payments/
/api/v1/earnings/
/api/v1/tickets/
/api/v1/documents/
/api/v1/notifications/
```

External integrations should be isolated:

```text
/api/v1/integrations/tally/
/api/v1/integrations/zoho-desk/
```

---

# 41. Background Jobs

Long-running operations must not block normal API requests.

Background jobs should handle:

* Tally synchronization
* Zoho Desk synchronization
* Email sending
* Document processing
* Invoice generation
* Notifications
* Payment synchronization
* GPS processing
* Scheduled follow-ups
* Document expiry reminders

A task queue such as Celery/RQ/Dramatiq may be used.

---

# 42. Authentication

## Admin/Web

Use secure session or token-based authentication.

## Driver App

Use:

```text
Phone
 ↓
OTP provider
 ↓
Authentication
 ↓
Access token
```

MSG91 may be used for OTP delivery.

Tokens should be revocable.

---

# 43. Authorization

The system must use role-based access control.

Example:

```text
Admin
Operations
Accounts
HR
Support
Manager
Driver
Client
```

Users must only access information permitted by:

* Role
* Company
* Branch
* Department
* Ownership

---

# 44. Security Requirements

The system must:

* Use HTTPS
* Hash passwords where passwords are used
* Encrypt sensitive data where appropriate
* Secure API tokens
* Never expose Tally's local port publicly
* Validate uploaded files
* Restrict document access
* Use signed URLs for private files
* Maintain audit logs
* Apply rate limiting
* Validate API input
* Protect against CSRF where applicable
* Protect against SQL injection
* Implement secure CORS configuration
* Rotate integration credentials where required

---

# 45. File Storage

Files must not be stored as Base64 in PostgreSQL.

Use S3-compatible object storage.

Possible providers:

* Wasabi
* Backblaze B2
* AWS S3

PostgreSQL stores metadata and object references.

---

# 46. Audit Logging

The system must record important actions.

Examples:

```text
Client created
Client modified
Driver created
Driver approved
Driver document uploaded
Booking assigned
Booking cancelled
Invoice synchronized
Payment synchronized
Ticket created
Ticket status changed
User permission changed
```

Audit records should include:

* User
* Action
* Entity
* Entity ID
* Timestamp
* Previous value where appropriate
* New value where appropriate
* IP/device information where appropriate

---

# 47. Data Model — High Level

Core entities:

```text
Company
User
Role
Client
ClientBranch
ClientContact
ClientDocument
Quotation
Agreement
FollowUp

Driver
DriverDocument
DriverAttendance
DriverAvailability
DriverEarning
DriverSettlement

Booking
Trip
Vehicle
Tariff
TariffRule

BillingRecord
Invoice
Payment
ReceivableFollowUp

SupportTicketReference
Notification

IntegrationAccount
IntegrationLog
AuditLog
```

---

# 48. External IDs

Every integration entity must maintain external identifiers.

Example:

```text
Client
├── rydcare_id
└── tally_ledger_id

Invoice
├── rydcare_id
└── tally_voucher_id

Ticket
├── rydcare_ticket_reference
└── zoho_ticket_id
```

This prevents duplication and allows reconciliation.

---

# 49. Integration Failure Handling

External systems may be unavailable.

Therefore:

```text
RydCare
   ↓
Operation created
   ↓
Integration Queue
   ↓
External API unavailable
   ↓
Retry
   ↓
Retry
   ↓
Success
```

The user should not lose the original operation simply because Tally or Zoho is temporarily unavailable.

Integration status should be visible:

```text
Synced
Pending
Failed
Retrying
Manual Review
```

---

# 50. Reporting

Initial reports:

### Operations

* Bookings by day
* Completed jobs
* Cancelled jobs
* Driver utilization
* Driver performance
* Branch activity

### Driver

* Jobs per driver
* Earnings
* Attendance
* Settlements
* Document expiry

### Client

* Jobs by client
* Jobs by branch
* Billing
* Outstanding
* Payment history

### Finance

* Invoice totals
* Outstanding receivables
* Overdue invoices
* Payment history
* Tally synchronization status

### Support

* Open tickets
* Closed tickets
* Tickets by category
* Tickets by client
* Tickets by driver
* SLA breaches

---

# 51. Non-Functional Requirements

## Performance

Normal API requests should target:

```text
<500 ms
```

under normal operating conditions, excluding external API calls.

Long-running operations should be asynchronous.

## Availability

The RydCare application should target:

```text
99.5%+
```

availability for production infrastructure, subject to hosting/provider limitations.

## Scalability

The architecture should support:

* Multiple companies
* Multiple branches
* Hundreds of drivers
* Thousands of bookings per month
* Increasing trip/GPS data
* Multiple admin users
* Multiple external integrations

The application must not assume a single company or branch.

---

# 52. Backup and Recovery

PostgreSQL:

* Daily full backups
* Point-in-time recovery where practical
* Off-site backup

Object storage:

* Versioning where supported
* Lifecycle policies
* Backup strategy for critical documents

Configuration/secrets:

* Securely backed up
* Never committed to source control

---

# 53. Deployment Architecture

Recommended production structure:

```text
                    Internet
                       │
                       ▼
                 Reverse Proxy
                    Nginx
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Next.js      Django API    Worker
                                    │
                                    ▼
                                Task Queue
          │
          ▼
      PostgreSQL

          │
          ├──────────────► S3 Storage
          │
          ├──────────────► Tally Integration
          │
          └──────────────► Zoho Desk
```

Docker may be used for deployment.

---

# 54. Recommended Integration Boundary

RydCare should use adapters/services rather than calling external APIs throughout the application.

Example:

```text
services/
├── tally/
│   ├── client.py
│   ├── invoices.py
│   ├── payments.py
│   ├── outstanding.py
│   └── sync.py
│
└── zoho_desk/
    ├── client.py
    ├── tickets.py
    ├── comments.py
    ├── webhooks.py
    └── sync.py
```

Business logic should not depend directly on Zoho/Tally-specific implementation.

---

# 55. Source of Truth

| Data                     | Source of Truth           |
| ------------------------ | ------------------------- |
| Clients operational data | RydCare                   |
| Branches                 | RydCare                   |
| Contacts                 | RydCare                   |
| Drivers                  | RydCare                   |
| Driver documents         | RydCare + Object Storage  |
| Bookings                 | RydCare                   |
| Trips                    | RydCare                   |
| GPS                      | RydCare / Tracking System |
| Tariffs                  | RydCare                   |
| Driver job earnings      | RydCare                   |
| Quotations               | RydCare                   |
| Agreements               | RydCare + Object Storage  |
| Accounting               | Tally                     |
| GST accounting           | Tally                     |
| Financial invoices       | Tally                     |
| Payments                 | Tally                     |
| Payroll accounting       | Tally                     |
| Support tickets          | Zoho Desk                 |
| Ticket SLA               | Zoho Desk                 |
| Support conversations    | Zoho Desk                 |

---

# 56. Critical Business Workflow

## Client to Invoice

```text
Client Onboarding
      ↓
Branch Creation
      ↓
Quotation
      ↓
Agreement
      ↓
Client Active
      ↓
Booking
      ↓
Driver Assignment
      ↓
Trip
      ↓
Trip Completed
      ↓
Tariff Calculation
      ↓
Billing Record
      ↓
Invoice
      ↓
Tally
      ↓
GST / Accounting
      ↓
Client Payment
      ↓
Tally
      ↓
Payment Sync
      ↓
RydCare
      ↓
Invoice Paid
```

---

# 57. Critical Driver Workflow

```text
Driver Application
       ↓
Onboarding
       ↓
Document Verification
       ↓
Driver Approved
       ↓
Driver App Account
       ↓
Available
       ↓
Booking Assigned
       ↓
Trip Started
       ↓
Trip Completed
       ↓
Driver Earnings Calculated
       ↓
Settlement
       ↓
Tally Financial Record
```

---

# 58. Critical Support Workflow

## Driver

```text
Driver App
    ↓
Create Ticket
    ↓
RydCare API
    ↓
Zoho Desk
    ↓
Support Team
    ↓
Response
    ↓
Zoho Webhook
    ↓
RydCare
    ↓
Push Notification
    ↓
Driver App
```

## Client

```text
Client
   ↓
Portal / Email
   ↓
Zoho Desk
   ↓
Support Team
   ↓
Resolution
```

---

# 59. Critical Receivables Workflow

```text
Completed Jobs
      ↓
RydCare Billing
      ↓
Tally Invoice
      ↓
Tally Outstanding
      ↓
Sync to RydCare
      ↓
Receivables Dashboard
      ↓
Follow-Up Task
      ↓
Client Communication
      ↓
Payment
      ↓
Tally
      ↓
Payment Sync
      ↓
RydCare
```

---

# 60. MVP Scope

The first production version should prioritize:

### Phase 1 — Core

* Authentication
* Company management
* Client management
* Branches
* Driver management
* Driver documents
* Booking
* Driver assignment
* Trip management
* Driver app
* Basic tariff engine

### Phase 2 — Billing

* Billing records
* Driver earnings
* Settlements
* Quotations
* Documents
* Tally read integration
* Invoice synchronization
* Outstanding synchronization

### Phase 3 — Support

* Zoho Desk integration
* Driver tickets
* Client tickets
* Ticket synchronization
* Push notifications
* Ticket status

### Phase 4 — Automation

* Payment follow-ups
* Document expiry alerts
* Automated notifications
* Advanced Tally synchronization
* Client portal
* Advanced reporting

---

# 61. Key Design Principle

RydCare should **not become a monolithic replacement for every business application**.

The system should follow this principle:

```text
RydCare
= Business Operations

Tally
= Accounting & Finance

Zoho Desk
= Support

Object Storage
= Documents

PostgreSQL
= Operational Database
```

Each system should do the job it is strongest at.

---

# 62. Final Target Architecture

```text
                         RYDCare
                    Django + PostgreSQL
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
    CLIENTS             DRIVERS              JOBS
       │                   │                   │
       ├── Onboarding      ├── Onboarding      ├── Bookings
       ├── Branches        ├── Documents       ├── Assignment
       ├── Contacts        ├── Employee         ├── Trips
       ├── Quotations      │   / Contractor     ├── GPS
       ├── Agreements      ├── Availability     ├── Tariffs
       ├── Documents       ├── Attendance       └── Billing
       └── Follow-ups      └── Earnings
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
        TALLY CONNECTOR              ZOHO DESK
             │                           │
             ▼                           ▼
         TALLY PRIME                SUPPORT SYSTEM
             │                           │
       ┌─────┼─────┐             ┌──────┴──────┐
       │     │     │             │             │
      GST  PAYMENTS INVOICES   CLIENTS       DRIVERS
       │     │     │             │             │
       └─────┴─────┘             └─────────────┘
```

**The fundamental rule is: RydCare runs the business operations; Tally records the money; Zoho Desk manages support.**
