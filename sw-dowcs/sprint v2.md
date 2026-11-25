# RYDCARE PLATFORM --- COMPLETE DEVELOPMENT SPRINT PLAN

## 1. PLATFORM VISION

RydCare is a centralized driver-service platform supporting two business
channels:

    B2B

        Corporate clients

        ↓

        Client Branches

        ↓

        Bookings

        ↓

        Drivers

        ↓

        Duties / Trips

        ↓

        Billing / Invoices

    B2C

        General Public

        ↓

        Customer Booking

        ↓

        Driver Matching / Assignment

        ↓

        Driver

        ↓

        Duties / Trips

        ↓

        Customer Payment

        ↓

        Driver Settlement

Both channels share the same operational core.

They should NOT become two separate systems.

The common core is:

    CUSTOMER / CLIENT

           ↓

        BOOKING

           ↓

       DISPATCH

           ↓

        DRIVER

           ↓

         DUTY

           ↓

          TRIP

           ↓

    FINANCIAL PROCESSING

------------------------------------------------------------------------

2.  CORE BUSINESS MODEL

------------------------------------------------------------------------

### Organization

Organization is primarily a financial / billing entity.

Examples:

-   Nine Star Call Drivers

-   Just Call Drivers

-   Future: Nine Star Call Drivers - Madurai

-   Future: Just Call - Karnataka

Organization controls:

-   Legal entity

-   GSTIN

-   Invoice issuer

-   Billing

-   Accounting

-   Financial ownership

### City

City is the primary operational boundary.

City affects:

-   Booking

-   Driver assignment

-   Driver availability

-   Operations

-   Dispatch

-   Tariffs

-   Branch operations

-   Reporting

Examples:

-   Chennai

-   Madurai

-   Coimbatore

-   Bangalore

-   Hyderabad

### Client

Client is a B2B customer.

Examples:

-   KUN Hyundai

-   Renault

-   SVM

-   ITC

-   Other corporate customers

### Branch

A Client can have multiple branches.

Every branch:

-   Belongs to exactly one Client

-   Operates in exactly one City

Example:

    KUN Hyundai

        ├── Guindy → Chennai

        ├── Ambattur → Chennai

        └── Madurai → Madurai

### B2C Customer

B2C customers are members of the general public.

They do NOT belong to:

-   A Client

-   A Branch

-   An Organization

They directly book RydCare driver services.

Example:

    Customer

       ↓

    RydCare

       ↓

    Driver

------------------------------------------------------------------------

## CORE SERVICE TYPES, DRIVER ENGAGEMENTS & PAYMENT MODEL

### 1. Global `service_types` Master

Maintain one global `service_types` master used consistently by
branches, drivers, bookings, trips, billings and job cards.

Initial values:

-   `RTO`
-   `Service`
-   `Valet`
-   `Customer - General Public`

Suggested fields:

-   `id`
-   `name`
-   `code`
-   `description`
-   `is_active`
-   `created_at`
-   `updated_at`

Use foreign keys to this master wherever a record needs to identify its
service type. Keep the service type as a shared operational
classification; do not create separate service types for monthly, acting
or weekly-paid drivers.

Also retain `booking_source = B2B | B2C` as a separate field.
`service_type` describes the selected category of service in RydCare,
while `booking_source` distinguishes corporate bookings from bookings
made by the general public. A B2C booking has a public customer and no
client or branch; a B2B booking belongs to a client and branch and may
also identify the end customer receiving the service.

### 2. Branch and Driver Service Eligibility

Use mappings to define where a service is offered and which services a
driver can perform:

-   `branch_service_types`: `branch_id`, `service_type_id`, active
    status and any branch-specific configuration.
-   `driver_service_types`: `driver_id`, `service_type_id`,
    verification/eligibility status and effective dates where required.

These mappings represent availability or capability. They do not define
how a driver is engaged or paid.

### 3. Driver Engagement Categories

Keep driver engagement separate from service type. Examples:

  Driver arrangement                  Service type   Engagement mode
  ----------------------------------- -------------- -----------------
  Valet - Acting                      Valet          `ACTING`
  Valet - Monthly                     Valet          `MONTHLY`
  Service - Monthly                   Service        `MONTHLY`
  Service - Job-based / Weekly paid   Service        `JOB_BASED`

The engagement mode can initially be a controlled enum or validated set
of values. Create a configurable master table only if administrators
need to add engagement modes without a code change.

Use a `driver_engagements` record to track the arrangement that applies
to a driver, potentially for a particular branch and time period.
Suggested fields:

-   `driver_id`
-   `service_type_id`
-   `engagement_mode`
-   `payment_scheme_id`
-   `branch_id` where the arrangement is branch-specific
-   `effective_from`
-   `effective_to`
-   `status`

Keep history when a driver's arrangement changes. Do not overwrite
historical arrangements that were used to calculate previous earnings.

### 4. Driver Payment Schemes

Use `driver_payment_schemes` to define how a driver is compensated and
settled. Suggested fields:

-   `id`
-   `name`
-   `compensation_method` (for example, job-based or monthly)
-   `settlement_frequency` (for example, weekly or monthly)
-   `incentive_rules` or a related incentive configuration
-   `effective_from`
-   `effective_to`
-   `is_active`

Example arrangements:

-   Valet - Acting: job-based earnings, settled weekly.
-   Valet - Monthly: monthly compensation, settled monthly, with
    applicable incentives.
-   Service - Monthly: monthly compensation, settled monthly, with
    applicable incentives.
-   Service - Job-based: job-based earnings, settled weekly.

These are examples, not hardcoded universal rules. The applicable scheme
should be configured and attached to the driver's engagement.

### 5. Separate Client Pricing from Driver Compensation

A client tariff and a driver's earnings are independent calculations.

-   The pricing engine calculates what RydCare charges the client or
    public customer.
-   The driver compensation rules calculate what the driver earns or
    owes.
-   Do not assume that the client price equals the driver's earning.
-   Preserve the applicable rate and calculation snapshot against the
    completed booking/trip or financial transaction so that later tariff
    changes do not rewrite history.

### 6. B2B Driver Earnings vs B2C Driver Commission

#### B2B

For corporate work, RydCare calculates the driver's earnings and
normally settles them according to the driver's payment scheme. The
corporate client is billed separately using the applicable client
tariff.

#### B2C --- Customer Pays the Driver Directly

For the general-public model where the customer pays the driver
directly:

1.  Calculate and record the customer fare for the completed booking.
2.  Apply the configured RydCare commission rule, initially expected to
    be within 10--15% where that rule applies.
3.  Record the commission amount the driver owes RydCare as a
    receivable/commission obligation.
4.  Record each commission remittance from the driver against that
    obligation.
5.  Keep any unpaid balance outstanding until it is settled, adjusted or
    written off through an authorized process.

Do not record the customer fare as cash received by RydCare if the
customer paid the driver directly.

Create `driver_commission_rules` with fields such as:

-   `id`
-   `service_type_id` where the rule varies by service
-   applicable city or other scope fields where required
-   `commission_type` (`PERCENTAGE` or `FIXED_AMOUNT`)
-   `commission_value`
-   `effective_from`
-   `effective_to`
-   `is_active`

