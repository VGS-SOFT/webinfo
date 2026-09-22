# 06 System Architecture & Component Data Flow Specification

## 1. Executive System Overview

`vrajvithalani.com` is engineered as a **hybrid Jamstack / Serverless Enterprise Web Platform** built on Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS, and MongoDB Atlas. The system prioritizes sub-second page loads (Core Web Vitals 100/100 targets), zero layout shift, strict search engine & LLM crawler extraction (GEO), and isolated administrative data control.

```
+-----------------------------------------------------------------------------------+
|                                  EDGE LAYER                                       |
|                  Cloudflare Edge CDN / SSL / DNS Proxy / WAF                      |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                              COMPUTE LAYER                                        |
|             Render Container / Vercel Serverless (Next.js 15 Engine)             |
|                                                                                   |
|  +---------------------------+  +-------------------+  +-----------------------+  |
|  | React Server Components   |  | API Route Handlers|  | Server Middleware     |  |
|  | (100% Static HTML / SSG)  |  | (REST Endpoints)  |  | (JWT Auth & Trailing) |  |
|  +---------------------------+  +-------------------+  +-----------------------+  |
+-----------------------------------------------------------------------------------+
                 |                         |                         |
                 v                         v                         v
+----------------------------------+ +-------------------+ +------------------------+
|       PERSISTENCE LAYER          | |   MEDIA ENGINE    | |    ANALYTICS ENGINE    |
|   MongoDB Atlas (Serverless DB)  | |   Cloudinary CDN  | | Dynamic Script Injection|
|  - Leads / Contacts Collection   | | (WebP Optimization| |  - GTM / GA4 / Pixel   |
|  - Settings Collection           | |   CDN Delivery)   | |  - Custom Head/Body JS  |
|  - Media Collection              | +-------------------+ +------------------------+
|  - AdminUser Collection          |
+----------------------------------+
```

---

## 2. Component Architecture (Server vs. Client Separation)

The application follows strict React Server Component (RSC) boundaries to ensure minimum client-side JavaScript bundle sizes.

```mermaid
graph TD
    A[Incoming Browser / Crawler Request] --> B[Next.js App Router]
    
    subgraph Server Boundary [Server Boundary - 100% Server Rendered HTML]
        B --> C[app/layout.tsx - Root Shell & Fonts]
        C --> D[app/page.tsx - Server Page Component]
        D --> E[components/GeoCallout.tsx - Static Schema/Text]
        D --> F[components/PersonSchema.tsx - JSON-LD Script Tag]
        D --> G[components/BrowserFrame.tsx - Static HTML Chrome]
    end
    
    subgraph Client Boundary [Client Boundary - Hydrated JavaScript 'use client']
        C --> H[components/ScriptManager.tsx - Dynamic Tracking Injector]
        D --> I[components/ContactForm.tsx - Lead Capture Form]
        D --> J[components/GoogleAdsCalculator.tsx - Interactive Simulator]
        D --> K[components/WhatsAppFloat.tsx - Floating Action Button]
    end
```

### Component Responsibility Breakdown
1. **Server Components (RSC)**: Render all structural HTML, typography, long-form copy, case study data, service specs, and inline `<script type="application/ld+json">` tags. They execute exclusively on the server and ship zero JS runtimes for text content.
2. **Client Components (`'use client'`)**: Isolated interactive islands handling form state, input validation, math calculation, local storage interaction, and dynamic DOM script injection.

---

## 3. Request / Response Data Flow Sequences

### A. Public Page Visit & Dynamic Script Ingestion Flow
When a public user visits any route (`/`, `/services/google-ads/`, `/case-studies/drvishva/`), the system fetches administrative tracking settings without blocking core page rendering.

