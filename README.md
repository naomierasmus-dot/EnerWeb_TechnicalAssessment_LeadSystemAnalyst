# Lead Systems Analyst — Technical Assessment
### Smart Metering & Consumption Intelligence Platform

**Candidate:** Naomi Erasmus  
**Date:** 8 June 2026  
**Position:** Lead Systems Analyst — Enerweb  

---

## 📋 Assessment Overview

This repository contains my completed technical assessment for the Lead Systems Analyst role. The assessment focuses on designing a Smart Metering & Consumption Intelligence Platform for an electricity solutions provider, covering systems design, data modelling, use case analysis, and quarterly delivery planning.

---

## 📁 Deliverables

| File | Description |
|------|-------------|
| `SystemsDesign.png` | High-level Systems Domain Model showing Metering, Pricing/Tariff, Billing, Customer, and Alerting domains |
| `UseCase.png` | Use Case Diagram for Metering and Pricing/Tariff domains |
| `ERD.png` | Production-grade Entity Relationship Diagram (Customers, Meters, Readings, Tariffs) |
| `ActivityDiagram.png and StateDiagram.png` | Activity Flow and State Diagram for the "Process Daily Meter Reading" use case |
| `NonFunctionalRequirements.docx` | Non-functional requirements: resilience, security, auditability, performance |
| `SQL.docx` | SQL queries for consumption analysis, tariff mismatches, and anomaly detection |
| `TechnicalQuestions.docx` | Answers to technical questions on tech debt, configuration, and system tradeoffs |
| `RoadmapPlanning.docx` | Quarterly roadmap and delivery plan |
| `AI_Prompts_Used.md` | AI tools and prompts used during the assessment, with reasoning |

---

## 🎥 Video Walkthrough

📽️ [Loom Video — Assessment Walkthrough](#) *(link to be added)*  
A 5–8 minute walkthrough explaining my design decisions, diagram rationale, roadmap priorities, and selected technical questions.

---

## 🛠 Tools Used

- **Diagrams:** draw.io (diagrams.net)
- **Readme:** Written in NotePad++
- **Documents:** Microsoft Word
- **SQL:** Written for MS SQL Server
- **AI Assistance:** Microsoft Copilot — prompts and reasoning documented in `AI_Prompts_Used.md`

---

## 💡 Key Design Decisions

- **Time alignment** between usage dates and tariff effective dates is treated as the central design challenge throughout all deliverables.
- The ERD supports **temporal pricing** (effective date-based tariff history) with a default price fallback mechanism.
- The "Process Daily Meter Reading" use case was selected for the Activity/State diagrams because it touches the most business-critical complexity: validation, tariff matching, cost calculation, and anomaly detection.

---

## 📬 Contact

**Naomi Erasmus**  
📧 naomi.s.britz@gmail.com 
🔗 https://candidate.offerzen.com/candidate/5bc09fe129d50700233f5837/profile