Snapshot the commission rate, commission basis, customer fare and
calculated commission amount on the applicable booking/financial record.
A later commission-rule change must not change historical commission
obligations.

Track B2C commission receivables and remittances separately from B2B
driver earnings payable. Do not force both into a single balance with an
ambiguous meaning.

### 7. Driver Finance Records

The driver financial module must distinguish at least:

-   Driver earnings payable by RydCare
-   Advances
-   Deductions
-   Adjustments
-   Payments made by RydCare to a driver
-   B2C commission receivable from a driver
-   Commission remittances received from a driver

Keep financial entries auditable and immutable. Corrections should be
represented by adjustment/reversal entries rather than silently editing
posted transactions. The balance shown to the user must clearly state
whether it is an amount RydCare owes the driver or an amount the driver
owes RydCare.

### 8. Sprint Impact

Apply these requirements within the existing sprints; a separate
microservice or separate booking engine is not required.

  -----------------------------------------------------------------------
  Sprint                              Requirements to include
  ----------------------------------- -----------------------------------
  Sprint 1 - Organization & City      Maintain city scope for service
  Management                          availability, pricing and
                                      commission rules where applicable.

  Sprint 2 - Client & Branch          Configure the service types offered
  Management                          by each branch through
                                      `branch_service_types`.

  Sprint 4 - Driver Management        Maintain driver service eligibility
                                      through `driver_service_types`.

  Sprint 5 - Driver Onboarding &      Assign and maintain a driver's
  Offboarding                         engagement arrangement and
                                      effective dates; include
                                      settlement/commission clearance
                                      during offboarding.

  Sprint 6 - Vehicles & Job Types     Keep vehicle/job types distinct
                                      from global service types and
                                      driver engagement modes. Use
                                      `service_types` as the shared
                                      service master.

  Sprint 8 - Tariff & Pricing Engine  Support B2B tariffs and B2C
                                      customer pricing, plus configurable
                                      driver commission rules. Keep
                                      client price and driver
                                      compensation separate.

  Sprint 9 - Core Booking Engine      Store `service_type_id`,
                                      `booking_source`,
                                      customer/client/branch
                                      relationships and applicable
                                      pricing/commission snapshots. B2C
                                      bookings must not require a client
                                      or branch.

  Sprint 11 - Duty & Trip Management  Carry the booking's service type
                                      through duty/trip records and
                                      retain the completed work needed
                                      for financial calculations.

  Sprint 14 - Driver Financial Ledger Track driver earnings payable and
                                      B2C commission
                                      receivable/remittances as distinct
                                      financial balances.

  Sprint 15 - B2B Billing & Invoices  Calculate corporate billing and
                                      receivables independently of driver
                                      earnings.

  Sprint 16 - B2C Payments & Customer Support the configured customer
  Checkout                            payment flow, including direct
                                      customer-to-driver payment where
                                      used, commission obligation
                                      creation, remittance tracking and
                                      outstanding balances.

  Sprint 23 - Reporting & Analytics   Report separately on B2B driver
                                      payables, B2C commission
                                      receivables, collected commission
                                      and outstanding commission.

  Sprint 24 - External Integrations   Isolate payment, messaging,
                                      tracking and accounting providers
                                      behind adapters; integrations must
                                      not replace RydCare's core
                                      financial records.
  -----------------------------------------------------------------------

3.  AUTHORIZATION MODEL

------------------------------------------------------------------------

RydCare uses:

    RBAC + RESOURCE SCOPE

Role answers:

    WHAT can the user do?

Scope answers:

    WHERE can the user do it?

Example:

    Role:

        Operations Manager

    Permission:

        booking.view

    Scope:

        Chennai

The user can view Chennai bookings.

They cannot automatically view:

    Madurai bookings

    Coimbatore bookings

### Scope Types

Potential scopes:

-   Global

-   Organization

-   City

-   Client

-   Branch

Not every role needs every scope.

Examples:

    Super Admin

        Global

    Operations Manager

        City = Chennai

    Finance

        Organization = Nine Star

    Client Admin

        Client = KUN Hyundai

    Branch User

        Branch = KUN Guindy

### Temporary Access

Temporary access must be supported.

Example:

    User:

        Arun

    Permanent:

        Chennai

    Temporary:

        Madurai

    Start:

        10 Oct

    End:

        20 Oct

### Important Rule

Do NOT create:

    Chennai Operations Manager

    Madurai Operations Manager

Instead create:

    Operations Manager

        +

    City Scope

------------------------------------------------------------------------

4.  HIGH-LEVEL PLATFORM ARCHITECTURE

------------------------------------------------------------------------

                         RYDCARE

                            │

             ┌──────────────┴──────────────┐

             │                             │

            B2B                           B2C

             │                             │

      Corporate Clients              General Public

             │                             │

         Branches                    Customer Booking

             │                             │

             └──────────────┬──────────────┘

                            │

                     COMMON BOOKING

                         ENGINE

                            │

                        DISPATCH

                            │

                         DRIVER

                            │

                          DUTY

                            │

                          TRIP

                            │

              ┌─────────────┴─────────────┐

              │                           │

          B2B BILLING                 B2C PAYMENT

              │                           │

          RECEIVABLES                 REFUNDS

              │                           │

              └─────────────┬─────────────┘

                            │

                       DRIVER FINANCE

------------------------------------------------------------------------

5.  DEVELOPMENT APPROACH

------------------------------------------------------------------------

Every sprint follows five aspects:

1.  Business Requirements

2.  DB Design

3.  API

4.  API Integration

5.  Testing

UI/UX is designed separately and integrated during API Integration.

Development principle:

    CRUD

      ↓

    Validation

      ↓

    Permissions

      ↓

    Scope Filtering

      ↓

    Business Rules

      ↓

    Workflows

      ↓

    Integrations

Do not build complex workflows before the underlying CRUDs

and relationships are stable.

**==================================================**

SPRINT 0 --- PLATFORM FOUNDATION & AUTHORIZATION

**==================================================**

### Objective

Build the technical and authorization foundation for the entire

RydCare platform.

RBAC and scope are Day-0 requirements.

### 0.1 Business Requirements

Define:

-   Centralized application

-   Authentication

-   Users

-   Roles

-   Permissions

-   Scopes

-   Temporary access

-   Organizations

-   Cities

-   Basic audit foundation

Initial roles:

-   Super Admin

-   Admin

-   Operations Manager

-   Operations Executive

-   Finance

-   Branch User

-   Support

-   Driver

### 0.2 DB Design

Create:

    users

    roles

    permissions

    role_permissions

    user_roles

    organizations

    cities

    user_organizations

    user_cities

    user_clients

    user_branches

### 0.3 API

Authentication:

    POST   /api/v1/auth/login

    POST   /api/v1/auth/logout

    POST   /api/v1/auth/refresh

    GET    /api/v1/auth/me

Users:

    GET    /api/v1/users

    POST   /api/v1/users

    GET    /api/v1/users/:id

    PATCH  /api/v1/users/:id

    DELETE /api/v1/users/:id

Roles:

    GET    /api/v1/roles

    POST   /api/v1/roles

    GET    /api/v1/roles/:id

    PATCH  /api/v1/roles/:id

Permissions:

    GET /api/v1/permissions

