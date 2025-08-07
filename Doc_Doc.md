# Vdriv Documentation


### 1.0 Introduction:

The purpose of this is to make yours and my life easier in understanding how to build  a system that will outlast the both of us. There is only one favour to ask from you and that is to understand and embed this thought in our soul that, any software, application, website, program, code or Ai, is subjected to working with DATA. That is it and nothing else, everything is dependant on data, and all we have to do is breakdown problems from a data-driven perspective, learn how it is generated, what operations it goes through and we can solve any Business Logic.

Now, that we have established the only needed thought process, let's get to understanding why we need to build this system. The goal of this is to bring order to a chaos filled industry, with inarguably more chaotic personnel. We will be orchestrating an elaborate yet simple to use system to help an average car driver never run out of work or have to even think of searching for work, we feed the drivers with so much oppoutunity that they need not look elsewhere, not in an evil-corporate manner but in a way to help them become better breadwinnners.

The Driver Industry is one that is scattered into small kingdoms and most of these provinces focus on the end customer rather than the driver, our goal is do the best of both, and that has been our vision, *To professionalize the driver ecosystem*, and inorder to do this we need to have just three key ingredients:

- Seamlessly gather intel where drivers are needed
- Make the driver aware of where he is needed
- Always have an eye on where they are and what they are upto

Yes, all we need to do develop a system that is capable of the above holy trinity and we are good to go and we are going to adopt a data driven approach to tackle it.

<div style="page-break-after: always;"></div>


### 1.1 Understanding: How we Operate?


Imagine you are a bird, you wake up everyday empty stomach, out of your nest you go trusting that the universe will feed you, well that is how we work. Almost all of our drivers are birds who hunt their wages every single day and the benefit of it is that they work as per their terms and hunt as much as their heart desires, seriously we have drivers who earn as much or probably even more than the average IT professional and they take leaves as much as they desire, they choose their work and location, travel to places, true freedom of work.

From all this you should understand that we are the universe that feed the driver with oppourtunities like no one else, and like every other type in the worls we have 4 types of job a driver can do within our shade.

- Customer Duty 
- Service Duty
- RTO Duty
- Valet Duty

You might be clueless or rather confused of what those above mean, so let's understand it pictorially, every one of those Duties follow the below template regardless of which one it is:

 ``` 
				 Driver Requirement Needy
				        |
				        |  requests driver
				        v
				  Us, the Facilitator
				        |
				        |  assigns driver
				        v
				      The Driver
				        |
				        |  does the job
				        v
				      The Job
				
	Financial Flow:
	Driver Requirement Needy  ---> pays --->  Us, the Facilitator
	Us, the Facilitator      ---> pays --->  The Driver
		
 ```


So now that is all you need to know to understand how we are going to build our application, every single aspect of the app would revolve around this, and will directly or indirectly help us achieve ease of operations for the above workflow.


### 2.0 The Three Applications That Power Our Vision

To transform the driver industry we will be operating through three main interconnected applications:

1. **Driver App (Mobile)** - For independent and contracted drivers.
2. **Rider/Customer App (Mobile)** - For individuals or corporate clients.
3. **Admin Panel (Web)** - For Vdriv staff and corporates with restricted access.

All modules communicate via a central **Laravel backend**, powered by **OTP-based phone authentication** and mobile apps are built with **multilingual support** for better regional adoption.


### 2.1 Core Philosophy

At the heart of Vdriv is a single principle:

Every feature exists to facilitate or optimize one of three outcomes:
- Identify where drivers are needed.
- Notify drivers of opportunities.	
- Track driver performance and movement.

### 2.1 Driver Application (Mobile)

### Purpose
To help drivers access opportunities, manage work, earnings, and maintain communication — with minimal literacy requirements and full multilingual support.

### Pages & Modules

