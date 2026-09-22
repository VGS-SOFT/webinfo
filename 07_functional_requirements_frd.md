# 07 Functional Requirements Document (FRD)

## 1. Document Overview & Purpose
This Functional Requirements Document (FRD) specifies the functional capabilities, user interactions, administrative workflows, and business logic for **vrajvithalani.com (V2)**. It serves as the authoritative, plain-language reference for what the system must do across both public user-facing routes and encrypted administrative portals (`/iamadmin/*`).

All features described herein are designed to uphold:
- **Single-Practitioner Execution**: Transparent positioning of Vraj Vithalani as sole lead strategist and engineer.
- **Critical Rule 9 (Contact Scarcity & Link Equity)**: Restricting public `/contact/` links on informational/case study routes to maximize PageRank flow into commercial Service Pillars.
- **Generative Engine Optimization (GEO)**: Structured 60–80 word short-answer callouts optimized for AI extraction (*ChatGPT, Perplexity, Google AI Overviews*).
- **Persistent Light Theme**: Uncompromising white (`#FFFFFF`) canvas with ink black (`#0A0A0A`) typography and soft emerald teal (`#14B8A6`) accents.

---

## 2. Public User-Facing Capabilities

### FR-01: Public Lead Capture & Qualification Form
- **Locations**: `/contact/`, modal triggers across Service Pillars (`/services/*`).
- **Description**: High-intent lead capture form designed for fast execution, minimal friction, and zero automated spam.
- **User Inputs**:
  1. `fullName`: Text input, mandatory (Min 2 chars).
  2. `email`: Email input, mandatory (Valid RFC 5322 syntax).
  3. `phone`: Tel input, mandatory (10-digit Indian mobile format or international E.164 syntax).
  4. `service`: Single-select dropdown, mandatory options:
     - *Google Ads High-Intent Campaign Management*
     - *Technical SEO & GEO (Generative Engine Optimization)*
     - *Custom Next.js & Hardened WordPress Development*
     - *Conversion Rate Optimization (CRO) & Automation*
     - *General Digital Strategy Consultation*
  5. `monthlyBudget`: Single-select dropdown, mandatory options:
     - *Under ₹25,000 / month*
     - *₹25,000 – ₹50,000 / month*
     - *₹50,000 – ₹1,00,000 / month*
     - *₹1,00,000+ / month (Enterprise)*
  6. `message`: Textarea, optional (Max 1,000 chars).
  7. `website_url`: Hidden text input (**Honeypot Trap**). Invisible to human users via CSS (`position: absolute; top: -9999px;`).
- **Functional Behavior**:
  - Validates all fields client-side before submission.
  - On submit, triggers a loading state ("Submitting Inquiry...").
  - If `website_url` contains any value (bot interaction), the server returns a HTTP 200 success response without saving data to MongoDB or firing notifications.
  - For legitimate submissions, posts data to `POST /api/contact`, displays a custom confirmation state ("Inquiry Received – Vraj Vithalani will reply within 24 hours"), and resets the form.

---

### FR-02: Interactive Google Ads Budget & ROI Simulator
- **Locations**: Embedded on `/services/google-ads/` and `/learn/google-ads/`.
- **Description**: Real-time client-side calculator allowing prospective advertisers to simulate click yields, lead volumes, and revenue projections based on real Indian market benchmarks.
- **Functional Inputs**:
  - **Monthly Ad Spend Slider**: Range from ₹15,000 to ₹3,000,000 (Step: ₹5,000, Default: ₹50,000).
  - **Target Industry Selector**: Dropdown options (*B2B Manufacturing/Export, Healthcare/Clinics, D2C E-Commerce, Local Services/SAB*).
- **Calculated Outputs (Real-Time)**:
  - Estimated Monthly Clicks: $	ext{Clicks} = rac{	ext{Monthly Budget}}{	ext{Avg. CPC (₹28.17)}}$
  - Estimated Qualified Leads: $	ext{Leads} = 	ext{Clicks} 	imes 	ext{Conversion Rate (5.0\%)}$
  - Projected Customer Conversions: $	ext{Conversions} = 	ext{Leads} 	imes 	ext{Sales Close Rate (20.0\%)}$
  - Estimated Revenue / Customer Lifetime Value (CLV).
- **CTA Action**: Includes a pre-filled button: *"Lock In This Budget Strategy on WhatsApp"*, which opens WhatsApp Web/App with pre-populated text containing the selected budget figure.

