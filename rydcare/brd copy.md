# BUSINESS REQUIREMENTS DOCUMENT (BRD)

## RydCare / Nine Star Call Drivers

**Project:** Custom Business Operations Management Platform
**Prepared for:** TaxiAppz
**Document Purpose:** Business requirements and scope for software estimation and development quotation
**Version:** 1.0

---

# 1. Business Overview

We operate a driver-service business providing professional drivers to automobile service centres, hotels, corporate clients and other businesses.

Our operations involve:

* Client and branch management
* Driver onboarding and management
* Driver assignment
* Booking management
* Pickup/drop services
* Trip/duty tracking
* Client-specific tariffs
* Driver payments
* Client billing
* Invoice follow-ups
* Client and driver document management
* Client and driver issue resolution

The business currently operates through a combination of manual processes, WhatsApp, phone calls, spreadsheets, emails and separate systems.

The purpose of this project is to create a **centralized business operations platform** that reduces manual work and gives management visibility over the complete workflow.

The software should support multiple operating companies under the same platform, including **RydCare, Nine Star Call Drivers and other related companies**, while maintaining company-level separation where required.

---
<!-- 
# 2. Current Business Problems

## 2.1 No Dedicated HR / Driver Management Function

There is currently no centralized system for managing the complete driver lifecycle.

Driver information, documents, availability, attendance, duties, earnings and payment-related information are spread across different processes.

This makes it difficult to answer questions such as:

* How many active drivers do we have?
* Which drivers are available?
* Which drivers are assigned?
* Which documents are expiring?
* How many duties has a driver completed?
* How much has a job-based driver earned?
* How much has a driver already been paid?
* Which drivers are salaried and which are paid per job? -->


<!-- # 3. Main Business Pain Points

The proposed system must address the following primary pain points:

### 1. No HR / Driver Management System

Centralized driver onboarding, documents, employee/contractor classification, attendance, availability and driver records.

### 2. Driver Onboarding and Duty Tracking

Drivers need to be onboarded properly and their duties must be recorded from assignment through completion.

### 3. No Centralized Booking Management

Client bookings are currently handled through multiple channels and need to be centralized.

### 4. No Standard Client Onboarding SOP

New clients require a defined onboarding workflow including company information, GST details, contacts, branches, documents, quotation and agreement.

### 5. No Centralized Driver Finance Management

The business needs to track driver earnings, advances, deductions, settlements and payment status.

### 6. No Invoice Follow-Up System

Outstanding invoices require manual tracking and follow-up.

### 7. No Centralized Client Tariff Management

Each client/branch may have different rates and tariff rules, making manual billing difficult.

### 8. No Centralized Issue Resolution System

Driver and client complaints, operational problems, payment issues and incidents are currently handled through WhatsApp, phone calls and other informal methods.

--- -->

# 4. Business Objectives

The software should:

- Create one central source of operational information.
- Standardize client onboarding.
- Standardize driver onboarding.
- Centralize booking and duty management.
- Provide real-time operational visibility.
- Automate driver earnings calculations.
- Improve invoice follow-up.
- Reduce billing errors through centralized tariffs.
- Provide proper issue/ticket tracking.
- Provide management dashboards and reports.
- Support multiple companies.
- Provide a Driver Mobile App and a Client App
- Integrate with existing accounting and support systems where required.

---

# 5. Proposed Users

The system should support the following user categories.

## 5.1 Super Administrator

Responsible for:

* System configuration
* Companies
* Users
* Roles
* Permissions
* Integrations
* Master data

## 5.2 Management

Responsible for:

* Business overview
* Client performance
* Driver performance
* Revenue/billing visibility
* Outstanding invoices
* Driver payment visibility
* Reports
* Escalated issues

## The Client:
- Add booking
- See their bookings
- see thier trips, invoices etc

## US - THe COmpany 
- Add boking 
- assign booking by client
- make driver payouts
- raise invoices

