# 09 User Stories & Acceptance Criteria

**Document Status**: Approved / Master Production Blueprint  
**System Target**: Vraj Vithalani Enterprise Platform (`vrajvithalani.com` V2)  
**Author**: Vraj Vithalani (Computer Engineer & Lead Architect)  
**Format**: Gherkin (`Given-When-Then`) Acceptance Criteria Standards  

---

## 1. Document Overview

This document defines the formal **User Stories** and **Acceptance Criteria** for all functional capabilities of the `vrajvithalani.com` enterprise web platform. Each story provides clear feature-level tasks with unambiguous "done" conditions using standard Gherkin syntax (`Given-When-Then`).

These stories ensure the single-developer implementation on Next.js 15 App Router meets exact operational, security, and conversion requirements without ambiguity.

---

## 2. Public User Experience & Conversion Stories

### US-01: Inbound Lead Capture & Qualification Form

* **User Role**: Prospective Commercial Client (Business Owner, Marketing Director)
* **Goal**: Submit a detailed project inquiry and receive instant confirmation while qualifying project scope.
* **Value**: Enables high-intent lead collection directly into the administrative CRM pipeline while shielding the infrastructure from automated spam.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Public Lead Capture Form

  Scenario: Successful high-intent lead submission
    Given a user is on the "/contact/" page or viewing a service pillar lead section
    When the user fills in "fullName" with "Rajesh Mehta"
    And the user fills in "email" with "rajesh@powercable.in"
    And the user fills in "phone" with "+91 98765 43210"
    And the user selects "service" as "Google Ads Management"
    And the user selects "monthlyBudget" as "₹50,000 - ₹1,00,000"
    And the user fills in "message" with "We need to scale B2B export campaign yields."
    And the honeypot field "website_url" is left empty
    And the user clicks the "Submit Consultation Request" button
    Then the client-side state transitions to "submitting"
    And a POST request is sent to "/api/contact"
    And the API saves the record to MongoDB Atlas with status "new"
    And the client receives a 200 OK response with "success: true"
    And an animated confirmation card displays "Inquiry Received! Vraj will respond within 24 hours."
    And form input fields are cleared.

  Scenario: Bot detection via invisible honeypot trap
    Given an automated spambot scrapes the contact form
    When the spambot populates the invisible "website_url" input with "http://spam-link.com"
    And the spambot submits the form to "/api/contact"
    Then the API interceptor detects non-empty "website_url"
    And the server skips all MongoDB Atlas database write operations
    And the server skips email webhook notifications
    And the server immediately returns a fake "200 OK" response with "{ success: true, message: 'Message sent successfully' }"
    And the spambot script concludes submission successfully without polluting the CRM pipeline.

  Scenario: Client-side validation failure
    Given a user attempts to submit the contact form
    When the user leaves mandatory field "email" blank or enters an invalid string "rajesh@"
    And the user clicks "Submit Consultation Request"
    Then form submission is halted on the client
    And an inline red validation error appears: "Please provide a valid corporate email address."
    And no HTTP network request is dispatched.
```

---

### US-02: Interactive Google Ads ROI & Budget Simulator

* **User Role**: Prospective Ad Client
* **Goal**: Dynamically estimate potential click yield, qualified lead counts, and projected return on ad spend (ROAS) based on monthly ad budgets.
* **Value**: Reduces sales friction by proving mathematical performance frameworks before initial sales calls.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Interactive Google Ads ROI Simulator

  Scenario: Dynamic budget slider calculation
    Given a user is on the "/services/google-ads/" page
    When the user drags the interactive budget slider to "₹30,000"
    Then the client-side state recalculates instantly without network calls
    And Projected Clicks displays "1,065 clicks" (Calculated as ₹30,000 / ₹28.17 Avg. CPC)
    And Projected Leads displays "53 leads" (Calculated as 1,065 clicks * 5.0% Conversion Rate)
    And Projected Closed Deals displays "10 customers" (Calculated as 53 leads * 20.0% Sales Close Rate)
    And the "Claim This Strategy on WhatsApp" CTA updates its deep-link URL to:
        "https://wa.me/919876543210?text=Hi%20Vraj,%20I%20used%20your%20calculator%20with%20a%20budget%20of%20₹30,000."

  Scenario: Custom CPC and target industry selection
    Given a user adjusts the ROI Simulator advanced inputs
    When the user inputs a custom industry benchmark CPC of "₹45.00"
    Then all projected yield cards update instantaneously using the new CPC denominator.
```

---

### US-03: Generative Engine Optimization (GEO) Short-Answer Extraction

* **User Role**: Search Engine Crawlers & AI Search Engines (*ChatGPT, Perplexity, Gemini, Google AI Overviews*)
* **Goal**: Parse 60–80 word self-contained factual summaries structured for direct entity extraction.
* **Value**: Maximizes visibility in conversational AI search results and direct-answer SERP feature blocks.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: GEO Short-Answer Component Rendering

  Scenario: Server-side rendering of GeoCallout block
    Given a request is made to any Core, Service, or Location route
    When Next.js renders the React Server Component
    Then a `<section>` container with class "geo-callout-block" is rendered in static HTML
    And the text contains a third-person entity summary between 60 and 80 words
    And the entity explicitly states "Vraj Vithalani", primary discipline, operating location, and core value proposition
    And no client-side JavaScript execution is required to read the callout text.