Organizations:

    GET    /api/v1/organizations

    POST   /api/v1/organizations

    GET    /api/v1/organizations/:id

    PATCH  /api/v1/organizations/:id

Cities:

    GET    /api/v1/cities

    POST   /api/v1/cities

    GET    /api/v1/cities/:id

    PATCH  /api/v1/cities/:id

### 0.4 API Integration

Build:

-   Login

-   User management

-   Role management

-   Permission management

-   Organization management

-   City management

-   User access management

### 0.5 Testing

Test:

-   Authentication

-   Permission checks

-   Organization restrictions

-   City restrictions

-   Multiple-city access

-   Temporary access

-   Expired access

-   Unauthorized API calls

-   Direct API manipulation

-   Cross-scope access

**==================================================**

SPRINT 1 --- ORGANIZATION & CITY MANAGEMENT

**==================================================**

### Objective

Establish the financial and operational structure.

### 1.1 Business Requirements

Organizations:

-   Legal entity

-   GSTIN

-   Billing details

-   Invoice settings

-   Status

Cities:

-   City name

-   State

-   Status

-   Operational configuration

### 1.2 DB Design

    organizations

    cities

    locations + outskirts

### 1.3 API

Organization CRUD.

City CRUD.

Organization status management.

City status management.

### 1.4 API Integration

Build:

-   Organization management

-   City management

-   Cities Location Management

-   Organization selection

-   City selection

### 1.5 Testing

Test:

-   CRUD

-   Duplicate GSTIN

-   Duplicate city

-   Inactive organization

-   Inactive city

-   Permission restrictions

**==================================================**

SPRINT 2 --- CLIENT & BRANCH MANAGEMENT

**==================================================**

### Objective

Build the B2B customer hierarchy.

### 2.1 Business Requirements

Client lifecycle:

    Lead

      ↓

    Onboarding

      ↓

    Active

      ↓

    Suspended / Inactive

Client contains:

-   Company details

-   GST details

-   Contacts

-   Documents

-   Billing information

-   Status

Client branches:

    Client

       ↓

    Branch

       ↓

    City

### 2.2 DB Design

    clients

    client_documents

    branches

    branches_documents

Relationships:

    Client

       ├── Branches

       ├── Contacts

       ├── Documents

       └── Users

### 2.3 API

Client:

    GET    /api/v1/clients

    POST   /api/v1/clients

    GET    /api/v1/clients/:id

    PATCH  /api/v1/clients/:id

    DELETE /api/v1/clients/:id

Branches:

    GET    /api/v1/branches

    POST   /api/v1/branches

    GET    /api/v1/branches/:id

    PATCH  /api/v1/branches/:id

    DELETE /api/v1/branches/:id

Nested:

    GET /api/v1/clients/:id/branches

### 2.4 API Integration

Build:

-   Client list

-   Client details

-   Branch list

-   Branch creation

-   Branch details

-   Client contacts

-   Client users

### 2.5 Testing

Test:

-   Client CRUD

-   Branch CRUD

-   Branch belongs to exactly one client

-   Branch belongs to exactly one city

-   Client scope

-   Branch scope

-   City scope

-   Client user access

**==================================================**

SPRINT 3 --- CLIENT USER & OPERATIONAL ACCESS

**==================================================**

### Objective

Complete the user-access model for B2B operations.

### 3.1 Business Requirements

Support:

-   Operations users

-   Finance users

-   Client users

-   Branch users

-   Support users

Examples:

    Operations Manager

        → Chennai

    Finance

        → Nine Star

    Client Admin

        → KUN Hyundai

    Branch User

        → KUN Guindy

Future:

    Operations Manager

        → Chennai

        → Branch Group

### 3.2 DB Design

Finalize:

    user_organizations

    user_cities

    user_clients

    user_branches

Future-ready:

    operation_teams

    operation_team_users

    operation_team_branches

Do not build complex team management until required.

### 3.3 API

User scope management:

    GET /users/:id/access

    POST /users/:id/organizations

    POST /users/:id/cities

    POST /users/:id/clients

    POST /users/:id/branches

Temporary access:

    POST /users/:id/access

    PATCH /users/:id/access/:accessId

    DELETE /users/:id/access/:accessId

### 3.4 API Integration

Build:

-   User access screen

-   Role assignment

-   City assignment

-   Organization assignment

-   Client assignment

-   Branch assignment

-   Temporary access

### 3.5 Testing

Test:

-   User with multiple cities

-   User with multiple organizations

-   Client user restrictions

-   Branch user restrictions

-   Temporary access

-   Expired access

-   Permission + scope combinations

**==================================================**

SPRINT 4 --- DRIVER MANAGEMENT

**==================================================**

### Objective

Create the driver master and operational driver pool.

### 4.1 Business Requirements

Driver:

-   Personal information

-   Contact

-   Address

-   Driver ID

-   Status

-   Documents

-   Emergency contact

-   Joining information

Driver lifecycle:

    Applicant

       ↓

    Onboarding

       ↓

    Active

       ↓

    Suspended

       ↓

    Offboarded

Driver operational model:

    Driver

       ↓

    Primary City

       ↓

    Optional Branch Associations

Branch association means:

    Eligible / Pinned Driver

NOT:

    Branch Owner

### 4.2 DB Design

    drivers

    driver_documents

    driver_cities

    driver_branches

### 4.3 API

Driver CRUD.

Driver city assignment.

Driver branch assignment.

Driver status management.

### 4.4 API Integration

Build:

-   Driver list

-   Driver profile

-   Driver creation

-   City assignment

-   Branch pinning

-   Driver search

-   Driver filters

### 4.5 Testing

Test:

-   CRUD

-   City scope

-   Branch eligibility

-   Driver status

-   Duplicate driver

-   Suspended driver

-   Offboarded driver

**==================================================**

SPRINT 5 --- DRIVER ONBOARDING & OFFBOARDING

**==================================================**

### Objective

Complete driver lifecycle management.

### 5.1 Business Requirements

Onboarding:

-   Personal details

-   Documents

-   Verification

-   Approval

-   City

-   Branch eligibility

-   Assets

Offboarding:

-   Pending earnings

-   Advances

-   Deductions

-   Assets

-   Accident records

-   Final settlement

-   Account deactivation

### 5.2 DB Design

    driver_verifications

    driver_assets

    driver_offboarding

### 5.3 API

Onboarding workflow.

Verification workflow.

Approval workflow.

Offboarding workflow.

### 5.4 API Integration

Build:

-   Driver onboarding

-   Document upload

-   Verification

-   Approval

-   Offboarding

### 5.5 Testing

Test:

-   Missing documents

-   Verification failure

-   Approval

-   Offboarding

-   Pending financial amounts

-   Asset clearance

**==================================================**

SPRINT 6 --- VEHICLES & JOB TYPES

**==================================================**

### Objective

Build operational master data.

### 6.1 Business Requirements

Job types:

-   Pickup / Drop

-   RTO

-   Valet

-   Monthly Driver

-   Acting Driver

-   Other future services

Vehicles:

-   Registration number

-   Make

-   Model

-   Vehicle type

-   Fuel type where required

### 6.2 DB Design

    vehicles

    vehicle_types

    job_types

Vehicles should be reusable across:

-   B2B bookings

-   B2C bookings

-   Customers

-   Trips

