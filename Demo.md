# 🪙 Mudra — Personal Finance Tracker
### *Complete Notion Template — Build Guide & Structure*

> **Mudra** (मुद्रा) — Sanskrit for *currency, seal, and symbol of value*.  
> A complete operating system for your personal finances, built inside Notion.

---

## 📐 Template Architecture Overview

```
MUDRA WORKSPACE
│
├── 🏠 HOME DASHBOARD          ← Master command center (linked views)
│
├── 💸 TRANSACTIONS DB         ← Every income & expense entry
├── 📅 BUDGET PLANNER DB       ← Monthly category budgets
├── 🎯 SAVINGS GOALS DB        ← Goal tracking with progress
├── 📈 INCOME TRACKER DB       ← All income sources
├── 🏦 ASSETS DB               ← What you own
├── 💳 LIABILITIES DB          ← What you owe
└── 📋 MONTHLY REPORTS DB      ← Auto-summary per month
```

---

## DATABASE 1 — 💸 TRANSACTIONS

**Purpose:** The core ledger — every single income or expense entry.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Name | Title | Description of transaction (e.g., "Swiggy order") |
| Date | Date | Transaction date |
| Amount | Number (₹) | Absolute amount (always positive) |
| Type | Select | `Income` / `Expense` |
| Category | Select | See category list below |
| Payment Method | Select | Cash / UPI / Card / Net Banking / EMI |
| Account | Select | SBI / HDFC / Paytm / etc. |
| Month | Formula | `formatDate(prop("Date"), "MMMM YYYY")` |
| Budget Link | Relation | → Budget Planner DB |
| Income Link | Relation | → Income Tracker DB |
| Notes | Text | Optional description |
| Recurring | Checkbox | Is this a recurring expense? |
| Receipt | Files | Upload receipts |

### Expense Categories (Select Options)
```
🛒 Groceries        🏠 Rent/EMI         🚗 Transport
🍔 Food & Dining    💊 Health            🎓 Education
🎬 Entertainment    👗 Shopping          ✈️ Travel
💡 Utilities        📱 Subscriptions     💰 Investment
🏋️ Fitness          🎁 Gifts             📦 Other
```

### Views to Create

| View Name | Type | Filter/Group |
|---|---|---|
| All Transactions | Table | — |
| This Month | Table | Date = this month |
| By Category | Board | Group by: Category |
| Calendar View | Calendar | Date property |
| Expenses Only | Table | Type = Expense |
| Income Only | Table | Type = Income |
| Recurring Bills | Table | Recurring = ✓ |

---

## DATABASE 2 — 📅 BUDGET PLANNER

**Purpose:** Set monthly spending limits per category and track usage.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Category | Title | Category name (must match Transactions) |
| Month | Select | January 2026, February 2026… |
| Budget Amount | Number (₹) | How much you plan to spend |
| Transactions | Relation | → Transactions DB |
| Spent | Rollup | Sum of Amount from related Transactions |
| Remaining | Formula | `prop("Budget Amount") - prop("Spent")` |
| % Used | Formula | `round(prop("Spent") / prop("Budget Amount") * 100)` |
| Status | Formula | See formula below |
| Emoji Status | Formula | See formula below |

### Key Formulas

**Status Formula:**
```
if(prop("% Used") >= 100, "Over Budget",
  if(prop("% Used") >= 80, "Almost Full",
    if(prop("% Used") >= 50, "On Track", "Good")))
```

**Emoji Status Formula:**
```
if(prop("% Used") >= 100, "🔴 Over Budget",
  if(prop("% Used") >= 80, "🟡 Almost Full",
    if(prop("% Used") >= 50, "🟢 On Track", "✅ Good")))
```

### Views to Create

| View Name | Type | Filter |
|---|---|---|
| Current Month Budget | Table | Month = current month |
| Budget Overview | Gallery | Group by Month |
| Over Budget | Table | Status = "Over Budget" |

---

## DATABASE 3 — 🎯 SAVINGS GOALS

