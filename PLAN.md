# SimpleSoft — Master Build Plan
**India-focused SMB ERP: "Tally's simplicity + Odoo's power"**
**Stack: FastAPI + React + React Native | Target: Indian SMBs ₹5–100 Cr revenue**

---

## Current System State (What's Built)

### Backend — Real Implementations
| Module | Status | Router |
|--------|--------|--------|
| Auth (login, register, JWT, refresh) | ✅ Done | `routers/auth.py` |
| Users + Workspace info + AI Config | ✅ Done | `routers/users.py` |
| Projects (CRUD, kanban, members) | ✅ Done | `routers/projects.py` |
| Tasks (CRUD, assignees, priorities) | ✅ Done | `routers/tasks.py` |
| Contacts (CRUD, search) | ✅ Done | `routers/contacts.py` |
| Teams (CRUD, members) | ✅ Done | `routers/teams.py` |
| HR (employees, basic) | ✅ Done | `routers/hr.py` |
| Roles & Permissions | ✅ Done | `routers/roles.py`, `routers/permissions.py` |
| Dashboard (stats) | ✅ Done | `routers/dashboard.py` |
| Notifications | ✅ Done | `routers/notifications.py` |
| SuperAdmin | ✅ Done | `routers/superadmin.py` |

### Backend — Stubs (return empty, need real implementation)
| Module | Status |
|--------|--------|
| CRM (leads, pipeline, stages) | 🔲 Stub |
| Sales Orders | 🔲 Stub |
| Attendance (check-in/out) | 🔲 Stub |
| Calendar Events | 🔲 Stub |
| Chat (channels, messages) | 🔲 Stub |
| Inventory (products, stock) | 🔲 Stub |
| HR Leave management | 🔲 Stub |
| Performance / Leaderboard | 🔲 Stub |
| Files (upload, directories) | 🔲 Stub |
| Notes (CRUD) | 🔲 Stub |
| Automation (workflows) | 🔲 Stub |

### Backend — Not Yet Created (ERP Core)
| Module | Status |
|--------|--------|
| Invoicing (create, PDF, email, UPI) | ❌ Not built |
| Accounting (GL, journal entries, P&L, balance sheet) | ❌ Not built |
| GST Compliance (GSTR-1, GSTR-3B, auto-calc) | ❌ Not built |
| Purchase Orders | ❌ Not built |
| Products/Pricing (for invoicing) | ❌ Not built |
| Module-based billing / feature gating | ❌ Not built |

### Frontend Pages (React)
All pages exist. Dashboard, Projects, Tasks, Contacts, Team, Profile, Settings, Notes, Chat, CRM, HR, Sales, Calendar, Inventory, Attendance, Automation, Performance, Files are present but most are UI-only shells calling stubs.

### Mobile App
`mobile_app/` directory exists. More feature-complete than web frontend.

---

## Product Vision

**Free forever core:** Invoicing, Accounting, GST, Contacts, Basic Reporting  
**Paid add-on modules:** Inventory, Sales Order, CRM, Attendance, HR, etc.  
**Pricing:** ₹0 free core → ₹299–₹2,499/month per add-on module  
**USP:** Setup in 1 day (vs Odoo 6 months), GST built-in (not plugin), mobile-first, India servers

---

## Module Pricing Map (for feature gating)

```
FREE FOREVER (no gating needed):
  - Invoicing, Accounting, GST, Contacts, Basic Reporting

PAID ADD-ONS (check workspace.active_modules before serving):
  ₹499/mo  — Inventory
  ₹499/mo  — Sales Order
  ₹499/mo  — Purchase Order
  ₹399/mo  — Advanced Reporting
  ₹749/mo  — CRM Lite
  ₹299/mo  — Attendance
  ₹299/mo  — Vendor Portal
  ₹299/mo  — Payment Reminders
  ₹299/mo  — SMS/WhatsApp Invoices
  ₹749/mo  — Bank Feed Auto-Match
  ₹749/mo  — Profit Analysis
  ₹999/mo  — Inventory Forecasting
  ₹1,499/mo — Expense AI
  ₹1,999/mo — CRM Full
  ₹2,499/mo — Multi-Location
  ₹1,999/mo — Subscription Billing
  ₹1,499/mo — Advanced Accounting
```

---

## Build Phases

---

### PHASE 1 — ERP Core (Highest Priority)
**Goal:** Reach beta with paying users. These 4 modules alone make SimpleSoft valuable.

