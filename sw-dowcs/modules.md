rydcarea-api/
│
├── app/
│   ├── modules/
│   │
│   ├── auth/
│   ├── tenancy/
│   ├── organization/
│   ├── users/
│   │
│   ├── clients/
│   ├── branches/
│   ├── contacts/
│   ├── documents/
│   │
│   ├── drivers/
│   ├── driver-documents/
│   ├── driver-attendance/
│   ├── driver-availability/
│   │
│   ├── bookings/
│   ├── dispatch/
│   ├── trips/
│   ├── tracking/
│   │
│   ├── tariffs/
│   ├── driver-finance/
│   ├── billing/
│   ├── invoices/
│   ├── payments/
│   │
│   ├── notifications/
│   ├── integrations/
│   └── audit/
│
├── database/
├── start/
└── config/


Tech foundation
- [ ] Create AdonisJS project
- [ ] PostgreSQL setup
- [ ] Environment/config management
- [ ] Database migrations
- [ ] Lucid ORM setup
- [ ] API versioning: /api/v1
- [ ] Standard API response format
- [ ] Error/exception handling
- [ ] Request validation
- [ ] Authentication foundation
- [ ] Authorization/RBAC foundation
- [ ] Logging
- [ ] CORS
- [ ] File storage abstraction
- [ ] Wasabi/S3 configuration
- [ ] Email/SMS abstraction
- [ ] Basic test setup
- [ ] Git/GitHub repository
- [ ] Docker development environment




RYDCARE
   │
   └── TENANT
         │
         ├── Cities / Geography
         │      ├── Chennai
         │      ├── Madurai
         │      └── ...
         │
         ├── Organizations
         │      ├── Nine Star Call Drivers
         │      └── Just Call Drivers
         │
         ├── Users / Staff
         │
         ├── Clients
         ├── Drivers
         ├── Bookings
         ├── Duties
         ├── Billing
         └── Finance