**Purpose:** Define financial goals and track deposits toward them.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Goal Name | Title | e.g., "Emergency Fund", "Goa Trip" |
| Target Amount | Number (₹) | Total amount needed |
| Saved So Far | Number (₹) | Current saved amount (manual update) |
| Monthly Target | Number (₹) | How much to save per month |
| Target Date | Date | When you want to achieve this |
| Priority | Select | 🔴 High / 🟡 Medium / 🟢 Low |
| Category | Select | Emergency / Travel / Lifestyle / Investment |
| Progress % | Formula | `round(prop("Saved So Far") / prop("Target Amount") * 100)` |
| Remaining | Formula | `prop("Target Amount") - prop("Saved So Far")` |
| Progress Bar | Formula | See below |
| Months Left | Formula | `dateBetween(prop("Target Date"), now(), "months")` |
| Status | Formula | `if(prop("Progress %") >= 100, "🎉 Achieved!", if(prop("Months Left") < 1, "⚠️ Overdue", "🎯 In Progress"))` |

### Progress Bar Formula
```
slice("██████████", 0, floor(prop("Progress %") / 10)) + 
slice("░░░░░░░░░░", 0, 10 - floor(prop("Progress %") / 10)) + 
" " + format(prop("Progress %")) + "%"
```
*Example output:* `████████░░ 80%`

### Sample Goals to Pre-populate
```
🆘 Emergency Fund      → ₹1,00,000   (6 months expenses)
✈️  Goa Trip 2026       → ₹25,000
📱 New iPhone           → ₹80,000
🏠 House Down Payment   → ₹5,00,000
💊 Health Insurance     → ₹15,000/yr
```

### Views to Create

| View Name | Type | Sort |
|---|---|---|
| All Goals | Gallery | Priority (High first) |
| In Progress | Table | Filter: Status ≠ Achieved |
| Achieved 🎉 | Table | Filter: Progress % ≥ 100 |

---

## DATABASE 4 — 📈 INCOME TRACKER

**Purpose:** Track all income sources across every month.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Source | Title | Salary / Freelance / Dividends / etc. |
| Amount | Number (₹) | Income amount |
| Date | Date | Date received |
| Type | Select | Active / Passive / One-time |
| Tax Deducted | Number (₹) | TDS or tax already cut |
| Net Amount | Formula | `prop("Amount") - prop("Tax Deducted")` |
| Month | Formula | `formatDate(prop("Date"), "MMMM YYYY")` |
| Account | Select | Bank account received into |
| Notes | Text | Invoice number, client name, etc. |
| Transaction Link | Relation | → Transactions DB |

### Income Source Categories
```
💼 Salary (Full-time)    🖥️  Freelance
📊 Investments           🏘️  Rental Income
🎯 Side Business         🎁 Bonus / Incentive
💹 Dividends             💰 Other
```

### Views to Create

| View Name | Type | Group/Filter |
|---|---|---|
| All Income | Table | Sorted by Date desc |
| By Month | Table | Group by Month |
| By Source | Board | Group by Type |
| Monthly Total | Table | Filter by current month |

---

## DATABASE 5 — 🏦 ASSETS

**Purpose:** Everything you own that has monetary value.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Asset Name | Title | e.g., "HDFC Savings", "NIFTY 50 SIP" |
| Category | Select | See categories below |
| Current Value | Number (₹) | Current market/book value |
| Purchase Value | Number (₹) | Original cost |
| Gain/Loss | Formula | `prop("Current Value") - prop("Purchase Value")` |
| Return % | Formula | `round((prop("Gain/Loss") / prop("Purchase Value")) * 100)` |
| Last Updated | Date | When value was last checked |
| Liquid | Checkbox | Can this be converted to cash quickly? |
| Notes | Text | Account number, folio, broker, etc. |

### Asset Categories
```
🏦 Bank Account          📈 Stocks / MF
🏠 Real Estate           🥇 Gold / Jewelry
💰 Fixed Deposits        🪙 Crypto
🚗 Vehicle               📦 Other
```

---

## DATABASE 6 — 💳 LIABILITIES