#### 1.1 Invoicing Module
**Backend tasks:**
- [ ] Create `models/invoice_sequence.py` — SequenceCounter model (safe per-workspace numbering, see Key Technical Decisions)
- [ ] Create `models/invoice.py` — Invoice, InvoiceLine models
  - Invoice fields: workspace_id, invoice_number (via SequenceCounter), customer_id, date, due_date, status (draft/sent/paid/overdue/cancelled), place_of_supply (2-digit state code), subtotal, discount_total, cgst_total, sgst_total, igst_total, total, notes
  - **LOCK RULE: invoice is immutable once status = "sent". Edits after send require a Credit/Debit Note.**
  - InvoiceLine fields: invoice_id, product_id (FK → products, nullable for ad-hoc lines), description, hsn_code, qty, unit_price, discount_amount, gst_rate, cgst_amount, sgst_amount, igst_amount, line_total
- [ ] Create `models/workspace_gst_config.py` — WorkspaceGSTConfig (gstin, legal_name, trade_name, state_code, is_composition_scheme — see Key Technical Decisions)
- [ ] Create `routers/invoices.py` with endpoints:
  - `POST /invoices` — create (auto-calc GST: compare workspace state_code vs place_of_supply)
  - `GET /invoices` — list (filter by status, customer, date)
  - `GET /invoices/{id}` — detail
  - `PATCH /invoices/{id}` — update (draft only — reject with 409 if status != "draft")
  - `DELETE /invoices/{id}` — delete (draft only)
  - `POST /invoices/{id}/send` — mark as sent + create journal entries (DR AR / CR Revenue + GST Liability)
  - `POST /invoices/{id}/mark-paid` — record payment + create journal entries (DR Bank / CR AR)
  - `GET /invoices/{id}/pdf` — generate PDF via WeasyPrint HTML template
  - `POST /invoices/{id}/email` — send PDF via email
  - `POST /invoices/{id}/upi-link` — generate Razorpay UPI link
- [ ] Add DB migrations for invoice, invoice_line, invoice_sequence, workspace_gst_config tables
- [ ] PDF generation using **WeasyPrint** (HTML → PDF via Jinja2 template, NOT reportlab)

**Frontend tasks:**
- [ ] `pages/invoicing/InvoicesPage.jsx` — list with status badges, filters
- [ ] `pages/invoicing/InvoiceFormPage.jsx` — create/edit form
  - Customer dropdown (from Contacts)
  - Dynamic line items table (add/remove rows)
  - GST auto-calculation per line (CGST+SGST or IGST)
  - Grand total display
- [ ] `pages/invoicing/InvoiceDetailPage.jsx` — view, download PDF, send email, UPI link
- [ ] Add "Invoicing" to Sidebar navigation

#### 1.2 Contacts Enhancement (Customer/Vendor Master)
**Backend tasks:**
- [ ] Add fields to contacts model: `contact_type` (customer/vendor/both), `gstin`, `billing_address`, `shipping_address`, `credit_limit`, `payment_terms`
- [ ] Add migration for new contact fields
- [ ] `GET /contacts/customers` — filtered list for invoice dropdown
- [ ] `GET /contacts/vendors` — filtered list for PO dropdown

**Frontend tasks:**
- [ ] Upgrade `pages/contacts/ContactsPage.jsx` — add GST number, address, contact type fields
- [ ] Customer/Vendor tabs in contacts list

#### 1.3 Accounting Module
**Backend tasks:**
- [ ] Create `models/accounting.py` — ChartOfAccount, JournalEntry, JournalEntryLine models
  - ChartOfAccount: account_code (1000-5999), name, type (asset/liability/equity/income/expense), is_active
  - JournalEntry: workspace_id, date, reference, description
  - JournalEntryLine: entry_id, account_id, debit, credit
- [ ] Seed 80 pre-built India-standard GL accounts on workspace creation
- [ ] Create `routers/accounting.py` with endpoints:
  - `GET /accounting/accounts` — chart of accounts
  - `POST /accounting/journal-entries` — manual journal entry
  - `GET /accounting/journal-entries` — list entries
  - `GET /accounting/reports/trial-balance?from=&to=` — trial balance
  - `GET /accounting/reports/profit-loss?from=&to=` — P&L statement
  - `GET /accounting/reports/balance-sheet?as_of=` — balance sheet
  - `GET /accounting/reports/daybook?date=` — daily transactions
