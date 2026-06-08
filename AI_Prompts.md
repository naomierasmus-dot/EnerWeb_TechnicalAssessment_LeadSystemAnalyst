# Lead Systems Analyst — Technical Assessment
### Smart Metering & Consumption Intelligence Platform

**Candidate:** Naomi Erasmus  
**Date:** 8 June 2026  
**Position:** Lead Systems Analyst — Enerweb  

---

## 
## 🛠 AI Prompts used

- **System Model:** 
Create a SysML Block Definition Diagram (BDD) with the title "bdd [Utility Business System Model]". Use UML/SysML block notation with stereotype labels. Each block must have the «block» stereotype displayed above the block name. Each block must contain two compartments: "Produces" and "Consumes". Use white background blocks with black borders.
Create the following 5 blocks:
BLOCK 1: «block» Customer Domain
Produces:
- Customer Profile (ID, name, contact, account type)
- Meter-to-Customer Mapping (1 customer : many meters)
- Tariff Plan Selection (per meter)
- Billing Preference (prepaid / postpaid per meter)
Consumes:
- Usage & Forecast Data (from Billing)
- Alerts (from Alerting)
- Mobile / Web App Access

BLOCK 2: «block» Metering Domain
Produces:
- Daily Consumption Totals (kWh per day per meter)
- Meter Status & Metadata
Consumes:
- Raw Meter Readings (captured every 15 minutes, aggregated to daily)
- Meter-to-Customer Mapping (from Customer Domain)

BLOCK 3: «block» Pricing / Tariff Domain
Produces:
- Active Tariff Rate Schedule (price per kWh by plan)
- Tariff Change History (effective date log)
- Plan Catalogue (available tariff plans)
Consumes:
- Tariff Plan Selection (from Customer Domain - which plan per meter)
- Regulatory / Business Rule Updates (external)

BLOCK 4: «block» Billing Domain
Produces:
- Itemised Invoice per Meter per Tariff (usage x rate on date of usage)
- Billing Summary per Customer
- Payment Status (prepaid balance / postpaid outstanding)
Consumes:
- Daily Consumption Totals (from Metering Domain)
- Tariff Rate effective on usage date (from Pricing / Tariff Domain)
- Customer Profile & Billing Preference (from Customer Domain)
- Meter-to-Customer Mapping (from Customer Domain)

BLOCK 5: «block» Alerting Domain
Produces:
- Customer Billing Alerts
- Customer Load Shedding Alerts
- Field Technician Load Shedding Alerts
Consumes:
- Billing Events & Payment Status (from Billing Domain)
- Load Shedding Schedule (external source)
- Customer Contact Details (from Customer Domain)

Now draw the following dependency relationships as dashed arrows with the label on the arrow:

STRONG DEPENDENCIES (black dashed arrows labelled «dependsOn»):
- Billing Domain «dependsOn» Metering Domain
- Billing Domain «dependsOn» Pricing / Tariff Domain
- Billing Domain «dependsOn» Customer Domain
- Alerting Domain «dependsOn» Billing Domain
- Alerting Domain «dependsOn» Customer Domain
OPTIONAL DEPENDENCIES (grey dashed arrows labelled «optionallyDependsOn»):
- Metering Domain «optionallyDependsOn» Customer Domain
- Alerting Domain «optionallyDependsOn» Metering Domain
NO DEPENDENCY (do not connect these two blocks directly):
- Metering Domain and Pricing / Tariff Domain must NOT have a direct dependency. They are independent streams that only converge inside Billing Domain.
Layout: Place Customer Domain at the top centre. Place Metering Domain on the left and Pricing / Tariff Domain on the right, both below Customer Domain. Place Billing Domain in the centre below Metering and Pricing. Place Alerting Domain at the bottom centre below Billing. Billing Domain is the convergence point. Alerting Domain is a pure consumer that reacts to other domains.

- **Use Case Diagram:**
Create a UML Use Case Diagram with two system boundary boxes and actors.

SYSTEM BOUNDARY 1: "Metering Domain" (large rectangle)
Contains these use cases (ovals):
- UC1: Receive Meter Reading
- UC2: Validate Reading
- UC3: Record Manual Reading
- UC4: Aggregate Daily Consumption
- UC5: Detect Usage Anomaly
- UC6: View Consumption History
- UC7: Manage Meter Lifecycle
- UC8: Flag Late Arrival Reading

SYSTEM BOUNDARY 2: "Pricing / Tariff Domain" (large rectangle)
Contains these use cases (ovals):
- UC9: Create Tariff Plan
- UC10: Update Tariff Price
- UC11: View Price History
- UC12: Apply Default Price Fallback
- UC13: Subscribe Customer to Plan

