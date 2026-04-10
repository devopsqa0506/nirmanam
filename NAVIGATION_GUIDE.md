# SiteBook - Code Navigation Guide

## What Changed
Your `index.html` (7499 lines) now has **clear section markers** throughout. No code was changed - only comments added.

---

## How to Find Things

### **Quick Find Method**
Press `Ctrl+F` (Windows/Linux) or `Cmd+F` (Mac) and search for:

```
<!-- SECTION:
```

This will show all major sections.

---

## Major Sections & What's Inside

### **1. HEAD & SETUP (Lines 1-750)**
**Search for:** `<!-- SECTION: LOGIN`

Contains:
- Google Sign-In configuration
- Setup wizard (Step 0: Welcome, Step 1: Sheet URL, Step 2: Gemini Keys)
- CSS variables and global styles

---

### **2. HOME PAGE (Lines ~799)**
**Search for:** `<!-- SECTION: HOME PAGE -->`

Contains:
- Dashboard display
- Recent projects list
- Quick action buttons
- Quick stats

**Related Functions:**
- `renderRecent()`
- `refreshUI()`
- `loadRecentProjects()`

---

### **3. AI CHAT PAGE (Lines ~821)**
**Search for:** `<!-- SECTION: AI CHAT PAGE -->`

Contains:
- Gemini chatbot integration
- Conversation history
- Message input and sending

**Related Functions:**
- `initChatPage()`
- `sendChatMessage()`
- `processChatWithGemini()`

---

### **4. PROJECTS PAGE (Lines ~856)**
**Search for:** `<!-- SECTION: PROJECTS PAGE -->`

Contains:
- Project list and filtering
- Create new project form
- House Construction vs Land Sale projects
- Project cards showing progress

**Related Functions:**
- `renderProjects()`
- `addProject()`
- `updateProject()`
- `deleteProject()`

---

### **5. EXPENDITURE PAGE (Lines ~872)**
**Search for:** `<!-- SECTION: EXPENDITURE PAGE -->`

Contains:
- Project-wise expense tracking
- Spending breakdown by category
- Expense analytics

**Related Functions:**
- `renderExpenditure()`
- `calculateProjectExpenses()`

---

### **6. CONTACTS PAGE (Lines ~887)**
**Search for:** `<!-- SECTION: CONTACTS PAGE -->`

Contains:
- Vendor list and management
- Contact information
- Phone, category, notes
- AI-powered vendor search

**Related Functions:**
- `upsertContact(opts)`
- `renderContacts()`
- `addVendor()`
- `autoAddVendorContact(name, phone, category)`

---

### **7. DOCUMENTS PAGE**
**Search for:** `<!-- SECTION: DOCUMENTS PAGE -->`

Contains:
- Bill upload form
- Bill photo processing with Gemini
- Line item extraction
- Document gallery
- Price change alerts

**Key Features:**
- Auto-extract bill items using Gemini 2.5 Flash
- Cross-vendor price history
- "Add to Ledger" quick button

**Related Functions:**
- `handlePhoto()` - Main bill processing
- `processBillLineItems()` - Gemini integration
- `buildBillAlertHtml()` - Price warnings
- `autoAddBillVendorContact()` - Auto-add vendor
- `buildBillConfirmCard()` - Confirmation UI

---

### **8. FINANCE PAGE (Lines ~966)**
**Search for:** `<!-- SECTION: FINANCE PAGE -->`

This is the most complex section. It has 4 sub-tabs:

#### **8.1 Fund Register (Loans)**
**Look for:** `id="fund-register"` inside finance section

Contains:
- List of all loans (bank, gold, hand)
- Loan summary (total borrowed, total repaid, balance)
- Add new loan button

**Related Functions:**
- `saveLoan()`
- `deleteLoan(loanId)`
- `renderFundRegister()`
- `getBalance(vendorId)` - Calculate remaining balance

#### **8.2 Loan Repayments**
**Look for:** `id="loan-repayments"` inside finance section

Contains:
- Repayment tracker for each loan
- Separate fields: Principal Amount + Interest Amount
- Date tracking
- Auto-calculated interest (prorated till today)
- Interest type: % per month, % per annum, flat ₹/month, zero

**Related Functions:**
- `savePaymentV2(repaymentData)` - NEW function (Apr 2026)
- `renderRepayments()`
- `calculateInterestAccrued(loanId, tillDate)`
- `splitInterestPrincipal()` - Separate accrued interest from principal

**New Schema (Apr 2026):**
```javascript
loanPayments[] = [
  {
    loanId: "...",
    date: "DD-MMM-YYYY",
    principalAmount: 50000,      // NEW: separate field
    interestAmount: 5000,          // NEW: separate field
    notes: "..."
  }
]
```

#### **8.3 Project Expenditure**
**Look for:** `id="project-expenditure"` inside finance section

Contains:
- Project-wise expense tracking
- Expense categories: builder, advance, registration, borewell, brokerage, misc
- Total spent per project

**Related Functions:**
- `renderProjectExpenditure()`
- `addProjectExpense()`

#### **8.4 P&L Statement (Profit & Loss)**
**Look for:** `id="project-pl"` inside finance section

Contains:
- Per-project P&L calculation
- Gross Income - Allocated Interest = Net Profit
- Interest allocated proportionally across projects
- ROI % calculation

**Related Functions:**
- `calculateProjectPL(projectId)`
- `renderProjectPL()`
- `allocateInterest()` - Distribute loan interest across projects

#### **8.5 Project Income**
**Look for:** `id="project-income"` inside finance section