### 6.3 API

CRUD for:

-   Vehicles

-   Vehicle types

-   Job types

### 6.4 API Integration

Build master screens.

### 6.5 Testing

Test:

-   CRUD

-   Duplicate registration

-   Inactive job type

-   Vehicle validation

-   Authorization

**==================================================**

SPRINT 7 --- B2C CUSTOMER & CUSTOMER IDENTITY

**==================================================**

### Objective

Introduce the public consumer side of RydCare.

B2C customers are independent users.

They do NOT belong to clients or branches.

### 7.1 Business Requirements

Customer:

-   Name

-   Phone

-   Email

-   Address

-   Status

-   Vehicles

-   Communication preferences

Authentication:

    Phone

      ↓

    OTP

      ↓

    Customer Account

Customer can:

-   Maintain profile

-   Maintain vehicles

-   Create bookings

-   View bookings

-   Track driver

-   View trip history

-   View payments

-   Raise issues

-   Provide feedback

B2B end customers may also exist on bookings,

but they are not client users.

### 7.2 DB Design

    customers

    customer_vehicles

    customer_sessions / access_tokens

    customer_feedback

Customer must remain independent from:

    clients

    branches

    organizations

### 7.3 API

Customer:

    POST   /api/v1/customer/auth/request-otp

    POST   /api/v1/customer/auth/verify-otp

    GET    /api/v1/customer/me

    PATCH  /api/v1/customer/me

Vehicles:

    GET    /api/v1/customer/vehicles

    POST   /api/v1/customer/vehicles

    PATCH  /api/v1/customer/vehicles/:id

    DELETE /api/v1/customer/vehicles/:id

### 7.4 API Integration

Build:

-   Customer mobile web / app foundation

-   OTP login

-   Profile

-   Saved vehicles

### 7.5 Testing

Test:

-   OTP

-   Authentication

-   Session expiry

-   Customer isolation

-   Vehicle ownership

-   Unauthorized access

**==================================================**

SPRINT 8 --- TARIFF & PRICING ENGINE

**==================================================**

### Objective

Build one pricing engine capable of supporting both B2B

and B2C pricing.

### 8.1 Business Requirements

Pricing may depend on:

-   Organization

-   Client

-   Branch

-   City

-   Booking source

-   Job type

-   Distance

-   Route

-   Vehicle type

-   Time

-   Effective date

B2B:

    Client-specific tariff

B2C:

    Public tariff

Pricing models:

-   Flat

-   Per Km

-   Distance slab

-   Route based

-   Time based

-   Hybrid

Historical bookings must retain the rate used.

### 8.2 DB Design

    tariffs

    tariff_rules

    tariff_versions

### 8.3 API

    GET  /api/v1/tariffs

    POST /api/v1/tariffs

    GET  /api/v1/tariffs/:id

    PATCH /api/v1/tariffs/:id

Pricing:

    POST /api/v1/pricing/calculate

### 8.4 API Integration

Build:

-   Tariff management

-   Pricing rules

-   Effective dates

-   Pricing preview

-   B2B pricing

-   B2C pricing

### 8.5 Testing

Test:

-   Flat pricing

-   Per-km

-   Distance slabs

-   Route pricing

-   Effective dates

-   Historical pricing

-   Client-specific tariff

-   Public tariff

-   Wrong city

-   Wrong branch

-   Wrong job type

**==================================================**

SPRINT 9 --- CORE BOOKING ENGINE

**==================================================**

### Objective

Build a common booking engine for both B2B and B2C.

This is one of the most important modules in the platform.

### 9.1 Business Requirements

Booking must support:

    B2B

    B2C

B2B booking:

    Client

       ↓

    Branch

       ↓

    City

       ↓

    End Customer

       ↓

    Vehicle

       ↓

    Driver

B2C booking:

    Customer

       ↓

    City

       ↓

    Vehicle

       ↓

    Driver

Client and Branch are therefore OPTIONAL at the booking level.

Customer is the end-service recipient.

Booking statuses:

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

### 9.2 DB Design

    bookings

Booking should support:

-   booking_source

-   organization

-   client nullable

-   branch nullable

-   city

-   customer

-   vehicle

-   job type

-   assigned driver

-   tariff

-   driver payable

-   customer/client billable amount

-   status

-   pickup

-   drop

-   scheduled time

-   notes

-   attachments

Conceptually:

    B2B:

    Source = B2B

    Client = KUN

    Branch = KUN Guindy

    Customer = Mr. Kumar

    B2C:

    Source = B2C

    Client = NULL

    Branch = NULL

    Customer = Mr. Kumar

### 9.3 API

CRUD:

    GET    /api/v1/bookings

    POST   /api/v1/bookings

    GET    /api/v1/bookings/:id

    PATCH  /api/v1/bookings/:id

Commands:

    POST /api/v1/bookings/:id/assign

    POST /api/v1/bookings/:id/reassign

    POST /api/v1/bookings/:id/confirm

    POST /api/v1/bookings/:id/cancel

    POST /api/v1/bookings/:id/postpone

### 9.4 API Integration

Build:

-   B2B booking form

-   B2B booking list

-   B2C booking creation

-   Booking details

-   Booking source

-   Booking filters

-   City filters

-   Client filters

-   Branch filters

### 9.5 Testing

Test:

-   B2B booking

-   B2C booking

-   Client-less booking

-   Branch-less booking

-   Customer ownership

-   City scope

-   Client scope

-   Branch scope

-   Status transitions

-   Invalid combinations

**==================================================**

SPRINT 10 --- DISPATCH & DRIVER ASSIGNMENT

**==================================================**

### Objective

Build the driver assignment engine.

This module serves both B2B operations and B2C marketplace bookings.

### 10.1 Business Requirements

B2B:

    Booking

       ↓

    Operations

       ↓

    Driver assignment

B2C:

    Booking

       ↓

    Driver discovery

       ↓

    Eligible drivers

       ↓

    Offer / assignment

       ↓

    Driver acceptance

Driver matching can consider:

-   City

-   Driver availability

-   Driver status

-   Branch eligibility

-   Location

-   Distance

-   Job type

-   Vehicle requirements

### 10.2 DB Design

    driver_availability

    driver_location_status

    driver_job_offers

    driver_assignments

### 10.3 API

B2B:

    POST /bookings/:id/assign

    POST /bookings/:id/reassign

B2C:

    POST /bookings/:id/request-driver

    GET  /bookings/:id/eligible-drivers

    POST /driver-offers/:id/accept

    POST /driver-offers/:id/reject

### 10.4 API Integration

Build:

-   Operations assignment

-   Driver availability

-   Driver offer

-   Driver acceptance

-   Assignment status

### 10.5 Testing

Test:

-   Eligible driver filtering

-   City restriction

-   Driver availability

-   Duplicate assignment

-   Driver acceptance

-   Driver rejection

-   Assignment timeout

-   B2B manual assignment

-   B2C assignment

**==================================================**

SPRINT 11 --- DUTY & TRIP MANAGEMENT

**==================================================**

### Objective

Convert bookings into actual driver operations.

### 11.1 Business Requirements

Flow:

    Booking

       ↓

    Assigned

       ↓

    Driver Confirmed

       ↓

    Start Duty

       ↓

    Trip

       ↓

    End Duty

       ↓

    Completed