---

### FR-03: GEO Short-Answer Callout Component
- **Locations**: Rendered at the top of every key route (`/`, `/about/`, `/services/*`, `/case-studies/*`, `/learn/*`).
- **Description**: Prominently framed callout box containing a 60–80 word direct third-person summary of Vraj Vithalani's expertise, performance metrics, and entity references.
- **Functional Requirements**:
  - Rendered server-side as pure HTML inside a `<div className="glass-card bg-teal-50/50 border-teal-200">`.
  - Structured specifically for direct indexing and passage extraction by AI crawlers (*GPTBot, PerplexityBot, ClaudeBot*).
  - Explicitly references core entity markers: Computer Engineer (GTU), Surat, 100% single-practitioner execution, and proven ROI metrics.

---

### FR-04: Floating WhatsApp Action Component (`WhatsAppFloat.tsx`)
- **Locations**: Present across all public pages in the bottom-right viewport corner.
- **Description**: Unobtrusive direct messaging widget providing immediate access to Vraj Vithalani.
- **Functional Requirements**:
  - Displays a green WhatsApp icon badge with an active status dot.
  - On click, opens `https://wa.me/91XXXXXXXXXX?text=Hi%20Vraj,%20I%20visited%20vrajvithalani.com%20and%20would%20like%20to%20discuss...` in a new tab.
  - Suppressed inside administrative routes (`/iamadmin/*`).

---

### FR-05: Global Navigation & Breadcrumb System
- **Locations**: Global Header & Sub-headers across all public pages.
- **Functional Requirements**:
  - **Header Bar**: Displays brand logo, primary nav links (*Services, Case Studies, Locations, Learn, About*), and primary CTA button (*"Direct Contact"*).
  - **Mobile Responsive Drawer**: Toggles smoothly on screen widths `< 768px` using pure Tailwind transition states without external heavy libraries.
  - **Breadcrumbs**: Hierarchical navigation path (`Home > Services > Google Ads`) rendered at the top of sub-pages with inline `BreadcrumbList` Schema.org JSON-LD markup.

---

## 3. Administrative Portal Capabilities (`/iamadmin/*`)

### FR-06: Encrypted Admin Authentication & Session Security
- **Location**: `/iamadmin/` (Login route) and all child routes `/iamadmin/*`.
- **Functional Requirements**:
  - Unauthenticated access attempts to `/iamadmin/*` automatically redirect to `/iamadmin/`.
  - Credentials verified against Bcrypt-hashed records in MongoDB (`AdminUser` collection).
  - Successful login issues an encrypted HTTP-Only, `SameSite=Strict`, `Secure` JWT cookie (`vraj_admin_token`).
  - Session duration enforced at 8 hours.
  - Single-click Logout button clears cookie and redirects to `/iamadmin/`.

---

### FR-07: Lead Pipeline Management System (`/iamadmin/leads/`)
- **Location**: `/iamadmin/leads/`.
- **Description**: Internal CRM dashboard for managing inbound leads submitted via public contact forms.
- **Functional Capabilities**:
  1. **Lead Table Display**: Renders lead list with fields: Date, Full Name, Email, Phone, Service Choice, Budget, Status, Page Source.
  2. **Filter & Search**: Search bar filtering by Name/Email/Phone; Status filter pills (*All, New, Contacted, In Pipeline, Closed, Archived*).
  3. **Status Updating**: Inline dropdown allowing one-click status transitions.
  4. **Notes Logging**: Modal popup allowing Vraj Vithalani to add timestamped internal notes to any lead record.
  5. **Hard Deletion**: Administrative trash button permanently removing lead records from MongoDB Atlas for DPDP Act compliance.

---

### FR-08: Admin Tracking & Analytics Script Manager (`/iamadmin/settings/`)
- **Location**: `/iamadmin/settings/`.
- **Description**: Dynamic administration panel enabling instant deployment of analytics pixels and tracking scripts without modifying code or triggering Vercel/Render redeployments.
- **Functional Capabilities**:
  1. **Container IDs**: Input fields for GTM Container ID (`GTM-XXXXXXX`), GA4 Measurement ID (`G-XXXXXXXXXX`), and Meta Pixel ID (`XXXXXXXXXXXXXXXX`).
  2. **Custom Head Scripts Textarea**: Code editor box accepting arbitrary HTML/JavaScript (e.g., Microsoft Clarity, Hotjar, Custom Conversion Pixels) injected into document `<head>`.
  3. **Custom Body Scripts Textarea**: Code editor box accepting scripts injected immediately after opening `<body>` tag.
  4. **Active Toggle Switches**: Global enable/disable toggles for tracking scripts.
  5. **Save & Sync**: One-click save button updating the `Settings` singleton collection in MongoDB.