## 5.7 Driver

Uses the mobile application for:

* Profile
* Duties
* Trip execution
* Attendance/check-in
* Earnings
* Documents
* Support tickets
* Notifications

---

# 6. Company Management

The platform must support multiple companies.

Example:

* RydCare
* Nine Star Call Drivers
* Just Call Drivers

Each company may have:

* Legal name
* Trade name
* GSTIN
* PAN
* Address
* Contact details
* Bank details
* Status

The system must maintain clear company association for:

* Clients
* Drivers
* Bookings
* Duties
* Billing
* Driver payments
* Reports

A driver may potentially work across more than one operating company, therefore the system must support this scenario without creating duplicate driver records.

---

# 7. Client Management

The system must provide a centralized client master.

## Client information

* Client name
* Legal name
* GSTIN
* PAN
* Address
* Billing address
* Contacts
* Email
* Phone
* Payment terms
* Client status
* Notes
* Documents

## Client:

* Client name
* Legal name
* GSTIN
* PAN
* Address
* Billing address
* Contacts
* Email
* Phone
* Payment terms
* Client status
* Notes
* Documents
* Active/inactive status
* Add services
* Add tariffs/rates
---

# 8. Client Onboarding SOP

The system must provide a standardized onboarding process.

### Required workflow

```text
New Client
    ↓
Client Details
    ↓
GST Details
    ↓
Contact Details
    ↓
Branch Creation
    ↓
Document Collection
    ↓
Quotation
    ↓
Agreement
    ↓
Approval
    ↓
Client Activated
```

The system should show the current onboarding stage and pending actions.

Management should be able to see:

* Clients currently being onboarded
* Pending documents
* Pending quotations
* Pending agreements
* Clients awaiting approval

---

# 9. Client Document Management

The system must allow documents to be associated with clients and branches.

Examples:

* GST certificate
* PAN
* Agreement
* Purchase order
* Quotation
* Rate agreement
* Other client documents

Documents should support:

* Upload
* Download
* Preview where possible
* Document type
* Upload date
* Expiry date where applicable
* Version/history
* Access control

---

# 10. Client Quotation Management

The system should allow staff to:

* Create quotations
* Select client
* Select branch
* Add services
* Add tariffs/rates
* Add terms and conditions
* Apply GST
* Generate PDF
* Send quotation by email
* Track quotation status

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

# 11. Client Follow-Up Management

The system must provide a follow-up/task system.

Examples:

* Quotation follow-up
* Agreement follow-up
* Payment follow-up
* Document follow-up
* New client follow-up
* Tariff revision follow-up

Each follow-up should contain:

* Client
* Contact
* Subject
* Assigned employee
* Due date
* Status
* Notes
* Next follow-up date

Dashboard views should include:

* Today's follow-ups
* Overdue follow-ups
* Upcoming follow-ups
* Completed follow-ups

---

# 12. Client Tariff Management

This is a critical requirement.

Different clients and branches may have different tariff structures.

The system must allow tariff configuration by:

* Company
* Client
* Branch
* Job type
* Distance
* Route
* Time/duration
* Other agreed conditions

Example:

```text
Below 15 km       ₹250
15–20 km          ₹300
Above 20 km       ₹6/km
```

Another client may have a completely different structure.

The system must use the correct tariff automatically when calculating billing.

## Tariff history

When a tariff changes:

* Existing completed jobs must retain the old tariff.
* New jobs should use the new tariff from the effective date.
* The system must maintain tariff history.

---

# 13. Booking Management

All bookings should be centralized in the platform.

A booking should contain:

* Booking ID
* Company
* Client
* Branch
* Customer name
* Customer contact
* Pickup location
* Drop location
* Requested date/time
* Vehicle number
* Vehicle details
* Job type
* Assigned driver
* Tariff
* Driver payable
* Client billable amount
* Notes
* Status

## Booking statuses

The system should support statuses such as:

* Pending
* Assigned
* Confirmed
* Customer No Response
* Ongoing
* Customer Cancelled
* Customer Postponed
* Vehicle Serviced
* Company Driver
* Completed

---

# 14. Booking Creation

Bookings should be possible through:

### Admin/Web

Operations staff can manually create bookings.

### Bulk Booking

Staff should be able to upload/create multiple bookings together.


---

# 15. Driver Management

The system must maintain one centralized driver master.

Driver information should include:

* Driver ID
* Name
* Phone
* Address
* Date of birth where required
* Emergency contact
* Company association
* Driver type
* Joining date
* Status
* Driving licence
* Licence expiry
* Bank information
* Documents
* Profile photo
* Notes

---

# 16. Driver Types

The system must support driver types.

## Salaried Driver

A driver employed and paid through a salary structure.

The system should track:

* Employment details
* Attendance
* Leave
* Duties
* Salary-related reference information

Accounting/payroll processing may remain in the existing accounting system.

## Job-Based Driver

A driver whose earnings are calculated based on completed duties/jobs.

The system must track:

* Jobs completed
* Rate per job
* Earnings
* Advances
* Deductions
* Adjustments
* Settlements
* Payment status

---

# 17. Driver Onboarding

Driver onboarding should follow a standard workflow.

```text
New Driver
    ↓
Personal Details
    ↓
Documents
    ↓
Licence Verification
    ↓
Bank Details
    ↓
Driver Type
    ↓
Approval
    ↓
Driver Account Created
    ↓
Active
```

The system should prevent a driver from becoming active if mandatory onboarding requirements are incomplete.

---

# 18. Driver Document Management

The system should support:

* Document upload
* Document verification
* Document expiry tracking
* Expiry notifications
* Document status
* Document history

Examples:

* Driving licence
* PAN
* ID documents
* Bank documents
* Employment documents
* Agreements
* Other required documents

---

# 19. Driver Availability

Operations staff should be able to see:

* Available drivers
* Assigned drivers
* Drivers currently on duty
* Drivers on leave
* Unavailable drivers
* Offline drivers

The system should prevent accidental assignment of a driver to overlapping duties.

---

# 20. Duty / Trip Management

Every assigned booking becomes a duty/trip.

The system should track:

```text
Booking
   ↓
Driver Assigned
   ↓
Driver Confirmed
   ↓
Driver Starts Duty
   ↓
Trip Ongoing
   ↓
Driver Completes Duty
   ↓
Duty Completed
```

The system should record:

* Start time
* End time
* Start location
* End location
* Distance
* Driver
* Booking
* Vehicle
* Photos
* Status

---

# 21. Driver Mobile Application

The Driver App should provide:

## Dashboard

* Today's duties
* Upcoming duties
* Current status
* Earnings
* Notifications

## Duty Management

* View duty
* View customer/job details
* Start duty
* End duty
* Navigation
* Upload required photos
* View duty history

## Attendance

* Check-in
* Check-out
* Attendance history

## Earnings

* Job earnings
* Completed jobs
* Advances
* Deductions
* Settlement history
* Payment status

## Documents

* View documents
* Upload documents
* Expiry information

## Support

* Create ticket
* View tickets
* Reply to ticket
* View ticket status

---

# 22. Driver Finance Management

This is a major requirement.

The system must provide a driver-level financial view.

Example:

```text
Driver: Ravi

Completed Duties: 42

Gross Earnings       ₹9,360
Advance              ₹2,000
Deduction               ₹300
Adjustment              ₹100
--------------------------------
Net Payable           ₹7,160
```

The system should support:

* Job earnings
* Advances
* Deductions
* Adjustments
* Settlement batches
* Payment status
* Payment history

The system should clearly distinguish:

### Earnings

Money earned from completed duties.

### Settlement

Money actually paid to the driver.

This distinction is important.

---

# 23. Driver Payment Processing

The system should allow authorized users to:

1. Select drivers.
2. Select completed/unpaid duties.
3. Generate a settlement batch.
4. Review earnings.
5. Apply advances/deductions.
6. Approve settlement.
7. Mark payment as processed.
8. Maintain payment history.

The system should prevent the same duty from being paid twice.

---

# 24. Client Billing

Completed duties should generate billable records.

Workflow:

```text
Completed Duty
      ↓
Tariff Calculation
      ↓
Billing Record
      ↓
Invoice
```

The system should maintain the relationship:

```text
Client
  ↓
Booking
  ↓
Duty
  ↓
Billing
  ↓
Invoice
```

---

# 25. Invoice Management

The system should provide visibility into:

* Invoice number
* Client
* Branch
* Invoice date
* Due date
* Invoice amount
* Paid amount
* Outstanding amount
* Payment status
* Overdue days

Accounting integration may be used for final accounting and GST processing.

---

# 26. Invoice Follow-Up

The system must address the current manual invoice follow-up problem.

Dashboard:

```text
OVERDUE INVOICES

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

Staff should be able to:

* View overdue invoices
* Create follow-up
* Record phone call
* Send email
* Add notes
* Set next follow-up date
* Record client response
* Mark payment received
* View follow-up history

---

# 27. Accounting Integration

The system should be capable of integration with the existing accounting system.

Current accounting platform:

**TallyPrime**

The preferred architecture is:

```text
RydCare
   ↓
Tally Integration Layer
   ↓
TallyPrime
```

The integration should support, subject to final technical feasibility:

* Customer/ledger synchronization
* Invoice creation
* Payment synchronization
* Outstanding balance synchronization
* Invoice status
* Financial references

The development vendor should clearly state whether Tally integration is:

* Included
* Partially included
* Third-party integration
* Additional development
* Additional recurring cost

---

# 28. Client / Driver Issue Management

The system requires a centralized issue resolution process.

Issues may include:

### Driver Issues

* Payment issue
* Duty issue
* Attendance issue
* App issue
* Booking issue
* Document issue
* Operational problem

### Client Issues

* Driver complaint
* Booking issue
* Billing issue
* Payment issue
* Service complaint
* Accident/incident
* Additional driver request

---

# 29. Ticket Workflow

The basic ticket workflow should be:

```text
Issue Raised
     ↓
Ticket Created
     ↓
Assigned
     ↓
In Progress
     ↓
Resolution
     ↓
Customer/Driver Confirmation
     ↓
Closed
```

The system should maintain:

* Ticket number
* Category
* Priority
* Reporter
* Client/driver
* Related booking
* Related duty
* Assigned employee
* Description
* Attachments
* Comments
* Status
* Resolution
* Created date
* Closed date

---

# 30. Driver Ticketing in Mobile App

This is a mandatory requirement.

The Driver App must include:

```text
HELP & SUPPORT

[ Create Ticket ]

My Tickets

#1024
Payment Issue
Open