Trip captures:

-   Start time

-   End time

-   GPS

-   Distance

-   Photos

-   Customer confirmation

-   Trip status

### 11.2 DB Design

    duties

    trips

### 11.3 API

    POST /duties/:id/confirm

    POST /duties/:id/start

    POST /duties/:id/end

    POST /duties/:id/complete

Trip APIs.

### 11.4 API Integration

Integrate:

-   Driver app

-   Operations dashboard

-   Customer experience

-   Trip status

### 11.5 Testing

Test:

-   Booking → Duty

-   Duty → Trip

-   Start duty

-   End duty

-   Duplicate start

-   Duplicate end

-   Invalid transitions

-   Driver authorization

-   Customer confirmation

**==================================================**

SPRINT 12 --- TRACKING & LOCATION

**==================================================**

### Objective

Provide live driver location capability.

This becomes important for both B2B operations and B2C.

### 12.1 Business Requirements

Track:

-   Driver location

-   Driver availability

-   Current trip

-   Last known location

-   ETA where supported

B2B:

    Operations

       ↓

    Track assigned driver

B2C:

    Customer

       ↓

    Track assigned driver

### 12.2 DB Design

Initial operational data may remain in PostgreSQL.

High-frequency location data can be handled through

Traccar / suitable tracking infrastructure when required.

Potential:

    driver_locations

    tracking_sessions

### 12.3 API

    GET /drivers/:id/location

    GET /trips/:id/location

    GET /customer/bookings/:id/tracking

### 12.4 API Integration

Integrate:

-   Traccar

-   Maps

-   Driver app

-   Customer tracking

-   Operations tracking

### 12.5 Testing

Test:

-   Location updates

-   Stale location

-   Tracking permissions

-   Customer tracking isolation

-   Driver privacy

-   Trip-only tracking

**==================================================**

SPRINT 13 --- DUTY VERIFICATION

**==================================================**

### Objective

Verify completed duties before financial processing.

### 13.1 Business Requirements

Operations verifies:

-   Driver

-   Distance

-   Start/end

-   Photos

-   Customer details

-   Exceptions

-   Trip information

Statuses:

    Pending Verification

    Verified

    Rejected

    Requires Correction

### 13.2 DB Design

    duty_verifications

### 13.3 API

    GET  /duties/pending-verification

    POST /duties/:id/verify

    POST /duties/:id/reject

    POST /duties/:id/request-correction

### 13.4 API Integration

Build:

-   Verification queue

-   Duty details

-   Verification actions

-   Correction flow

### 13.5 Testing

Test:

-   Verification

-   Rejection

-   Correction

-   Permission restrictions

-   Unverified duty cannot enter final financial processing

**==================================================**

SPRINT 14 --- DRIVER FINANCIAL LEDGER

**==================================================**

### Objective

Calculate what RydCare owes each driver.

### 14.1 Business Requirements

Ledger transaction types:

    EARNING

    ADVANCE

    DEDUCTION

    ADJUSTMENT

    PAYMENT

Formula:

    Driver Payable

    =

    Earnings

    + Adjustments

    - Advances

    - Deductions

    - Payments

Flow:

    Completed Duty

         ↓

    Driver Tariff

         ↓

    Driver Earning

         ↓

    Ledger Entry

This must support both:

    B2B driver earnings

and:

    B2C driver earnings

For B2C:

    Customer Payment

         ↓

    RydCare

         ├── Driver Earnings

         └── RydCare Commission

### 14.2 DB Design

    driver_earnings

    driver_ledger_entries

    driver_advances

    driver_deductions

    driver_adjustments

    driver_settlements

    driver_payments

Ledger entries should be immutable.

### 14.3 API

Earnings:

    GET /driver-earnings

    GET /drivers/:id/earnings

Advances:

    GET

    POST

    PATCH

Deductions:

    GET

    POST

    PATCH

Adjustments:

    GET

    POST

Payments:

    GET

    POST

Settlements:

    GET

    POST

    PATCH

Balance:

    GET /drivers/:id/ledger

    GET /drivers/:id/balance

### 14.4 API Integration

Build:

-   Driver earnings

-   Advances

-   Deductions

-   Adjustments

-   Payments

-   Settlement

-   Balance payable

### 14.5 Testing

Test:

-   Earnings

-   Advances

-   Advance recovery

-   Deductions

-   Adjustments

-   Partial payments

-   Full payments

-   Settlement

-   Ledger consistency

-   B2C commission

-   Historical earnings

**==================================================**

**Additional B2C requirement:** In addition to the driver-payable
ledger, track commission receivable from drivers for B2C bookings where
customers pay drivers directly. Record commission obligations and
remittances separately from earnings payable; never treat a direct
customer-to-driver payment as money received by RydCare.

SPRINT 15 --- B2B BILLING & INVOICES

**==================================================**

### Objective

Generate customer billing for corporate clients.

### 15.1 Business Requirements

Flow:

    Completed Duties

         ↓

    Billing Records

         ↓

    Invoice

         ↓

    Organization

         ↓

    Client

Organization controls:

-   Legal entity

-   GSTIN

-   Invoice issuer

-   Invoice numbering

Client / Branch / City control:

-   Services

-   Tariffs

-   Billing records

### 15.2 DB Design

    billing_records

    invoices

    invoice_items

    bill_books

### 15.3 API

Billing:

    GET  /billing-records

    POST /billing-records

    GET  /billing-records/:id

Invoices:

    GET  /invoices

    POST /invoices

    GET  /invoices/:id

    POST /invoices/:id/generate

    POST /invoices/:id/cancel

### 15.4 API Integration

Build:

-   Billing list

-   Billing preview

-   Invoice generation

-   Invoice details

-   Invoice PDF

-   Invoice download

### 15.5 Testing

Test:

-   Correct organization

-   Correct GST

-   Correct client

-   Correct branch

-   Correct tariff

-   Correct amount

-   Duplicate invoice prevention

-   Invoice numbering

-   Invoice cancellation

**==================================================**

SPRINT 16 --- B2C PAYMENTS & CUSTOMER CHECKOUT

**==================================================**

### Objective

Build the consumer payment flow.

### 16.1 Business Requirements

Flow:

    Customer

       ↓

    Booking

       ↓

    Fare

       ↓

    Payment

       ↓

    Booking Confirmation

       ↓

    Driver

       ↓

    Trip

Support:

-   Online payment

-   Payment status

-   Payment confirmation

-   Failed payment

-   Refund

-   Cancellation charges

-   Customer receipt

### 16.2 DB Design

    customer_payments

    payment_transactions

    refunds

    refund_transactions

### 16.3 API

    POST /customer/bookings/:id/payment

    GET  /customer/payments

    GET  /customer/payments/:id

Refund:

    POST /payments/:id/refund

### 16.4 API Integration

Integrate:

-   Razorpay or equivalent payment gateway

-   Customer checkout

-   Payment status

-   Receipt

-   Refund

### 16.5 Testing

Test:

-   Successful payment

-   Failed payment

-   Duplicate payment

-   Payment timeout

-   Refund

-   Partial refund where supported

-   Cancellation charges

-   Webhook idempotency

**==================================================**

SPRINT 17 --- B2C CUSTOMER APP / WEB

