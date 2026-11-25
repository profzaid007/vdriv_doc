# RYDCARE PLATFORM
## Business Requirements Document (BRD)

**Document Version:** 1.0  
**Status:** Draft for Development / Vendor Discussion  
**Product:** RydCare Operations & Driver Management Platform  
**Business:** RydCare / Nine Star Call Drivers / Just Call Drivers

---

# 1. Executive Summary

RydCare is a driver-service business providing driver-based services to corporate clients, service centres, hotels, and other organizations.

The business currently depends heavily on manual processes for:

- Driver onboarding
- Driver document management
- Driver duty management
- Booking management
- Driver assignment
- Driver payments
- Client billing
- Tariff management
- Client onboarding
- Issue resolution
- Driver engagement
- Invoice follow-up

The purpose of the RydCare Platform is to centralize these processes into a single operational system.

The platform should allow RydCare to manage the complete lifecycle of a driver and client, from onboarding through daily duties, verification, billing, payments, and eventual offboarding.

The core operational flow is:

```text
CLIENT REQUEST
      ↓
BOOKING
      ↓
DRIVER ASSIGNMENT
      ↓
DRIVER ACCEPTANCE
      ↓
DRIVER DUTY
      ↓
DUTY VERIFICATION
      ↓
┌───────────────┬────────────────┐
│               │                │
DRIVER EARNING  CLIENT BILLING
│               │
DRIVER LEDGER   INVOICE
│               │
PAYOUT          PAYMENT
```

---

# 2. Business Problem

RydCare currently faces the following operational problems.

## 2.1 Driver Management

- No centralized driver onboarding system
- Driver information is distributed across different records
- Documents require manual tracking
- Bank account changes need historical tracking
- Assets issued to drivers are difficult to track
- Joining fees/deposits need proper tracking
- Driver offboarding requires manual checking
- Driver performance and activity are not centralized

## 2.2 Booking Operations

- Client booking requests require manual handling
- No centralized booking management
- Driver assignment is operationally dependent on staff
- Driver acceptance/rejection needs better tracking
- Customer cancellations/postponements need proper recording
- Duty completion information is distributed across systems

## 2.3 Duty Verification

- Difficult to verify whether a duty actually happened
- Time and distance need to be tracked
- GPS information needs to be connected to duties
- Supporting photos/proofs need centralized storage
- Unusual or suspicious duties need to be identified

## 2.4 Driver Finance

- Driver earnings are job-based
- Advances need to be tracked
- Deductions and recoveries need to be tracked
- Weekly/monthly payouts need to be reconciled
- There is no single view of what RydCare owes a driver
- Historical transactions need to remain available

## 2.5 Client Billing

- Client tariffs vary by client/branch
- Billing depends on completed duties
- Bill books and slip numbers need tracking
- Invoice generation and payment status require centralized management
- Invoice follow-up is currently manual

## 2.6 Client Management

- No standardized client onboarding SOP
- Client requirements need centralized storage
- Branch-specific requirements need tracking
- Agreements and quotations need to be maintained
- Client fulfillment needs to be measurable

## 2.7 Driver Retention

RydCare wants to create a structured driver benefits and engagement program covering:

- Joining benefits
- Referrals
- Work milestones
- Community events
- Recognition
- Festival appreciation

The platform should eventually track these activities.

---

# 3. Business Objectives

The platform should achieve the following objectives:

1. Create a centralized driver database.
2. Create a centralized client and branch database.
3. Centralize all bookings.
4. Improve driver assignment and communication.
5. Track complete driver duties.
6. Verify completed duties using operational evidence.
7. Automatically calculate driver earnings.
8. Automatically calculate client billing.
9. Maintain a driver financial ledger.
10. Maintain client billing and invoice records.
11. Standardize driver onboarding and offboarding.
12. Standardize client onboarding.
13. Track issues and incidents.
14. Measure client fulfillment.
15. Reduce manual office work.
16. Provide management visibility through reports.
17. Build a structured driver benefits and retention program.
18. Provide a foundation that can later be integrated with accounting, messaging and automation systems.

---

# 4. Platform Scope

The platform will consist of the following major modules:

```text
1. Authentication & Users
2. Driver Management
3. Client Management
4. Branch Management
5. Document Management
6. Booking Management
7. Driver Assignment
8. Duty Management
9. Duty Verification
10. Pricing Management
11. Driver Financial Ledger
12. Driver Payouts
13. Client Billing
14. Invoices
15. Issues / Incidents
16. Notifications
17. Reporting
18. Driver Program
19. Audit Logging
20. Integrations
```

---

# 5. User Types

The initial platform should support the following user categories.

## 5.1 Management / Admin

Responsible for:

- Overall platform management
- Drivers
- Clients
- Pricing
- Financial oversight
- Reports
- Configuration
- Approvals

## 5.2 Operations Staff

Responsible for:

- Bookings
- Driver assignment
- Duty monitoring
- Driver communication
- Cancellations
- Operational issues
- Duty verification

## 5.3 Accounts Staff

Responsible for:

- Driver financial records
- Payouts
- Client billing
- Invoices
- Payment records
- Financial reconciliation

## 5.4 Driver

Responsible for:

- Self-onboarding
- Document submission
- Viewing assigned duties
- Accepting/rejecting duties
- Starting duties
- Completing duties
- Uploading required evidence
- Viewing relevant earnings/payout information
- Participating in driver programs

## 5.5 Client / Branch User

Responsible for:

- Creating bookings
- Viewing booking status
- Providing booking information
- Viewing relevant service information
- Receiving invoices/documents where permitted

---

# 6. Driver Management

## 6.1 Driver Profile

The system shall maintain a unique driver record.

Driver information shall include:

- Unique Driver ID
- Name
- Phone number
- Address
- Date of birth
- Profile photo
- Status
- Joining date
- Other required operational information

## 6.2 Driver Documents

The system shall support management of:

- Aadhaar
- PAN
- Driving Licence

The system should record:

- Document type
- Document number
- Uploaded document
- Verification status
- Verification date
- Verified by
- Expiry date where applicable
- Rejection reason
- Document history

## 6.3 Driver Bank Accounts

The system shall support multiple historical bank accounts for a driver.

Business rule:

> A driver may have only one active bank account at a time.

When a bank account is changed:

- The old account must remain in history.
- The new account becomes the active account.
- Historical financial transactions must remain associated with their original records.

## 6.4 Driver Assets

The system shall track assets issued to drivers.

Initial assets may include:

- ID Card
- T-Shirt
- Uniform
- Bag
- Bottle
- Cable
- Powerbank
- Other company-issued assets

The system should record:

- Asset
- Issue date
- Driver
- Condition
- Return required
- Return date
- Returned/lost/damaged status

---

# 7. Driver Onboarding

The platform shall support both office-assisted onboarding and driver self-onboarding.

## 7.1 Office-Assisted Onboarding

```text
NEW DRIVER
    ↓
Basic Information
    ↓
Documents
    ↓
Bank Account
    ↓
Joining Fee / Deposit
    ↓
Assets Issued
    ↓
Agreement / Declaration
    ↓
Verification
    ↓
ACTIVE DRIVER
```

## 7.2 Driver Self-Onboarding

```text
APPLICATION
    ↓
DOCUMENTS PENDING
    ↓
VERIFICATION PENDING
    ↓
READY
    ↓
ACTIVE
```

## 7.3 Driver Statuses

The system shall support at minimum:

- Application
- Documents Pending
- Verification Pending
- Ready
- Active
- Suspended
- Offboarded

The system should prevent a driver from being marked Active until the required onboarding/verification conditions are satisfied, unless an authorized user overrides the process.

---

# 8. Driver Offboarding

The system shall support a controlled offboarding workflow.

```text
ACTIVE DRIVER
    ↓
REQUEST OFFBOARDING
    ↓
CHECK
    ├── Outstanding Balance
    ├── Deposit
    ├── Assets
    ├── Accidents / Issues
    ├── Pending Duties
    └── Documents
    ↓
SETTLEMENT
    ↓
RETURN ASSETS
    ↓
RETURN DOCUMENTS
    ↓
FINAL PAYOUT / REFUND
    ↓
OFFBOARDED
```

The system shall allow staff to record:

- Offboarding reason
- Outstanding amount
- Deposit settlement
- Asset return
- Document return
- Pending duties
- Accident/issue status
- Final settlement
- Final payout/refund
- Offboarding date

Historical driver records must remain accessible after offboarding.

---

# 9. Client Management

The system shall maintain a master record for every client.

Client information may include:

- Company name
- Contacts
- Contact information
- Agreement
- Requirements
- Pricing
- Branches
- Users
- Bookings
- Billing
- Payments
- Issues

---

# 10. Client Onboarding

The platform shall provide a standardized client onboarding workflow.

```text
LEAD / NEW CLIENT
      ↓
REQUIREMENTS COLLECTED
      ↓
QUOTATION
      ↓
AGREEMENT
      ↓
CONFIRMED
      ↓
BRANCH SETUP
      ↓
TARIFF SETUP
      ↓
USERS CREATED
      ↓
SERVICE ACTIVE
```

The system should provide an onboarding checklist to ensure required steps are completed.

---

# 11. Branch Management

A client may have multiple branches.

Each branch shall support:

- Branch name
- Address
- Contact
- Working hours
- Booking rules
- Tariff/pricing
- Bill book
- Branch users
- Other branch-specific requirements

Example:

```text
CLIENT
│
├── Branch A
│   ├── Contact
│   ├── Address
│   ├── Working Hours
│   ├── Booking Rules
│   ├── Tariff
│   └── Bill Book
│
├── Branch B
│
└── Branch C
```

Branch-specific configuration must be supported because different branches may operate under different tariffs or processes.

---

# 12. Booking Management

Booking management is the core operational module.

## 12.1 Booking Lifecycle

```text
CLIENT REQUEST
     ↓
BOOKING
     ↓
DRIVER ASSIGNMENT
     ↓
DRIVER ACCEPTS
     ↓
START DUTY
     ↓
ONGOING
     ↓
END DUTY
     ↓
VERIFICATION
     ↓
COMPLETED
```

The system shall support booking statuses including, where applicable:

- Pending
- Assigned
- Confirmed
- Customer No Response
- Ongoing
- Customer Cancelled
- Customer Postponed
- Vehicle Serviced
- Company Driver
- Completed

Additional statuses may be added where operationally required.

## 12.2 Booking Information

A booking may include:

- Client
- Branch
- Booking date
- Booking time
- Customer information
- Vehicle information
- Pickup location
- Drop location
- Service type
- Driver
- Special instructions
- Slip number
- Billing information
- Booking status

---

# 13. Driver Assignment

Operations users shall be able to assign drivers to bookings.

Workflow:

```text
BOOKING
    ↓
AVAILABLE DRIVERS
    ↓
DRIVER SELECTED
    ↓
ASSIGNMENT
    ↓
DRIVER NOTIFIED
    ↓
ACCEPT / REJECT
```

The system should record:

- Driver assigned
- Assignment time
- Assignment user/system
- Driver response
- Response time
- Rejection reason where applicable
- Reassignment history

---

# 14. Driver Duty

A booking becomes a driver duty when operational execution begins.

The duty lifecycle shall support:

```text
ASSIGNED
    ↓
ACCEPTED
    ↓
ARRIVED
    ↓
STARTED
    ↓
ONGOING
    ↓
ENDED
    ↓
VERIFICATION
    ↓
COMPLETED
```

The duty should record:

- Driver
- Booking
- Start time
- End time
- Start location
- End location
- GPS/location information
- Distance
- Photos
- Customer information
- Slip number
- Duty status
- Verification status

---

# 15. Driver App

The driver application should initially provide:

## Login / Access

- Driver authentication
- Driver profile
- Account status

## Duties

- View assigned duties
- Accept duty
- Reject duty
- View duty details
- Start duty
- End duty

## Verification Data

- Start location
- End location
- GPS tracking
- Required photos
- Other proof required by the booking

## Driver Information

- Relevant earnings
- Payout information
- Driver program information
- Notifications

The driver application should remain simple and focused on completing duties.

---

# 16. Duty Verification

The system shall capture sufficient information to verify whether a duty was legitimately completed.

Verification data may include:

- Start time
- End time
- Start location
- End location
- GPS track
- Distance
- Start photo
- End photo
- Customer confirmation
- Slip number

The system should support rule-based exception detection.

Example:

```text
Expected Distance: 18 km
Actual Distance:   19.2 km

Result: NORMAL
```

Exception:

```text
Expected Distance: 18 km
Actual Distance:   57 km

Result: REVIEW REQUIRED
```

Staff should be able to review and approve/reject flagged duties.

---

# 17. Client Fulfillment

The platform shall calculate client service fulfillment.

Example:

```text
Requests:        1,000
Fulfilled:         940
Unfulfilled:        60

Fulfillment Rate:   94%
```

The system should support reporting on:

- Total requests
- Completed duties
- Unfulfilled requests
- Cancellation rate
- Driver assignment time
- Driver acceptance
- Branch performance
- Period-based fulfillment

---

# 18. Pricing Management

Pricing must be configurable rather than hardcoded.

The platform shall support:

## 18.1 Fixed Rates

Example:

```text
Service / Route
₹350
```

## 18.2 Distance-Based Pricing

Example:

```text
Below 15 km       ₹250
15–20 km          ₹300
Above 20 km       ₹6/km
```

## 18.3 Location-Based Pricing

Example:

```text
Chengalpet        ₹400
Guduvanchery      ₹450
Tiruthani         ₹500
Minjur            ₹600
```

## 18.4 Hours-Based Pricing

Support:

- Base hours
- Additional hours
- Overtime

## 18.5 Pricing Rules

Pricing may depend on:

- Client
- Branch
- Service type
- Route
- Distance
- Location
- Hours
- Effective date

Historical billing must not be affected when future tariff rules are changed.

---

# 19. Driver Financial Ledger

The system shall maintain a financial ledger for each driver.

The ledger shall support:

### Credits

- Duty earnings
- Incentives
- Referral rewards
- Refunds
- Other approved credits

### Debits

- Advances
- Deductions
- Recoveries
- Payouts
- Other approved debits

The ledger must maintain transaction history.

The platform should be able to calculate:

> Current amount payable to the driver.

and:

> Current amount recoverable from the driver.

---

# 20. Driver Payouts

The system shall support:

- Weekly payouts
- Monthly payouts
- Other approved payout cycles

Payout workflow:

```text
DRIVER EARNINGS
      ↓
LEDGER
      ↓
PAYOUT CALCULATION
      ↓
ADVANCES / DEDUCTIONS
      ↓
FINAL PAYABLE
      ↓
PAYOUT
      ↓
LEDGER ENTRY
```

A payout should retain:

- Driver
- Period
- Gross earnings
- Advances
- Deductions
- Other adjustments
- Net payout
- Payment date
- Payment reference
- Payment status

---

# 21. Client Billing

Completed and approved duties should enter the billing process.

```text
COMPLETED DUTIES
       ↓
PRICING ENGINE
       ↓
CLIENT CHARGE
       ↓
BILLING QUEUE
       ↓
BILL BOOK / SLIP NUMBER
       ↓
INVOICE
       ↓
SENT TO CLIENT
       ↓
PAYMENT
```

The system should support:

- Billing periods
- Branch-wise billing
- Bill books
- Slip numbers
- Invoice generation
- Invoice status
- Payment status
- Outstanding invoices
- Invoice follow-up

---

# 22. Billing Rules

The platform should ensure that:

1. Only eligible/completed duties enter billing.
2. A duty should not be billed twice.
3. Historical invoices remain unchanged unless explicitly corrected.
4. Pricing used for billing is traceable.
5. Billing records remain linked to the original duty.
6. Slip numbers can be recorded against the relevant duty/billing record.
7. Invoice status is maintained separately from duty status.

---

# 23. Issues and Incident Management

The platform shall provide a simple issue-management system.

Issues may relate to:

- Driver
- Client
- Branch
- Booking
- Duty
- Accident
- Payment
- Billing
- Customer complaint
- Operational problem

Issue lifecycle:

```text
OPEN
 ↓
UNDER REVIEW
 ↓
ACTION REQUIRED
 ↓
RESOLVED
 ↓
CLOSED
```

Each issue should support:

- Issue type
- Related record
- Description
- Raised by
- Assigned staff
- Attachments
- Notes
- Status
- Resolution
- Date/time history

---

# 24. Notifications

The platform shall support notifications for important operational events.

## Driver Notifications

- New duty assignment
- Duty reminder
- Booking changes
- Cancellation
- Payment/payout notification
- Incentive notification
- Program/reward notification

## Client Notifications

- Booking confirmation
- Driver assignment
- Booking changes
- Cancellation
- Invoice notification
- Payment reminders

## Staff Notifications

- Unassigned booking
- Driver rejection
- Verification exception
- Invoice overdue
- Issue requiring attention

Initial/future notification channels may include:

- Push notifications
- WhatsApp
- Email
- In-app notifications

---

# 25. Document Management

The system shall provide centralized document storage.

Documents may belong to:

- Drivers
- Clients
- Branches
- Agreements
- Quotations
- Invoices
- Issues
- Other business records

The system should support:

- Upload
- Download
- Verification
- Document status
- Historical versions where required
- Access restrictions
- Audit trail

---

# 26. Audit Logging

Important system actions should be recorded.

Examples:

- Driver created
- Driver status changed
- Document verified
- Bank account changed
- Asset issued
- Asset returned
- Booking created
- Driver assigned
- Booking cancelled
- Duty completed
- Duty approved
- Invoice generated
- Payout processed
- Issue closed

Audit records should include:

- User
- Action
- Related record
- Date/time
- Previous value where appropriate
- New value where appropriate

---

# 27. Driver Program — RydCare Driver Program

The Driver Program is a driver retention and engagement initiative.

It should be implemented after the core operational system is stable.

## 27.1 Join

Initial joining benefits may include:

- ID Card
- T-Shirt
- Bottle

## 27.2 Refer

Drivers may receive:

- ₹300–₹500 referral reward

Referral eligibility should depend on the referred driver becoming active and completing the required qualifying activity.

## 27.3 Work

Trip milestones unlock rewards.

Example:

```text
50 Trips
   ↓
Reward

100 Trips
   ↓
Reward

250 Trips
   ↓
Reward
```

The actual milestones and rewards should be configurable.

## 27.4 Community

Possible activities:

- Monthly driver lunch
- Quarterly driver events
- Small community gatherings

## 27.5 Recognition

Possible program:

**Driver of the Month**

Rewards may include:

- Cash
- Gift
- Recognition
- Certificate

## 27.6 Festival Benefits

Possible occasions:

- Pongal
- Diwali
- Ramzan
- Christmas
- Other relevant occasions

The system should track:

- Eligibility
- Reward
- Issue date
- Cost/value
- Driver
- Status

---

# 28. Driver Feedback and Assessment

The system should eventually support driver performance records.

Possible sources:

- Client feedback
- Customer feedback
- Staff feedback
- Duty performance
- Complaints
- Incidents
- Attendance/duty participation

This data may later be used for:

- Driver recognition
- Incentives
- Training
- Service quality
- Driver ranking
- Suspension/review

---

# 29. Reporting

The platform should provide management reports.

## Driver Reports

- Active drivers
- Inactive drivers
- New drivers
- Driver duties
- Driver earnings
- Advances
- Deductions
- Payouts
- Outstanding balances
- Referrals
- Rewards

## Client Reports

- Bookings
- Fulfillment
- Cancellations
- Branch performance
- Billing
- Revenue
- Outstanding invoices

## Operations Reports

- Pending bookings
- Unassigned bookings
- Assignment time
- Duty completion
- Verification exceptions
- Issues
- Accidents

## Finance Reports

- Driver liabilities
- Driver advances
- Driver payouts
- Client billing
- Client receivables
- Outstanding invoices
- Revenue

---

# 30. Core Business Rules

The following rules are fundamental to the platform.

### Driver

1. Every driver must have a unique Driver ID.
2. A driver may have multiple historical bank accounts.
3. Only one bank account can be active at a time.
4. Driver history must remain available after offboarding.
5. Driver assets must be traceable.
6. Driver financial transactions must not be deleted without proper authorization/audit.

### Booking