#1021
Duty Issue
Resolved
```

Drivers must be able to:

* Create ticket
* Select category
* Describe issue
* Attach images/documents
* Reference a duty/booking
* View responses
* Reply
* View status
* Receive notification when updated

---

# 31. Client Ticketing

Client support should be accessible through:

* Web interface
* Email integration
* Future client portal

Clients should be able to raise and track issues.

---

# 32. Support Assignment and Escalation

Tickets should support:

* Priority
* Assignment
* Department
* Escalation
* Due date
* SLA where required
* Resolution notes

Management should be able to identify unresolved issues.

---

# 33. Notifications

The system should support:

* Push notifications
* Email
* WhatsApp where applicable
* SMS where applicable
* In-app notifications

Notifications may be triggered by:

* New booking
* Driver assignment
* Duty reminder
* Booking change
* Driver document expiry
* Ticket reply
* Ticket escalation
* Payment update
* Follow-up reminder

---

# 34. Dashboard

The management dashboard should provide a business overview.

### Operations

* Today's bookings
* Pending bookings
* Ongoing duties
* Completed duties
* Available drivers
* Drivers on duty
* Cancelled bookings

### Clients

* Active clients
* Clients onboarding
* Pending quotations
* Pending agreements
* Follow-ups due

### Finance

* Current billing
* Invoices
* Outstanding
* Overdue invoices
* Payment follow-ups
* Driver payables

### Support

* Open tickets
* High-priority tickets
* Overdue tickets
* Driver issues
* Client issues

---

# 35. Reports

The system should provide exportable reports.

## Client Reports

* Client-wise bookings
* Branch-wise bookings
* Client billing
* Client outstanding
* Client payment history

## Driver Reports

* Driver duties
* Driver earnings
* Driver attendance
* Driver performance
* Driver settlements
* Driver document expiry

## Operations Reports

* Daily duties
* Completed duties
* Cancelled duties
* Driver utilization
* Branch activity

## Support Reports

* Open tickets
* Closed tickets
* Tickets by client
* Tickets by driver
* Tickets by category
* Resolution time

---

# 36. Search and Filters

The system should provide global search and filtering.

Users should be able to search by:

* Client
* Branch
* Driver
* Booking ID
* Duty ID
* Invoice number
* Ticket number
* Phone number
* Vehicle number

---

# 37. User Roles and Permissions

The system must support role-based access.

Example roles:

* Super Admin
* Management
* Operations
* Accounts
* HR
* Support
* Driver
* Client

Permissions should be configurable.

Example:

Accounts users should not necessarily be able to modify driver operational information.

Drivers should only access their own information and assigned duties.

---

# 38. Multiple Companies

The system must support multiple operating companies under one platform.

Example:

```text
Company A
Nine Star Call Drivers

Company B
RydCare