**==================================================**

### Objective

Build the public RydCare customer experience.

### 17.1 Business Requirements

Customer should be able to:

-   Login with OTP

-   Manage profile

-   Manage vehicles

-   Select service

-   Enter pickup/drop

-   Select vehicle

-   See fare

-   Book driver

-   Track driver

-   View trip

-   View payment

-   View receipts

-   Cancel booking

-   Raise issue

-   Provide feedback

### 17.2 DB Design

Use existing:

    customers

    customer_vehicles

    bookings

    trips

    payments

Additional where required:

    customer_feedback

### 17.3 API

Customer-facing APIs:

    /customer/auth

    /customer/profile

    /customer/vehicles

    /customer/bookings

    /customer/trips

    /customer/payments

    /customer/support

### 17.4 API Integration

Build:

    RydCare Customer App / Mobile Web

Core screens:

-   Login

-   Home

-   Book Driver

-   Fare

-   Booking confirmation

-   Driver assigned

-   Driver tracking

-   Trip history

-   Payment history

-   Profile

-   Support

### 17.5 Testing

Test complete journey:

    Login

      ↓

    Vehicle

      ↓

    Booking

      ↓

    Fare

      ↓

    Payment

      ↓

    Driver

      ↓

    Tracking

      ↓

    Trip

      ↓

    Receipt

**==================================================**

SPRINT 18 --- DRIVER APP

**==================================================**

### Objective

Build the common driver experience for both B2B and B2C.

### 18.1 Business Requirements

Driver app:

-   Login

-   Profile

-   Availability

-   Jobs

-   Job offers

-   Accept / reject

-   Navigation

-   Start duty

-   OTP

-   Trip

-   End duty

-   Photos

-   Customer confirmation

-   Earnings

-   Ledger

-   Support

### 18.2 DB Design

Use existing driver/duty/trip/finance structures.

FCM token support.

Device/session support.

### 18.3 API

Driver APIs:

    /driver/auth

    /driver/profile

    /driver/availability

    /driver/jobs

    /driver/offers

    /driver/duties

    /driver/trips

    /driver/earnings

    /driver/support

### 18.4 API Integration

Flutter application:

-   Driver login

-   Job list

-   Job acceptance

-   Navigation

-   Start/End trip

-   Location tracking

-   Earnings

### 18.5 Testing

Test:

-   Driver authentication

-   Availability

-   Job acceptance

-   Job rejection

-   Location

-   Duty

-   Trip

-   Earnings

-   Offline / network recovery

**==================================================**

SPRINT 19 --- CUSTOMER COMMUNICATION & NOTIFICATIONS

**==================================================**

### Objective

Centralize communication across B2B, B2C and Driver channels.

### 19.1 Business Requirements

Channels:

-   Push / FCM

-   SMS / MSG91

-   Email

-   WhatsApp where applicable

Events:

-   Booking created

-   Booking confirmed

-   Driver assigned

-   Driver arriving

-   Duty started

-   Duty completed

-   Payment received

-   Payment failed

-   Invoice generated

-   Document expiring

-   Ticket update

### 19.2 DB Design

    notifications

    notification_templates

    notification_logs

### 19.3 API

    GET /notifications

    GET /notification-templates

### 19.4 API Integration

Integrate:

-   FCM

-   MSG91

-   Email

-   Customer notifications

-   Driver notifications

-   B2B notifications

### 19.5 Testing

Test:

-   Delivery

-   Failure

-   Retry

-   Duplicate prevention

-   Template rendering

-   Invalid recipient

-   Provider failure

**==================================================**

SPRINT 20 --- ISSUES / SUPPORT / INCIDENTS

**==================================================**

### Objective

Manage operational and customer issues.

### 20.1 Business Requirements

Issues can originate from:

-   B2B client

-   B2B branch

-   B2C customer

-   Driver

-   Booking

-   Duty

-   Accident

-   Billing

-   Payment

Workflow:

    Issue

      ↓

    Assigned

      ↓

    Investigation

      ↓

    Resolution

      ↓

    Closed

### 20.2 DB Design

    issues

    issue_comments

    issue_attachments

    issue_assignments

### 20.3 API

CRUD.

Commands:

    assign

    resolve

    reopen

    close

Customer APIs must only expose the customer's

own issues.

### 20.4 API Integration

Build:

-   Internal support interface

-   Client support

-   Branch support

-   Customer support

-   Driver support

### 20.5 Testing

Test:

-   Scope

-   Customer isolation

-   Client isolation

-   Branch isolation

-   Driver isolation

-   Assignment

-   Resolution

-   Reopening

**==================================================**

SPRINT 21 --- DOCUMENTS & ASSETS

**==================================================**

### Objective

Centralize document and asset management.

### 21.1 Business Requirements

Documents:

-   Client documents

-   Driver documents

-   Agreements

-   Invoices

-   Accident documents

-   Operational documents

Assets:

-   ID cards

-   T-shirts

-   Bottles

-   Bags

-   Power banks

-   Other driver assets

### 21.2 DB Design

    documents

    assets

    asset_assignments

Storage:

    Wasabi / S3-compatible storage

Metadata:

    PostgreSQL

### 21.3 API

Documents:

    Upload

    Download

    Delete

    Expiry

Assets:

    Create

    Assign

    Return

    Transfer

    Track

### 21.4 API Integration

Build:

-   Document manager

-   Document upload

-   Document preview

-   Expiry tracking

-   Asset manager

-   Driver asset history

### 21.5 Testing

Test:

-   File permissions

-   Private files

-   Upload

-   Download

-   Delete

-   Expiry

-   Asset assignment

-   Asset return

**==================================================**

SPRINT 22 --- AUDIT LOGGING

**==================================================**

### Objective

Track important system actions.

### 22.1 Business Requirements

Audit:

-   User

-   Action

-   Resource

-   Old value

-   New value

-   Timestamp

-   IP/device where required

Examples:

    Booking reassigned

    Driver status changed

    Invoice edited

    Payment recorded

    Permission changed

    Scope changed

    Refund processed

### 22.2 DB Design

    audit_logs

### 22.3 API

    GET /audit-logs

    GET /audit-logs/:id

### 22.4 API Integration

Build:

-   Audit history

-   Resource history

-   User activity

### 22.5 Testing

Verify:

-   Critical actions create audit records

-   Audit records cannot be modified by normal users

-   Audit records cannot be deleted

**==================================================**

SPRINT 23 --- REPORTING & ANALYTICS

**==================================================**

### Objective

Build operational, financial and management reporting.

### 23.1 Business Requirements

B2B:

-   Bookings

-   Completed duties

-   Client performance

-   Branch performance

-   City performance

B2C:

-   Customer bookings

-   Trip volume

-   Customer retention

-   Booking conversion

-   Cancellation

-   Driver acceptance

-   Payment performance

Drivers:

-   Utilization

-   Earnings

-   Trips

-   Acceptance

-   Performance

Finance:

-   Billing

-   Invoices

-   Outstanding

-   Collections

-   Driver payable

-   B2C collections

-   Commission

Management:

-   City performance

-   Client performance

-   B2B vs B2C

-   Revenue

-   Driver cost

-   Margin where supported

### 23.2 DB Design

Initially use PostgreSQL queries and views.

