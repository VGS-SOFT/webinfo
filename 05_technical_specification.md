# 05 Technical Specification

## 1. Full-Stack Architecture & Framework Stack

`vrajvithalani.com` is engineered as a zero-bloat, high-performance, single-practitioner web application built on **Next.js 15 (App Router)**, **React 19**, **TypeScript 5.x**, and **Tailwind CSS v3/v4**.

### Core Technology Stack

| Layer | Technology | Version | Justification |
| :--- | :--- | :--- | :--- |
| **Framework** | Next.js (App Router) | `15.x` | Hybrid Static Site Generation (SSG) & Server-Side Rendering (SSR) for instant sub-second TTFB and Core Web Vitals optimization. |
| **UI Library** | React | `19.x` | Native React Server Components (RSC) architecture minimizing client JS bundle overhead. |
| **Language** | TypeScript | `5.x` | Strict type safety across database schemas, API payloads, and UI component props. |
| **Styling** | Tailwind CSS | `3.4+` / `4.x` | Utility-first, zero-runtime CSS with glassmorphism utilities and persistent light mode theme enforcement. |
| **Icons** | `lucide-react` | `0.4x+` | Lightweight, tree-shakeable SVG vector icons integrated directly as React components. |
| **Database ODM** | Mongoose | `8.x` | Strongly typed object modeling for MongoDB Atlas with strict schema validation. |
| **Authentication** | `jsonwebtoken` / `jose` | `9.x` | Statetag cryptographic JWT session verification stored in `HttpOnly` cookies for `/iamadmin/`. |
| **Media CDN** | Cloudinary SDK | `2.x` | Direct server-side upload, automatic WebP format transformation (`f_auto`), and global CDN asset delivery. |

---

## 2. Server vs. Client Component Conventions

To achieve 100/100 Core Web Vitals performance scores, `vrajvithalani.com` enforces a strict **Server-Component-First Architecture**:

```
                              ┌───────────────────────────────────┐
                              │     React Server Component (RSC)  │
                              │  - Fetches MongoDB / Local Data   │
                              │  - Generates JSON-LD Schema       │
                              │  - Renders Static HTML Markup     │
                              └─────────────────┬─────────────────┘
                                                │
                                    Passes minimal props
                                                │
                                                ▼
                              ┌───────────────────────────────────┐
                              │     React Client Component ('use client') │
                              │  - Interactive ROI Calculators    │
                              │  - Form State & Validation        │
                              │  - Admin Dashboard Uploader       │
                              │  - WhatsApp Floating Widget       │
                              └───────────────────────────────────┘
```

### Server Components (`default`)
* All page layouts (`app/**/page.tsx`), navigation footers, content pillars, case study long-form posts, and GEO short-answer callout blocks.
* Zero client-side JavaScript shipped for static text rendering.
* JSON-LD `@graph` schema injection (`components/SchemaGraph.tsx`) executes entirely on the server.

### Client Components (`'use client'`)
* **`components/ContactForm.tsx`**: Form state, client-side validation, honeypot inputs, and async fetch dispatching.
* **`components/GoogleAdsCalculator.tsx`**: Real-time slider calculations for monthly budget, click yield, and ROI projections.
* **`components/WhatsAppFloat.tsx`**: Dynamic window scroll detection and click-to-chat triggers.
* **`app/iamadmin/**`**: Interactive admin pipeline, status update dropdowns, tracking script code textareas, and Cloudinary drag-and-drop uploader.

---

## 3. Hosting, Deployment & Edge CDN Pipeline

The site is configured for hybrid deployment across **Vercel** and **Render** with a **Cloudflare CDN** proxy layer.

```
  [ Client Request ] ──► [ Cloudflare Edge CDN ] ──► [ Next.js 15 App Server ] ──► [ MongoDB Atlas ]
                                  │                               │
                      - Edge SSL & DNS                - Serverless API Routes
                      - WebP Caching                  - SSG / ISR Page Render
                      - WAF Anti-DDoS                 - Admin JWT Auth
                                                                  │
                                                                  ▼
                                                          [ Cloudinary CDN ]
```