ACTORS (stick figures placed outside the boundaries):
Left side actors:
- Smart Meter (connected to UC1)
- Field Technician (connected to UC3 and UC7)
- Scheduler (connected to UC4)
- Customer (connected to UC6 and UC13)

Right side actors:
- Billing System (connected to UC4)
- Alerting System (connected to UC5)
- Tariff Administrator (connected to UC9, UC10, UC11, UC13)

RELATIONSHIPS (dashed arrows):
Include relationships:
- UC1 --include--> UC2
- UC3 --include--> UC2

Extend relationships:
- UC2 --extend--> UC5 (condition: if anomaly detected)
- UC2 --extend--> UC8 (condition: if reading arrived late)
- UC10 --extend--> UC12 (condition: if no plan price exists)

Cross-domain dependencies (dashed arrows):
- UC4 --dependsOn--> UC13 (label: needs active tariff plan to tag reading)
- UC8 --dependsOn--> UC10 (label: late arrival may use wrong tariff price)


- **Entity Relationship Diagram:** 
Create a detailed Entity Relationship Diagram (ERD) using Crow's Foot notation for a Utility Electricity Billing System. Use rectangular boxes for entities with the entity name in bold at the top of each box. Each entity must show all attributes listed below. Mark primary keys with "PK" and foreign keys with "FK". Use solid lines for identifying relationships and dashed lines for non-identifying relationships. Show cardinality using Crow's Foot notation (one-to-many, etc.). Label every relationship line with its meaning.

Create the following 6 entities with their exact attributes:

ENTITY 1: CUSTOMERS
- CustomerID (PK, INT, NOT NULL)
- FirstName (VARCHAR, NOT NULL)
- LastName (VARCHAR, NOT NULL)
- Email (VARCHAR, UNIQUE, NOT NULL)
- Phone (VARCHAR)
- AccountType (ENUM: Prepaid, Postpaid, NOT NULL)
- Address (VARCHAR)
- CreatedDate (DATETIME, NOT NULL)
- IsActive (BOOLEAN, DEFAULT TRUE)

ENTITY 2: METERS
- MeterID (PK, INT, NOT NULL)
- CustomerID (FK → CUSTOMERS.CustomerID, NOT NULL)
- MeterSerialNumber (VARCHAR, UNIQUE, NOT NULL)
- InstallationDate (DATE, NOT NULL)
- MeterLocation (VARCHAR)
- MeterStatus (ENUM: Active, Inactive, Faulty, NOT NULL)
- CreatedDate (DATETIME, NOT NULL)

ENTITY 3: TARIFF_PLANS
- TariffPlanID (PK, INT, NOT NULL)
- PlanName (VARCHAR, UNIQUE, NOT NULL)
- PlanType (ENUM: TimeOfUse, FlatRate, Prepaid, Postpaid, NOT NULL)
- Description (VARCHAR)
- DefaultPricePerKWh (DECIMAL(10,4), NOT NULL)
- IsActive (BOOLEAN, DEFAULT TRUE)
- CreatedDate (DATETIME, NOT NULL)

ENTITY 4: TARIFF_PRICE_HISTORY
- TariffPriceID (PK, INT, NOT NULL)
- TariffPlanID (FK → TARIFF_PLANS.TariffPlanID, NOT NULL)
- PricePerKWh (DECIMAL(10,4), NOT NULL)
- EffectiveFromDate (DATE, NOT NULL)
- EffectiveToDate (DATE, NULLABLE — NULL means currently active)
- ChangeReason (VARCHAR)
- CreatedDate (DATETIME, NOT NULL)
- CONSTRAINT: UNIQUE(TariffPlanID, EffectiveFromDate)
- INDEX: Composite index on (TariffPlanID, EffectiveFromDate, EffectiveToDate) for fast time-aligned price lookups

ENTITY 5: CUSTOMER_TARIFF_SUBSCRIPTIONS
- SubscriptionID (PK, INT, NOT NULL)
- CustomerID (FK → CUSTOMERS.CustomerID, NOT NULL)
- MeterID (FK → METERS.MeterID, NOT NULL)
- TariffPlanID (FK → TARIFF_PLANS.TariffPlanID, NOT NULL)
- EffectiveFromDate (DATE, NOT NULL)
- EffectiveToDate (DATE, NULLABLE — NULL means currently active)
- SubscriptionStatus (ENUM: Active, Expired, Cancelled, NOT NULL)
- CreatedDate (DATETIME, NOT NULL)
- CONSTRAINT: No overlapping date ranges per MeterID