- [ ] Auto-create journal entries when invoice is created/paid:
  - Invoice created (sent): DR Accounts Receivable / CR Sales Revenue + GST Liability
  - Invoice paid: DR Bank/Cash / CR Accounts Receivable
- [ ] Add DB migrations for accounting tables

**Frontend tasks:**
- [ ] `pages/accounting/AccountingPage.jsx` — tabbed: Trial Balance, P&L, Balance Sheet
- [ ] `pages/accounting/JournalPage.jsx` — manual journal entry form + entry list
- [ ] Date range selectors, export buttons (PDF/Excel)
- [ ] Add "Accounting" to Sidebar

#### 1.4 GST Compliance Module
**Backend tasks:**
- [ ] WorkspaceGSTConfig model already created in 1.1 — just add the GST router endpoints here
- [ ] DO NOT add gstin/state_code directly to Workspace model — use WorkspaceGSTConfig table instead
- [ ] Create `routers/gst.py` with endpoints:
  - `GET /gst/config` — get workspace GST settings
  - `POST /gst/config` — save GST settings
  - `GET /gst/gstr1?month=&year=` — generate GSTR-1 data (JSON + preview)
  - `GET /gst/gstr3b?month=&year=` — generate GSTR-3B data
  - `GET /gst/gstr1/export?month=&year=` — download GSTR-1 JSON file
  - `GET /gst/gstr3b/export?month=&year=` — download GSTR-3B JSON file
  - `GET /gst/reports?from=&to=` — tax summary by rate
- [ ] GST calculation logic:
  - Intra-state: CGST (rate/2) + SGST (rate/2) — detect by same state code
  - Inter-state: IGST (full rate) — different state
  - GSTR-1 categorization: B2B (GSTIN present), B2C (no GSTIN, <₹2.5L), B2CS (>₹2.5L)

**Frontend tasks:**
- [ ] `pages/gst/GSTPage.jsx` — tabbed: Config, GSTR-1, GSTR-3B, Reports
- [ ] Month picker + preview table + Export JSON button
- [ ] Instructions for uploading to GST portal
- [ ] Add "GST" to Sidebar

---

### PHASE 2 — Commerce Modules (Paid Add-ons)
**Goal:** Convert free users to paying. Each module is feature-gated by `workspace.active_modules`.

#### Feature Gating Pattern
```python
# In each paid router:
def check_module(workspace: Workspace, module_name: str):
    if module_name not in (workspace.active_modules or []):
        raise HTTPException(403, f"Module '{module_name}' not active. Upgrade to enable.")
```
- [ ] Add `active_modules: JSON` field to Workspace model (list of active module keys)
- [ ] Add migration for `active_modules` column
- [ ] Create `POST /workspace/modules/activate` endpoint (admin, for testing/admin activation)

#### 2.1 Inventory Module (₹499/mo)
**Backend tasks:**
- [ ] Create `models/inventory.py` — Product (with stock), StockMovement, Warehouse models
  - Product: name, SKU, barcode, category, unit, cost_price, selling_price, gst_rate, reorder_level, current_stock
  - StockMovement: product_id, warehouse_id, type (in/out/adjustment), quantity, reference, date
- [ ] Replace stub endpoints in `routers/stubs.py` → new `routers/inventory.py`
  - Full CRUD for products
  - `POST /inventory/stock-in` — receive stock
  - `POST /inventory/stock-out` — ship/adjust stock
  - `GET /inventory/movements` — stock movement history
  - `GET /inventory/low-stock` — products below reorder level
  - `GET /inventory/valuation` — total stock value
- [ ] Auto-reduce stock when invoice line item matches a product

**Frontend tasks:**
- [ ] Upgrade `pages/inventory/InventoryPage.jsx` — real product list, stock levels, low-stock alerts
- [ ] Product form (add/edit), stock adjustment modal

#### 2.2 Sales Order Module (₹499/mo)
**Backend tasks:**
- [ ] Create `models/sales.py` — SalesOrder, SalesOrderLine models
  - SalesOrder: workspace_id, customer_id, so_number, date, delivery_date, status (draft/confirmed/dispatched/delivered/invoiced), total
  - SalesOrderLine: so_id, product_id, qty, unit_price, gst_rate, line_total
