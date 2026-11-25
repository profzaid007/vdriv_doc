# BUSINESS REQUIREMENTS DOCUMENT (BRD)

## RydCare / Nine Star Call Drivers

**Project:** Custom Business Operations Management Platform
**Prepared for:** TaxiAppz
**Purpose:** To explain our business requirements and allow TaxiAppz to propose, customize and quote a suitable software solution.

---

# 1. About Our Business

We operate a B2B driver-service business providing professional drivers to:

* Automobile service centres

Our operations include:

* Client and branch management
* Driver management
* Booking and duty management
* Driver assignment
* Pickup/drop services
* Valet and acting drivers
* Client-specific tariffs
* Driver earnings and settlements
* Client billing
* Invoice follow-up
* Client and driver documents
* Client and driver issue management


We want to bring the main business operations into **one centralized platform**.


---

# 2. What We Want From This Project

We are not looking for a generic taxi-booking application.

Our business is primarily a **B2B driver-service operation**.

We want a practical system that helps us manage the complete flow:

**Client → Booking → Driver → Duty → Driver Earnings → Billing → Invoice → Payment Follow-up**

Along with:

**Client / Driver → Issue → Ticket → Resolution**


We are open to using existing TaxiAppz functionality wherever it fits our business. Where customization is required, we expect TaxiAppz to recommend the appropriate solution.

---

# 3. Main Business Problems

The platform should help us solve these current problems:

### Driver Management

We need one place to manage driver information, documents, availability, duties and earnings.

### Driver Onboarding

We need a simple process to collect driver details and documents and activate drivers properly.

### Booking Management

Bookings currently come through different channels. We need one centralized booking system.

### Client Onboarding

New clients should follow a standard process covering client details, branches, documents, quotations and agreements.

### Tariff Management

Different clients and branches have different rates. The system should apply the correct tariff when calculating billing.

### Driver Finance

For job-based drivers, we need to know:

* What duties they completed
* What they earned
* Advances
* Deductions
* Adjustments
* What has already been paid
* What is still payable

### Billing & Invoice Follow-up

We need better visibility of completed jobs, billing, invoices, outstanding amounts and payment follow-ups.

### Issue Management

Client and driver complaints/issues are currently handled through WhatsApp and phone calls. We need proper ticket/issue tracking.

---

# 4. Client Management

The system should have a centralized client database.

Client information should include, where applicable:

* Client name
* Legal name
* GST details
* PAN
* Address
* Billing details
* Contact persons
* Email
* Phone
* Payment terms
* Branches
* Documents
* Services
* Tariffs/rates
* Notes
* Active/inactive status

---

# 6. Client Documents

The system should allow us to store and access documents related to clients and branches.

Examples:

* GST certificate
* PAN
* Agreements
* Purchase orders
* Quotations
* Rate agreements
* Other business documents

Documents should be easy to upload, view and download.

Expiry dates should be supported where required.

---


# 8. Tariff Management

**This is an important part of our business.**

Different clients and branches can have completely different tariff structures.

Tariffs may depend on:

* Client
* Branch
* Job type
* Distance
* Route
* Time/duration
* Other agreed conditions

For example:

| Distance    |  Rate |
| ----------- | ----: |
| Below 15 km |  ₹250 |
| 15–20 km    |  ₹300 |
| Above 20 km | ₹6/km |

Another client may have a completely different tariff.

The system should allow us to configure these rules and automatically calculate the applicable client billing amount.

When a tariff changes, old completed jobs should continue to use the applicable old tariff.

---

# 9. Booking & Duty Management

All bookings should be managed from one system.

A booking should generally contain:

* Client
* Branch
* Customer details
* Pickup
* Drop
* Date/time
* Vehicle number
* Job type
* Assigned driver
* Applicable tariff
* Driver payable
* Client billing amount
* Notes
* Status

Bookings should be possible through the admin/web system.

Bulk booking/import would also be useful for clients or staff handling multiple bookings.

A typical booking flow is:

**Booking Received**

↓

**Driver Assigned**

↓

**Driver Confirmed**

↓

**Duty Started**

↓