Contains:
- House sale income
- Land sale income
- Advances received
- Income tracking

**Related Functions:**
- `renderProjectIncome()`
- `addProjectIncome()`

---

### **9. SETTINGS PAGE**
**Search for:** `<!-- SECTION: SETTINGS PAGE -->`

Contains:
- SCRIPT_URL configuration
- SHEET_ID configuration
- Gemini API Key configuration
- Data export/import
- Clear data button

**Related Functions:**
- `initSettingsPage()`
- `saveSettings()`

---

## JavaScript Functions by Category

### **Navigation**
- `showSc(name)` - Show/hide page screens
- `gotoStep(n)` - Setup wizard step navigation

### **Data Persistence**
- `loadCfg()` - Load config from localStorage
- `saveCfg()` - Save config to localStorage
- `loadData()` - Load app data
- `saveData()` - Save app data

### **Backend Communication**
- `pushToSheet(sheetName, headers, row)` - Send data to Apps Script
- `syncFromSheet()` - Fetch data from Apps Script
- `mergeSheetData(sheetData)` - Merge remote data with local

### **UI Helpers**
- `toast(msg, type)` - Show notification
- `rup(n)` - Format as Indian currency (₹)
- `fmtIN(n)` - Format number in Indian style
- `today()` - Get today's date in DD-MMM-YYYY
- `escH(s)` - Escape HTML entities

### **Finance-Specific**
- `calculateProjectPL(projectId)` - Compute profit/loss
- `allocateInterest()` - Distribute loan interest
- `savePaymentV2(repaymentData)` - Save loan repayment
- `calculateInterestAccrued(loanId, tillDate)` - Proratedinterest

---

## Common Tasks

### **I want to fix a bug in Loan Repayments**
1. Search for: `<!-- SECTION: FINANCE PAGE -->`
2. Find: `id="loan-repayments"`
3. Look for function: `savePaymentV2()`
4. Or search for: `// FUNCTION: savePaymentV2()`

### **I want to add a new field to Projects**
1. Search for: `<!-- SECTION: PROJECTS PAGE -->`
2. Find the form HTML
3. Look for function: `addProject()`
4. Also update: `renderProjects()`

### **I want to modify the bill processing logic**
1. Search for: `<!-- SECTION: DOCUMENTS PAGE -->`
2. Find function: `handlePhoto()` (main entry point)
3. Or: `processBillLineItems()` (Gemini integration)

### **I want to change how Finance P&L is calculated**
1. Search for: `id="project-pl"`
2. Find function: `calculateProjectPL(projectId)`
3. Also check: `allocateInterest()`

---

## File Structure

```
index_ORGANIZED.html (7499 lines)
├── HEAD & SETUP (750 lines)
│   ├── Google Sign-In
│   ├── CSS Variables & Styles
│   └── Setup Wizard
├── HTML PAGES (950 lines)
│   ├── Home
│   ├── AI Chat
│   ├── Projects
│   ├── Expenditure
│   ├── Contacts
│   ├── Documents
│   ├── Finance (Loans, Repayments, P&L, Income)
│   └── Settings
├── STYLES (300 lines)
│   ├── Global utilities
│   ├── Form styles
│   └── Page-specific styles
└── JAVASCRIPT (5200 lines)
    ├── Configuration & State
    ├── Setup & Init
    ├── Data Sync
    ├── Page Logic
    ├── Finance Module
    ├── Documents Processing
    ├── Utility Functions
    └── Apps Script Integration
```

---

## Search Shortcuts

| Want to find... | Search for |
|---|---|
| Home page code | `<!-- SECTION: HOME PAGE -->` |
| Finance module | `<!-- SECTION: FINANCE PAGE -->` |
| Loan repayments | `savePaymentV2` |
| Bill processing | `handlePhoto` |
| Project logic | `addProject` |
| Data sync | `syncFromSheet` |
| Contact management | `upsertContact` |
| Contacts rendering | `renderContacts` |
| Any function start | `function functionName()` |
| Any function comment | `// FUNCTION:` |

---

## Recent Changes (April 2026)

### **Loan Repayment Overhaul**
- **Old schema:** `principalPart`, `interestPart` (calculated)
- **New schema:** `principalAmount`, `interestAmount` (user enters)
- **New function:** `savePaymentV2()` replaces old `saveRepayment()`
- **Location:** Inside `<!-- SECTION: FINANCE PAGE -->`

### **Gemini API Fallback Chain**
- Primary: gemini-2.5-flash
- Fallback 1: gemini-3.0-pro
- Fallback 2: gemini-3.1-pro-preview
- Timeout: 6 minutes
- **Location:** Inside `handlePhoto()` function, search for "503 error"

### **Document Upload Bug Fix**
- Fixed Gemini 2.5 Flash 503 overload errors
- Added model fallback logic
- **Location:** `handlePhoto()` function in documents section

---

## Tips for Adding Features

1. **Find the right section** using the search markers above
2. **Add your new HTML** inside the relevant `<!-- SECTION: ... -->`
3. **Add your new function** after finding the related functions
4. **Follow naming conventions:**
   - HTML IDs: `id="feature-name"`
   - Functions: `featureName()` (camelCase)
   - Comments: `// FUNCTION: featureName()`
5. **Test thoroughly** - changes affect all pages since it's single file

---

## Still Need Help?

When you ask for a fix, tell me:
1. **Which page/feature:** "Finance > Loan Repayments"
2. **What you want:** "Add a 'Notes' field"
3. **What's broken:** "The interest calculation is off by ₹100"

I'll search for the exact section and fix it. ✅