- [ ] Replace sales stubs → new `routers/sales_orders.py`
  - Full CRUD + status workflow
  - `POST /sales/orders/{id}/confirm` — reserve stock
  - `POST /sales/orders/{id}/dispatch` — mark dispatched
  - `POST /sales/orders/{id}/invoice` — create invoice from SO (one-click)
  - `GET /sales/orders/open` — pending orders
- [ ] Stock reservation on SO confirmation
- [ ] SO → Invoice conversion (copy customer, lines, auto-fill)

**Frontend tasks:**
- [ ] Upgrade `pages/sales/SalesPage.jsx` — real SO list, kanban by status
- [ ] SO creation form (customer, products, delivery date)
- [ ] "Create Invoice" button on confirmed SO

#### 2.3 Purchase Order Module (₹499/mo)
**Backend tasks:**
- [ ] Create `models/purchase.py` — PurchaseOrder, PurchaseOrderLine, GoodsReceipt models
- [ ] Create `routers/purchase_orders.py`
  - Full CRUD + workflow (draft/sent/confirmed/received/billed)
  - `POST /purchase/orders/{id}/receive` — goods receipt, update stock
  - `POST /purchase/orders/{id}/bill` — create vendor bill
- [ ] 3-way match: PO qty vs GR qty vs vendor bill

**Frontend tasks:**
- [ ] New `pages/purchase/PurchasePage.jsx` — PO list, creation form, GR form
- [ ] Add "Purchase" to Sidebar

#### 2.4 Real Attendance (₹299/mo)
**Backend tasks:**
- [ ] Create `models/attendance.py` — AttendanceRecord model
  - Fields: user_id, workspace_id, check_in_time, check_out_time, date, work_hours, location (GPS optional)
- [ ] Replace attendance stubs → `routers/attendance_real.py`
  - `POST /attendance/check-in` — record check-in with timestamp
  - `POST /attendance/check-out` — record check-out, calculate hours
  - `GET /attendance/me` — my attendance history
  - `GET /attendance/team` — team attendance (manager/admin)
  - `GET /attendance/report?from=&to=` — monthly attendance report

**Frontend tasks:**
- [ ] Upgrade `pages/attendance/AttendancePage.jsx` — real check-in/out button, history table

#### 2.5 Real HR Leaves (free — part of HR module)
**Backend tasks:**
- [ ] Create `models/leave.py` — LeaveType, LeaveAllocation, LeaveRequest models
- [ ] Replace HR leave stubs with real implementation
- [ ] Auto-deduct from allocation on approval

#### 2.6 Real Notes (free)
**Backend tasks:**
- [ ] Create `models/note.py` update — add `content`, `is_pinned`, `color`, `tags`
- [ ] Replace notes stubs with real CRUD backed by DB

#### 2.7 Real Chat (free)
**Backend tasks:**
- [ ] Create `models/chat.py` — Channel, Message models
- [ ] Replace chat stubs with real implementation (or WebSocket if needed)

---

### PHASE 3 — Paid Add-on Modules (Revenue Growth)

#### 3.1 CRM Lite (₹749/mo)
- [ ] Replace CRM stubs with real Lead/Opportunity models
- [ ] Kanban pipeline (Qualified → Proposal → Won/Lost)
- [ ] Activity log (calls, emails, meetings)
- [ ] Lead → SO → Invoice conversion chain
- [ ] Upgrade `pages/crm/CRMPage.jsx`

#### 3.2 Payment Reminders (₹299/mo)
- [ ] Cron job: scan overdue invoices, send email reminders at 7/15/30 days
- [ ] Reminder log per invoice
- [ ] Settings: enable/disable, customize message templates

#### 3.3 Bank Feed Auto-Match (₹749/mo)
- [ ] `POST /accounting/bank-statement/upload` — accept CSV
- [ ] Auto-match by amount + date against invoices
- [ ] Manual match UI
- [ ] Auto-create journal entry on match

#### 3.4 SMS/WhatsApp Invoices (₹299/mo)
- [ ] Integrate Twilio/MSG91 for SMS
- [ ] WhatsApp Business Cloud API
- [ ] Auto-send invoice PDF link on `POST /invoices/{id}/send`
- [ ] Delivery tracking

#### 3.5 Profit Analysis (₹749/mo)
- [ ] `GET /reports/profit-by-product` — margin per product
- [ ] `GET /reports/customer-ltv` — customer lifetime value
- [ ] `GET /reports/revenue-trend` — month-on-month