```mermaid
sequenceDiagram
    autonumber
    actor User as Public Visitor / Browser
    participant CDN as Cloudflare Edge CDN
    participant Server as Next.js 15 Server (RSC)
    participant DB as MongoDB Atlas
    participant ScriptEngine as ScriptManager Component

    User->>CDN: GET /services/google-ads/
    CDN->>Server: Forward Cache Miss
    Server->>DB: Fetch Singleton Settings (GTM, GA4, Custom Scripts)
    DB-->>Server: Return Settings Document
    Server->>Server: Render Static HTML + Embed JSON-LD + Inject ScriptManager
    Server-->>CDN: Return Rendered HTML Stream
    CDN-->>User: Deliver HTML Page
    User->>User: DOM Interactive Parse
    ScriptEngine->>ScriptEngine: Execute next/script (afterInteractive)
    ScriptEngine->>User: Inject GTM / GA4 / Meta Pixel Async
```

---

### B. Public Lead Capture & Honeypot Processing Flow
Handles public form submissions on `/contact/` and embedded service page CTA blocks with anti-spam traps.

```mermaid
sequenceDiagram
    autonumber
    actor User as Prospect / Spambot
    participant Form as Client ContactForm Component
    participant API as POST /api/contact Route Handler
    participant DB as MongoDB Atlas

    User->>Form: Submit Form Data
    Form->>API: POST /api/contact { fullName, email, phone, website_url, ... }
    
    alt Spambot Triggered (website_url is populated)
        API->>API: Detect Honeypot Trap Violation
        API-->>Form: Return HTTP 200 OK { success: true } (Fake Success)
        Form-->>User: Display "Thank You" Message (No DB Write)
    else Legitimate User Submission (website_url is empty)
        API->>API: Validate Schema (Zod / Mongoose)
        API->>DB: Contact.create({ fullName, email, status: 'new', ipAddress, ... })
        DB-->>API: Document Saved
        API-->>Form: Return HTTP 201 Created { success: true }
        Form-->>User: Render Interactive Confirmation & Clear Form
    end
```

---

### C. Admin Vault Authentication & Session Verification Flow
Governs access to encrypted administrative endpoints (`/iamadmin/*`).

```mermaid
sequenceDiagram
    autonumber
    actor Admin as Consultant (Vraj Vithalani)
    participant Page as /iamadmin/login UI
    participant AuthAPI as POST /api/admin/auth/login
    participant Middleware as Next.js Server Middleware
    participant DB as MongoDB Atlas

    Admin->>Page: Enter Credentials
    Page->>AuthAPI: POST { username, password }
    AuthAPI->>DB: AdminUser.findOne({ username })
    DB-->>AuthAPI: User Document (Bcrypt Hash)
    AuthAPI->>AuthAPI: Verify Password Hash & Check Lockout
    
    alt Invalid Password
        AuthAPI->>DB: Increment failedLoginAttempts
        AuthAPI-->>Page: HTTP 401 Unauthorized
    else Valid Password
        AuthAPI->>AuthAPI: Generate JWT (userId, role, tokenVersion)
        AuthAPI-->>Admin: Set Cookie: admin_token (HttpOnly, Secure, SameSite=Strict)
        AuthAPI-->>Page: HTTP 200 OK { redirect: '/iamadmin/leads' }
    end

    Admin->>Middleware: Request GET /iamadmin/leads
    Middleware->>Middleware: Extract admin_token Cookie & Verify JWT Signature
    Middleware-->>Admin: Allow Request & Render Vault Interface
```

---

### D. Cloudinary Media Manager Workflow
Governs drag-and-drop file uploads inside `/iamadmin/media/` for instant blog and case study Markdown insertion.

```mermaid
sequenceDiagram
    autonumber
    actor Admin as Consultant (Vraj Vithalani)
    participant UI as /iamadmin/media UI
    participant API as POST /api/admin/media Handler
    participant Cloudinary as Cloudinary API / CDN
    participant DB as MongoDB Atlas

    Admin->>UI: Drag & Drop Image File + Enter Alt Text
    UI->>API: POST multipart/form-data
    API->>API: Validate Admin JWT Cookie
    API->>Cloudinary: Upload Stream (folder: 'vrajvithalani/production')
    Cloudinary-->>API: Return { public_id, secure_url, width, height, format }
    API->>DB: Media.create({ publicId, secureUrl, altText, dimensions, ... })
    DB-->>API: Document Persisted
    API-->>UI: Return HTTP 201 { markdownUrl: '![Alt](https://res.cloudinary.com/...)' }
    UI-->>Admin: Render Image Thumbnail & Copy Markdown Button
```