#### 1. **Authentication & Onboarding**
- `Signup` (Phone OTP)
- `Service Selection` (RTO / Valet / Service / Customer Duty)
- `Personal Info`
- `Document Upload` (Aadhar, License, etc.)
- `Bank Info`
 
#### 2. **Main App Layout**
- Bottom Navigation:
  - `Home`
  - `Bookings`
  - `Profile`
  - `Price Check`

#### 3.**Home Dashboard**
- Live Job Feed (Dispatch & Campaigns)
- Availability Toggle
- Earnings Summary

#### 4. **Bookings**
- Incoming Trip Requests (Accept/Reject)
- Ongoing Trips
- Trip History
- Ride Summary

#### 5. **Profile**
- Personal Info
- Bank Details
- Support & FAQs
- Terms & Conditions
- Logout

#### 6. **Price Check**
- Fare Estimation (non-interactive)


### 2.1.1 Driver App - Campaigns Module

### 2.1.2 Target User:
- Temporary drivers (post verification)
- Registered drivers (optional for campaign access)

---

### 2.1.3 Purpose:
To enable drivers to discover, accept, and complete short-term jobs posted by corporates — specifically **RTO** and **Valet** duties — while ensuring accurate attendance, tracking, and payment verification.

---

### 2.1.4 Eligibility Criteria:
To access Campaigns, driver must:
1. Complete mobile OTP authentication.
2. Submit:
   - Aadhaar Number + Aadhaar Document
   - Driving License Number + DL Upload
   - Select Primary Work Location
3. Driver will now be assigned the role: `Temporary Driver`.

---

## 2.1.5 Campaign Listing Page

### 2.1.6 Fields per Campaign:
- `Campaign Name`
- `Work Type`: RTO or Valet
- `Corporate Location`: with Google Maps link
- `Available Time Slots`: Multiple selectable
- `Estimated Hours`
- `Estimated Pay`
- CTA: `Select Slot & Accept`

### 2.1.7 Actions:
- Drivers can view all **Available Campaigns**
- Upon selecting a slot, they are marked as **"Willing to Work"** for that time

---

## 2.1.8 Accepted Campaign Lifecycle (Driver View)

#### 1. **Status: Enroute**
- Button: `Start Journey`
- Shows:
  - Job Location with ETA
  - Contact Info of Onsite Supervisor (Name + Mobile Number)
- Status on Admin Dashboard: `Driver Enroute`


#### 2. **Status: Arrived at Location**
- Trigger: Driver taps `Reached`
- Prompt: Guide + Onsite Instructions
- Admin sees: `Driver Arrived at Site`

#### 3. Selfie Prompt
- App opens camera, selfie required
- Geo-tag is captured
- For **RTO** only:
  - Additional Upload Prompt: Vehicle Photo(s)

#### 4. Duty Timer Starts
- Timer begins from Selfie capture
- Status: `Duty Started`

#### 5. **Selfie Check-Out**
- Prompted upon `End Duty`
- Geo-tag captured again

### 6. **Slip Upload**
- Upload mandatory for **RTO Duties**
- Driver uploads photo of paper slip received onsite

#### 7. **Submit Duty Report**

## 2.1.10 RTO - Additional Work Details Form
(Shown during or before duty ends)

Auto-filled Fields:
- Driver ID
- Driver Name
- Work Location / Corporate Name

Manually Entered by Driver:
- Chassis Number
- Start Time (auto timestamped)
- Bill Number (from physical slip)

---

## 2.1.11 Work Flow Chart:

```
                            Driver Logs In
                                   |
                      Uploads Aadhaar & DL
                                   |
                        Chooses Work Location
                                   |
                      Becomes Temporary Driver
                                   |
                     Views Available Campaigns
                                   |
                       Selects Campaign Slot
                                   |
                         Status: Willing to Work
                                   |
                           Tap 'Start Journey'
                                   |
                    Status: Enroute to Job Location
                                   |
               View Job Location + Supervisor Info
                                   |
                        Tap 'Reached Location'
                                   |
                        Status: Arrived at Site
                                   |
                   Check-in Selfie + Geo Tag
                                   |
                              Work Type?
              ------------------|------------------
              |                                   |
             RTO                                Valet
              |                                   |
  Upload Vehicle Photo (RTO Only)      Skip Extra Photo
              |                                   |
       Start Duty Timer                  Start Duty Timer
              |                                   |
     Fill RTO Slip (if RTO)                 (Skip this step)
              |                                   |
         Tap 'End Duty'                     Tap 'End Duty'
              |                                   |
  Check-out Selfie + Geo Tag      Check-out Selfie + Geo Tag
              |                                   |
Upload Slip Photo (RTO Only)         Skip Slip Upload
              |                                   |
           Submit Duty                        Submit Duty
              \                                   /
               --------Status: Completed---------
                             |
                  Payment Pending → Admin Approves
                             |
                   → Payment Released to Driver

```


## 2.1.12 Duty Statuses (Driver Perspective)

| Status             | Trigger                              |
|--------------------|--------------------------------------|
| Willing to Work    | Slot selected                        |
| Enroute            | Driver tapped "Start Journey"        |
| Arrived            | Driver tapped "Reached"              |
| Started Duty       | Check-in selfie + geo-tag            |
| On Duty            | Timer running                        |
| Duty Ending        | Check-out selfie + optional form     |
| Completed          | Submission successful                |
| Payment Pending    | Awaiting internal approval/review    |
| Paid               | Amount credited to driver            |



## 2.1.13 Admin/Backend Considerations (Preview)
- Prevent duplicate slip uploads
- Slip validation → Payment release
- GPS tagging for fraud prevention
- Duty time caps (e.g., if timer exceeds N hours)
- Store timestamps for all key actions

---

## Summary of Key Data Captured Per Campaign Duty

```
 ---------------------------------------------------------------
| Field                    | Notes                              |
|--------------------------|------------------------------------|
| Driver ID                | Auto filled                        |
| Work Type                | RTO / Valet                        |
| Slot ID                  | Maps to campaign                   |
| Campaign ID              | Backend linking                    |
| Geo-locations            | Check-in, Check-out                |
| Selfie (Start/End)       | File + GPS Tag                     |
| Vehicle Photo (RTO only) | Optional at start                  |
| Duty Timer               | Start, End time                    |
| Bill No                  | Manual Entry (RTO only)            |
| Slip Upload              | Required for RTO                   |
| Payment Status           | Pending / Paid                     |
 ---------------------------------------------------------------
```

## 2.2 Customer Duty Module

Customer Duty is the most straightforward and frequently used feature. It enables individual riders or clients to request a driver for a trip. Trips fall into three categories:

* **One Way**: Pickup and drop at different locations.
* **Round Trip**: Pickup and drop at the same location.
* **Outstation**: Long-distance, charged per day (often includes food for the driver).

Pricing is calculated based on time (hourly tariff) and location (for One Way and Round Trip).

---

## 2.2.1 Tariff & Charges Breakdown

| Trip Type  | Tariff Model | Conditions                                |
| ---------- | ------------ | ----------------------------------------- |
| One Way    | Hourly       | Minimum 4 hours, outskirt charges apply   |
| Round Trip | Hourly       | Minimum 4 hours, night & outskirt charges |
| Outstation | Per Day      | Includes food; long-distance booking      |

### Additional Charges:

* Night Time Charges
* Extra Hour Charges
* Outskirts Surcharge

### Optional:

* **Insurance Plan**: Rs.25/add-on covering accidental damages (up to Rs.8000 liability covered)

---

## 2.2.2 Customer Journey

### Step 1: **Customer Login**

* Name, Contact Number, OTP
* Optional: Saved Locations, Vehicles

### Step 2: **Vehicle Management Page**

