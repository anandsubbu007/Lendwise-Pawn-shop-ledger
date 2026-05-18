<div align="center">

<!-- COVER IMAGE — Replace with your bento-style screenshot: 1280×640px, Android + Desktop + Tablet -->

![LendWise Cover](./assets/cover.png)

<br>

# LendWise

### When rural fintech, field hardware, offline sync, and enterprise architecture collide.

<br>

[![Play Store](https://img.shields.io/badge/Play%20Store-Live%20v1.5.0-brightgreen?logo=google-play&logoColor=white)](https://play.google.com/store/apps/details?id=com.subbu.lend_wise)
[![Flutter](https://img.shields.io/badge/Flutter-6%20Platforms-54C5F8?logo=flutter&logoColor=white)](https://flutter.dev)
[![Oracle](https://img.shields.io/badge/Oracle-AI%20Database-F80000?logo=oracle&logoColor=white)](https://cloud.oracle.com)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-Backend-6DB33F?logo=springboot&logoColor=white)](https://spring.io)
[![Firebase](https://img.shields.io/badge/Firebase-Auth%20%26%20Push-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com)
[![Redis](https://img.shields.io/badge/Redis-Cache-DC382D?logo=redis&logoColor=white)](https://redis.io)

</div>

---

## Built by a Single Engineer

> One codebase. One architecture. One production-grade delivery.

| Metric        | Delivery                                                           |
| ------------- | ------------------------------------------------------------------ |
| Platforms     | Android · iOS · Tablet · Windows · macOS · Linux                   |
| Screens       | 100+                                                               |
| APIs          | 120+                                                               |
| Integration   | Native Android hardware bridges · USB microscope support           |
| Deployment    | Oracle Cloud · Firebase auth · Redis cache                         |
| Product focus | Finance domain · offline branch sync · enterprise ledger integrity |

This was built end-to-end with no templates, no boilerplate, and a strong emphasis on architecture, resilience, and audit safety.

---

## This Is Not A CRUD App

Most apps save records.

LendWise **prevents financial loss.**

Every loan ticket is:

- **audited** — immutable closure records
- **validated** — receivable vs principal guards
- **versioned** — every sync delta tracked
- **conflict-safe** — offline edits never silently overwrite
- **sync-safe** — version-vector based, not timestamp
- **replay-safe** — reopen creates a reversal record, never deletes

Because money cannot be "eventually consistent."

---

## Shipping Reality

|                         |                                                  |
| ----------------------- | ------------------------------------------------ |
| **Version**             | 1.5.0                                            |
| **Platforms**           | Android · iOS · Tablet · Windows · macOS · Linux |
| **Production releases** | Multiple                                         |
| **APK size**            | 60MB+                                            |
| **Live usage**          | Active · Real businesses · Real money            |
| **Active Users**        | 50+                                              |

---

## What It Manages

- **Gold & silver loans** — ticket creation, ornament valuation, interest calculation, closure
- **Deposit accounts** — Saving / Flexi / Fixed with compound/simple/reinvested interest
- **Investors** — deposits, returns, linked accounts
- **Auction lifecycle** — cut-off dates, unredeemed tickets, sold/unsold asset routing
- **Multi-ledger accounting** — Cash ledger, Service ledger, Asset transactions
- **Multi-user access** — Admin + employees, per-user access control
- **Cash flow** — live balance, transaction vouchers, P&L, balance sheet, day report
- **Offline operations** — works without internet, syncs when back online

---

## Things I Built That Flutter Was Never Designed For

### USB Microscope Integration

Real-time collateral inspection at the counter.

- Native Android UVC device bridge — **no Flutter plugin exists for this**
- `UsbManager` → permission intent → bulk-transfer endpoint → MJPEG stream
- Platform channel feeds live frames into Flutter
- Operator taps to capture → stored against the loan ticket for its lifetime
- Used to inspect gold ornament purity and hallmarks

### Desktop Financial Workstations

- Keyboard-first navigation across dense loan grids
- Multi-monitor aware layouts
- Financial reports designed for large screens
- Same codebase as the Android phone app — zero duplication

### Offline Branch Sync

- Branch survives **days** without internet
- Local DB is primary truth — not a cache
- Sync resumes safely on reconnect
- No duplicate money entries, no silent overwrites
- Version-based delta: server sends only what changed since your last known version

### Native Android Camera Bridge

- Direct camera access for pledger photo and ID proof capture
- Bypasses Flutter's camera abstraction where needed
- Images stored per loan ticket, per pledger profile

---

## Zero-Handwritten API Layer

Every backend deployment:

```
Spring Boot starts
        ↓
Swagger spec generated automatically
        ↓
Codegen scripts execute
        ↓
Dio HTTP clients regenerated into Flutter monorepo
        ↓
Strongly typed request/response models updated
        ↓
Flutter modules compile — type errors surface immediately
```

No manual API drift.  
No "why is this field null."  
The compiler is the QA engineer.

---

## Architecture

```
┌──────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                         │
│  Android · iOS · Tablet · Windows · macOS · Linux        │
│  Flutter (Dart) — Monorepo — Clean Architecture          │
│  Feature-first modules · Repository pattern · DI         │
│  Offline-first local DB · Background sync queue          │
└───────────────────┬──────────────────────────────────────┘
                    │
      ┌─────────────┼──────────────┐
      ▼             ▼              ▼
 Local DB      Dio Client      Firebase
 (offline)   (Swagger-gen)   Auth · Push · Config
      │             │
      └──────┬──────┘
             │  version-delta sync
             ▼
┌──────────────────────────────────────────────────────────┐
│                     SERVER LAYER                         │
│  Spring Boot · Oracle Cloud AI DB · Redis Cache          │
│  Swagger OpenAPI · Email Automation                      │
└──────────────────────────────────────────────────────────┘
             │
┌──────────────────────────────────────────────────────────┐
│                   HARDWARE LAYER                         │
│  USB Microscope (native UVC bridge) · Camera bridge      │
└──────────────────────────────────────────────────────────┘
```

---

## Architecture Decisions

### Why not NoSQL?

Financial data cannot tolerate eventual consistency.

### Why Oracle?

- ACID transactions
- Row-level locking
- Auditability built in
- Mature indexing and enterprise reporting
- Oracle AI DB: anomaly detection and pledger risk scoring on the roadmap — no extra ML infrastructure needed

### Why Redis?

Hot cash-flow dashboards under 100ms.  
"Cash in hand," "live tickets," "interest receivable" — aggregated on every screen refresh without hammering Oracle.

### Why Firebase?

Identity solved. Not business logic.  
Auth, push, and remote config handled. Everything else stays in-house.

### Why version-based sync (not timestamp)?

Timestamps lie. Devices have clock drift.  
A tablet offline for 4 days with a slightly fast clock silently overwrites newer data under timestamp-based sync.  
Version vectors are deterministic. Same model Git uses.

### Why Flutter desktop?

The accountant uses Windows.  
The owner uses Android.  
The admin uses an iPad.  
One codebase. One CI pipeline. One engineer.

### Why Swagger code generation?

Manually written API clients drift.  
Backend changes a field → Flutter doesn't know → null pointer in production.  
With generation, the compiler tells you what broke — before customers see it.

---

## Domain: What a Loan Ticket Actually Is

Not a record. A **legal instrument.**

| Field            | Detail                                                              |
| ---------------- | ------------------------------------------------------------------- |
| Pledger          | Photo + ID proof embedded                                           |
| Ornament         | Gross weight · Net weight · Purity · Market price · Assessed price  |
| Interest rate    | Derived from a scheme (range-based lookup) — never manually entered |
| Advance          | Inspection charge deducted at closure                               |
| Service charge   | Routed to separate ledger depending on shop config                  |
| Closure date     | May be rounded per shop rule before interest is calculated          |
| Auction status   | May be linked to an active auction                                  |
| Partial payments | Tracked and tallied at closure                                      |
| Receivable guard | Blocks closure if receivable < principal                            |

---

## Ledger Configuration — Three Modes

Owner picks the model matching how their accountant thinks.

### Mode 1 — Service Ledger Inclusive

| Event                                     | Ledger         | Direction |
| ----------------------------------------- | -------------- | --------- |
| Loan create: Principal                    | Cash           | OUT       |
| Loan create: Advance, Doc Charge, Service | Service        | IN        |
| Loan close: Principal                     | Cash           | IN        |
| Loan close: Basic Interest                | Service → Cash | transfer  |

### Mode 2 — Cash Ledger Only

Everything in Cash. No service ledger entries.

### Mode 3 — Hybrid

Service charge → Service ledger. Everything else → Cash.

---

## Interest Engines

Three modes. One codebase. Decimal-safe arithmetic throughout — no floating-point money math.

| Method     | Formula                                         |
| ---------- | ----------------------------------------------- |
| Simple     | `(P × R × T) / 100`                             |
| Compound   | `P × (1 + R / (n × 100))^(n×T) − P`             |
| Reinvested | Simple per period, principal updated each cycle |

**Date round-off engine** — closure date is a derived value, not user input.  
Shop defines ranges (e.g. "1st–7th → rounds to 7th"). Interest calculated to the rounded date.

---

## Auction Lifecycle

```
All unredeemed tickets before cut-off date
              │
              ▼
        Auction initiated
              │
        ┌─────┴─────┐
        │           │
    Redeemed     Unredeemed
    before        on auction
    auction          day
        │           │
    Excluded     Sold publicly
                     │
                ┌────┴────┐
                │         │
              Sold      Unsold
                │         │
           Update      Move to
           ledger     org assets
```

- Closing an auction **locks all associated tickets** — cannot be edited post-closure
- Reopening **deletes the asset transaction** and unlocks all tickets (reversal record created)

---

## Audit & Safety Guards

| Guard                            | Behaviour                                               |
| -------------------------------- | ------------------------------------------------------- |
| Receivable < Principal           | Hard warning · shows exact rupee shortfall              |
| Receivable < Principal + Advance | Critical warning · capital loss alert                   |
| Auction closed                   | All linked tickets locked · cannot be edited            |
| Reopen                           | Creates reversal record · never deletes                 |
| Sync conflict                    | Surfaces as visible warning · manager resolves manually |

**Closed loan color coding:**

| Color         | Meaning                          |
| ------------- | -------------------------------- |
| ⚪ White      | Fully settled                    |
| 🟡 Yellow     | Within round-off tolerance       |
| 🟠 Amber      | Less than interest on close date |
| ⚪ Light grey | Paid less than advance           |
| 🔴 Red        | Below principal — urgent         |

---

## Engineering Fingerprints

- **USB Microscope — Native Android UVC Bridge**
  - No plugin exists for this workflow.
  - Built custom native bridge from `UsbManager` to Flutter via platform channels.
  - Streams MJPEG frames, captures images, and stores them against loan tickets.

- **Date Round-Off Engine**
  - Closure date is derived from shop-defined rounding rules.
  - Interest is calculated against the rounded date, not the raw close date.
  - Enables accurate compliance for local pawn accounting practices.

- **Three Interest Engines in One Codebase**
  - Supports Simple, Compound, and Reinvested interest workflows.
  - All formulas are verified and use decimal-safe arithmetic.
  - Switchable per loan type and scheme configuration.

- **Configurable Ledger Topology**
  - Supports three distinct financial flow models.
  - Owner selects the ledger routing strategy for the pawn shop.
  - Implements full remapping of cash and service ledger behavior.

- **Swagger → Dio Code Generation Pipeline**
  - Backend Swagger spec is generated automatically.
  - Flutter Dio clients and models are regenerated on deploy.
  - Ensures API contract compatibility with compiler-level validation.

- **24-Column CSV Import with Row-Level Validation**
  - Bulk-imports years of paper records.
  - Creates pledger and ticket records atomically.
  - Reports malformed rows with explicit row number and field error.

- **Audit Color Semantics on Closed Loans**
  - UI encodes financial health states, not just status flags.
  - Uses consistent color semantics for settlement condition reporting.
  - Enables quick visual triage for accounting teams.

- **Receivable Safety Guards**
  - Blocks closure when receivable is below principal.
  - Escalates to critical warning if receivable is below principal plus advance.
  - Displays exact rupee difference and requires explicit confirmation.

---

## Battle Scars

- **Partial payment double-count**
  - Problem: Reopen/close cycles could duplicate partial payment transactions.
  - Fix: Reopen now explicitly resets both the closure transaction and the partial payment tally.
  - Impact: Prevented financial duplication and preserved ledger integrity.

- **Sync version collision**
  - Problem: Two devices edited the same ticket while offline.
  - Fix: Version-based conflict detection surfaces a visible warning instead of silently overwriting.
  - Impact: Protected audit trails and prevented inconsistent loan data.

- **Compound interest decimal drift**
  - Problem: IEEE 754 float arithmetic produced errors over multi-period calculations.
  - Fix: All interest math migrated to decimal-safe arithmetic.
  - Impact: Ensured financial precision and compliance across loan computations.

- **Subscription expiry mid-operation**
  - Problem: Users lost context when the subscription expired during active workflows.
  - Fix: Renewal notice now includes company details and preserves ongoing operations.
  - Impact: Reduced user friction and prevented data loss in production.

- **Receivable below principal — silent capital loss**
  - Problem: Incorrect interest could generate a receivable lower than the disbursed principal.
  - Fix: System now blocks closure with a hard warning and explicit rupee shortfall.
  - Impact: Prevented capital-loss scenarios and enforced financial safety.

## Deposit Types

| Type           | Interest                  | Key Feature                                               |
| -------------- | ------------------------- | --------------------------------------------------------- |
| Saving Account | Daily, credited monthly   | Payable toggle — credited to linked account or compounded |
| Flexi Deposit  | Compound, dynamic balance | Flexible withdrawal · daily interest visibility           |
| Fixed Deposit  | Compound, locked term     | Auto-renewal option · prevailing rate on renewal          |

---

## Multi-User Access Model

```
Admin (Owner)
├── Full access: all modules
├── Creates and manages employee accounts
├── Approves loan closures (configurable)
└── Device password for fast admin switch on shared device

Employee
├── Access controlled per-user by admin
├── Can create loan tickets
└── Restricted from financial reports if not granted
```

---

## CSV Import Pipeline

Migrate years of paper records in bulk.

- 24-column format: pledger + ornament + loan details
- Validates: mobile format, date format, enum values (Gold/Silver, Open/Closed/Auction)
- Atomic pledger + ticket creation
- Row-level error reporting — row number and field name on failure
- No silent failures

---

## Screenshots

<!-- Replace with your actual screenshots -->
<!-- Folder: ./assets/screenshots/ -->

|                    Dashboard                     |                   Loan Ticket                   |                  Loan List                  |
| :----------------------------------------------: | :---------------------------------------------: | :-----------------------------------------: |
| ![Dashboard](./assets/screenshots/dashboard.png) | ![Ticket](./assets/screenshots/loan_ticket.png) | ![List](./assets/screenshots/loan_list.png) |

|             Transaction Report             |                    Settings                    |                 Desktop View                 |
| :----------------------------------------: | :--------------------------------------------: | :------------------------------------------: |
| ![Report](./assets/screenshots/report.png) | ![Settings](./assets/screenshots/settings.png) | ![Desktop](./assets/screenshots/desktop.png) |

---

## Tech Stack

**Frontend**

- Dart · Flutter · Monorepo
- Clean Architecture · Feature-first modules
- Repository pattern · Dependency injection
- Offline-first local database · Background sync queue

**Backend**

- Spring Boot
- Oracle Cloud AI Database (primary store, ACID)
- Redis (hot read cache — dashboards under 100ms)
- Swagger OpenAPI → Dio client auto-generated
- Email automation (backend-triggered)

**Auth & Messaging**

- Firebase Authentication
- Push notifications · Remote config

**Hardware**

- USB microscope — native Android UVC bridge (custom)
- Native Android camera bridge

**Platforms**

- Android · iOS · Tablet · Windows · macOS · Linux

---

## Built By

**Anand Alagappan**  
CEO · Subbu App Tech · Mannargudi, Tamil Nadu

📧 contactus@subbuapptech.in  
🌐 [lendwise.subbuapptech.in](https://lendwise.subbuapptech.in)  
📱 [Play Store](https://play.google.com/store/apps/details?id=com.subbu.lend_wise)

---

_LendWise © 2024–2026 · Subbu App Tech · Built in Mannargudi. Deployed in Production._
