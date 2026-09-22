# 08 Non-Functional Requirements (NFR)

## 1. Document Overview & Scope

This document specifies the non-functional requirements (NFRs) for the **Vraj Vithalani Enterprise Web Platform (`vrajvithalani.com`)**. These criteria define the system's operational quality, speed performance targets, security boundaries, accessibility compliance, device compatibility, and reliability expectations. Every requirement is quantifiable and testable to ensure production excellence for a single-practitioner personal brand.

---

## 2. Core Web Vitals & Speed Performance Standards

All public pages must achieve top-tier performance scores evaluated on real-world mobile devices (4G network simulation) and desktop environments under Google PageSpeed Insights and Real User Monitoring (RUM) criteria.

| Metric | Target (75th Percentile) | Critical Threshold | Optimization Strategy |
| :--- | :--- | :--- | :--- |
| **Lighthouse Performance Score** | **100 / 100** (Desktop & Mobile) | ≥ 95 / 100 | Zero un-utilized JS/CSS, server-side pre-rendering (RSC), inline critical CSS. |
| **Largest Contentful Paint (LCP)** | **< 1.2s** | < 2.5s | Next.js `<Image>` pre-loading, Cloudinary auto-WebP (`f_auto,q_auto`), priority hero asset headers. |
| **Interaction to Next Paint (INP)** | **< 50ms** | < 200ms | Zero heavy main-thread blocking scripts, deferred tracking script execution (`next/script`). |
| **Cumulative Layout Shift (CLS)** | **0.00** | < 0.10 | Explicit `width` and `height` aspect ratio reservation on images and containers. |
| **First Contentful Paint (FCP)** | **< 0.8s** | < 1.8s | Edge HTML caching via Cloudflare CDN and serverless edge rendering. |
| **Time to First Byte (TTFB)** | **< 200ms** | < 600ms | Static Site Generation (SSG) for static routes, edge response caching. |

---

## 3. Asset & Network Payload Budgets

To guarantee sub-second load speeds across tier-2 and tier-3 Indian mobile network infrastructure (e.g., Surat, Ahmedabad, regional 4G/5G), strict bundle size limits are enforced.

* **Initial JS Bundle Size**: **< 100 KB** (gzipped) for initial page hydration.
* **Total Page Weight (Core Routes)**: **< 1.5 MB** including all images, fonts, and scripts.
* **Font Payload**: Maximum **30 KB** total font asset weight using system font stack fallbacks and Google Font subsetting (`subsets: ['latin']`).
* **Image Compression Budget**:
  * Desktop Banners / Mockups: **< 150 KB** per image.
  * Mobile Inline Images / Headshots: **< 60 KB** per image.
  * Formats: WebP or AVIF default with fallback PNG/JPEG.
* **Script Optimization Budget**: Third-party tracking scripts (GA4, GTM, Meta Pixel) must execute asynchronously *after* page hydration (`strategy="afterInteractive"` or `strategy="lazyOnload"`).

---

## 4. Accessibility & Inclusivity Standards (WCAG 2.1 AA)

The site must be fully accessible to users with visual, auditory, motor, or cognitive impairments.

* **WCAG Conformance**: Full compliance with **WCAG 2.1 Level AA** standards across all 50 routes.
* **Color Contrast Ratios**:
  * Normal Body Text (`#0A0A0A` on `#FFFFFF`): **20.4:1 contrast ratio** (exceeds 4.5:1 requirement).
  * Brand Accent (`#14B8A6` on `#0A0A0A` / `#FFFFFF`): Minimum **4.5:1 ratio** for readable elements.
* **Keyboard Navigation**:
  * 100% interactive elements accessible via `Tab` key.
  * Visible focus ring indicators (`focus-visible:outline-2 focus-visible:outline-brand-teal`).
* **Screen Reader Compatibility**:
  * Semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<article>`, `<aside>`, `<footer>`).
  * Explicit `aria-label` tags on icon-only buttons (e.g., mobile hamburger menu, floating WhatsApp action, modal triggers).
  * Mandatory `alt` text on every image registered in the Cloudinary Media Vault.
* **Reduced Motion Support**:
  * Respect browser `prefers-reduced-motion` settings by disabling CSS/JS animations and smooth scrolls for sensitive users.

---

## 5. Cross-Browser & Multi-Device Support Matrix

The visual display and functional logic must render predictably across all modern desktop, tablet, and mobile browsers.

| Category | Supported OS / Devices | Target Browsers | Screen Resolutions |
| :--- | :--- | :--- | :--- |
| **Mobile** | iOS 15+, Android 10+ | Safari Mobile, Chrome Mobile, Samsung Internet | 320px – 430px (e.g., iPhone SE to Pro Max) |
| **Tablet** | iPadOS, Android Tablets | Safari, Chrome Mobile | 768px – 1024px |
| **Desktop** | macOS, Windows 10/11, Linux | Chrome, Safari, Firefox, Edge (latest 2 versions) | 1280px, 1440px, 1920px (Full HD), 2560px (2K), 3840px (4K) |

### Device-Specific Quirks & Rules:
* **iOS Safari Safe Areas**: Support bottom notch and home bar spacing (`padding-bottom: env(safe-area-inset-bottom)`) for the floating WhatsApp action widget.
* **Touch Target Sizes**: Minimum touch target area of **44x44 pixels** for all buttons, links, and input fields on touch displays.

---

## 6. System Reliability, Uptime & Resilience

* **Service Availability**: Target **99.9% Uptime SLA** backed by Render / Vercel container hosting and Cloudflare edge proxying.
* **Database Fault Tolerance**:
  * MongoDB Atlas M0 connection pooling singleton (`lib/db.ts`) with automatic retries on transient connection drops.
  * Failure Graceful Degradation: If MongoDB is unreachable, public static pages continue serving normally without crashing (API submission displays a friendly error notice).
* **Cold-Start Mitigation**: Serverless API routes optimized for execution times under **200ms**, with light middleware footprints.

---

## 7. Crawl Efficiency & GEO AI Search Latency

* **Crawl Response Time**: Server response to search engine crawlers (*Googlebot, Bingbot*) and AI search agents (*GPTBot, PerplexityBot, ClaudeBot*) must be under **300ms** to maximize crawl budget.
* **XML Sitemap Freshness**: Dynamic `/sitemap.ts` re-generation upon new route deployment within **30 seconds**.
* **Zero Broken Canonical Policy**: 100% self-referential canonical tags enforcing a uniform trailing slash (`trailingSlash: true`) policy to prevent duplicate page indexing.
