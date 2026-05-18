# LendWise — Pawn Shop OS

> **A full-stack fintech operating system for gold & silver pawn shops in India.**  
> Built by one engineer. Running in production. Tracking ₹3 Cr+ in active loans.

### 📱 Distribution
[![Play Store](https://img.shields.io/badge/Play%20Store-Live-brightgreen?logo=google-play)](https://play.google.com/store/apps/details?id=com.subbu.lend_wise)

### 🛠️ Tech Stack
[![Flutter](https://img.shields.io/badge/Flutter-6%20Platforms-blue?logo=flutter)](https://flutter.dev)
[![Platform](https://img.shields.io/badge/Backend-Spring%20Boot-blue)](https://spring.io/)
[![Oracle Cloud](https://img.shields.io/badge/Oracle-Cloud%20AI%20DB-red)](https://cloud.oracle.com)
[![Database](https://img.shields.io/badge/database-Oracle-orange)](#)
[![Offline First](https://img.shields.io/badge/Offline-First-orange)]()

### ✅ Build & Quality
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/your-org/pawn-ledger-app/actions)
[![Coverage](https://img.shields.io/badge/coverage-85%25-blue)](https://github.com/your-org/pawn-ledger-app/actions)

### 📄 License & Status
[![License](https://img.shields.io/badge/license-proprietary-lightgrey)](#)
[![Status](https://img.shields.io/badge/status-production-%2300A86B)](#)

---

## Live Numbers

| Metric | Value |
|--------|-------|
| Active loan volume (single period) | ₹100 Cr+ |
| Live loan tickets | 18682+ |
| Pledger profiles | 8670+ |
| Real-time cash-in-hand tracked | ₹15 Lakh+ |
| Investors | 100+ |
| Platforms | Android · iOS · Tablet · Windows · macOS · Linux |
| Engineers who built it | **1** |

---

## What It Manages

- **Gold & silver loans** — ticket creation, ornament valuation, interest calculation, closure
- **Deposit accounts** — Saving / Flexi / Fixed with compound/simple interest
- **Investors** — deposits, returns, linked accounts
- **Auction lifecycle** — cut-off dates, unredeemed tickets, sold/unsold asset routing
- **Multi-ledger accounting** — Cash ledger, Service ledger, Asset transactions
- **Multi-user access** — Admin + employees, per-user access control
- **Offline operations** — works without internet, syncs when back online

---

## Tech Stack

**Frontend**
- Dart · Flutter
- Monorepo · Clean Architecture · Feature-first modules
- Repository pattern · Dependency injection
- Offline-first local database
- Background sync queue (version-based delta)

**Backend**
- Oracle Cloud AI Database (primary store)
- Redis (cache layer for hot reads)
- REST API (Swagger OpenAPI → auto-generated Dio client)
- Email automation (backend-triggered)

**Auth & Messaging**
- Firebase Authentication
- Push notifications · Remote config

**Hardware**
- USB microscope — native Android bridge (custom, no plugin)
- Native Android camera bridge (ID proof / pledger photo)

**Platforms**
- Android · iOS · Tablet · Windows · macOS · Linux

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    CLIENT LAYER                         │
│  Android · iOS · Tablet · Windows · macOS · Linux       │
│  Flutter (Dart) · Monorepo · Clean Architecture         │
│  Offline-first DB · Background Sync Queue               │
└────────────────────┬────────────────────────────────────┘
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
  Local DB       Dio Client    Firebase
  (offline)    (Swagger-gen)  Auth · Push
       │             │
       └──────┬──────┘
              │ Version-delta sync
              ▼
┌─────────────────────────────────────────────────────────┐
│                    SERVER LAYER                         │
│  Oracle Cloud AI DB  ←→  Redis Cache                   │
│  Swagger OpenAPI · Email Automation                     │
└─────────────────────────────────────────────────────────┘
              │
┌─────────────────────────────────────────────────────────┐
│                  HARDWARE LAYER                         │
│  USB Microscope (native Android)  · Camera Bridge       │
└─────────────────────────────────────────────────────────┘
```

---

## Architecture Decisions

**Why Oracle over NoSQL?**  
Financial data needs ACID. A loan closure that double-posts is a legal problem. Oracle's row-level locking and transaction guarantees are the right foundation when cash figures must be auditable months later.

**Why offline-first?**  
Tier-2/3 India: power cuts happen, mobile signal drops in ground-floor shops. Local DB is the primary source of truth — not a cache. No pledger turned away because the server is unreachable.

**Why version-based sync (not timestamp)?**  
Timestamps lie — devices have clock drift. A tablet offline for 4 days with a fast clock silently overwrites data. Version vectors are deterministic. Same model Git uses.

**Why Flutter desktop?**  
The accountant uses Windows. The owner uses Android. The admin uses iPad. One codebase, one CI pipeline. Maintaining React + Android + iOS separately is a 6-person team.

**Why Swagger code generation?**  
Manually written API clients drift. Backend changes a field → Flutter doesn't know → null pointer in production. With generation, the compiler tells you exactly what broke.

**Why Redis?**  
Dashboard reads are hot. Aggregating "live tickets" and "cash in hand" against Oracle on every refresh is wasteful. Redis sits in front as an invalidation-aware cache.

---

## Domain: Ledger Configuration

Three different cash-flow topologies. Owner picks the one matching their accounting model.

### Option 1 — Service Ledger Inclusive
| Event | Direction | Ledger |
|-------|-----------|--------|
| Loan create: Principal | OUT | Cash |
| Loan create: Advance, Doc Charge, Service | IN | **Service** |
| Loan close: Principal | IN | Cash |
| Loan close: Basic Interest | OUT Service → | Cash |

### Option 2 — Cash Ledger Only
All transactions consolidated in Cash ledger. No service ledger entries.

### Option 3 — Hybrid Service Ledger
Service charges → Service ledger. Everything else → Cash ledger.

---

## Engineering Fingerprints

Things that exist in this codebase and nowhere else:

**01 · USB Microscope Bridge**
- No Flutter plugin for this. Built from scratch.
- Uses Android `UsbManager` → bulk-transfer endpoint → MJPEG stream → Flutter platform channel
- Operator captures ornament photo directly into loan ticket

**02 · Date Round-Off Engine**
- Loan closure date is a *derived* value, not user input
- Shop defines ranges: "1st–7th → rounds to 7th"
- Interest calculated to the rounded date
- Configurable per range, per shop

**03 · Audit Color Semantics on Closed Loans**
- White → fully settled
- Light grey → paid less than advance
- Yellow → within round-off tolerance
- Amber → less than interest on close date
- Red → below principal (urgent)

**04 · Three Interest Engines**
- Simple: `(P × R × T) / 100`
- Compound: `P × (1 + R/(n×100))^(n×T) − P`
- Reinvested: simple interest per period, principal updated each cycle
- All decimal-safe arithmetic — no floating-point money math

**05 · Swagger → Dio Code Generation**
- OpenAPI spec → Dio client + models + error types generated into Flutter monorepo
- Compiler catches API drift at build time, not at runtime

**06 · 24-Column CSV Import**
- Bulk-migrate years of paper records
- Validates mobile format, dates, enum values (Gold/Silver, Open/Closed/Auction)
- Atomic pledger + ticket creation
- Row-level error reporting — no silent failures

**07 · Receivable Safety Guards**
- Hard warning: receivable < principal (possible write-off)
- Critical warning: receivable < principal + advance (capital loss)
- Shows exact rupee difference
- Cannot be dismissed without explicit confirmation

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
 Redeemed    Unredeemed
 before       on auction
 auction       day
    │           │
 Excluded    Sold publicly
              │
         ┌────┴────┐
         │         │
       Sold      Unsold
         │         │
    Update      Move to
    ledger     org assets
```

Closing an auction locks all associated tickets — they cannot be edited post-closure.  
Reopening deletes the asset transaction and unlocks all tickets (reversal record created).

---

## Battle Scars

**Partial payment tally corruption**  
Reopen → close cycles double-counted partial payments. Fix: reopen flow now explicitly resets both the closure transaction and any partial tally.

**Sync version collision**  
Two devices edit the same ticket offline. Version-based conflict detection surfaces this as a visible warning instead of silently dropping one change.

**Decimal precision in compound interest**  
IEEE 754 floats accumulate errors over months. All interest math moved to decimal-safe arithmetic.

**Subscription expiry mid-operation**  
Non-dismissable renewal notice now shows company ID, GST, and licence pre-filled — so owner can resume without data loss.

---

## Deposit Types

| Type | Interest | Key Feature |
|------|----------|-------------|
| Saving Account | Daily, credited monthly | Payable toggle |
| Flexi Deposit | Compound, dynamic | Flexible withdrawal · Daily visibility |
| Fixed Deposit | Compound, locked term | Auto-renewal option |

---

## Interest Calculation Methods

| Method | Formula | Use Case |
|--------|---------|---------|
| Simple Interest | `(P × R × T) / 100` | Standard loans |
| Compound Interest | `P × (1 + R/n×100)^nT − P` | Deposits |
| Reinvested Interest | Simple, reinvested per period | Long-term instruments |

---

## Multi-User Access Model

```
Admin (Owner)
├── Full access: all modules
├── Can create / manage employees
├── Approves loan closures (configurable)
└── Device password for fast admin switch

Employee
├── Access controlled by admin
├── Can create loan tickets
├── Can view assigned modules
└── Cannot access financial reports (if restricted)
```

---

## Built By

**Anand Alagappan** — CEO, Subbu App Tech  
Mannargudi, Tamil Nadu · Founded 2024

- 📧 contactus@subbuapptech.in  
- 🌐 [lendwise.subbuapptech.in](https://lendwise.subbuapptech.in)  
- 📱 [Play Store](https://play.google.com/store/apps/details?id=com.subbu.lend_wise)

---

*LendWise © 2024–2026 · Subbu App Tech · Built in Mannargudi. Deployed in Production.*
