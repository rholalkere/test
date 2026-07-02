# PearlEgg Fresh: Complete Operational Reference Manual
*A Unified Guide to Managing D2C Retail & B2B Wholesale Channels Under a Single Commercial Ecosystem*

---

## 1. Executive Summary & Core USP

### 🥚 The Dual-Model Advantage
For a growing agricultural supply enterprise in Karnataka, managing two distinct business lines usually requires twice the effort, twice the software, and twice the headcount. PearlEgg Fresh changes this paradigm by running a **Direct-to-Consumer (D2C) Retail Storefront** and a **Business-to-Business (B2B) Wholesale Portal** side-by-side on a single website. 

By unifying these operations, the business can directly connect poultry farms in rural Karnataka to two highly valuable customer segments:
1.  **Everyday Retail Consumers:** Buying organic brown eggs, omega-3 packs, or fresh dairy addons for their homes.
2.  **Corporate HoReCa Partners:** Hotels, restaurants, and cafes purchasing bulk crates on credit terms to fuel their professional kitchens.

This dual-model setup gives the business a massive operational edge. Instead of maintaining separate websites, separate warehouses, and separate staff, everything flows through a single system. The public-facing site operates as a premium consumer store, while a secure login unlocks a private wholesale portal for registered corporate partners.

### 🖼️ Unified Logistics & Supply Chain Infographic
This diagram illustrates the farm-to-table logistics workflow implemented for PearlEgg Fresh, separating the D2C Home Delivery and the B2B Wholesale Kitchen channels:

![PearlEgg Unified Logistics & Supply Chain Infographic](file:///home/rahulholalkere/.gemini/antigravity-cli/brain/6989ac84-132d-43ab-ae6f-34894a4b1a1f/pearlegg_workflow_1782968577747.jpg)

### 📈 Business Efficiency
Integrating these two models into a single platform maximizes operational efficiency:
*   **Preventing Accidental Over-selling:** Because all sales deduct from a single inventory list, there is no risk of a wholesale order for 5,000 eggs selling stock that was already promised to retail home buyers.
*   **Shared Operating Overhead:** The business owner manages the entire enterprise through one back-office dashboard. Product catalogs are updated once, delivery drivers follow unified geofenced routes, and sales data is consolidated into a single report.
*   **Consolidated Reporting:** Monthly revenues, tax liabilities, and order volumes are collected in one place, allowing the business owner to monitor retail and wholesale performance at a glance.

### 🖼️ Platform Architecture Diagram

```
+-----------------------------------------------------------------------+
|                       CENTRAL DATABASE REGISTER                       |
|          (Single Unified Product Catalog & Warehouse Inventory)        |
+------------------------------------+----------------------------------+
                                     |
                  +------------------+------------------+
                  |                                     |
                  v                                     v
     +--------------------------+          +--------------------------+
     |   PUBLIC RETAIL SHOP     |          |  SECURE WHOLESALE PORTAL |
     |      (D2C Segment)       |          |      (B2B Segment)       |
     +--------------------------+          +--------------------------+
     | * Visible to all guests  |          | * Invisible to guests    |
     | * Standard retail MRP    |          | * Business login required|
     | * Individual egg packs   |          | * Bulk case/crate packs  |
     | * Instant UPI/Card pay   |          | * Custom negotiated price|
     | * Home delivery options  |          | * Monthly credit terms   |
     +--------------------------+          +--------------------------+
```

---

## 2. Core Features: Retail (D2C) vs. Wholesale (B2B)

The table below outlines how the system branches out the purchasing experience depending on whether a retail customer or a corporate client is using the site:

| Business Element | Retail (D2C) Implementation | Wholesale (B2B) Implementation |
| :--- | :--- | :--- |
| **Target Audience** | Individual households, retail shoppers, and family consumers. | Commercial food services: Hotels, Restaurants, Cafes, and Caterers (HoReCa). |
| **Access Level** | Open to the general public. No registration is required to browse products. | Protected portal. Features are hidden until a verified business account logs in. |
| **Pricing Mechanics** | Standard retail pricing per pack (MRP) with occasional automated discount codes. | Negotiated pricing based on quantities, matching custom price ranges (e.g. rate per egg). |
| **Minimum Order Limits** | No minimum purchase limits. Customers can buy a single pack of 6 eggs. | Minimum order quantity is 500 eggs (packed in large commercial crates/cases). |
| **Payment Rules** | Paid immediately during checkout using Razorpay (UPI, Credit/Debit Cards, Netbanking). | Charged to a pre-allocated monthly corporate credit line, settled later via bank transfer. |
| **Taxation Handling** | Prices displayed on the storefront are inclusive of standard retail GST (consumer-friendly). | GST is explicitly extracted and listed on invoices for corporate input tax credit verification. |

> [!IMPORTANT]
> **Core Operational Rule:** Guests who visit the site without logging in will only see D2C retail options. The wholesale portal remains completely invisible. Business buyers must log in to unlock bulk pricing and credit payment options.

---

## 3. User Profiling & Authentication

### 👥 Active System Personas & Roles
The platform separates system permissions to ensure that staff and customers only access the features relevant to their roles:

1.  **Retail Guest / Registered Consumer:**
    *   *Capabilities:* Can browse the public catalog, add eggs/groceries to their cart, select home delivery addresses, checkout, and manage monthly skip calendars.
    *   *Limits:* Cannot access wholesale pricing, submit RFQs, or view corporate credit ledgers.
2.  **Wholesale Purchaser:**
    *   *Capabilities:* An employee of a verified business client. Can log into the business portal, browse bulk quantities, submit RFQ price bids, and place orders using the company's credit limit.
    *   *Limits:* Cannot edit company profile details or adjust overall credit boundaries.
3.  **Wholesale Admin:**
    *   *Capabilities:* The owner or primary manager of a client business. Can add or remove corporate purchasers, track outstanding balances, view monthly SLA invoices, and check remaining credit lines.
    *   *Limits:* Cannot adjust their own credit limit (which must be updated by the store owner).
4.  **Store Operations Manager:**
    *   *Capabilities:* Internal warehouse staff. Can update stock counts, edit product pricing, toggle catalog availability, review and approve/reject wholesale RFQs, and mark orders as dispatched.
    *   *Limits:* Cannot view system developer settings or modify critical database configurations.

### 📂 Information Captured & Isolation Rules
To protect business security and user privacy, the platform collects different details based on account type and stores them separately:

*   **Retail Accounts:** The system collects the customer's name, email, mobile phone number (verified via simulated OTP), and home delivery addresses (linked to map coordinates).
*   **Wholesale Accounts:** The platform requires verified business registration details, including the business name, GSTIN (GST Identification Number) for tax compliance, corporate PAN, delivery details for commercial trucks, and an assigned credit limit.
*   **Data Isolation:** The database keeps retail profiles and corporate registrations in separate data structures. If a wholesale customer places a bulk order, their personal contact details and company credit registers are stored separately, preventing cross-access.

---

## 4. Catalog & Inventory Management

### 📦 One Catalog, Two Audiences
The platform maintains a single master catalog of products, eliminating the need to update separate retail and wholesale databases. Product listings (such as *Organic Cage-Free Brown Eggs*) include fields for both retail prices and wholesale price targets. The system filters this data dynamically depending on whether a customer is logged in as a retail consumer or a wholesale buyer.

### 🗺️ Catalog Filtering Flowchart

```
           +---------------------------------------+
           |       User visits the storefront       |
           +-------------------+-------------------+
                               |
                               v
            /-------------------------------------\
           <  Is the user logged in to a verified  >
           <           wholesale account?          >
            \-------------------------------------/
                               |
                      +--------+--------+
                   NO |                 | YES
                      v                 v
        +---------------------------+   +---------------------------+
        |   SHOW RETAIL SHOP VIEW   |   | SHOW WHOLESALE PORTAL VIEW|
        | * Retail prices (MRP)     |   | * Bulk cases/crates       |
        | * Individual egg packs    |   | * RFQ bulk quote builder  |
        | * Public dairy addons     |   | * Credit payment terms    |
        +---------------------------+   +---------------------------+
```

### 🔄 Inventory Allocation & Low-Stock Alerts
To keep operations running smoothly, the catalog uses automated inventory rules:
*   **Real-Time Stock Deduction:** Whether a sale is a 30-pack of eggs bought by a retail shopper or a 1,500-egg shipment ordered by a hotel, the stock is deducted from the same inventory count immediately.
*   **Buffer Stock Management:** The system reserves a designated buffer stock (e.g., 200 eggs) exclusively for the retail storefront. This ensures that home customers can always purchase eggs, even if a wholesale buyer places a large bulk order that exhausts main warehouse supplies.
*   **Back-Office Stock Alerts:** When warehouse inventory drops below a designated threshold (e.g., 500 eggs), the system triggers an alert on the admin dashboard, prompting the manager to restock.

---

## 5. Functional Capabilities & Core Modules

### 🛒 Cart & Checkout Engine
The platform uses two distinct checkout paths to process orders based on customer type:

1.  **D2C Checkout Path:**
    *   *Step 1:* Customer adds eggs and dairy addons to their cart.
    *   *Step 2:* Customer selects their address on the map, which checks serviceability boundaries.
    *   *Step 3:* Standard retail GST (inclusive) and delivery fees (free for orders over ₹499) are applied.
    *   *Step 4:* Customer pays instantly via UPI, Card, or Netbanking using the Razorpay gateway.
2.  **B2B RFQ Checkout Path:**
    *   *Step 1:* Business buyer builds their bulk request.
    *   *Step 2:* The system verifies the order meets the **minimum bulk threshold of 500 eggs**.
    *   *Step 3:* The system checks the order value against the client's **remaining credit balance**. If the order exceeds the limit, checkout is blocked until the balance is settled.
    *   *Step 4:* The RFQ is submitted to the approvals queue. Upon approval, stock is reserved, and the corporate credit balance is updated.

### 🚚 Order Fulfillment Journey
The table below shows what each order status means for warehouse staff processing retail and wholesale orders:

| Order Status | Meaning for Retail (D2C) Orders | Meaning for Wholesale (B2B) Orders |
| :--- | :--- | :--- |
| **Pending** | Order is placed and paid; awaiting staff packing. | RFQ is submitted; awaiting margin and credit checks. |
| **Processing** | Staff pick individual egg packs and dairy addons, packing them in protective boxes. | Staff compile bulk egg crates onto transport pallets and generate a commercial E-Way bill. |
| **Dispatched** | Order is loaded onto a local delivery bike for home delivery. | Pallets are loaded onto temperature-controlled trucks for bulk delivery. |

### 🔌 Connected Business Tools
Our platform connects several systems to keep daily business operations running smoothly:
*   **Payment Infrastructure (Razorpay):** Handles instant retail checkout payments, subscription autopay mandates, and card/UPI validation.
*   **Logistics Modules:** Calculates delivery routing, checks serviceability regions on the map, and triggers automated dispatch notifications.
*   **Accounting Export Systems:** Consolidates all retail receipts and wholesale invoices into a single format, making tax filing and business reporting straightforward.
