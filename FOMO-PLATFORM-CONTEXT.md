# FOMO PLATFORM — FULL PROJECT CONTEXT
## Paste this into any AI tool to continue work

---

## WHAT IS THIS PROJECT?

A complete single-file SaaS platform (`index.html`) that sells AI-powered stores to Indian businesses. Built by Prayag Kumawat. Live at: **fomo-platform.vercel.app** | GitHub: **github.com/kumawatprayag27-spec/fomo-platform**

The ENTIRE platform is one HTML file — all CSS, JS, and HTML together. No frameworks, no backend, no database. Everything runs in the browser using localStorage.

---

## TECH STACK

- Pure HTML + CSS + JavaScript (single file)
- Anthropic Claude API (`claude-sonnet-4-20250514`) for AI features
- localStorage for all data storage
- Deployed on Vercel via GitHub (auto-deploys on push)
- No npm, no React, no backend

---

## PLATFORM STRUCTURE — 9 PAGES (all in one file)

```
1. loginPage      — Login screen (prayag/1234 = admin, client auto-credentials)
2. landingPage    — Public marketing page with pricing
3. paymentPage    — UPI payment + transaction ID entry
4. setupPage      — Business info form (name, type, state, city, phone, API key)
5. dashPage       — Owner dashboard with sidebar navigation
6. storePage      — Customer-facing product store
7. productPage    — Individual product detail page
8. cartPage       — Shopping cart + checkout
9. chatOnlyPage   — AI chat only (for ₹1,999 plan)
```

Pages switch via `showPage('pageName')` function.

---

## 4 SUBSCRIPTION PLANS

| Plan | Monthly Price | Annual Price | Key Feature |
|------|-------------|--------------|-------------|
| AI Receptionist | ₹1,999/mo | ₹14,999/yr | Basic AI chat only |
| AI Sales Employee | ₹13,999/mo | ₹1,19,999/yr | Full dashboard + store |
| AI Smart Store | ₹24,999/mo | ₹1,99,999/yr | All 60+ features + premium UI |
| Enterprise | Custom | Custom | Everything |

**Annual logic:** When customer toggles Annual ON on the pricing page:
- Plan 1: Shows ₹1,499/month → payment = ₹14,999
- Plan 2: Shows ₹11,999/month → payment = ₹1,19,999
- Plan 3: Shows ₹24,999/month → payment = ₹1,99,999

---

## KEY JAVASCRIPT VARIABLES (Global State)

```javascript
let bizInfo = {};          // Business details {name, type, owner, phone, state, city, plan, lang}
let selectedPlan = {};     // {name, amount}
let API_KEY = '';          // Anthropic API key (hidden from clients)
let products = [];         // Array of product objects
let orders = [];           // Array of order objects
let cart = [];             // Current cart items
let customerMode = false;  // true = customer opened store link, false = owner
```

---

## KEY FUNCTIONS

```javascript
showPage(id)           // Switch between pages
switchTab(tab)         // Switch dashboard sidebar tabs
planHas(feature)       // Check if current plan has a feature
buildSidebar()         // Build dynamic sidebar based on plan
initDash()             // Initialize dashboard
renderStore()          // Render customer store
openProduct(id)        // Open product detail
addToCart(id)          // Add to cart
launchBusiness()       // Complete setup and go to dashboard
saveAccount()          // Save to localStorage
generateCustomerLink() // Generate ?c= encoded link for customers
checkCustomerMode()    // Check if opened as customer via ?c= link
togglePlan(n)          // Toggle monthly/annual on pricing page
injectPremiumDesign()  // Add motion design (AI Smart Store plan only)
```

---

## PLAN FEATURES SYSTEM

```javascript
const PLAN_FEATURES = {
  'AI Receptionist': { store: true, ai_basic: true, ... },
  'AI Sales Employee': { store: true, analytics: true, followup: true, ... },
  'AI Smart Store': { store: true, ai_chatbot: true, flash_sale: true, revenue_graph: true, ... ALL 60+ },
  'AI Business OS': { ... everything: true }
};

function planHas(key) {
  return PLAN_FEATURES[bizInfo.plan]?.[key] === true;
}
```

---

## CUSTOMER LINK SYSTEM (Option B — Separate Owner/Customer)

- Owner clicks "Get Customer Link" → generates `?c=base64encodedData` URL
- Encoded data includes: bizInfo + API_KEY + plan + products
- Customer opens link → goes directly to store (no dashboard)
- If owner opens their own customer link → yellow "Owner Preview Mode" banner appears
- Real customers see ZERO dashboard features

```javascript
function generateCustomerLink() {
  var data = { bizInfo, key: API_KEY, plan: bizInfo.plan, products };
  var encoded = btoa(encodeURIComponent(JSON.stringify(data))...);
  return baseURL + '?c=' + encoded;
}
```

---