```

---

## 3. Administrative Vault & Operational Stories (`/iamadmin/*`)

### US-04: Secure Administrative Authentication & Session Security

* **User Role**: Vraj Vithalani (System Administrator)
* **Goal**: Gain secure access to the administrative vault using cryptographic JWT cookies.
* **Value**: Prevents unauthorized access to proprietary client lead data and tracking settings.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Admin Auth & Session Management

  Scenario: Successful admin login
    Given the admin navigates to "/iamadmin/login/"
    When the admin inputs correct "username" and "password"
    And clicks "Authenticate Vault"
    Then the server verifies credentials via Bcrypt comparison against `AdminUser` collection
    And the server generates a signed JWT payload containing `userId`, `role`, and `tokenVersion`
    And the server sets an `auth_token` HTTP cookie with `HttpOnly=true`, `Secure=true`, `SameSite=Strict`
    And the browser redirects smoothly to "/iamadmin/leads/".

  Scenario: Unauthenticated route guard intercept
    Given an unauthenticated visitor attempts to navigate directly to "/iamadmin/settings/"
    When the Next.js edge middleware inspects request headers
    Then missing or invalid `auth_token` cookie is identified
    And the middleware redirects the visitor immediately to "/iamadmin/login/?redirect=/iamadmin/settings/".

  Scenario: Admin session logout
    Given an authenticated admin is inside "/iamadmin/leads/"
    When the admin clicks "Logout"
    Then a POST request is sent to "/api/admin/auth/logout"
    And the server responds by setting `auth_token` cookie expiration to Epoch 0
    And the client redirects to "/iamadmin/login/".
```

---

### US-05: Inbound Lead Pipeline & CRM Management

* **User Role**: System Administrator
* **Goal**: Filter, review, update status, add internal notes, and manage inbound leads stored in MongoDB.
* **Value**: Streamlines lead conversion tracking and client pipeline administration.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Lead CRM Management

  Scenario: Searching and updating lead status
    Given the admin is authenticated inside "/iamadmin/leads/"
    When the admin types "Powercable" into the pipeline search input
    Then the client filters rows matching name, email, or message text in real time
    When the admin selects status dropdown for lead ID "65f..." and changes status from "new" to "contacted"
    And types "Sent initial proposal via WhatsApp" into internal notes
    And clicks "Save Changes"
    Then a PATCH request is dispatched to "/api/admin/leads"
    And MongoDB updates `status="contacted"` and appends note object with ISO timestamp
    And a success toast notification appears: "Lead updated successfully."
```

---

### US-06: Dynamic Analytics & Tracking Script Injection Manager

* **User Role**: System Administrator
* **Goal**: Add, edit, or toggle tracking scripts (GTM, GA4, Meta Pixel, custom JS) dynamically without code redeployments.
* **Value**: Enables instant tracking adjustments without triggering Next.js build pipelines on Render/Vercel.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Analytics Script Manager

  Scenario: Updating tracking scripts via admin panel
    Given the admin is inside "/iamadmin/settings/"
    When the admin inputs GTM Container ID "GTM-XXXXXXX"
    And inputs GA4 Measurement ID "G-YYYYYYYYY"
    And pastes custom Meta Pixel snippet into "Custom Head Scripts" code editor
    And clicks "Save Tracking Configuration"
    Then a PUT request is dispatched to "/api/admin/settings"
    And MongoDB Atlas updates the singleton `Settings` document
    And public pages instantly inject scripts on next request using Next.js `ScriptManager` (`strategy="afterInteractive"`)
    And core performance scores remain unimpacted.
```

---

### US-07: Cloudinary Media & Asset Manager

* **User Role**: System Administrator / Blog Author
* **Goal**: Drag-and-drop upload images to Cloudinary, auto-format to WebP, and copy 1-click Markdown image tags.
* **Value**: Speeds up content publishing by eliminating manual image editing, hosting, and path formatting.

#### Acceptance Criteria (Gherkin):

```gherkin
Feature: Cloudinary Media Manager

  Scenario: Uploading image asset and copying Markdown URL
    Given the admin is inside "/iamadmin/media/"
    When the admin drops file "seo-audit-dashboard.png" into the upload dropzone
    And fills in mandatory Alt Text field "Technical SEO Audit Dashboard showing Core Web Vitals"
    And clicks "Upload Asset"
    Then a POST request uploads the file directly to Cloudinary folder `/vrajvithalani/production/`
    And Cloudinary returns optimized WebP URL `https://res.cloudinary.com/.../f_auto,q_auto/...`
    And MongoDB registers asset metadata in `Media` collection
    And an asset card appears in the grid with a prominent "Copy Markdown Link" button
    When the admin clicks "Copy Markdown Link"
    Then string `![Technical SEO Audit Dashboard showing Core Web Vitals](https://res.cloudinary.com/...)` is copied to clipboard.
```

---

## 4. Summary Matrix of Acceptance Conditions

| Story ID | Feature Name | Critical Acceptance Criteria | Target SLA / Metric |
| :--- | :--- | :--- | :--- |
| **US-01** | Lead Capture | Valid submission saves to DB; Honeypot silently drops bots | DB write < 150ms |
| **US-02** | ROI Simulator | Real-time slider calculation using exact formulas | Client render < 16ms |
| **US-03** | GEO Callouts | Server-rendered 60–80 word direct answer blocks | 0ms JS overhead |
| **US-04** | Admin Auth | Cryptographic HTTP-Only JWT cookies & route protection | Token expiry: 8 hrs |
| **US-05** | Lead CRM | Search, filter, status transition, and notes logging | Search response < 100ms |
| **US-06** | Script Manager | Dynamic injection of GTM/GA4/Pixel without code push | Runtime injection < 10ms |
| **US-07** | Media Manager | Drag-and-drop Cloudinary upload with 1-click Markdown link | WebP compression < 1s |

---
