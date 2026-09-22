# 18 Test Plan & Quality Assurance (QA) Checklist

## 1. Executive QA Strategy & Governance

This Quality Assurance (QA) Master Plan establishes the end-to-end verification framework for the **Vraj Vithalani Enterprise Digital Platform**. Designed for a single-practitioner brand architecture built on **Next.js 15 (App Router)**, **TypeScript**, **MongoDB Atlas**, and **Cloudinary CDN**, this test suite ensures **100% bug-free deployments**, zero security vulnerabilities, **100/100 Core Web Vitals performance**, and strict adherence to **Critical Rule 9 (PageRank Link Scarcity)** and **Generative Engine Optimization (GEO)** standards.

### 1.1 Testing Methodology & Environments
Testing occurs across three controlled environments following our **3-Branch CI/CD Promotion Model**:

| Environment | Host Infrastructure | Trigger Branch | Primary Test Objective |
| :--- | :--- | :--- | :--- |
| **Local Dev** | WSL2 / Local Node.js 22 | `main` | Unit testing, TypeScript compilation (`tsc --noEmit`), component renders, and developer regression checks. |
| **Staging** | Vercel Serverless | `staging` | End-to-End (E2E) automation, cross-browser/device testing, GTM/GA4 preview debugging, and client verification. |
| **Production** | Hostinger Cloud Startup (Node.js) | `production` | Post-deployment smoke tests, SSL/Cloudflare edge validation, live API health monitoring, and analytics sanity. |

---

### 1.2 Bug Severity Defect Matrix

| Severity Level | Definition | Response SLA | Target Resolution Gate |
| :--- | :--- | :--- | :--- |
| **P0 - Blocker** | Core functionality down (e.g., lead form failing, database connection error, security vulnerability, admin lockout, site crashes). | Immediate (< 2 Hours) | **Must be 0** prior to any merge into `production`. |
| **P1 - Critical** | Major user impact or performance regression (e.g., Core Web Vitals LCP > 2.5s, broken ROI calculator math, broken Schema `@graph`, analytics untracked). | < 12 Hours | **Must be 0** prior to staging sign-off. |
| **P2 - Major** | Non-critical functional or layout flaw (e.g., minor UI misalignment on mobile, missing alt text on non-critical asset, slow Cloudinary load). | < 24 Hours | Max 2 allowed for staging; 0 for production. |
| **P3 - Minor** | Cosmetic defect or minor typo in non-conversion copy (e.g., slight spacing variance, minor copy formatting issue). | Next Sprint | Documented and queued for post-launch maintenance. |

---

## 2. Comprehensive Test Suites & Verification Checklists

---

### TS-01: Functional & Lead Capture Verification Suite

**Objective**: Verify that all public lead entry points validate inputs correctly, trigger anti-spam honeypot guards, persist valid leads to MongoDB, and provide proper UI feedback.

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-FN-01** | **Valid Inbound Lead Submission** | 1. Navigate to `/contact/` or any inline lead section.<br>2. Fill `fullName`, `email`, `phone`, select `service` and `monthlyBudget`.<br>3. Submit form. | Form submits successfully. UI displays green confirmation message. Lead record created in MongoDB with status `new`. | [ ] Pass<br>[ ] Fail |
| **TC-FN-02** | **Form Input Validation Rules** | 1. Attempt submission with empty required fields.<br>2. Input invalid email format (`vraj@`).<br>3. Input short phone number (`123`). | Client-side validation triggers inline error labels. Submit button remains disabled or handles error state without page refresh. | [ ] Pass<br>[ ] Fail |
| **TC-FN-03** | **Spambot Honeypot Trap Enforcement** | 1. Fill visible contact fields.<br>2. Inject value into hidden `website_url` input field.<br>3. Click Submit. | Server returns fake `200 OK` response. **No lead is saved to MongoDB**. Spambot is tricked without leaking pipeline state. | [ ] Pass<br>[ ] Fail |
| **TC-FN-04** | **Lead Status Pipeline State Transitions** | 1. Log into `/iamadmin/leads/`.<br>2. Select a `new` lead record.<br>3. Change status to `contacted`, then `in_pipeline`, then `closed`. | Status updates immediately via PATCH API. Table tag reflects new color status. Updated timestamp saved in database. | [ ] Pass<br>[ ] Fail |
| **TC-FN-05** | **Lead Internal Notes Logging** | 1. Open lead detail panel in `/iamadmin/leads/`.<br>2. Append note: "Follow-up call scheduled for Friday."<br>3. Save and refresh page. | Note persists with author timestamp. Previous notes remain intact. | [ ] Pass<br>[ ] Fail |
| **TC-FN-06** | **Permanent Lead Record Erasure (DPDP Act)** | 1. Click "Delete" on lead record in `/iamadmin/leads/`.<br>2. Confirm modal prompt. | Record is permanently purged from MongoDB collection. API returns `200 OK`. | [ ] Pass<br>[ ] Fail |