Company C
Just Call Drivers
```

The system should support:

* Company-level clients
* Company-level bookings
* Company-level billing
* Company-level reports
* Company-level driver payment records

Where a driver works across companies, the same driver profile should not need to be duplicated unnecessarily.

---

# 39. Document Storage

Documents should be stored using secure cloud/object storage.

The software vendor should specify:

* Storage provider
* Storage cost
* Backup method
* File size limits
* File access control
* Document retention
* Whether storage is included in quotation

---

# 40. Audit Trail

The system should maintain an audit trail for important actions.

Examples:

* Client created
* Client modified
* Driver created
* Driver approved
* Booking created
* Driver assigned
* Booking cancelled
* Duty completed
* Earnings modified
* Settlement approved
* Invoice created
* Payment updated
* Ticket created
* Ticket closed

The audit trail should record:

* User
* Action
* Date/time
* Record
* Previous value where relevant
* New value where relevant

---

# 41. Data Ownership

All business data belongs to the customer.

The vendor should clearly confirm:

* Full database ownership
* Full source-code ownership if applicable
* Ability to export data
* Ability to migrate away from the vendor
* No vendor lock-in
* Documentation provided
* Deployment credentials provided

TaxiAppz currently advertises full source-code ownership on some of its offerings, but this must be explicitly included in the final commercial agreement rather than assumed.

---

# 42. Deployment

The vendor should provide a clear deployment proposal.

The proposal should specify whether the solution will be:

* Cloud hosted
* Customer VPS
* Vendor cloud
* Self-hosted

The customer should have control over:

* Server
* Database
* Source code
* Domain
* Application credentials
* API keys
* Storage

---

# 43. Mobile Application

The Driver App should preferably support:

* Android
* iOS where commercially justified

The vendor must specify:

* Technology used
* Source code availability
* App Store / Play Store deployment
* Push notification infrastructure
* Update process
* Maintenance requirements

---

# 44. Integrations

Potential integrations include:

### Mandatory / High Priority

* TallyPrime
* SMS / OTP provider
* Push notifications
* Email

### Required for Support

* Ticketing system / support backend
* Driver App ticket integration

### Potential Future

* WhatsApp
* GPS/tracking systems
* Maps/routing
* Payment gateway
* Client portal

The vendor should clearly identify which integrations are included in the quotation and which require additional charges.

---

# 45. Non-Functional Requirements

The proposed system should be:

* Secure
* Scalable
* Mobile-friendly
* Reliable
* Easy for non-technical employees to use
* Capable of supporting multiple companies
* Capable of supporting multiple branches
* API-ready
* Maintainable
* Backup-enabled

The system should support future expansion without requiring complete redevelopment.

---

# 46. Customization Requirement

The vendor should not assume that a standard taxi/ride-hailing workflow is sufficient.

RydCare's business is primarily a **B2B driver-service operation**, not a conventional consumer taxi marketplace.

Important differences include:

* Corporate clients
* Service-center branches
* Pre-booked duties
* Acting drivers
* Job-based driver payments
* Salaried drivers
* Client-specific tariffs
* Monthly billing
* Driver settlements
* Client invoice follow-ups
* Client and driver support
* Multiple operating companies

Therefore, the quotation should clearly separate:

1. Existing TaxiAppz functionality
2. Configuration of existing functionality
3. Custom development
4. Third-party integrations
5. Third-party licensing/subscriptions
6. Future enhancements

---

# 47. Vendor Quotation Requirements

TaxiAppz is requested to provide the quotation in the following structure.

## A. Development Cost

Break down by module:

* Client Management
* Client Onboarding
* Quotation Management
* Document Management
* Driver Management
* Driver Onboarding
* Duty/Booking Management
* Tariff Management
* Driver Finance
* Billing
* Invoice Follow-Up
* Issue/Ticket Management
* Driver Mobile App
* Admin Panel
* Reports
* Notifications
* Integrations

## B. Integration Cost

Separately specify:

* Tally integration
* Email
* SMS/OTP
* WhatsApp
* GPS
* Maps
* Ticketing

## C. Infrastructure Cost

Specify:

* Server
* Database
* File storage
* Backup
* Monitoring
* SSL
* Push notifications

## D. Third-Party Costs

Clearly identify any recurring costs.

## E. Maintenance

Specify:

* Warranty period
* Bug-fix period
* AMC
* Support response times
* Upgrade charges
* Store maintenance charges

---

# 48. Development Timeline

Vendor should provide:

* Requirement finalization date
* UI/UX completion
* Backend development
* Admin panel
* Driver app
* Integrations
* Testing
* UAT
* Production deployment

The vendor should identify dependencies and client-side requirements.

---

# 49. Acceptance Criteria

The project should not be considered complete merely because screens have been developed.

Each major module should be tested using actual business workflows.

Examples:

### Client

A staff member must be able to onboard a new client from start to activation.

### Booking

An operations employee must be able to create a booking, assign a driver and complete the duty.

### Driver

A driver must be able to view and complete an assigned duty from the mobile application.

### Driver Finance

The system must calculate earnings from completed job-based duties and generate a settlement.

### Billing

Completed jobs must produce accurate billing based on the client's tariff.

### Invoice Follow-Up

Accounts must be able to identify overdue invoices and record follow-ups.

### Ticketing

A driver must be able to raise a ticket from the mobile app and receive the support team's response.

### Multi-company

The system must correctly separate company-level data and reporting.

---

# 50. Priority Classification

The vendor should classify requirements as:

### P0 — Mandatory

Required for initial production launch.

### P1 — Important

Required shortly after launch.

### P2 — Future

Can be developed later.

---

# 51. Initial P0 Requirements

The following should be considered mandatory for the first production version:

1. Multi-company management
2. Client management
3. Client branches
4. Client onboarding workflow
5. Client documents
6. Quotations
7. Client follow-ups
8. Driver master
9. Driver onboarding
10. Driver documents
11. Employee/Job-based driver classification
12. Driver availability
13. Booking management
14. Driver assignment
15. Duty tracking
16. Driver mobile application
17. Driver earnings
18. Driver advances/deductions
19. Driver settlements
20. Client tariff management
21. Billing
22. Invoice/outstanding tracking
23. Invoice follow-up
24. Driver issue/ticket management
25. Client issue/ticket management
26. Driver ticket creation from mobile app
27. Notifications
28. Management dashboard
29. Basic reports
30. Tally integration

---

# 52. Expected End-to-End Business Workflow

The completed system should allow the business to operate approximately as follows:

```text
CLIENT
  ↓