**Purpose:** Everything you owe — loans, credit cards, EMIs.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Liability Name | Title | e.g., "Home Loan - SBI", "HDFC Credit Card" |
| Category | Select | Home Loan / Car Loan / Personal Loan / Credit Card |
| Total Amount | Number (₹) | Original loan amount |
| Outstanding | Number (₹) | Current remaining balance |
| EMI | Number (₹) | Monthly payment amount |
| Interest Rate | Number (%) | Annual interest rate |
| Due Date | Date | Monthly due date |
| Monthly Interest | Formula | `round(prop("Outstanding") * prop("Interest Rate") / 100 / 12)` |
| Notes | Text | Account/loan number, lender |

---

## DATABASE 7 — 📋 MONTHLY REPORTS

**Purpose:** Auto-summarized snapshot of each month's finances.

### Properties

| Property Name | Type | Purpose |
|---|---|---|
| Month | Title | e.g., "June 2026" |
| Total Income | Rollup | Sum from Income Tracker (filter by month) |
| Total Expenses | Rollup | Sum from Transactions where Type=Expense |
| Net Savings | Formula | `prop("Total Income") - prop("Total Expenses")` |
| Savings Rate % | Formula | `round(prop("Net Savings") / prop("Total Income") * 100)` |
| Biggest Expense | Text | Top spending category (manual) |
| Financial Health | Formula | See below |
| Goal Progress | Text | Notes on savings goals this month |
| Notes | Text | Monthly reflection |

### Financial Health Formula
```
if(prop("Savings Rate %") >= 30, "💚 Excellent",
  if(prop("Savings Rate %") >= 20, "🟡 Good",
    if(prop("Savings Rate %") >= 10, "🟠 Average",
      "🔴 Needs Work")))
```

---

## 🏠 HOME DASHBOARD — Layout Guide

The HOME page is the command center. Use **Notion Linked Views** (not duplicates).

### Section Layout

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🪙 MUDRA — Your Financial Command Center
  Welcome back! [Current Month] [Year]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  [Callout] 💡 Quick Add Transaction → [Link to DB]

  ┌─────────────────────────────────────────────┐
  │  SECTION 1: THIS MONTH SNAPSHOT             │
  │  Linked View: Monthly Reports (Gallery)     │
  │  Filter: Month = current month              │
  └─────────────────────────────────────────────┘

  ┌──────────────────┐  ┌──────────────────────┐
  │ RECENT EXPENSES  │  │  BUDGET STATUS       │
  │ Linked View:     │  │  Linked View:        │
  │ Transactions     │  │  Budget Planner      │
  │ Filter: This     │  │  Filter: This month  │
  │ month, Expense   │  │  Gallery view        │
  └──────────────────┘  └──────────────────────┘

  ┌─────────────────────────────────────────────┐
  │  SAVINGS GOALS                              │
  │  Linked View: Savings Goals (Gallery)       │
  │  Sort: Priority High → Low                  │
  └─────────────────────────────────────────────┘

  ┌──────────────────┐  ┌──────────────────────┐
  │ NET WORTH        │  │  UPCOMING BILLS      │
  │ = Assets -       │  │  Linked View:        │
  │   Liabilities    │  │  Transactions        │
  │ (Manual calc)    │  │  Filter: Recurring=✓ │
  └──────────────────┘  └──────────────────────┘