---

### TS-02: Interactive Google Ads ROI Simulator Suite

**Objective**: Validate mathematical accuracy, real-time reactive UI slider updates, and WhatsApp deep-link generation.

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-ROI-01** | **Default Math Calculation Verification** | 1. Navigate to `/services/google-ads/`.<br>2. Observe default monthly budget slider (₹50,000). | Formula outputs verified:<br>• Estimated Clicks: `1,775`<br>• Leads (5% CR): `89`<br>• Sales (20% SCR): `18` | [ ] Pass<br>[ ] Fail |
| **TC-ROI-02** | **Interactive Slider Re-calculation** | 1. Drag budget slider from ₹50,000 to ₹1,000,000. | All output numbers update in real-time (< 16ms frame rendering) without page lag. | [ ] Pass<br>[ ] Fail |
| **TC-ROI-03** | **Dynamic WhatsApp Deep-Link Context** | 1. Adjust budget to ₹1,000,000.<br>2. Click "Lock in This Strategy on WhatsApp". | Opens `https://wa.me/919974558231` with URL-encoded text containing current budget and calculated lead expectations. | [ ] Pass<br>[ ] Fail |

---

### TS-03: Security & Admin Authentication Verification Suite

**Objective**: Guarantee administrative isolation, JWT token cookie protection, and injection prevention.

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-SEC-01** | **Bcrypt Password Hash Verification** | 1. Submit valid credentials on `/iamadmin/login/`. | Password verified against stored Bcrypt hash (`$2b$12$...`). HTTP-Only `token` cookie set. Redirected to `/iamadmin/leads/`. | [ ] Pass<br>[ ] Fail |
| **TC-SEC-02** | **Unauthenticated Route Middleware Guard** | 1. Clear browser cookies.<br>2. Attempt direct URL access to `/iamadmin/leads/` or `/iamadmin/settings/`. | Next.js middleware intercepts request and redirects instantly to `/iamadmin/login/` with `401 Unauthorized` state. | [ ] Pass<br>[ ] Fail |
| **TC-SEC-03** | **HTTP-Only Cookie Security Attributes** | 1. Inspect set cookie header for `token`. | Attributes verified: `HttpOnly = true`, `Secure = true`, `SameSite = Strict`, `Path = /`. `document.cookie` in JS console returns empty. | [ ] Pass<br>[ ] Fail |
| **TC-SEC-04** | **Instant Session Revocation (`tokenVersion`)** | 1. Increment `tokenVersion` in database for admin user.<br>2. Refresh `/iamadmin/leads/` in authenticated browser. | Existing JWT is rendered invalid. User is immediately logged out and redirected to login page. | [ ] Pass<br>[ ] Fail |
| **TC-SEC-05** | **NoSQL Injection & XSS Prevention** | 1. Submit `{"$gt": ""}` into login email field.<br>2. Submit `<script>alert(1)</script>` into contact message field. | Mongoose strict typing rejects NoSQL operator. React auto-escapes string content in UI. No code executes. | [ ] Pass<br>[ ] Fail |