Client Onboarding
  ↓
Documents + Agreement
  ↓
Quotation
  ↓
Client Activated
  ↓
Booking Received
  ↓
Driver Assigned
  ↓
Driver Completes Duty
  ↓
Driver Earnings Calculated
  ↓
Billing Generated
  ↓
Invoice
  ↓
Tally
  ↓
Payment / Outstanding
  ↓
Follow-Up
  ↓
Payment Received
```

At the same time:

```text
DRIVER
  ↓
Onboarding
  ↓
Documents
  ↓
Approval
  ↓
Driver App
  ↓
Duty Assignment
  ↓
Trip
  ↓
Earnings
  ↓
Settlement
```

And:

```text
CLIENT / DRIVER ISSUE
        ↓
      Ticket
        ↓
    Assignment
        ↓
    Investigation
        ↓
    Resolution
        ↓
      Closure
```

---

# 53. Final Business Requirement

The objective is not to create another generic taxi-booking application.

The objective is to create a **business operating system for a B2B driver-service company** that connects:

```text
CLIENT
   ↓
BOOKING
   ↓
DRIVER
   ↓
DUTY
   ↓
DRIVER EARNINGS
   ↓
CLIENT BILLING
   ↓
INVOICE
   ↓
PAYMENT
```

with a parallel support workflow:

```text
CLIENT / DRIVER
       ↓
     ISSUE
       ↓
    TICKET
       ↓
   RESOLUTION
```

The platform should reduce manual coordination, improve accountability, standardize business processes and provide management with a single operational view of the company.

---

# 54. Vendor Response Requested

TaxiAppz is requested to respond to this BRD with:

1. Module-wise quotation
2. Development timeline
3. Existing features that can be reused
4. Features requiring customization
5. Features requiring new development
6. Integration costs
7. Third-party costs
8. Hosting/infrastructure costs
9. Source-code ownership terms
10. Database ownership terms
11. Deployment model
12. Warranty period
13. AMC/support cost
14. Post-launch support terms
15. Data export/migration capability
16. API availability
17. Documentation provided
18. Payment milestones
19. UAT process
20. Estimated total project cost

---

# 55. Important Commercial Clarification

The vendor should provide **two separate numbers where possible**:

### Option A — Existing Platform + Customization

Cost of using existing TaxiAppz modules and customizing them for RydCare.

### Option B — Fully Custom Development

Cost if the required workflows need substantial changes to the underlying platform.

This will allow RydCare to understand how much of the quotation represents existing functionality versus custom development.

---

# 56. Project Success Criteria

The project will be considered successful when the business can operate its major daily workflows without relying on scattered spreadsheets, WhatsApp messages and manual tracking.

The primary success measures are:

* Every client exists in one centralized system.
* Every active driver has a complete profile.
* Every booking has a traceable status.
* Every duty has a driver and completion record.
* Every job-based driver earning is traceable to completed duties.
* Every client has an applicable tariff.
* Every invoice can be traced back to completed jobs.
* Outstanding invoices can be identified without manually checking multiple files.
* Follow-ups are assigned and tracked.
* Driver and client issues have ticket numbers and resolution history.
* Drivers can raise and track issues from the mobile application.
* Management can view the operational status of the business from one dashboard.
* Financial accounting remains synchronized with the accounting system.

**End of Business Requirements Document**