---

### FR-09: Cloudinary Media & Asset Vault (`/iamadmin/media/`)
- **Location**: `/iamadmin/media/`.
- **Description**: Integrated asset manager allowing instant drag-and-drop image uploads directly to Cloudinary CDN for use in blog posts and case study updates.
- **Functional Capabilities**:
  1. **Drag-and-Drop Uploader**: Accepts PNG, JPG, WebP, SVG files up to 10MB.
  2. **Automatic CDN Optimization**: Uploads directly to Cloudinary `/vrajvithalani/production/` folder, auto-converting to WebP (`f_auto,q_auto`).
  3. **Accessibility Alt Text**: Prompts for mandatory image alt text prior to upload completion.
  4. **Media Grid & Search**: Displays chronological asset gallery with image previews, dimensions, file size, and upload date.
  5. **1-Click Markdown Copy**: Includes a "Copy Markdown Link" button generating ready-to-paste snippets: `![Alt Text](https://res.cloudinary.com/...)`.
  6. **Asset Deletion**: Allows permanent removal of media files from Cloudinary and MongoDB index.

---

## 4. Rule 9 & SEO Functional Enforcement

### FR-10: Critical Rule 9 PageRank Scarcity Enforcement
- **Constraint**: Public informational routes (`/learn/*`, `/case-studies/*`) MUST NOT contain direct HTML links (`<a href="/contact/">`) in their body content.
- **Functional Alternative**: Body content must feature soft-pitch CTA blocks linking upward to their parent **Service Pillar** (`/services/google-ads/`, `/services/seo/`, etc.). Direct `/contact/` links are strictly restricted to Service Pillars and the global header button.

---

### FR-11: Dynamic Sitemap & Search Engine Protocol
- **Locations**: `/sitemap.xml`, `/robots.txt`.
- **Functional Capabilities**:
  - `/sitemap.xml` dynamically queries MongoDB and Next.js route trees, generating valid XML sitemaps with `<lastmod>` timestamps for all 50 active routes.
  - `/robots.txt` explicitly allows indexing by major search crawlers (*Googlebot, Bingbot*) and AI search bots (*GPTBot, PerplexityBot, ClaudeBot*), while disallowing access to `/iamadmin/*` and `/api/*`.

---

## 5. Functional Matrix & Acceptance Summary

| Feature ID | Name | User Role | Trigger / Access | Storage Target |
| :--- | :--- | :--- | :--- | :--- |
| **FR-01** | Lead Capture Form | Public Visitor | Route `/contact/` or CTA modals | MongoDB `Contact` Collection |
| **FR-02** | Google Ads ROI Calculator | Public Visitor | Slider interaction on `/services/google-ads/` | Client-side execution |
| **FR-03** | GEO AI Callout Block | Public / AI Bots | Page Load (RSC) | Static HTML rendering |
| **FR-04** | Floating WhatsApp Action | Public Visitor | Fixed viewport click | External `wa.me` URL |
| **FR-05** | Global Header & Nav Drawer | Public Visitor | Page Load / Mobile menu toggle | Client-side UI state |
| **FR-06** | JWT Admin Authentication | Superadmin | `/iamadmin/` login form | HTTP-Only `vraj_admin_token` Cookie |
| **FR-07** | Lead Pipeline CRM | Authenticated Admin | `/iamadmin/leads/` dashboard | MongoDB `Contact` CRUD |
| **FR-08** | Analytics Script Manager | Authenticated Admin | `/iamadmin/settings/` dashboard | MongoDB `Settings` Singleton |
| **FR-09** | Cloudinary Media Manager | Authenticated Admin | `/iamadmin/media/` dashboard | Cloudinary API & MongoDB `Media` |
| **FR-10** | Rule 9 PageRank Protection | Search Engines | Structural Route compilation | RSC Link Routing Rules |
| **FR-11** | Dynamic Sitemap & Robots | Crawlers & Bots | `/sitemap.xml` / `/robots.txt` fetch | Dynamic Next.js Route Handlers |
