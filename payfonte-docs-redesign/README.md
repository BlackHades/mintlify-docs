# PayFonte Documentation Redesign

## Overview

This folder contains the redesigned documentation structure for docs.payfonte.com built on Mintlify. The redesign addresses the following objectives:

1. **Clear Product Definitions** - Concisely define Payment Orchestration/IaaS and Payment Aggregation
2. **Enhanced Landing Page** - Highlight unique value propositions with SEO-optimized keywords
3. **Simplified Information Access** - Clear environment URLs, API key locations, and webhook setup
4. **Provider Technical Details** - Decimal places, limits, and specifications (like PawaPay)
5. **API Reference Unchanged** - Existing API reference section preserved

---

## File Structure

```
payfonte-docs-redesign/
├── docs.json                           # Mintlify configuration
├── guides/
│   ├── introductions/
│   │   ├── getting-started.mdx         # NEW - Redesigned landing page
│   │   ├── products.mdx                # NEW - Product definitions
│   │   ├── quick-start.mdx             # NEW - Quick start guide
│   │   ├── faq.mdx                     # NEW - FAQ & Features Explained
│   │   ├── environments.mdx            # UPDATED - Clear environment URLs
│   │   ├── provider-details.mdx        # NEW - Decimal places & specs
│   │   └── [existing files...]
│   ├── collections/
│   │   ├── overview.mdx                # NEW - Collections overview
│   │   └── [existing files...]
│   └── disbursements/
│       ├── overview.mdx                # NEW - Disbursements overview
│       └── [existing files...]
└── README.md                           # This file
```

---

## Key Changes

### 1. Getting Started Page (getting-started.mdx)

**Before:** Generic "What is Payfonte?" text with minimal structure

**After:**
- Clear headline: "Africa's leading payment orchestration platform"
- Stats bar: 14+ Markets | $100M+ TPV | 99.99% Uptime | T+1-3 Settlement
- 6 USPs in accordion format (matching sales deck)
- Two product cards linking to detailed pages
- Quick start steps (Create Account → Get API Keys → Make First Call)
- Security & compliance badges
- SEO keywords embedded naturally

**SEO Keywords Added:**
- Payment orchestration platform Africa
- Mobile money API integration
- African payment gateway
- Payment aggregator Nigeria Kenya Ghana
- White label payment solution Africa

### 2. Products Page (products.mdx)

**NEW PAGE** with detailed definitions:

**Payment Orchestration / Infrastructure-as-a-Service:**
- For fintechs, enterprises, banks
- Use your own MNO contracts
- White-label dashboard and checkout
- Custom routing rules
- Getting started guide for this product

**Payment Aggregation:**
- For merchants, startups, SaaS
- Use Payfonte's aggregated partnerships
- Instant go-live capability
- Simplified pricing
- Getting started guide for this product

**Comparison table** showing feature differences

### 3. Quick Start Page (quick-start.mdx)

**NEW PAGE** with simplified onboarding:

- Step 1: Create Sandbox Account at sandbox-app.payfonte.com
- Step 2: Get API Credentials from Settings → API Keys/Webhooks
- Step 3: Set Up Webhook Callback under API Keys/Webhooks
- Step 4: Make First API Call with code examples
- Step 5: Redirect to Checkout
- Step 6: Handle Webhook

**Code examples in:** cURL, Node.js, Python, PHP

### 4. Environments Page (environments.mdx)

**UPDATED** with clear URL tables:

```
SANDBOX:
  API Base URL:    https://sandbox-api.payfonte.com
  Dashboard:       https://sandbox-app.payfonte.com
  Checkout:        https://sandbox-checkout.payfonte.com
  
PRODUCTION:
  API Base URL:    https://api.payfonte.com
  Dashboard:       https://app.payfonte.com
  Checkout:        https://checkout.payfonte.com
```