ENTITY 6: READINGS
- ReadingID (PK, BIGINT, NOT NULL)
- MeterID (FK → METERS.MeterID, NOT NULL)
- CustomerID (FK → CUSTOMERS.CustomerID, NOT NULL)
- TariffPlanID (FK → TARIFF_PLANS.TariffPlanID, NOT NULL)
- UsageDate (DATE, NOT NULL)
- DailyUsageKWh (DECIMAL(12,4), NOT NULL)
- ReadingReceivedTimestamp (DATETIME, NOT NULL)
- ReadingStatus (ENUM: Received, Validated, Costed, Billed, NOT NULL)
- IsLateArrival (BOOLEAN, DEFAULT FALSE)
- CostCalculated (DECIMAL(12,2), NULLABLE)
- AppliedPricePerKWh (DECIMAL(10,4), NULLABLE)
- CreatedDate (DATETIME, NOT NULL)
- CONSTRAINT: UNIQUE(MeterID, UsageDate) — prevents duplicate readings per meter per day
- PARTITION BY: UsageDate — keeps queries fast as data grows to millions
- INDEX: Composite index on (CustomerID, UsageDate)
- INDEX: Composite index on (MeterID, UsageDate)

Now draw the following relationships using Crow's Foot notation:

RELATIONSHIP 1: CUSTOMERS ||--o{ METERS
Label: "owns"
Cardinality: One customer has many meters. Each meter belongs to exactly one customer.

RELATIONSHIP 2: CUSTOMERS ||--o{ CUSTOMER_TARIFF_SUBSCRIPTIONS
Label: "subscribes to"
Cardinality: One customer can have many subscriptions over time (plan changes tracked).

RELATIONSHIP 3: METERS ||--o{ CUSTOMER_TARIFF_SUBSCRIPTIONS
Label: "assigned plan"
Cardinality: One meter can have many subscriptions over time (each meter gets a tariff plan).

RELATIONSHIP 4: TARIFF_PLANS ||--o{ CUSTOMER_TARIFF_SUBSCRIPTIONS
Label: "selected as"
Cardinality: One tariff plan can be selected by many customer-meter combinations.

RELATIONSHIP 5: TARIFF_PLANS ||--o{ TARIFF_PRICE_HISTORY
Label: "has price history"
Cardinality: One tariff plan has many price records over time. Each price record belongs to one plan.

RELATIONSHIP 6: METERS ||--o{ READINGS
Label: "produces"
Cardinality: One meter produces many daily readings. Each reading comes from one meter.

RELATIONSHIP 7: CUSTOMERS ||--o{ READINGS
Label: "billed for"
Cardinality: One customer has many readings across their meters. Each reading belongs to one customer.

RELATIONSHIP 8: TARIFF_PLANS ||--o{ READINGS
Label: "priced using"
Cardinality: Each reading is tagged with the active tariff plan at time of usage. One plan appears on many readings.

Layout: Place CUSTOMERS at the top centre as the anchor entity. Place METERS to the left of CUSTOMERS and slightly below. Place TARIFF_PLANS to the right of CUSTOMERS and slightly below. Place CUSTOMER_TARIFF_SUBSCRIPTIONS in the centre between METERS and TARIFF_PLANS, below CUSTOMERS. Place TARIFF_PRICE_HISTORY to the right below TARIFF_PLANS. Place READINGS at the bottom centre as the largest transactional table. Use a light yellow background for master data entities (CUSTOMERS, METERS, TARIFF_PLANS). Use a light blue background for time-based tracking entities (TARIFF_PRICE_HISTORY, CUSTOMER_TARIFF_SUBSCRIPTIONS). Use a light green background for the transactional entity (READINGS).

Add a notes box titled "Performance and Robustness Checks" with this content:
1. UNIQUE(MeterID, UsageDate) — Prevents duplicate readings per meter per day
2. Composite index on TariffPriceHistory — Fast time-aligned price lookups
3. Partition READINGS by UsageDate — Keeps queries fast as data grows to millions
4. EffectiveToDate enables range joins — Avoids expensive subqueries for price lookup
5. IsLateArrival flag — Quick identification of readings with wrong tariff risk
6. Separate PriceHistory from Plans — Normalised, no update anomalies
7. ReadingStatus column — Tracks lifecycle: Received, Validated, Costed, Billed


- **AI Assistance:** 
Microsoft Copilot, prompts and reasoning documented in `AI_Prompts_Used.md`