## DASHBOARD SIDEBAR SECTIONS

```
MENU: Overview, Products, Add Product, Orders, Analytics, Follow-Up
⚡ SMART STORE: Flash Sale, Revenue Graph, AI Sales Bot, Smart Bundles,
                Reorder AI, WA Orders, Live Chat, Inventory, Coupons,
                Banners, Customers, Payments, Notifications
🚀 POWER TOOLS: Invoice PDF, Multi-Language, Product QR, Abandoned Cart,
                Loyalty Points, AI Content, Smart Pricing, Sales Target,
                Booking, Pre-Order
🌟 ULTRA FEATURES: Voice AI, Smart Reco, Review Writer, WA Catalog,
                   Complaint AI, Daily Report, Birthday Tracker, Referral,
                   Staff Monitor, Refund Manager, Digital Menu, Table Order,
                   Delivery Zones, Bulk Import, Product Compare
```

Items not in the user's plan are completely hidden (not shown at all).

---

## PAYMENT SYSTEM

1. Customer picks plan → clicks button → goes to paymentPage
2. Pays via UPI to Prayag's QR (UPI: 9998090047@ybl)
3. Enters Transaction ID
4. Enters Activation Code (currently: `1234`)
5. Goes to setupPage → fills business info → clicks "Launch My Business AI"
6. Gets auto-generated credentials: `bizname@fomo.ai` / `BizName@123`
7. Plan gets locked in localStorage — cannot be changed after activation

---

## PRICING PAGE DESIGN (Webflow-inspired)

3 dark cards side-by-side + enterprise row below:
- Card 1 (AI Receptionist): Dark gray `#141519`, left-rounded corners
- Card 2 (AI Sales Employee): Blue `#0d1f3c` with blue border, highlighted
- Card 3 (AI Smart Store): Dark gray `#141519`, right-rounded corners
- Enterprise: Full-width dark row with "Request Setup →" button

Each card has its own ANNUAL toggle (top-right of card).

---

## PREMIUM DESIGN (AI Smart Store only)

When `bizInfo.plan === 'AI Smart Store'`, `injectPremiumDesign()` runs:
- Floating purple particles (18 particles rising)
- 2 moving glowing purple orbs
- Animated CSS grid background
- Neon glow on dashboard logo
- Glowing sidebar items
- Stat cards with purple glow + hover lift
- Shimmer effect on buttons

---

## SETUP FORM

- Business Name, Business Type (dropdown)
- State (all 34 Indian states/UTs dropdown)
- City (auto-loads based on state, 15-20 cities per state)
- Phone validation: digits only, 10 digits, must start with 6-9
- Business Description, Tagline, Owner Name
- API Key field (admin only — hidden from clients in production)

---

## CREDENTIALS (CHANGE BEFORE REAL LAUNCH)

```
Admin login:    prayag / 1234
Activation code: 1234
Owner's UPI:    9998090047@ybl
Owner's WA:     +919998090047
```

---

## CSS VARIABLES

```css
--bg: #06030f       /* main background */
--s1: #0f0920       /* surface 1 */
--s2: #160b2a       /* surface 2 */
--p:  #9b30ff       /* primary purple */
--p2: #b44fff       /* primary purple light */
--text: #f0eaff     /* text color */
--muted: rgba(240,234,255,0.35)
--border: rgba(155,48,255,0.15)
--green: #22c55e
--gold: #f59e0b
```

---

## WHAT'S WORKING ✅

- All 9 pages
- All 4 subscription plans with feature locking
- 60+ dashboard features (render functions for all)
- Customer link system (owner vs customer separation)
- Annual/monthly toggle per plan card
- Premium motion design for AI Smart Store plan
- State + city dropdown + phone validation in setup
- Plan switcher removed from client dashboard (only badge shows)
- Payment confirmation system with WhatsApp auto-notify
- Subscription expiry + renewal popup system
- Deployed on Vercel

---

## CURRENT FILE INFO

- File name: `fomo-platform.html` (on Vercel it's `index.html`)
- File size: ~270KB, ~3931 lines
- Single HTML file — all CSS in `<style>`, all JS in `<script>`

---

## HOW TO CONTINUE

1. Download the latest `fomo-platform.html`
2. Open it in VS Code or any editor
3. Make your changes
4. Go to github.com/kumawatprayag27-spec/fomo-platform
5. Click index.html → pencil icon → replace all code → Commit
6. Vercel auto-deploys in 30 seconds
7. Test at fomo-platform.vercel.app

---

## EXAMPLE TASKS YOU CAN ASK

- "Add a new feature X to the dashboard sidebar"
- "Fix the bug where Y is not working"
- "Redesign the Z section to look like [image]"
- "Add a new section to the landing page"
- "Change the color scheme"
- "Add WhatsApp integration to X feature"

---

*Context created for FOMO Platform — AI Business SaaS by Prayag Kumawat*