**Clear instructions for:**
- Where to find API credentials (Settings → API Keys/Webhooks)
- How to set up webhooks (Settings → API Keys/Webhooks)
- KYB form link (https://form.jotform.com/240038360690048)

### 5. Provider Details Page (provider-details.mdx)

**NEW PAGE** replicating PawaPay structure with:

| Provider | Slug | Currency | Type | Decimals | Min | Max |
|----------|------|----------|------|----------|-----|-----|
| MTN MoMo | mtn-momo-nigeria | NGN | payment | 2 places | - | - |

**Organized by region:**
- West Africa (XOF Zone): Burkina Faso, Benin, Ivory Coast, Senegal, Mali, Togo
- Central Africa (XAF Zone): Cameroon
- West Africa (Non-CFA): Nigeria, Ghana
- East Africa: Kenya, Tanzania
- Other Markets: South Sudan

**Includes:**
- Decimal places explanation (Not supported vs 2 places)
- Amount formatting examples
- API endpoint for dynamic provider configuration

### 6. FAQ & Features Explained (faq.mdx)

**NEW PAGE** covering common questions and mobile money concepts:

**Getting Started:**
- What is Payfonte?
- How do I get started?
- Sandbox vs Production differences
- KYB requirements

**Mobile Money Explained:**
- How mobile money payments work
- STK Push vs USSD authorization
- Customer payment experience
- Pre-authorization flow

**Transaction Processing:**
- Transaction statuses explained
- 100% final state resolution
- Provider timeout windows
- Handling failed transactions

**Webhooks & Callbacks:**
- How to set up webhooks
- Webhook payload format
- Best practices for handling
- Retry logic

**Settlement & disbursements:**
- Settlement times by country
- Disbursement capabilities
- Wallet balance checking

**Technical Integration:**
- Amount formatting (decimal places)
- Phone number formatting
- Direct Charge API
- Hybrid Orchestration (BYOK)
- Failover and intelligent routing

**Security & Compliance:**
- PCI DSS compliance
- Data privacy
- Fraud prevention

**Troubleshooting:**
- Pending transactions
- Authentication errors
- Sandbox test scenarios

### 7. Collection & Disbursement Overviews

**NEW PAGES** providing:
- Integration method comparison
- Payment flow diagrams (Mermaid)
- Code examples
- Best practices
- Links to detailed documentation

---

## Implementation Instructions

### Step 1: Backup Existing Docs

```bash
cd /path/to/mintlify-docs
git checkout -b backup/pre-redesign
git push origin backup/pre-redesign
```

### Step 2: Copy New Files

```bash
# Copy new configuration
cp payfonte-docs-redesign/docs.json ./docs.json

# Copy new/updated pages
cp -r payfonte-docs-redesign/guides/* ./guides/
```

### Step 3: Update Existing Files

The following existing files should be reviewed and may need minor updates:
- `guides/introductions/platform-overview.mdx` - Link to new products page
- `guides/introductions/authorization.mdx` - Reference new environments page
- `guides/introductions/supported-providers.mdx` - Link to new provider-details page

### Step 4: Add Required Images

Create these images in the `/images` folder:
- `hero-light.svg` - Hero illustration (light mode)
- `hero-dark.svg` - Hero illustration (dark mode)
- `api-keys-location.png` - Screenshot of API Keys in dashboard
- `api-keys-dashboard.png` - Dashboard settings screenshot
- `orchestration-diagram.svg` - Architecture diagram

### Step 5: Test Locally

```bash
npm i -g mintlify
mintlify dev
```

Visit `http://localhost:3000` and verify:
- [ ] Landing page displays correctly
- [ ] Products page shows both offerings
- [ ] Quick start guide is easy to follow
- [ ] Environment URLs are correct
- [ ] Provider details tables render properly
- [ ] Navigation works as expected
- [ ] API Reference section is unchanged

### Step 6: Deploy

```bash
git add .
git commit -m "Docs redesign: Products, Quick Start, Provider Details"
git push origin main
```

Mintlify will automatically deploy via GitHub integration.

---

## SEO Improvements

### Target Keywords (Integrated Throughout)

The following SEO keywords have been naturally embedded across all documentation pages:

**Primary Keywords (High Priority):**
1. `payment orchestration platform Africa` - Landing page, Products
2. `Africa payment orchestration` - All pages
3. `payment orchestration API` - Quick Start, Products, FAQ
4. `mobile money API` - All pages
5. `Africa mobile money API` - Getting Started, FAQ, Provider Details

**Provider-Specific Keywords:**
6. `MTN MoMo API integration` - FAQ, Collections, Provider Details
7. `M-Pesa API` - FAQ, Collections, Provider Details
8. `Airtel Money API` - FAQ, Collections, Provider Details
9. `Wave Money integration` - FAQ, Products, Collections
10. `mobile money integration Africa` - All pages

**Alternative Payment Keywords:**
11. `alternative payment methods APM` - Getting Started, Products, FAQ
12. `local payment methods in Africa` - Getting Started, Products, Collections

**Business Model Keywords:**
13. `mobile money payment aggregator` - Getting Started, Products
14. `multi-PSP integration Africa` - Products, FAQ
15. `smart payment routing Africa` - Getting Started, Products, FAQ
16. `enterprise payment orchestration Africa` - Products, FAQ

### Meta Tags (in docs.json)

```json
{
  "metadata": {
    "og:site_name": "Payfonte Documentation - Africa Payment Orchestration Platform",
    "og:description": "Payment orchestration platform Africa. Mobile money API integration for MTN MoMo, M-Pesa, Airtel Money, Wave. Accept local payment methods across 14+ African markets with smart payment routing.",
    "keywords": "payment orchestration platform Africa, Africa payment orchestration, payment orchestration API, mobile money API, Africa mobile money API, MTN MoMo API integration, M-Pesa API, Airtel Money API, Wave Money integration, mobile money integration Africa, alternative payment methods APM, local payment methods Africa, mobile money payment aggregator, multi-PSP integration Africa, smart payment routing Africa, enterprise payment orchestration Africa"
  }
}
```

### Page-Level SEO

Each page includes optimized frontmatter with:
- `title` - Primary keyword
- `description` - Multiple secondary keywords naturally embedded
- `seo.title` - Expanded title with keywords
- `seo.description` - Full keyword-rich description
- `seo.keywords` - Comma-separated keyword list

### Content SEO Strategy

Keywords are integrated naturally in:
- H1 and H2 headings
- Opening paragraphs
- Feature descriptions
- Accordion titles and content
- Card descriptions
- Table headers
- Code example descriptions
- FAQ questions and answers

---

## Maintenance

### Adding New Providers

1. Update `provider-details.mdx` with new provider table row
2. Update `supported-providers.mdx` if new country
3. Update collection/disbursement overviews if new payment method type

### Adding New Countries

1. Add to `provider-details.mdx` under appropriate region
2. Update stats on landing page if needed
3. Add country flag and details to supported providers

---

## Questions?

Contact: support@payfonte.com