---

### TS-04: SEO, Schema & GEO AI Search Verification Suite

**Objective**: Verify search engine indexability, Schema.org `@graph` syntax, canonical hygiene, and GEO direct answer extraction.

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-SEO-01** | **Canonical URL Trailing Slash Enforcement** | 1. Access `https://vrajvithalani.com/about` (without trailing slash). | 301 Redirects to `https://vrajvithalani.com/about/`. `<link rel="canonical">` points self-referentially with trailing slash. | [ ] Pass<br>[ ] Fail |
| **TC-SEO-02** | **Schema.org `@graph` JSON-LD Validation** | 1. Test Homepage and Service URLs in Google Rich Results Test & Schema Validator. | Single unified `@graph` block detected containing `WebSite`, `Person` (`#person`), `Organization`, and `Service` nodes with zero errors or warnings. | [ ] Pass<br>[ ] Fail |
| **TC-SEO-03** | **GEO 60–80 Word Callout Block Inspection** | 1. Inspect DOM on core pages for `.geo-callout` component. | Paragraph contains concise 60–80 word direct answer summary with high entity density suitable for LLM RAG extraction. | [ ] Pass<br>[ ] Fail |
| **TC-SEO-04** | **Critical Rule 9 Link Equity Audit** | 1. Scan internal links on case studies (`/case-studies/dr-vishva/`) and learn guides (`/learn/core-web-vitals-guide/`). | Direct body links point upward exclusively to commercial Service Pillars. **Zero direct `/contact/` links present in deep article copy**. | [ ] Pass<br>[ ] Fail |
| **TC-SEO-05** | **Dynamic Sitemap & Robots.txt Verification** | 1. Fetch `/sitemap.xml` and `/robots.txt`. | Sitemap lists all 50 valid routes with trailing slashes. `robots.txt` explicitly allows `GPTBot`, `PerplexityBot`, and `ClaudeBot`. | [ ] Pass<br>[ ] Fail |

---

### TS-05: Core Web Vitals & Performance Verification Suite

**Objective**: Enforce 100/100 Lighthouse performance and zero layout shifts.

| Metric / Test | Target Benchmark | Verification Tool | Pass/Fail Gate |
| :--- | :--- | :--- | :--- |
| **Lighthouse Performance** | **100 / 100** (Desktop & Mobile 4G) | Chrome Lighthouse Audit | [ ] Pass  [ ] Fail |
| **Largest Contentful Paint (LCP)** | **< 1.2 Seconds** | PageSpeed Insights / WebVitals.js | [ ] Pass  [ ] Fail |
| **Cumulative Layout Shift (CLS)** | **0.00** | PageSpeed Insights | [ ] Pass  [ ] Fail |
| **Interaction to Next Paint (INP)** | **< 50 milliseconds** | Chrome User Experience Report (CrUX) | [ ] Pass  [ ] Fail |
| **Initial JS Bundle Budget** | **< 100 KB** (Gzipped) | Next.js Build Output Analyzer (`npm run build`) | [ ] Pass  [ ] Fail |
| **Total Page Weight** | **< 1.5 MB** Across All Assets | Chrome Network Tab | [ ] Pass  [ ] Fail |

---

### TS-06: Analytics & Tracking Verification Suite

