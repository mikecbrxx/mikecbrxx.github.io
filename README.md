# Road Eagles MC Thailand — Club Management App

A private progressive web app (PWA) for managing club operations, finances, merchandise, events and membership for Road Eagles MC Thailand.

## Overview

Single-file HTML application with no server-side dependencies. All data is stored in [Supabase](https://supabase.com) and synced across devices. The app runs in any modern mobile browser and can be installed as a PWA on Android or iOS.

## Features

- **Dashboard** — account balances with month opening figures, stock alerts, tab summary
- **Ledger** — full income/expense tracking across Club and Charity accounts
- **Members** — member management, fee tracking, annual fee reset
- **Merchandise** — catalogue with member/visitor pricing, stock tracking, merch sales
- **Tab Ledger** — open merch tabs, unpaid transactions, PO outstanding balances
- **Purchase Orders** — full PO lifecycle with payments, stock receipt, and assembly groups (BOM)
- **Events** — event management with income/expense tracking
- **Suppliers** — full supplier records
- **Reports** — Monthly Statement, Annual Summary, Membership, Merchandise Sales, Stock, Donations, Purchase Ledger, Cost of Goods, Analysis, Club Meeting Report, Audit Trail
- **Categories & Config** — configurable income/expense categories, product categories, event types, sizes, colours, roles, assembly groups
- **Backup & Restore** — JSON export/import with configurable filename prefix and reminder system

## Access & Roles

| Role | Access |
|------|--------|
| Admin | Full access including settings, delete, audit trail, backup |
| Officer | Standard operational access |
| Title Only | Display role, no app login |

Access is controlled by 4-digit PIN. PINs are managed by Admin in More → Settings.

## Getting Started

### New Device Setup

1. Open the app URL in Chrome or Brave
2. The app automatically syncs from Supabase before the PIN screen appears
3. Enter your 4-digit PIN
4. No credentials setup needed — hardcoded in the app

### Updating to a New Version

1. In Chrome: Settings → Privacy → Clear browsing data → **Cached images and files only**
2. Reload the page — the new version loads from GitHub
3. Do **not** clear site data or cookies — this is not needed and not recommended

### If Login is Blocked

If the app shows a connection error on startup, check your internet connection and tap **↺ Try Again**. Login is intentionally blocked when Supabase cannot be reached to prevent stale local data being used.

## File Structure

```
road-eagles-mc-final.html   — Live production app
remc-test.html              — Test system (separate Supabase database)
README.md                   — This file
```

## Technical Details

- **Storage**: Supabase PostgreSQL via REST API (`club_data` table, `main` row)
- **Local cache**: Browser localStorage (`remct4` key)
- **Credentials**: Hardcoded in app JS as fallback, also stored in `remct_sync` localStorage key
- **Sync**: On every save, data pushes to Supabase. On app open, data pulls from Supabase before PIN entry
- **Offline**: App blocks login when Supabase is unreachable — intentional by design

## Test System

The test app (`remc-test.html`) connects to a separate Supabase project and uses separate localStorage keys (`remct4_test`, `remct_sync_test`). Live and test data are completely independent.

## Supabase Maintenance

Run annually on both live and test projects (before October 2026):

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON public.club_data TO anon;
ALTER TABLE public.club_data ENABLE ROW LEVEL SECURITY;
SELECT * FROM pg_policies WHERE tablename = 'club_data';
```

## Backup

- Admin only: More → Settings → Backup & Restore
- Configure filename prefix (default: `RE-Backup`)
- Set reminder interval in days (0 = disabled)
- Backups are full JSON exports of all club data
- Restore accepts any valid backup file regardless of filename

## Version History

| Version | Notes |
|---------|-------|
| v3.16 | Configurable backup filename, assembly group stock fix, report improvements |
| v3.00 | Assembly groups, dynamic stock tabs, tab ledger edit, report opening/closing balances |
| v2.93 | Startup fetch gate, hardcoded credentials, login blocked offline |
| v2.88 | Data loss fix — manualSync only pushes on empty remote |
| v2.76 | Roles system with app access flags |

---

*Road Eagles MC Thailand — Private use only*