Do NOT create a separate reporting warehouse initially.

### 23.3 API

Reporting APIs for:

-   Operations

-   Drivers

-   Clients

-   Branches

-   Customers

-   Billing

-   Payments

-   Driver finance

-   B2B/B2C comparison

### 23.4 API Integration

Build:

-   Management dashboard

-   Operations dashboard

-   Finance dashboard

-   City dashboard

-   Client dashboard

-   B2C dashboard

-   Driver dashboard

-   Exports

### 23.5 Testing

Verify reporting totals against source records.

Test:

-   City filters

-   Client filters

-   Branch filters

-   Organization filters

-   B2B/B2C filters

-   Date filters

-   Permission restrictions

**==================================================**

SPRINT 24 --- EXTERNAL INTEGRATIONS

**==================================================**

### Objective

Connect RydCare with external systems.

### 24.1 Business Requirements

Integrations:

-   Zoho Books

-   Tally where required

-   FCM

-   MSG91

-   Email

-   Traccar

-   OSRM

-   Wasabi / S3

-   Razorpay

-   Other future providers

Architecture:

    RydCare

       │

       ├── Zoho Adapter

       ├── Tally Adapter

       ├── MSG91 Adapter

       ├── FCM Adapter

       ├── Traccar Adapter

       ├── OSRM Adapter

       ├── Razorpay Adapter

       └── Storage Adapter

External systems must not directly control

core RydCare business logic.

### 24.2 DB Design

    integration_accounts

    integration_logs

Store:

-   Provider

-   External ID

-   Status

-   Last sync

-   Error information

-   Retry information

### 24.3 API

    GET  /integrations

    GET  /integrations/:id

    POST /integrations/:id/sync

    POST /integrations/:id/retry

### 24.4 API Integration

Implement integrations individually.

Each integration must be isolated behind an adapter/service.

### 24.5 Testing

Test:

-   Successful sync

-   Failure

-   Retry

-   Timeout

-   Duplicate prevention

-   Idempotency

-   Partial failure

-   Sync recovery

**==================================================**

SPRINT 25 --- DRIVER PROGRAM & RETENTION

**==================================================**

### Objective

Build the RydCare driver engagement program.

### 25.1 Business Requirements

Join:

-   ID Card

-   T-shirt

-   Bottle

Refer:

-   Referral rewards

-   Referral tracking

Milestones:

-   Trip milestone gifts

-   Performance rewards

Community:

-   Monthly / quarterly driver lunch

-   Driver events

Recognition:

-   Driver of the Month

Festival:

-   Festival gifts

-   Special rewards

### 25.2 DB Design

    driver_programs

    driver_referrals

    driver_rewards

    driver_milestones

    driver_events

### 25.3 API

Program CRUD.

Referral APIs.

Reward APIs.

Milestone APIs.

Eligibility APIs.

### 25.4 API Integration

Integrate with:

-   Driver app

-   Driver profile

-   Notifications

### 25.5 Testing

Test:

-   Referral calculation

-   Milestone calculation

-   Reward eligibility

-   Duplicate rewards

-   Driver status

-   Reward history

**==================================================**

CORE BOOKING ARCHITECTURE

**==================================================**

The most important architectural decision is:

             B2B                  B2C

              │                    │

              │                    │

        Client / Branch        Customer

              │                    │

              └────────┬───────────┘

                       ↓

                  BOOKING ENGINE

                       ↓

                    DISPATCH

                       ↓

                     DRIVER

                       ↓

                      DUTY

                       ↓

                      TRIP

                       ↓

             ┌─────────┴─────────┐

             │                   │

         B2B BILLING         B2C PAYMENT

             │                   │

         RECEIVABLES          REFUNDS

             │                   │

             └─────────┬─────────┘

                       ↓

                 DRIVER FINANCE

The booking engine must NOT be designed as a

B2B-only system.

**==================================================**

BOOKING SOURCE MODEL

**==================================================**

Every booking should identify its source:

    B2B

    B2C

### B2B

    Organization

         ↓

    Client

         ↓

    Branch

         ↓

    City

         ↓

    End Customer

         ↓

    Vehicle

         ↓

    Driver

### B2C

    Customer

         ↓

    City

         ↓

    Vehicle

         ↓

    Driver

Therefore:

    Client = nullable on booking

    Branch = nullable on booking

    Customer = end-service customer

    City = operationally important

**==================================================**

B2C MARKETPLACE MODEL

**==================================================**

The B2C experience should eventually work like a

driver marketplace such as DriveU / Drivly.

Customer:

    Login

       ↓

    Select Vehicle

       ↓

    Select Pickup

       ↓

    Select Drop

       ↓

    Select Service

       ↓

    Fare Calculation

       ↓

    Payment

       ↓

    Driver Matching

       ↓

    Driver Acceptance

       ↓

    Driver Tracking

       ↓

    Trip

       ↓

    Completion

       ↓

    Receipt

Driver:

    Online

       ↓

    Receive Job

       ↓

    Accept

       ↓

    Navigate

       ↓

    Start Duty

       ↓

    Complete Trip

       ↓

    Earnings

**==================================================**

B2B MODEL

**==================================================**

Client:

    Client

       ↓

    Branch

       ↓

    Booking

       ↓

    Operations

       ↓

    Driver

       ↓

    Duty

       ↓

    Billing

       ↓

    Invoice

       ↓

    Receivable

       ↓

    Payment

**==================================================**

DRIVER FINANCIAL MODEL

**==================================================**

RydCare maintains the operational driver ledger.

Ledger:

    EARNING

    ADVANCE

    DEDUCTION

    ADJUSTMENT

    PAYMENT

Formula:

    Driver Payable

    =

    Earnings

    + Adjustments

    - Advances

    - Deductions

    - Payments

### B2B

    Client

       ↓

    RydCare

       ↓

    Driver

### B2C

    Customer

       ↓

    RydCare

       ├── Driver Earning

       └── RydCare Commission

The same driver ledger should support both models.

**==================================================**

AUTHORIZATION FLOW

**==================================================**

Every protected request:

    Request

       ↓

    Authentication

       ↓

    Identify User

       ↓

    Load Roles

       ↓

    Load Permissions

       ↓

    Resolve Scope

       ↓

    Check Resource

       ↓

    Authorize

       ↓

    Controller

       ↓

    Service

       ↓

    Database

Example:

    Operations Manager

        +

    booking.view

        +

    Chennai

        ↓

    Can view Chennai bookings

But:

    Operations Manager

        +

    booking.view

        +

    Chennai

        +

    Madurai booking

        ↓

    DENY

B2C customer authorization is separate:

    Customer

       ↓

    Customer Identity

       ↓

    Booking Ownership

       ↓

    Allowed Customer Resource

A B2C customer must never become a full RydCare

internal user.

**==================================================**

PLATFORM MODULE DEPENDENCY

