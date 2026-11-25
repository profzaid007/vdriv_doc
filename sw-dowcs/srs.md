# RydCare Platform — Master Product Blueprint

## 1. Platform Overview

RydCare is an operations platform for managing driver-based mobility and valet services.

The platform connects:

- Drivers
- Clients
- Client branches
- Bookings
- Driver duties
- Duty verification
- Driver finances
- Client billing
- Payments
- Operational issues
- Driver engagement and benefits

The central operational flow is:

```text
                         RYDCARE PLATFORM
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
       DRIVERS               CLIENTS              BOOKINGS
          │                     │                     │
          └──────────────┬──────┴─────────────────────┘
                         │
                       DUTIES
                         │
              ┌──────────┴──────────┐
              │                     │
          VERIFICATION           BILLING
              │                     │
              └──────────┬──────────┘
                         │
                      PAYMENTS
```

The **Booking/Duty** is the central operational object.

A completed duty can produce:

- Driver earnings
- Client billing
- Operational records
- GPS/location data
- Verification data
- Driver performance data
- Billing records
- Financial ledger entries

---

# 2. Core Platform Structure

```text
                            RYDCARE
                               │
       ┌───────────────────────┼───────────────────────┐
       │                       │                       │
    DRIVERS                 CLIENTS                  STAFF
       │                       │
       │                       │
       └───────────────┬───────┘
                       │
                    BOOKINGS
                       │
                 DRIVER ASSIGNMENT
                       │
                   DRIVER DUTY
                       │
                DUTY VERIFICATION
                       │
             ┌─────────┴─────────┐
             │                   │
        DRIVER LEDGER       CLIENT BILLING
             │                   │
          PAYOUTS              INVOICE
             │                   │
             └─────────┬─────────┘
                       │
                    REPORTING
```

### Cross-Cutting Systems

These support multiple areas of the platform:

```text
                         RYDCARE
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
   Notifications         Documents            Issues
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                         Audit Log
```

---

# 3. Driver Management

A driver is a central master record containing personal information, documents, financial information, assets, duties and activity.

```text
DRIVER
│
├── Profile
│   ├── Name
│   ├── Phone
│   ├── Address
│   ├── DOB
│   └── Driver ID
│
├── Documents
│   ├── Aadhaar
│   ├── PAN
│   └── Driving Licence
│
├── Bank Accounts
│   ├── Current Account
│   └── Previous Accounts
│
├── Assets
│   ├── T-Shirt
│   ├── Uniform
│   ├── Bag
│   ├── Bottle
│   └── Powerbank
│
├── Financial Account
│   ├── Joining Fee
│   ├── Deposit
│   ├── Advances
│   ├── Earnings
│   ├── Deductions
│   └── Payouts
│
└── Activity
    ├── Trips
    ├── Incentives
    ├── Referrals
    ├── Feedback
    └── Issues
```

## Driver Status

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
    ↓
SUSPENDED
    ↓
OFFBOARDED
```

---

# 4. Driver Onboarding

There are two onboarding paths.

## 4.1 Office-Assisted Driver Onboarding

The office can create and onboard a driver manually.

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

## 4.2 Driver Self-Onboarding

The driver can submit their information through the driver application.

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

The office/admin team remains responsible for verification and approval.

---

# 5. Driver Documents

The system should maintain driver documents and their verification status.

Initial documents:

- Aadhaar
- PAN Card
- Driving Licence

Each document should be capable of having:

- Document type
- Document number
- Uploaded file
- Verification status
- Verification date
- Verified by
- Expiry date where applicable
- Rejection reason where applicable
- Document history

Possible status:

```text
NOT SUBMITTED
     ↓
SUBMITTED
     ↓
UNDER REVIEW
     ↓
VERIFIED
```

Or:

```text
SUBMITTED
     ↓
REJECTED
     ↓
RESUBMISSION
```

---

# 6. Driver Bank Accounts

A driver may have multiple historical bank accounts, but only **one active bank account** at a time.

```text
DRIVER
│
├── Current Bank Account
│
└── Bank Account History
    ├── Previous Account
    ├── Previous Account
    └── Previous Account
```

The system must preserve historical bank details rather than replacing them permanently.

This is important for financial records and auditability.

---

# 7. Driver Assets

Assets issued to drivers should be tracked.

Examples:

- T-Shirt
- Uniform
- Bag
- Bottle
- Powerbank
- Cable
- ID Card
- Other company-issued items

The system should record:

```text
Asset
    ↓
Issued To Driver
    ↓
Issue Date
    ↓
Condition
    ↓
Return Required?
    ↓