---

## 4. Search Engine & AI Crawler (GEO) Retrieval Architecture

To ensure dominant visibility across both traditional search engines (Google, Bing) and Generative AI engines (ChatGPT Search, Perplexity, Google AI Overviews, Claude), the system implements a dual-ingestion architecture.

```mermaid
graph LR
    subgraph Site Engine [vrajvithalani.com Platform]
        A[Static Pre-rendered HTML Pages]
        B[Schema.org @graph JSON-LD Ingestion]
        C[GEO Third-Person Callout Blocks]
        D[Dynamic sitemap.xml & robots.txt]
    end

    subgraph Search & AI Crawlers [Crawlers & Search Engines]
        E[Googlebot / Bingbot]
        F[GPTBot / OpenAI Search]
        G[PerplexityBot]
        H[ClaudeBot / Anthropic]
    end

    D -->|Explicit Index Directives| E
    D -->|Explicit Index Directives| F
    D -->|Explicit Index Directives| G
    D -->|Explicit Index Directives| H

    A -->|Fast DOM Structural Parse| E
    B -->|Entity Knowledge Graph Mapping| E
    B -->|Entity Relationship Verification| F
    C -->|Direct Answer Snippet Extraction| F
    C -->|Direct Answer Snippet Extraction| G
    C -->|Direct Answer Snippet Extraction| H
```

### GEO Extraction Optimization Rules
1. **Third-Person Semantic Clarity**: Every GEO callout box explicitly states *"Vraj Vithalani is an independent computer engineer and digital marketing consultant based in Surat, Gujarat..."* so AI parsers attribute skills directly to the `#person` entity ID.
2. **Schema Entity Graphs**: Every page links its schema node back to `https://vrajvithalani.com/#person` under `author`, `publisher`, or `provider` properties.
3. **Robots.txt Directive Compliance**: AI crawlers (`GPTBot`, `PerplexityBot`, `ClaudeBot`) are explicitly granted `Allow: /` access while keeping administrative paths (`Disallow: /iamadmin/`, `Disallow: /api/admin/`) strictly private.

---

## 5. System Infrastructure & Host Topology

```
                                  +-----------------------+
                                  |   Cloudflare Edge     |
                                  | DNS / SSL / WAF Proxy |
                                  +-----------+-----------+
                                              |
                                              v
                                  +-----------------------+
                                  | Render / Vercel Host  |
                                  | Next.js App Server    |
                                  +-----+-----------+-----+
                                        |           |
                     +------------------+           +------------------+
                     |                                                 |
                     v                                                 v
        +-------------------------+                       +-------------------------+
        |   MongoDB Atlas DB      |                       |   Cloudinary Asset CDN  |
        | Serverless Cluster      |                       | Media Delivery Network  |
        | - Leads Collection      |                       | - Blog Images           |
        | - Settings Collection   |                       | - Screenshots           |
        | - Media Collection      |                       | - Auto WebP Transform   |
        | - AdminUser Collection  |                       +-------------------------+
        +-------------------------+
```

### Infrastructure Configuration Standards
* **Hosting Platform**: Render / Vercel Serverless Node.js runtime.
* **Database**: MongoDB Atlas M0/Serverless Cluster with TLS encrypted connections.
* **Media Delivery**: Cloudinary CDN with automatic WebP conversion and adaptive image quality.
* **DNS & Security Proxy**: Cloudflare Free/Pro Proxy tier handling SSL termination, HTTP/3 delivery, and rate limiting.