**==================================================**

                    FOUNDATION

                 Authentication

                     RBAC

                    Scopes

                       │

          ┌────────────┼────────────┐

          │            │            │

    Organizations    Cities       Users

          │            │            │

          └────────────┼────────────┘

                       │

              CLIENTS / BRANCHES

                       │

          ┌────────────┴────────────┐

          │                         │

       DRIVERS                  CUSTOMERS

          │                         │

          └────────────┬────────────┘

                       │

                    VEHICLES

                       │

                  JOB TYPES

                       │

                  PRICING ENGINE

                       │

                 BOOKING ENGINE

                       │

                   DISPATCH

                       │

                  DUTY / TRIP

                       │

                  TRACKING

                       │

                 VERIFICATION

                       │

              DRIVER FINANCE

                 ┌─────┴─────┐

                 │           │

             B2B BILLING   B2C PAYMENT

                 │           │

             RECEIVABLES   REFUNDS

                 │           │

                 └─────┬─────┘

                       │

                   REPORTING

Supporting modules:

    Notifications

    Support

    Documents

    Assets

    Audit

    Integrations

    Driver Program

**==================================================**

IMPORTANT ARCHITECTURE RULES

**==================================================**

1.  NO TENANT TABLE.

2.  RydCare is one centralized application.

3.  Organization is primarily a financial / billing entity.

4.  City is the primary operational boundary.

5.  Client is a B2B customer.

6.  Branch belongs to exactly one Client and one City.

7.  B2C customers do not belong to Clients or Branches.

8.  B2C customers are independent RydCare consumers.

9.  B2B and B2C must share the same Booking, Driver,

    Duty and Trip infrastructure.

10. Booking must support both B2B and B2C.

11. Client and Branch must be nullable on B2C bookings.

12. Role and Scope are separate concepts.

13. Permissions are centralized.

14. Authorization happens on the backend.

15. Frontend visibility is not security.

16. Temporary access must be supported.

17. Do not create city-specific roles.

18. Drivers are operational resources.

19. Driver branch associations represent eligibility / pinning,

    not ownership.

20. Finance can be organization-scoped without being city-scoped.

21. Client users can be client/branch scoped.

22. Operations users can be city scoped.

23. B2C customer access must be isolated from internal RydCare access.

24. B2C payment and B2B billing are separate financial flows.

25. Driver finance must support both B2B and B2C earnings.

26. Pricing must support both B2B tariffs and B2C public pricing.

27. Tracking infrastructure must support both operations and

    customer-facing tracking.

28. Every API must respect authorization and scope.

29. CRUD comes before complex business workflows.

30. Business rules belong in services/domain modules,

    not controllers.

31. External integrations must be isolated behind adapters.

32. Do not introduce microservices just because the platform

    may eventually become large.

33. Build service boundaries before extracting services.

34. Keep the initial system as a modular monolith.

35. Scale individual components only when actual business load

    justifies it.

**==================================================**

INITIAL TECHNICAL ARCHITECTURE

**==================================================**

Frontend:

    Next.js

        │

        ├── Internal RydCare Web

        └── B2C Customer Web / App

Backend:

    AdonisJS

        │

        └── Modular Monolith

Mobile:

    Flutter

        │

        └── Driver App

Database:

    PostgreSQL

Storage:

    Wasabi / S3-compatible storage

External services:

    MSG91

    FCM

    Razorpay

    Traccar

    OSRM

    Zoho Books

    Tally where required

Conceptually:

                         NEXT.JS

                    ┌─────────────┐

                    │             │

              Internal B2B     B2C Web

                    │             │

                    └──────┬──────┘

                           │

                       ADONISJS

                    MODULAR MONOLITH

                           │

          ┌────────────────┼────────────────┐

          │                │                │

       PostgreSQL       Wasabi          External APIs

          │                                 │

          │                    ┌────────────┼─────────────┐

          │                    │            │             │

          │                  MSG91        FCM         Razorpay

          │

          │                    Traccar

          │                    OSRM

          │                    Zoho

          │                    Tally

          │

          └──────────────────────────────────────────────

**==================================================**

WHAT NOT TO BUILD INITIALLY

**==================================================**

Do NOT start with:

-   Microservices

-   Kubernetes

-   RabbitMQ

-   Kafka

-   API Gateway

-   Multiple databases

-   Data warehouse

-   Complex event infrastructure

-   Complex repository abstraction

-   Separate B2B and B2C backends

Instead:

    ONE APPLICATION

    ONE PRIMARY DATABASE

    MODULAR MONOLITH

    STRONG RBAC

    STRONG RESOURCE SCOPE

    COMMON BOOKING ENGINE

    COMMON DRIVER ENGINE

    COMMON DUTY/TRIP ENGINE

Extract a component later only when there is a real reason.

Likely future extraction candidates:

-   Tracking

-   Notifications

-   AI/OCR

-   Integrations

-   Heavy reporting

-   Dispatch / matching

**==================================================**

FINAL PRODUCT STRUCTURE

**==================================================**

RYDCARE

├── PLATFORM

│ ├── Authentication

│ ├── RBAC

│ ├── Permissions

│ ├── Scopes

│ ├── Users

│ └── Audit

│

├── ORGANIZATION

│ ├── Organizations

│ └── Billing Entities

│

├── OPERATIONS

│ ├── Cities

│ ├── Clients

│ ├── Branches

│ ├── Drivers

│ ├── Vehicles

│ ├── Job Types

│ ├── Bookings

│ ├── Dispatch

│ ├── Duties

│ ├── Trips

│ └── Tracking

│

├── B2B

│ ├── Client Users

│ ├── Branch Users

│ ├── Client Bookings

│ ├── Billing

│ ├── Invoices

│ └── Receivables

│

├── B2C

│ ├── Customers

│ ├── Customer Vehicles

│ ├── Customer Booking

│ ├── Driver Matching

│ ├── Customer Payments

│ ├── Tracking

│ ├── Receipts

│ └── Customer Support

│

├── DRIVER

│ ├── Driver App

│ ├── Availability

│ ├── Job Offers

│ ├── Duties

│ ├── Trips

│ ├── Earnings

│ ├── Ledger

│ └── Driver Program

│

├── FINANCE

│ ├── Driver Finance

│ ├── B2B Billing

│ ├── B2C Payments

│ ├── Receivables

│ └── Settlements

│

├── SUPPORT

│ ├── Issues

│ ├── Incidents

│ ├── Customer Support

│ └── Driver Support

│

├── COMMUNICATION

│ ├── Notifications

│ ├── SMS

│ ├── Push

│ └── Email

│

├── DOCUMENTS

│ ├── Client Documents

│ ├── Driver Documents

│ ├── Agreements

│ └── Assets

│

└── INTEGRATIONS

    ├── Zoho Books

    ├── Tally

    ├── MSG91

    ├── FCM

    ├── Razorpay

    ├── Traccar

    ├── OSRM

    └── Wasabi

**==================================================**

END STATE

**==================================================**

The final platform should support:

    B2B

        Client

        Branch

        Operations

        Booking

        Driver

        Duty

        Billing

        Invoice

        Receivable

    B2C

        Customer

        Vehicle

        Booking

        Fare

        Payment

        Driver Matching

        Driver

        Duty

        Trip

        Tracking

        Receipt

Both use:

    Common Driver Pool

    Common Booking Engine

    Common Dispatch

    Common Duty Engine

    Common Trip Engine

    Common Tracking

    Common Driver Finance

    Common Support

    Common Notifications

This allows RydCare to operate as:

    A B2B Driver Service Platform

AND

    A B2C Driver Marketplace

without maintaining two separate operational systems.