Returned / Lost / Damaged
```

This allows assets to be checked during offboarding.

---

# 8. Driver Financial Account

Driver finances should be represented through a **driver ledger**.

The ledger should track:

- Joining fees
- Deposits
- Trip earnings
- Advances
- Incentives
- Deductions
- Recoveries
- Payouts
- Refunds

Conceptually:

```text
TRIP COMPLETED
       ↓
Driver earning
       ↓
CREDIT DRIVER LEDGER
```

```text
ADVANCE GIVEN
       ↓
DEBIT DRIVER LEDGER
```

```text
DRIVER PAYOUT
       ↓
DEBIT DRIVER LEDGER
```

```text
INCENTIVE
       ↓
CREDIT DRIVER LEDGER
```

The system should always be able to answer:

> How much does RydCare currently owe this driver?

and:

> How much does this driver currently owe RydCare?

---

# 9. Driver Offboarding

Offboarding should be a controlled settlement process.

```text
ACTIVE DRIVER
    ↓
REQUEST OFFBOARDING
    ↓
CHECK
    ├── Outstanding Balance?
    ├── Deposit?
    ├── Assets?
    ├── Accidents?
    ├── Pending Duties?
    └── Documents?
    ↓
SETTLEMENT
    ↓
Return Assets
    ↓
Return Documents
    ↓
Final Payout / Refund
    ↓
OFFBOARDED
```

The system should prevent accidental closure while important financial or operational items remain unresolved.

---

# 10. Client Management

A client is a master record containing the company relationship and all associated branches, users, pricing, bookings and billing.

```text
CLIENT
│
├── Company Information
├── Contacts
├── Branches
├── Requirements
├── Agreement
├── Pricing
├── Bookings
├── Billing
├── Payments
└── Issues
```

---

# 11. Client Onboarding

The client onboarding workflow:

```text
LEAD / NEW CLIENT
      ↓
Requirements Collected
      ↓
Quotation
      ↓
Agreement
      ↓
Confirmation
      ↓
Branch Setup
      ↓
Tariff Setup
      ↓
Users Created
      ↓
SERVICE ACTIVE
```

The onboarding process should have a checklist so the office knows what has and has not been completed.

---

# 12. Client Branches

A client may have multiple branches.

```text
CLIENT
│
├── Branch 1
├── Branch 2
├── Branch 3
└── Branch 4
```

Each branch may contain:

```text
BRANCH
│
├── Contact
├── Address
├── Working Hours
├── Booking Rules
├── Tariff
├── Bill Book
└── Branch Users
```

Branch-specific configuration should be supported because different branches may have different:

- Rates
- Contacts
- Booking requirements
- Working hours
- Billing requirements
- Service rules

---

# 13. Booking Management

Bookings are the central operational workflow.

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
     ↓
DRIVER EARNING
     ↓
CLIENT BILLING
```

Bookings should support:

- Client bookings
- Branch bookings
- Internal bookings
- Single bookings
- Bulk bookings
- Driver assignment
- Driver acceptance/rejection
- Customer cancellation
- Customer postponement
- No response
- Company driver
- Completed duties
- Billing information

---

# 14. Driver Assignment

Once a booking is created, operations should be able to assign an appropriate driver.

```text
BOOKING
    ↓
AVAILABLE DRIVERS
    ↓
DRIVER SELECTED
    ↓
ASSIGNMENT
    ↓
PUSH NOTIFICATION
    ↓
DRIVER ACCEPTS / REJECTS
```

The driver should receive the necessary duty information.

Example:

```text
NEW DUTY

Client: Renault
Branch: Ambattur
Time: 10:30 AM

Pickup:
Service Centre

Drop:
Customer Location

[ACCEPT]
[REJECT]
```

---

# 15. Driver Duty

A booking becomes an active operational duty.

Basic duty lifecycle:

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

The duty should record relevant operational data such as:

- Driver
- Booking
- Start time
- End time
- Start location
- End location
- GPS tracking
- Distance
- Photos
- Customer information
- Slip number
- Duty status
- Verification status

---

# 16. Duty Verification

The system should verify whether a completed duty appears legitimate.

Possible verification data:

```text
DUTY
│
├── Start Time
├── End Time
├── Start Location
├── End Location
├── GPS Track
├── Distance
├── Start Photo
├── End Photo
├── Customer Confirmation
└── Slip Number
```

The system can use rules to flag unusual duties.

Example:

```text
Expected Distance: 18 km
GPS Distance:      19.2 km

Expected Duration: 1h 20m
Actual Duration:   1h 28m

STATUS: NORMAL
```

Potential abnormal case:

```text
Expected Distance: 18 km
Actual Distance:   57 km

STATUS: REVIEW REQUIRED
```