**Trip Ongoing**

↓

**Duty Completed**

The system should retain the complete history of the duty.

---

# 10. Driver Management

We need one centralized driver database.

Driver information may include:

* Driver ID
* Name
* Phone number
* Address
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

The System must be able to calcualate driver retention rate

The system should support both:

### Salaried Drivers

Drivers who are employed and paid through a salary arrangement.

### Job-Based Drivers

Drivers whose earnings are based on completed duties/jobs.

A driver may also work across our related operating companies, so the system should avoid unnecessary duplicate driver profiles.

---

# 11. Driver Onboarding & Documents

Driver onboarding should provide a simple process for:

* Personal details
* Documents
* Licence verification
* Bank details
* Driver type
* Approval
* Activation

Driver documents should support expiry tracking where required.

Examples:

* Driving licence
* PAN
* ID documents
* Bank documents
* Employment documents
* Agreements

---

# 12. Driver Availability & Assignment

Operations staff should be able to see the general availability of drivers.

For example:

* Available
* Assigned
* On duty
* On leave
* Unavailable

The system should help prevent assigning the same driver to overlapping duties.

---

# 13. Driver Mobile Application

A Driver App is required.

The app should allow drivers to:

### View Duties

* See assigned duties
* View customer/job details
* View pickup/drop information
* Start duty
* Complete duty
* View duty history
* Track Live Location

### Trip Execution

Where applicable:

* Start/end trip
* Location information
* Navigation
* Upload required photos

### Attendance

* Check-in
* Check-out
* View attendance history

### Earnings

Drivers should be able to see relevant information about:

* Completed jobs
* Earnings
* Advances
* Deductions
* Settlements
* Payment status

### Documents

Drivers should be able to view/upload required documents.

### Support

Drivers must be able to:

* Create a support ticket
* Select an issue
* Add details
* Attach photos/documents
* View their tickets
* Reply to support
* See ticket status

---

# 14. Driver Earnings & Settlements

This is another important part of our business.

For job-based drivers, the system should calculate earnings from completed duties.

Example:

**Driver: Ravi**

Completed duties: 42

Gross earnings: ₹9,360
Advance: ₹2,000
Deduction: ₹300
Adjustment: ₹100

**Net payable: ₹7,160**

The system should clearly distinguish between:

**Earnings** – amount earned from completed duties

and

**Settlement** – amount actually paid to the driver.

The system should allow us to:

1. Select completed unpaid duties
2. Calculate driver earnings
3. Apply advances/deductions
4. Review the settlement
5. Approve it
6. Mark payment as completed
7. Maintain payment history

The same duty should not be paid twice.

---

# 15. Client Billing

Completed duties should become billable records based on the applicable client tariff.

Typical flow:

**Completed Duty**

↓

**Tariff Calculation**

↓

**Billing Record**

↓

**Invoice**

The system should allow us to trace an invoice back to the underlying completed jobs/duties.

---

# 16. Invoice & Payment Follow-up

We need visibility of:

* Invoice number
* Client
* Branch
* Invoice date
* Due date
* Invoice amount
* Paid amount
* Outstanding amount
* Payment status
* Overdue status

Staff should be able to see overdue invoices and record follow-ups.

A follow-up may include:

* Date
* Client/contact
* Notes
* Client response
* Next follow-up date
* Payment status

This should give management a clear view of outstanding receivables without checking multiple spreadsheets.

---


# 18. Client & Driver Support / Issue Management

We need a simple centralized system for handling issues.

### Driver issues may include:

* Payment issues
* Duty issues
* Attendance issues
* App issues
* Booking issues
* Document issues
* Operational problems

### Client issues may include:

* Driver complaints
* Booking issues
* Billing issues
* Payment issues
* Service complaints
* Accident/incident matters
* Additional driver requirements

A basic workflow should be:

**Issue Raised → Assigned → In Progress → Resolved → Closed**

The issue should retain its history and relevant attachments.

Drivers must be able to raise issues directly from the Driver App.

---

# 19. Notifications

The system should support notifications where useful.

Potential notification channels:

* Push notifications
* Email
* SMS
* WhatsApp where applicable

Examples include:

* New booking
* Driver assignment
* Duty reminder
* Booking changes
* Document expiry
* Ticket updates
* Payment updates
* Follow-up reminders

TaxiAppz can recommend which notification methods are most practical.

---

# 20. Management Dashboard

Management should have a simple dashboard showing the current state of the business.

### Operations

* Today's bookings
* Pending bookings
* Ongoing duties
* Completed duties
* Available drivers
* Drivers currently on duty
* Cancelled bookings

### Clients

* Active clients
* Clients being onboarded
* Pending quotations
* Pending follow-ups

### Finance

* Current billing
* Invoices
* Outstanding amounts
* Overdue invoices
* Driver payables

### Support

* Open issues
* Pending issues
* Driver issues
* Client issues

The dashboard should focus on information that helps management understand what is happening in the business.

---

# 21. Reports

The system should provide basic reports that can be viewed and exported.

Useful reports include:

### Client

* Client-wise bookings
* Branch-wise bookings
* Client billing
* Outstanding amounts
* Payment history

### Driver

* Driver duties
* Driver earnings
* Driver attendance
* Driver settlements
* Document expiry

### Operations

* Daily duties
* Completed duties
* Cancelled duties
* Driver utilization
* Branch activity

### Support

* Open issues
* Closed issues
* Issues by client
* Issues by driver
* Issue categories

---

# 22. Multiple Companies

The platform should be capable of supporting our related companies under the same system.

For example:

* RydCare
* Nine Star Call Drivers
* Just Call Drivers

Where required, bookings, billing, driver payments and reports should be associated with the correct company.

A driver may work for more than one of our companies, so we should not have to create duplicate driver records unnecessarily.

---

# 23. Documents & File Storage

The platform will contain business documents and driver documents.

The vendor should propose a suitable secure storage solution.

Please clearly mention:

* Where files will be stored
* Storage cost
* Backup approach
* Any storage limits
* Whether storage is included in the quotation

---

# 24. Mobile & Web Platform

We expect:

### Admin / Business Platform

A web-based system for management and staff to manage the business.

### Driver App

A mobile application for drivers to receive and complete duties, view earnings and raise support issues.

### Client Access

We would like clients to have access to relevant information such as bookings, trips and invoices.

TaxiAppz can recommend whether this should be provided through a client portal, web application or mobile application based on the most practical approach.

---

# 25. Integrations

Potential integrations include:

### Required / Important

* OTP/SMS provider
* Email
* Push notifications

### Depending on the proposed solution

* WhatsApp
* GPS/tracking
* Maps/routing
* Payment gateway

TaxiAppz should clearly identify which integrations are included in the proposed quotation and which involve additional costs.

---

# 26. Important Business Considerations

Our business is different from a normal consumer taxi/ride-hailing business.

Some important characteristics are:

* B2B clients
* Service-centre branches
* Pre-booked duties
* Acting drivers
* Job-based driver payments
* Salaried drivers
* Client-specific tariffs
* Monthly client billing
* Driver settlements
* Invoice follow-ups
* Client and driver support
* Multiple operating companies

Therefore, we need the final product to be adapted to these workflows rather than simply using a standard taxi-booking flow.

---

# 27. What We Need From TaxiAppz

We would like TaxiAppz to review the above requirements and propose the most practical solution.

For each major requirement, please indicate whether it is:

**1. Already available in the existing TaxiAppz platform**

**2. Can be achieved through configuration**

**3. Requires customization**

**4. Requires new development**

**5. Requires a third-party service**

This will help us understand exactly what we are paying for.

---

# 28. Commercial Proposal

Please provide:

### Development

A clear quotation for the proposed system.

### Existing Platform

Please mention which existing TaxiAppz modules/features can be reused.

### Customization

Please mention what needs to be customized specifically for our business.







Pain Points:

- No HR Body 
- Driver Onboarding and duty tracking
- No centralized booking management for clients
- No Client Onboarding SOPs 
- No Driver Finance management
- No Invoice followups
- No Client Tariff management
- No Driver / Client issue resolution management system