7. A booking belongs to a client and branch.
8. A booking may be assigned to a driver.
9. Driver assignment and driver acceptance must be separately tracked.
10. Booking cancellation must retain its history/reason.
11. A completed duty must remain linked to its originating booking.

### Duty

12. A duty must have a clear lifecycle.
13. Completed duties should be subject to verification.
14. Approved completed duties can generate driver earnings.
15. Approved completed duties can generate client billing.

### Pricing

16. Pricing must be configurable.
17. Pricing changes must not unexpectedly alter historical billing.
18. The pricing rule used for a bill should be traceable.

### Finance

19. Driver financial activity must be ledger-based.
20. Advances must be recorded as financial transactions.
21. Payouts must create ledger entries.
22. Historical transactions must remain auditable.

### Billing

23. A completed duty should not be billed twice.
24. Invoice records must remain linked to the underlying billing records.
25. Invoice status must be trackable.

---

# 31. Core Data Relationships

At a high level:

```text
CLIENT
  │
  ├── BRANCH
  │     │
  │     ├── USERS
  │     ├── TARIFF / PRICING
  │     ├── BILL BOOK
  │     └── BOOKINGS
  │
  └── AGREEMENTS / DOCUMENTS


DRIVER
  │
  ├── DOCUMENTS
  ├── BANK ACCOUNTS
  ├── ASSETS
  ├── DUTIES
  ├── LEDGER
  ├── PAYOUTS
  ├── REFERRALS
  ├── REWARDS
  └── ISSUES


BOOKING
  │
  └── DUTY
       │
       ├── DRIVER
       ├── GPS / LOCATION
       ├── VERIFICATION
       ├── DRIVER EARNING
       └── CLIENT BILLING
                    │
                    └── INVOICE
```

---

# 32. Platform-Wide Requirements

## Security

The system should:

- Authenticate users securely.
- Restrict access according to user type/permission.
- Protect sensitive driver documents and financial information.
- Maintain audit history for important actions.

## Data Integrity

The system should:

- Prevent duplicate financial transactions.
- Prevent duplicate billing.
- Maintain historical records.
- Use appropriate validation.
- Maintain relationships between bookings, duties, billing and payments.

## Availability

The platform should be designed for reliable day-to-day operational use.

## Scalability

The platform should support growth in:

- Drivers
- Clients
- Branches
- Bookings
- Duties
- Financial transactions
- Documents

The initial architecture should avoid unnecessary complexity while allowing future scaling.

## Mobile Support

The driver-facing workflow must work effectively on mobile devices.

---

# 33. Integrations

The platform may integrate with external services.

## Initial / Planned Integrations

### Authentication / OTP

- MSG91

### Push Notifications

- Firebase Cloud Messaging

### Location Tracking

- Traccar / GPS infrastructure

### File Storage

- S3-compatible storage such as Wasabi or Backblaze B2

### Messaging

- WhatsApp

### Email

- Email service/provider

### Accounting

- Zoho Books

### Payment

- Payment gateway where required

Integrations should be treated as replaceable services where practical.

---

# 34. Product Development Phases

The platform should be developed incrementally.

## Phase 1 — Foundation

1. Users / Authentication
2. Drivers
3. Clients
4. Branches
5. Documents

### Goal

RydCare can maintain its core business records.

---

## Phase 2 — Core Revenue Workflow

6. Bookings
7. Driver Assignment
8. Driver App
9. Start / End Duty
10. Duty Verification

### Goal

A client request can become a completed and verified duty.

---

## Phase 3 — Money

11. Driver Ledger
12. Driver Payouts
13. Pricing Engine
14. Client Billing
15. Invoices

### Goal

A completed duty can generate both:

- Driver earning
- Client billing

---

## Phase 4 — Operations

16. Client Fulfillment
17. Issues / Incidents
18. Client Onboarding Checklist
19. Driver Onboarding Checklist
20. Reports

### Goal

The platform becomes the operational control centre.

---

## Phase 5 — Driver Retention

21. Referral Program
22. Driver Milestones
23. Rewards
24. Driver of the Month
25. Community Events
26. Festival Gifts

### Goal

Create the RydCare Driver Benefits Network.

---

## Phase 6 — Automation