* Field: `Vehicle Number`
* Field: `Vehicle Model` (Dropdown: Sedan, SUV, Hatchback, etc.)

### Step 3: **Booking Creation**

* Select Trip Type: One Way / Round Trip / Outstation
* Enter Pickup Location & Time
* Enter Drop Location (if One Way)
* Select Duration (Hours/Days)
* Optional Add-ons: Insurance

---

## 2.2.3 Admin & Driver Flow

1. **Booking Pops in Admin Panel**
2. Admin assigns a driver
3. Driver receives request and can **Accept / Reject**
4. If accepted:

   * Admin and Customer notified
   * Trip status: `Driver Assigned`
   * Driver details visible to customer (like Ola/Uber)
5. If rejected or cancelled by driver:

   * Trip becomes `Unassigned`
   * Admin reassigns

---

## 2.2.4 Trip Lifecycle (Driver View)

### Step 1: **Enroute**

* Driver taps `Start Journey`
* Admin & Customer notified: `Driver Enroute`

### Step 2: **Driver Arrived**

* 4-digit PIN shown to Customer
* Driver must enter this PIN to begin trip

### Step 3: **Trip Started**

* Trip begins, timer starts
* Admin, Driver, Customer see status: `On Trip`

### Step 4: **Trip Completed**

* Driver taps `End Trip`
* Fare Calculation Shown

### Step 5: **Payment**

* Customer can pay:

  * In-App
  * Cash to Driver
* Trip marked as:

  * `Paid` (if complete)
  * `Pending` (if payment pending)

---

## 2.2.5 Wallet Logic (Driver Perspective)

* Drivers must maintain a **wallet balance** of 10% commission for trip eligibility
* If Customer pays via app:

  * Platform deducts 10% commission
  * Balance is credited to driver's wallet
  * Wallet is auto-adjusted per trip

---

## 2.2.6 Workflow Diagram
```
Customer Login (OTP Based)
         |
         | Manage Vehicles (Add Number + Type)
         v
Book Driver (Select Trip Type, Locations, Time, Vehicle)
         |
         | See Fare Estimate + Insurance Option
         v
Submit Booking (Pending Assignment)
         |
         | Admin Assigns Driver (Manual or Auto)
         v
Driver Notified (Accept/Reject)
         |
         | Driver Enroute
         v
Driver Arrived
         |
         | Driver Enters 4-digit PIN → Trip Begins
         v
Trip In Progress
         |
         | Driver Ends Trip
         v
Fare Displayed to Customer
         |
         | Payment Done (Cash or Online)
         v
Driver Wallet Adjustment
         |
         v
The Trip is Closed
```


---

## 2.2.7 Key Data Captured Per Customer Duty Trip

```
 -------------------------------------------------------------------
| Field                    | Notes                                  |
|--------------------------|----------------------------------------|
| Customer ID              | From login                             |
| Vehicle ID (optional)    | If customer adds a vehicle             |
| Trip Type                | One Way / Round Trip / Outstation      |
| Pickup & Drop Location   | Geo and Address                        |
| Driver ID                | Assigned driver                        |
| Driver Status Updates    | Enroute, Arrived, Started, Completed   |
| 4-digit Trip PIN         | Required to start trip                 |
| Start Time               | Timestamp (auto)                       |
| End Time                 | Timestamp (auto)                       |
| Duration                 | Derived from start/end timestamps      |
| Fare Breakdown           | Based on time & surcharges             |
| Payment Status           | Paid / Pending                         |
| Payment Method           | App / Cash                             |
| Wallet Deductions        | 10% commission logic                   |
| Insurance Opted          | Yes / No                               |
 -------------------------------------------------------------------F
```

---

## 2.2.8 Admin Considerations

* Real-time visibility of driver status
* Manual reassignment on cancellation
* Fare calculation logic based on config
* Trip completion verification
* Wallet settlement checks
* Duplicate trip prevention
* Surge pricing or discounts (future scope)

---