### Edge Caching & Headers Strategy
* **Static Assets (`/_next/static/*`)**: `Cache-Control: public, max-age=31536000, immutable`.
* **Dynamic HTML Pages (`/services/*`, `/case-studies/*`)**: Statically generated at build time (SSG) with Incremental Static Regeneration (ISR) revalidation timers.
* **API Endpoints (`/api/*`)**: `Cache-Control: no-store, max-age=0`.

---

## 4. Third-Party Integrations & Service Contracts

### A. MongoDB Atlas Database
* **Connection Pooling**: Serverless singleton connection pattern (`lib/db.ts`) preventing `MongoServerError: Too many connections`.
* **IP Whitelisting**: Restricted to Render/Vercel egress IP ranges and administrator static IPs.

### B. Cloudinary Asset Pipeline
* **Target Directory**: `/vrajvithalani/production/`
* **Auto Transformations**: `f_auto,q_auto,w_1600` for case study screenshots; `f_auto,q_auto,w_800` for inline blog images.
* **Secure Upload Signature**: Generated via server-side API route using `CLOUDINARY_API_SECRET`.

### C. Script Injection Engine (`components/ScriptManager.tsx`)
* Loads GTM (`gtmContainerId`), GA4 (`ga4MeasurementId`), Meta Pixel (`metaPixelId`), and custom scripts directly from MongoDB settings.
* Implemented using Next.js `next/script` with `strategy="afterInteractive"` to prevent blocking First Contentful Paint (FCP).

---

## 5. Package Dependencies & Version Constraints

### Production Dependencies (`package.json`)

```json
{
  "name": "vrajvithalani-website",
  "version": "2.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^15.1.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "mongoose": "^8.9.0",
    "jsonwebtoken": "^9.0.2",
    "cloudinary": "^2.5.1",
    "lucide-react": "^0.468.0",
    "clsx": "^2.1.1",
    "tailwind-merge": "^2.5.5"
  },
  "devDependencies": {
    "@types/node": "^22.10.0",
    "@types/react": "^19.0.0",
    "@types/jsonwebtoken": "^9.0.7",
    "typescript": "^5.7.2",
    "tailwindcss": "^3.4.16",
    "postcss": "^8.4.49",
    "autoprefixer": "^10.4.20",
    "eslint": "^9.17.0",
    "eslint-config-next": "^15.1.0"
  }
}
```

---

## 6. Environment Variables Matrix (`.env.local`)

| Variable Name | Required | Scope | Description |
| :--- | :---: | :---: | :--- |
| `NEXT_PUBLIC_SITE_URL` | Yes | Public | Primary production URL (`https://vrajvithalani.com`). |
| `MONGODB_URI` | Yes | Server | MongoDB Atlas connection string with embedded auth credentials. |
| `JWT_SECRET` | Yes | Server | 256-bit cryptographic secret key for signing admin authentication tokens. |
| `ADMIN_DEFAULT_EMAIL` | Yes | Server | Default administrator email address for initial setup. |
| `ADMIN_DEFAULT_PASSWORD` | Yes | Server | Initial bcrypt hashed administrator password. |
| `CLOUDINARY_CLOUD_NAME` | Yes | Server/Public | Cloudinary account identifier string. |
| `CLOUDINARY_API_KEY` | Yes | Server | API Key for media upload authentication. |
| `CLOUDINARY_API_SECRET` | Yes | Server | API Secret key for signing Cloudinary upload signatures. |

---

## 7. Build Verification & Quality Gates

Before any deployment is promoted to production, the codebase must pass four automated build verification gates:

1. **TypeScript Strict Typecheck**: `npx tsc --noEmit` must return 0 errors.
2. **Next.js Linter**: `npm run lint` must pass without unhandled warnings or missing dependency array errors.
3. **SSG Route Compilation**: `npm run build` must cleanly generate all 50 static HTML routes without dynamic server usage bailouts.
4. **Environment Variable Assertion**: `lib/env.ts` must validate that all mandatory server environment variables are present before starting the server.