#### 3.6 Expense AI (₹1,499/mo — your moat)
- [ ] `POST /expenses/receipt` — upload receipt image
- [ ] Call OCR API (Google Vision or Tesseract) to extract amount/vendor/date
- [ ] Call AI (Claude/GPT) to suggest GL account
- [ ] User approves in 1 click → auto-create journal entry
- [ ] Bulk upload support

---

### PHASE 4 — Module Marketplace Infrastructure

#### 4.1 Marketplace UI
- [ ] `pages/marketplace/MarketplacePage.jsx` — grid of available modules
- [ ] Each module card: name, description, price/mo, "Active" or "Buy" button
- [ ] 7-day free trial per module
- [ ] `pages/billing/BillingPage.jsx` — active subscriptions, renewal dates, cancel

#### 4.2 Razorpay Subscription Integration
- [ ] `POST /billing/subscribe` — create Razorpay subscription for a module
- [ ] Razorpay webhook: `POST /billing/webhook` — update `workspace.active_modules` on payment success/failure
- [ ] `POST /billing/cancel` — cancel module subscription

#### 4.3 Feature Gating Middleware
- [ ] Dependency `require_module("inventory")` — reusable FastAPI dep
- [ ] All Phase 2+ paid routers use this dep
- [ ] Frontend: detect 403 with module key → show "Unlock Module" prompt

---

### PHASE 5 — Advanced Modules (Month 13+)
- CRM Full (₹1,999/mo) — email sync, territory, campaigns
- Multi-Location (₹2,499/mo) — multiple warehouses/stores, consolidated reports
- Manufacturing Lite (₹999/mo) — BOM, production orders, scrap tracking
- Subscription Billing (₹1,999/mo) — recurring invoices, churn analytics
- Advanced Accounting (₹1,499/mo) — depreciation, budgeting, cost centers

---

## Implementation Order (Start Here)

```
Week 1-2:  Invoice model + API (backend)
Week 2-3:  Product catalog + Invoice PDF generation
Week 3-4:  Invoice UI (list + form + detail)
Week 4-5:  Contacts enhancement (GST number, address, type)
Week 5-6:  Accounting model + GL seeding + basic reports API
Week 6-7:  Accounting UI (Trial Balance, P&L, Balance Sheet)
Week 7-8:  GST config + GSTR-1/3B generation + export
Week 8-9:  GST UI + test with real invoice data
Week 9-10: Dashboard upgrade (add invoice KPIs, GST due, cash position)
Week 10:   Deploy, get 50 beta users, collect feedback

Month 4+:  Start Phase 2 (Inventory first — 80% of users need it)
Month 5+:  Sales Order + Purchase Order
Month 6+:  Feature gating + Razorpay billing integration
Month 7+:  Phase 3 add-ons based on user demand
```

---

## Key Technical Decisions

### Database Migrations Pattern
Use the `_MIGRATIONS` list in `database.py` — safe idempotent ALTER TABLE statements run on startup. This is the existing pattern, continue using it.

### Module Feature Gating
```python
# backend/app/core/deps.py — add this:
def require_module(module_key: str):
    async def _check(
        current_user: User = Depends(get_current_user),
        db: AsyncSession = Depends(get_db)
    ):
        ws = await db.get(Workspace, current_user.workspace_id)
        modules = ws.active_modules or []
        if module_key not in modules:
            raise HTTPException(403, detail={
                "code": "MODULE_LOCKED",
                "module": module_key,
                "message": f"Upgrade to unlock {module_key}"
            })
        return current_user
    return _check
```

### GST Calculation Logic
Determine intra vs inter-state by comparing workspace `state_code` (from WorkspaceGSTConfig) against the invoice `place_of_supply` field. Never use a boolean flag — always compare state codes explicitly.

```python
def calc_gst(amount: float, rate: float, workspace_state_code: str, place_of_supply: str):
    tax = amount * rate / 100
    if workspace_state_code == place_of_supply:   # intra-state
        return {"cgst": round(tax/2, 2), "sgst": round(tax/2, 2), "igst": 0}
    return {"cgst": 0, "sgst": 0, "igst": round(tax, 2)}   # inter-state
```

`place_of_supply` is the 2-digit GST state code stored explicitly on every Invoice. The GST portal requires it for GSTR-1 — it cannot be inferred from the customer's address after the fact.