**Objective**: Verify non-blocking script loading and custom event firing across GTM, GA4, Meta Pixel, and Microsoft Clarity.

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-TRK-01** | **Dynamic Script Loading via `/iamadmin/settings/`** | 1. Update GTM Container ID in admin panel.<br>2. Save and inspect public page DOM. | GTM script loads dynamically using `next/script` with `strategy="afterInteractive"`. Core Web Vitals unimpacted. | [ ] Pass<br>[ ] Fail |
| **TC-TRK-02** | **`lead_form_submit` Event Verification** | 1. Open GTM Tag Assistant / GA4 DebugView.<br>2. Complete lead submission. | `lead_form_submit` event fires with `service`, `monthlyBudget`, and hashed user data (`sha256_email`). | [ ] Pass<br>[ ] Fail |
| **TC-TRK-03** | **WhatsApp Click Tracking Event** | 1. Click floating WhatsApp widget or ROI CTA. | `whatsapp_click` event fires in dataLayer with target page URL context. | [ ] Pass<br>[ ] Fail |
| **TC-TRK-04** | **Meta Pixel & CAPI Deduplication** | 1. Trigger lead event in browser while monitoring Meta Event Manager. | Single `Lead` event registered with matching `event_id` between Browser and Server CAPI triggers. | [ ] Pass<br>[ ] Fail |

---

### TS-07: Cross-Browser & Multi-Device Responsive Suite

**Objective**: Guarantee visual perfection from 320px screens to 4K displays.

| Device Category | Target Viewport | Screen / Browser Target | Specific Verification Items | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **Mobile Small** | `320px - 375px` | iPhone SE, Galaxy S20 (Safari / Chrome) | Mobile drawer menu rendering, 44x44px touch targets, zero horizontal scrolling. | [ ] Pass<br>[ ] Fail |
| **Mobile Standard** | `390px - 428px` | iPhone 14/15 Pro Max, Pixel 8 | Floating WhatsApp widget clears bottom safe areas (`env(safe-area-inset-bottom)`). | [ ] Pass<br>[ ] Fail |
| **Tablet** | `768px - 1024px` | iPad Air / Pro (Portrait & Landscape) | Responsive grid collapses cleanly from 3-column to 2-column or 1-column layouts. | [ ] Pass<br>[ ] Fail |
| **Desktop / 4K** | `1440px - 3840px` | Chrome, Firefox, Safari, Edge | Max content container constraints (`max-w-7xl`) prevent line length blowout. | [ ] Pass<br>[ ] Fail |

---

### TS-08: Deployment & CI/CD Pipeline Verification Suite

**Objective**: Ensure smooth code promotion across `main`, `staging` (Vercel), and `production` (Hostinger).

| Test Case ID | Test Item / Scenario | Execution Steps | Expected Outcome | Pass/Fail Gate |
| :--- | :--- | :--- | :--- | :--- |
| **TC-DEP-01** | **`main` Branch Local Compilation** | 1. Run `npm run build` locally. | TypeScript compilation succeeds with 0 errors. All 50 static pages generated cleanly. | [ ] Pass<br>[ ] Fail |
| **TC-DEP-02** | **`staging` Branch Automated Build** | 1. Push commit to `staging` branch. | Vercel deployment completes. Preview URL generated. Staging environment variable values active. | [ ] Pass<br>[ ] Fail |
| **TC-DEP-03** | **`production` Branch Hostinger Deployment** | 1. Merge `staging` into `production` branch. | Hostinger GitHub action builds container, restarts Node.js process via PM2, and serves live site on `vrajvithalani.com`. | [ ] Pass<br>[ ] Fail |
| **TC-DEP-04** | **Post-Deployment Smoke Test** | 1. Ping `/api/health` or public routes on production host. | Returns `200 OK`. Database singleton connected. Cloudflare SSL certificate active. | [ ] Pass<br>[ ] Fail |

---

## 3. QA Sign-Off Execution Summary

```
Total Test Cases Executed: 32
Passed: ____ / 32
Failed: ____ / 32
P0 Blockers Remaining: ____ (Must be 0)
P1 Critical Defects Remaining: ____ (Must be 0)

QA Lead / Lead Developer Signature: Vraj Vithalani
Sign-Off Date: ____________________
Target Launch Window: Phase 4 Completion
```