27. Push Notifications
28. WhatsApp
29. Email
30. Zoho Books Integration
31. Automated Reminders
32. Analytics

### Goal

Reduce repetitive manual work.

---

# 35. MVP Definition

The MVP should **not** attempt to implement the entire platform.

The first usable version should answer one question:

> Can RydCare take a client request, assign a driver, complete and verify the duty, calculate what the driver earned, and calculate what the client should be billed?

Therefore the initial MVP should prioritize:

```text
CLIENT
   ↓
BOOKING
   ↓
DRIVER ASSIGNMENT
   ↓
DRIVER APP
   ↓
DUTY
   ↓
VERIFICATION
   ↓
DRIVER EARNING
   ↓
CLIENT BILLING
```

Everything else can be layered around this core.

---

# 36. Out of Initial MVP Scope

The following should not block the first operational release:

- Advanced AI
- Complex HR/payroll system
- Advanced analytics
- Complex incentive engine
- Advanced accounting automation
- Full CRM
- Complex workflow builder
- AI-based duty verification
- Extensive employee management
- Microservice architecture solely for future scale

These can be added after the core workflow is stable.

---

# 37. Acceptance Criteria for Core Platform

The core system should be considered operational when the following workflow can be completed successfully:

### Step 1 — Client

A client/branch exists with its requirements and pricing.

### Step 2 — Booking

A booking can be created.

### Step 3 — Assignment

An available driver can be assigned.

### Step 4 — Driver

The driver receives the duty and accepts it.

### Step 5 — Duty

The driver can start and complete the duty.

### Step 6 — Verification

The system records required time, location, distance and supporting evidence.

### Step 7 — Driver Finance

The system calculates the driver's earning and records it in the driver's ledger.

### Step 8 — Client Billing

The system applies the correct pricing rule and generates the client billing record.

### Step 9 — Invoice

The billing record can be converted into an invoice.

### Step 10 — Payment

The payment can be recorded against the invoice.

If this workflow works reliably, the core RydCare platform is functional.

---

# 38. Long-Term Vision

The long-term objective is to make RydCare a centralized operating system for the driver-service business.

The platform should eventually provide:

```text
                         RYDCARE
                            │
       ┌────────────────────┼────────────────────┐
       │                    │                    │
     DRIVERS              CLIENTS             OPERATIONS
       │                    │                    │
       └────────────────────┼────────────────────┘
                            │
                         BOOKINGS
                            │
                          DUTIES
                            │
                ┌───────────┴───────────┐
                │                       │
             FINANCE                VERIFICATION
                │                       │
        ┌───────┴───────┐               │
        │               │               │
   DRIVER LEDGER    CLIENT BILLING      │
        │               │               │
     PAYOUTS         INVOICES           │
        │               │               │
        └───────────────┼───────────────┘
                        │
                    REPORTING
                        │
               ┌────────┴────────┐
               │                 │
           AUTOMATION       DRIVER PROGRAM
                                 │
                      RydCare Driver
                      Benefits Network
```

The platform should evolve from a basic operational management system into a complete **RydCare Business Operating Platform**.

---

# 39. Guiding Principle

The platform should be built around the actual business workflow rather than around isolated features.

The fundamental RydCare business transaction is:

> **Client Request → Booking → Driver → Duty → Verification → Driver Earning + Client Billing → Payment**

Every major module should support, extend, or report on this flow.

Features that do not directly contribute to the current operational requirement should be prioritized for a later phase rather than delaying the core platform.

---

# 40. Summary

RydCare requires a centralized platform to manage:

- Drivers
- Driver onboarding
- Driver documents
- Driver bank accounts
- Driver assets
- Driver duties
- Driver finances
- Driver payouts
- Driver programs
- Clients
- Client branches
- Client onboarding
- Bookings
- Driver assignment
- Duty verification
- Pricing
- Client billing
- Invoices
- Issues
- Notifications
- Reporting
- Audit history

The platform should be developed incrementally, beginning with the **core booking-to-duty-to-money workflow**, followed by operational management, driver retention and automation.

The objective is not to build every feature at once.

The objective is to build a reliable operational backbone that RydCare can use every day and expand over time.