### Invoice Number: SequenceCounter (NOT count-based)
The `count()` approach has two fatal flaws: (1) deleting a draft drops the count so the next number collides with an already-sent invoice; (2) concurrent creates under load race to the same number. Use a dedicated sequence counter table that only ever increments.

```python
# models/invoice_sequence.py
class InvoiceSequence(Base):
    __tablename__ = "invoice_sequences"
    workspace_id: Mapped[int] = mapped_column(ForeignKey("workspaces.id"), primary_key=True)
    last_number: Mapped[int] = mapped_column(Integer, default=0)

# In invoice creation — run inside the same transaction as the invoice INSERT:
async def next_invoice_number(workspace_id: int, db: AsyncSession) -> str:
    # SELECT FOR UPDATE locks the row so concurrent requests queue up
    result = await db.execute(
        select(InvoiceSequence)
        .where(InvoiceSequence.workspace_id == workspace_id)
        .with_for_update()
    )
    seq = result.scalar_one_or_none()
    if not seq:
        seq = InvoiceSequence(workspace_id=workspace_id, last_number=0)
        db.add(seq)
    seq.last_number += 1
    year = datetime.now().year
    return f"INV-{year}-{seq.last_number:05d}"
    # Caller commits — the incremented row is persisted atomically with the invoice
```

### Invoice Lock Rule (Double-Entry Integrity)
Once an invoice status transitions to `"sent"`, it is **immutable**. Any PATCH to a non-draft invoice returns HTTP 409. If a user needs to correct a sent invoice, they must issue a Credit Note (negative invoice) — the GL entries from the original invoice are never modified directly, only reversed. This keeps the General Ledger uncorrupted.

```python
# In PATCH /invoices/{id}:
if invoice.status != "draft":
    raise HTTPException(409, "Invoice cannot be edited after it has been sent. Issue a Credit Note instead.")
```

### WorkspaceGSTConfig Model
Store GST identity separately from the Workspace row — makes it optional to configure, and avoids polluting the Workspace table with India-specific fields.

```python
class WorkspaceGSTConfig(Base):
    __tablename__ = "workspace_gst_configs"
    workspace_id: Mapped[int] = mapped_column(ForeignKey("workspaces.id"), primary_key=True)
    gstin: Mapped[Optional[str]] = mapped_column(String(15), nullable=True)
    legal_name: Mapped[str] = mapped_column(String(255), nullable=False)
    trade_name: Mapped[Optional[str]] = mapped_column(String(255), nullable=True)
    state_code: Mapped[str] = mapped_column(String(2), nullable=False)  # e.g., "27" for Maharashtra
    is_composition_scheme: Mapped[bool] = mapped_column(Boolean, default=False)
```

### PDF Invoice
Use **WeasyPrint** (HTML → PDF via Jinja2 template). Do NOT use `reportlab` — pixel-level layout tweaks in reportlab take days. WeasyPrint renders any Tailwind/CSS-styled HTML, so the invoice template can be maintained as a `.html` file and previewed in a browser. This approach shaves 3–4 days off the Week 2–3 timeline.

```python
# In GET /invoices/{id}/pdf:
from weasyprint import HTML
html_str = jinja_env.get_template("invoice.html").render(invoice=invoice, config=gst_config)
pdf_bytes = HTML(string=html_str, base_url=base_url).write_pdf()
return Response(content=pdf_bytes, media_type="application/pdf")
```

---

## Go-To-Market (Post Phase 1)

1. **CA Partner Program** — recruit 10 Chartered Accountants; ₹1,000/referred client; they get free lifetime access
2. **YouTube Hindi content** — "Free GST invoice in 5 mins", "Tally vs SimpleSoft"
3. **WhatsApp communities** — traders, distributors, retailer groups
4. **Target metric** — 100 beta users in 3 months, ₹25K MRR by Month 3 end

---

## Success Metrics

| Milestone | Target |
|-----------|--------|
| Phase 1 complete (Invoicing + Accounting + GST) | Month 3 |
| Beta users | 100 |
| First paying module users | 10+ |
| MRR | ₹25,000+ |
| Phase 2 complete (Inventory + SO) | Month 5 |
| Total users | 300+ |
| MRR | ₹60,000+ |
| Phase 3 (first 3 addons live) | Month 8 |
| MRR | ₹1,50,000+ |