```

---

## 🧮 KEY FORMULAS REFERENCE

### Net Worth (calculate on Dashboard callout)
```
Total Assets (sum) - Total Liabilities (sum) = Net Worth
```

### Savings Rate
```
(Total Income - Total Expenses) / Total Income × 100
```

### Budget Health Score
```
Number of categories under budget / Total categories × 100
```

### EMI-to-Income Ratio (keep below 40%)
```
Total EMIs / Monthly Income × 100
```

---

## 📋 SAMPLE DATA TO PRE-POPULATE

### Transactions (10 rows)
| Name | Amount | Type | Category | Date |
|---|---|---|---|---|
| Monthly Salary | ₹65,000 | Income | 💼 Salary | June 1 |
| Swiggy Order | ₹450 | Expense | 🍔 Food | June 2 |
| House Rent | ₹15,000 | Expense | 🏠 Rent/EMI | June 5 |
| Netflix | ₹649 | Expense | 📱 Subscriptions | June 6 |
| Grocery - DMart | ₹3,200 | Expense | 🛒 Groceries | June 10 |
| Petrol | ₹1,200 | Expense | 🚗 Transport | June 12 |
| Freelance Project | ₹12,000 | Income | 🖥️ Freelance | June 15 |
| Doctor Visit | ₹800 | Expense | 💊 Health | June 18 |
| SIP - NIFTY 50 | ₹5,000 | Expense | 💰 Investment | June 20 |
| Movie - PVR | ₹600 | Expense | 🎬 Entertainment | June 25 |

### Budget Planner (June)
| Category | Budget |
|---|---|
| 🛒 Groceries | ₹5,000 |
| 🏠 Rent/EMI | ₹15,000 |
| 🍔 Food & Dining | ₹3,000 |
| 🚗 Transport | ₹2,000 |
| 🎬 Entertainment | ₹1,500 |
| 💊 Health | ₹2,000 |
| 📱 Subscriptions | ₹1,000 |
| 💰 Investment | ₹10,000 |

---

## 🛠️ SETUP INSTRUCTIONS (For Quick-Start Guide)

**Step 1 — Duplicate**
> Click the Notion template link → "Duplicate to my Notion"

**Step 2 — Set your currency**
> Open Settings page → Change currency symbol from ₹ to your local one

**Step 3 — Add your budgets**
> Open Budget Planner → Create entries for current month with your limits

**Step 4 — Add your assets & liabilities**
> Open Assets & Liabilities → Add your bank balances, loans, investments

**Step 5 — Start logging**
> Every time you spend or earn → Add to Transactions in under 30 seconds

**Step 6 — Review monthly**
> Every month-end → Open Monthly Reports → Review savings rate → Adjust budgets

---

## 📦 WHAT TO DELIVER TO BUYERS

```
mudra-notion-template/
├── 📄 Notion Template Link  (share link from Notion)
├── 📋 Quick-Start Guide.pdf
├── 🎬 Loom walkthrough video (5 min)  [optional but converts well]
└── 📧 Welcome email sequence
    ├── Email 1: Template link + setup steps
    ├── Email 2 (Day 3): "How to review your first week"
    └── Email 3 (Day 10): "Your first monthly report"
```

---

## 💰 PRICING STRATEGY

| Tier | Price (USD) | Price (INR) | What's Included |
|---|---|---|---|
| Starter | $5 | ₹420 | 4 core DBs (Transactions, Budget, Income, Dashboard) |
| Complete | $9 | ₹750 | All 7 DBs + Reports + Quick-Start Guide |
| Bundle | $15 | ₹1,250 | Complete + Food Blog Finance Tracker combo |

### Where to List
1. **Gumroad** — gumroad.com (lowest friction, 10% fee)
2. **Etsy** — etsy.com/shop/MudraTemplates (high organic traffic)
3. **Notion Template Gallery** — notion.so/templates (free, builds credibility)
4. **Ko-fi** — ko-fi.com (great for Indian creators, Razorpay integration)

---

## 📣 MARKETING COPY SNIPPETS

### Twitter/X Thread Hook
> "I was bleeding money every month without knowing where it went.
> Then I built MUDRA — a Notion template that shows you exactly where every rupee goes.
> Here's what's inside 🧵👇"

### Instagram Caption
> मुद्रा (Mudra) = control over your money 💰
> Stop guessing, start knowing. Track every ₹ with this free-to-duplicate Notion template.
> Link in bio 👆
> #PersonalFinance #Notion #MoneyManagement #FinancialFreedom #IndianFinance

### Reddit Post (r/IndiaInvestments)
> **Title:** I built a free Notion template to track every rupee — here's the exact structure [OC]
> 
> Been using this for 3 months. Went from zero savings to 35% savings rate.
> Sharing the template link in comments. No email needed, just duplicate and use.

---

*Built with ❤️ by [Your Name] | Mudra Template v1.0*