The initial system does not require AI for this. Rule-based verification is sufficient.

---

# 17. Client Fulfillment

The platform should measure how well RydCare fulfills client requirements.

Example:

```text
CLIENT PERFORMANCE

Requests:       1,000
Fulfilled:        940
Unfulfilled:       60

Fulfillment:      94%
```

Possible metrics:

- Total requests
- Completed duties
- Unfulfilled requests
- Cancellation rate
- Assignment time
- Driver acceptance rate
- Average completion time
- Branch performance

---

# 18. Pricing Engine

Pricing should not be hardcoded into bookings.

The platform should support multiple pricing models.

### Fixed Rate

```text
Route / Service
₹350
```

### Distance Based

```text
< 15 km       ₹250
15–20 km      ₹300
> 20 km       ₹6/km
```

### Location Based

```text
Chengalpet     ₹400
Guduvanchery   ₹450
Tiruthani      ₹500
Minjur         ₹600
```

### Hours Based

```text
Base Hours
Additional Hours
Overtime
```

The pricing engine should determine the applicable client charge based on the client's configured pricing rules.

---

# 19. Client Billing

Completed duties enter the billing process.

```text
COMPLETED DUTIES
       ↓
PRICING ENGINE
       ↓
CALCULATE CLIENT CHARGE
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

Billing should support:

- Bill books
- Slip numbers
- Client invoices
- Branch-wise billing
- Billing periods
- Invoice status
- Payment status
- Invoice follow-up

---

# 20. Driver Earnings and Payouts

Driver earnings are generated from completed duties.

```text
COMPLETED DUTY
       ↓
DRIVER EARNING RULE
       ↓
DRIVER LEDGER
       ↓
WEEKLY / MONTHLY PAYOUT
```

The system should support:

- Per-duty earnings
- Advances
- Deductions
- Incentives
- Recoveries
- Weekly payouts
- Monthly payouts
- Final settlements

---

# 21. Issues and Incident Management

The platform should provide a simple issue-management system.

An issue can be linked to:

- Driver
- Client
- Branch
- Booking
- Duty
- Invoice
- Accident
- Payment
- Operational problem

Example:

```text
ISSUE #1045

Type:
Accident

Related Duty:
RYD10245

Raised By:
Client

Amount:
₹10,000

Status:
OPEN

Assigned To:
Operations Manager
```

Basic lifecycle:

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

---

# 22. Notifications

Notifications should be available across the platform.

Channels may include:

- Push notifications
- WhatsApp
- Email
- Internal notifications

Examples:

### Driver

- New duty assigned
- Duty reminder
- Booking cancelled
- Duty changed
- Payment processed
- Incentive unlocked

### Client

- Booking confirmation
- Driver assigned
- Booking cancellation
- Invoice generated
- Payment reminder

### Staff

- Unassigned booking
- Driver rejected duty
- Verification required
- Invoice overdue
- Issue requiring attention

---

# 23. Documents

Documents are a cross-platform capability.

Documents may belong to:

- Drivers
- Clients
- Branches
- Agreements
- Quotations
- Invoices
- Issues
- Other business records

Documents should support:

- Upload
- Download
- Verification
- Version/history where required
- Access control
- Audit trail

---

# 24. Audit Log

Important actions should be recorded.

Examples:

```text
Driver status changed
Bank account changed
Document verified
Driver assigned
Booking cancelled
Invoice generated
Payment recorded
Payout processed
Issue closed
```

Example:

```text
AUDIT LOG

User: Admin
Action: Driver Bank Account Changed
Driver: RYD00125
Time: 03 Oct 2026, 14:32
```

This becomes important as the company grows.

---

# 25. RydCare Driver Program

The Driver Program should be built **after the core operational system**.

## Join

Driver receives:

- ID Card
- T-Shirt
- Bottle

## Refer

Driver receives:

- ₹300–₹500 referral reward

Eligibility should be based on the referred driver's successful activation and defined activity milestone.

## Work

Trip milestones unlock useful rewards.

Examples:

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

## Community

Monthly or quarterly driver events.

Examples:

- Driver lunch
- Small gatherings
- Community meetings

## Recognition

Driver of the Month.

Possible rewards:

- Cash
- Gift
- Certificate
- Recognition

## Festival

Small appreciation gifts during:

- Pongal
- Diwali
- Ramzan
- Christmas
- Other relevant occasions

---

# 26. Driver Program Tracking

The application should track all Driver Program activity.

```text
DRIVER
│
├── Referral History
├── Milestones
├── Rewards
├── Events
├── Recognition
└── Festival Benefits
```

Rewards should ideally connect to the driver's financial or asset records where appropriate.

For example:

```text
MILESTONE ACHIEVED
       ↓
REWARD APPROVED
       ↓
₹500 Incentive
       ↓
DRIVER LEDGER
```

or:

```text
MILESTONE ACHIEVED
       ↓
POWERBANK
       ↓
ASSET ISSUED
```

---

# 27. Driver Feedback and Assessment

The system should eventually capture driver performance and feedback.

Possible inputs:

- Client feedback
- Staff feedback
- Duty performance
- Attendance/duty participation
- Complaints
- Incidents
- Customer ratings where applicable

This information can later support:

- Driver ranking
- Driver of the Month
- Incentives
- Training
- Suspension decisions
- Service quality analysis

---

# 28. Reporting

The platform should eventually provide reports across the business.

## Driver Reports

- Active drivers
- Inactive drivers
- Driver utilization
- Duties completed
- Driver earnings
- Advances
- Payouts
- Outstanding balances
- Incentives
- Referrals

## Client Reports

- Total bookings
- Fulfillment rate
- Branch performance
- Cancellation rate
- Revenue
- Outstanding invoices

## Operations Reports

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
- Client invoices
- Client receivables
- Revenue
- Outstanding payments

---

# 29. Platform Build Phases

The platform should **not** be built as one giant project.

Build it incrementally.

---

## Phase 1 — Foundation

### Goal

Create the core records and identity system.

1. Users / Authentication
2. Drivers
3. Clients
4. Branches
5. Documents

At the end of Phase 1:

> RydCare can maintain its drivers and clients.

---

## Phase 2 — Core Revenue Workflow

### Goal

Make the actual service operational.

6. Bookings
7. Driver Assignment
8. Driver App
9. Start / End Duty
10. Duty Verification

At the end of Phase 2:

> A client request can become a completed, verified driver duty.

---

## Phase 3 — Money

### Goal

Connect operational activity to money.

11. Driver Ledger
12. Driver Payouts
13. Pricing Engine
14. Client Billing
15. Invoices

At the end of Phase 3:

> Every completed duty can produce both a driver earning and a client charge.

---

## Phase 4 — Operations

### Goal

Replace manual office processes.

16. Client Fulfillment
17. Issues / Incidents
18. Client Onboarding Checklist
19. Driver Onboarding Checklist
20. Reports

At the end of Phase 4:

> The platform becomes the operational control centre for RydCare.

---

## Phase 5 — Driver Retention

### Goal

Build the RydCare Driver Benefits Network.

21. Referral Program
22. Driver Milestones
23. Rewards
24. Driver of the Month
25. Community Events
26. Festival Gifts

At the end of Phase 5:

> The platform doesn't just manage drivers; it helps RydCare retain them.

---

## Phase 6 — Automation and Integrations

### Goal

Reduce manual work.

27. Push Notifications
28. WhatsApp
29. Email
30. Zoho Books Integration
31. Automated Reminders
32. Analytics

At the end of Phase 6:

> Repetitive operational and financial tasks become increasingly automated.

---

# 30. What NOT to Build Initially

The following should **not** block the first working version:

- Advanced AI
- Complex HR system
- Advanced analytics
- Sophisticated incentive engine
- Complex accounting automation
- Extensive CRM
- Complicated role systems
- Over-engineered microservices
- Complex workflow builders
- AI-based duty verification

The initial goal is not to build an enterprise ERP.

The initial goal is:

> **Take a client request → assign a driver → complete the duty → verify it → calculate driver earning → calculate client billing.**

If that workflow works reliably, RydCare has its operational backbone.

---

# 31. Core Business Flow

The entire platform can ultimately be understood as:

```text
CLIENT
  │
  │ requests service
  ↓
BOOKING
  │
  │ driver assigned
  ↓
DRIVER
  │
  │ performs duty
  ↓
DUTY
  │
  ├───────────────┐
  │               │
  ↓               ↓
VERIFICATION   PRICING
  │               │
  ↓               ↓
COMPLETED       CLIENT CHARGE
  │               │
  ↓               ↓
DRIVER EARNING   INVOICE
  │               │
  ↓               ↓
DRIVER LEDGER    CLIENT RECEIVABLE
  │
  ↓
PAYOUT
```

This is the **core economic engine of RydCare**.

---

# 32. Long-Term Platform Vision

Once the core platform is stable, RydCare can evolve beyond a simple driver management system.

The long-term platform becomes:

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
           AUTOMATION        DRIVER PROGRAM
                                 │
                       RydCare Driver
                       Benefits Network
```

The platform should therefore be designed around the **core operational entities and workflows**, while advanced programs and automation are layered on top as the business grows.