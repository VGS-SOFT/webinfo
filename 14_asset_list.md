# 14 Master Asset Inventory & AI Image Prompt Guide

## Executive Summary & Scope

This document defines the strict, complete inventory of **all 31 visual image assets** for the 50-page `vrajvithalani.com` platform. To ensure zero bloat, high performance, and rapid maintainability, the site reuses these 31 core assets across all core pages, service pillars, location landing pages, case studies, and knowledge hub guides.

All assets are hosted via **Cloudinary CDN** under the production root directory `/vrajvithalani/production/`, served strictly in auto-optimized **WebP** format (`f_auto,q_auto`), with reserved width and height dimensions to guarantee **CLS = 0.00** Core Web Vitals performance.

---

## Asset Directory Structure & CDN Naming Conventions

```
https://res.cloudinary.com/vrajvithalani/image/upload/vrajvithalani/production/
├── brand/
│   ├── vraj-headshot-square.webp
│   ├── vraj-desk-google-ads.webp
│   ├── vraj-desk-seo.webp
│   ├── vraj-desk-developer.webp
│   └── vraj-desk-cro.webp
├── banners/
│   ├── banner-google-ads-funnel.webp
│   ├── banner-technical-seo.webp
│   ├── banner-nextjs-webdev.webp
│   ├── banner-cro-automation.webp
│   ├── geo-ai-overviews-hero.webp
│   ├── surat-landmark-abstract.webp
│   ├── ahmedabad-landmark-abstract.webp
│   └── bangalore-landmark-abstract.webp
├── og/
│   ├── og-default.webp
│   ├── og-services.webp
│   ├── og-learn.webp
│   ├── og-locations.webp
│   └── og-case-studies.webp
└── proof/
    ├── powercable-desktop.webp
    ├── drvishva-desktop.webp
    ├── drvishva-ads-dashboard.webp
    ├── vitthalshringar-desktop.webp
    ├── vitthalshringar-product.webp
    ├── trust-ngo-desktop.webp
    ├── trust-ngo-desktop-spoke.webp
    ├── little-genius-thumbnail.webp
    ├── parv-travels-thumbnail.webp
    ├── synergy-tutorials-thumbnail.webp
    ├── soni-classes-thumbnail.webp
    ├── vm-graphite-thumbnail.webp
    └── iota-anpr-thumbnail.webp
```

---

## Detailed Master Asset Inventory (31 Core Assets)

### 1. Personal Branding & Headshots (5 Assets)

| Asset ID / Slug | File Format & Dim. | Aspect Ratio | Target Pages / Reused In | Mandatory Alt Text | Production Source / Prompt Guide |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `vraj_headshot_square` | WebP (800x800px) | 1:1 | Homepage, About, Contact, Author Bio schema | "Vraj Vithalani - Technical SEO & Google Ads Specialist" | Studio photo of Vraj Vithalani in modern business casual attire, soft studio light, crisp neutral white/gray background, warm authentic smile. |
| `vraj_desk_google_ads` | WebP (1200x800px) | 3:2 | Google Ads Pillar, Google Ads Learn Hub | "Vraj Vithalani managing Google Ads campaign performance" | High-resolution photo of Vraj working at a clean dual-monitor workstation displaying Google Ads campaign metrics and PPC dashboards, soft teal accent ambient lighting. |
| `vraj_desk_seo` | WebP (1200x800px) | 3:2 | Technical SEO Pillar, SEO Learn Hub | "Vraj Vithalani conducting technical SEO audit" | Vraj reviewing search console indexing data and Core Web Vitals diagnostics on a modern desk setup with clean modern desk accessories. |
| `vraj_desk_developer` | WebP (1200x800px) | 3:2 | Web Dev Pillar, Web Dev Learn Hub | "Vraj Vithalani building Next.js web architecture" | Vraj coding in VS Code on an ultra-wide monitor, showing Next.js 15 TypeScript code, minimalist desk environment, soft ambient lighting. |
| `vraj_desk_cro` | WebP (1200x800px) | 3:2 | CRO Pillar, CRO Learn Hub | "Vraj Vithalani analyzing conversion rate optimization heatmaps" | Vraj reviewing conversion funnels and user heatmaps on tablet and desktop screens, focused professional expression. |

---

### 2. Service Banners & Feature Highlights (5 Assets)

| Asset ID / Slug | File Format & Dim. | Aspect Ratio | Target Pages / Reused In | Mandatory Alt Text | Production Source / Prompt Guide |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `banner_google_ads_funnel` | WebP (1920x1080px) | 16:9 | Google Ads Service Pillar, PPC Case Studies | "Google Ads search funnel and conversion tracking diagram" | Abstract 3D visual representing a high-converting PPC campaign funnel, glowing emerald teal data nodes on a clean white background with crisp typography. |
| `banner_technical_seo` | WebP (1920x1080px) | 16:9 | Technical SEO Service Pillar, SEO Case Studies | "Technical SEO crawler architecture and Schema graph" | Abstract visualization of web crawlers indexing structured JSON-LD entity nodes, clean white background, soft teal connected lines. |
| `banner_nextjs_webdev` | WebP (1920x1080px) | 16:9 | Web Dev Service Pillar, Web Dev Case Studies | "Next.js App Router serverless architecture diagram" | Clean 3D UI wireframe showing Next.js server components, 100/100 performance score gauge, modern minimalist tech aesthetic. |
| `banner_cro_automation` | WebP (1920x1080px) | 16:9 | CRO Service Pillar, CRO Case Studies | "Conversion rate optimization A/B test split funnel" | Sleek visual graph showing revenue uplift from 1.2% to 4.8% conversion rate, subtle teal trend line, stark white backdrop. |
| `geo_ai_overviews_hero` | WebP (1920x1080px) | 16:9 | Homepage Hero, Learn Hub Pillar | "Generative Engine Optimization AI Search extraction concept" | Abstract futuristic UI representing AI search engines (ChatGPT, Perplexity, Gemini) citing Vraj Vithalani entity knowledge nodes on a clean light theme canvas. |

---

### 3. Regional Landmark Visuals (3 Assets)

| Asset ID / Slug | File Format & Dim. | Aspect Ratio | Target Pages / Reused In | Mandatory Alt Text | Production Source / Prompt Guide |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `surat_landmark_abstract` | WebP (1200x800px) | 3:2 | Surat HQ & All 4 Surat Micro-location pages | "Surat Gujarat industrial and digital technology landscape" | Minimalist vector-line abstract architectural silhouette of Surat (Surat Diamond Bourse, Cable Bridge) with soft teal digital circuit overlays on a stark white canvas. |
| `ahmedabad_landmark_abstract` | WebP (1200x800px) | 3:2 | All Ahmedabad SAB location pages | "Ahmedabad SG Highway tech corridor abstract silhouette" | Minimalist vector architectural silhouette of Ahmedabad SG Highway tech parks, clean white aesthetic with subtle emerald teal accents. |
| `bangalore_landmark_abstract` | WebP (1200x800px) | 3:2 | All Bangalore SAB location pages | "Bangalore Koramangala and HSR Layout tech hub abstract" | Minimalist line art abstract silhouette of Bangalore's IT corridors, modern clean design on white backdrop. |

---

### 4. OpenGraph (OG) Social Sharing Cards (5 Assets)

| Asset ID / Slug | File Format & Dim. | Aspect Ratio | Target Pages / Reused In | Mandatory Alt Text | Production Source / Prompt Guide |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `og_default` | WebP (1200x630px) | 1.91:1 | Global Default, Homepage, About, Contact | "Vraj Vithalani - Google Ads, SEO, Web Dev & CRO Specialist" | High-impact social share card: Stark white background, black bold typography, emerald teal status dot, headshot thumbnail, crisp value proposition statement. |
| `og_services` | WebP (1200x630px) | 1.91:1 | Service Hub & All 4 Service Pillars | "Commercial Digital Marketing Services - Vraj Vithalani" | Social card featuring the 4 core service pillars (Google Ads, SEO, Next.js, CRO) with performance metrics badges. |
| `og_learn` | WebP (1200x630px) | 1.91:1 | Learn Hub & All 12 Knowledge Articles | "Technical Marketing & Growth Engineering Articles by Vraj Vithalani" | Social card displaying the Knowledge Hub brand, clean typography, entity authority badge. |
| `og_locations` | WebP (1200x630px) | 1.91:1 | Location Hub & All 12 City Pages | "Digital Marketing Services in Surat, Ahmedabad & Bangalore" | Social card displaying regional coverage map (Surat HQ, Ahmedabad, Bangalore) with clean light theme styling. |
| `og_case_studies` | WebP (1200x630px) | 1.91:1 | Case Study Hub & All 13 Client Studies | "Client Case Studies & Verified Growth Results - Vraj Vithalani" | Social card highlighting real client metrics (Powercable 4.8x B2B inquiries, Dr. Vishva 729 clicks @ ₹28.17 CPC). |

---

### 5. Client Case Studies & Proof Screenshots (13 Assets)

| Asset ID / Slug | File Format & Dim. | Aspect Ratio | Target Pages / Reused In | Mandatory Alt Text | Production Source / Prompt Guide |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `powercable_desktop` | WebP (1600x1000px) | 16:10 | Powercable Case Study, Homepage Proof | "Powercable B2B portal website desktop preview" | High-resolution framed browser mockup of the custom B2B Powercable portal, showing industrial wire product catalog and RFQ lead form. |
| `drvishva_desktop` | WebP (1600x1000px) | 16:10 | Dr. Vishva Case Study, Medical SEO pages | "Dr. Vishva Physiotherapy Clinic website preview" | Framed browser mockup of Dr. Vishva's clinic site showcasing appointment booking flow and local Surat SEO rankings. |
| `drvishva_ads_dashboard` | WebP (1600x1000px) | 16:10 | Dr. Vishva Case Study, Google Ads Pillar | "Google Ads performance dashboard for Dr. Vishva Clinic" | Verified Google Ads dashboard screenshot showing 729 conversions at ₹28.17 average cost per lead (sensitive client account numbers redacted). |
| `vitthalshringar_desktop` | WebP (1600x1000px) | 16:10 | Vitthal Shringar Case Study, E-commerce pages | "Vitthal Shringar Shringar collection e-commerce store" | Framed browser mockup of Vitthal Shringar e-commerce store displaying traditional gold/silver ornaments with instant WhatsApp checkout. |
| `vitthalshringar_product` | WebP (1200x800px) | 3:2 | Vitthal Shringar Case Study, CRO Pillar | "Vitthal Shringar 125-micron lamination product detail" | Product showcase screenshot highlighting the 125-micron protective lamination USP callout card. |
| `trust_ngo_desktop` | WebP (1600x1000px) | 16:10 | Devkunvarben Trust NGO Case Study | "Devkunvarben Trust NGO website desktop preview" | Framed browser mockup of Devkunvarben Charitable Trust, showing healthcare initiative pages and transparent donation portal. |
| `trust_ngo_desktop_spoke` | WebP (1600x1000px) | 16:10 | Devkunvarben Trust NGO Case Study, About Page | "Devkunvarben Trust vocational training initiative page" | Secondary screenshot showing Soni Computer Institute vocational training and computer literacy program placement metrics. |
| `little_genius_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "Little Genius Preschool website redesign thumbnail" | Portfolio thumbnail card showing Little Genius preschool 10x enrollment lift landing page. |
| `parv_travels_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "Parv Travels cab booking portal thumbnail" | Portfolio thumbnail card for Parv Travels online cab rental booking flow. |
| `synergy_tutorials_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "Synergy Tutorials education portal thumbnail" | Portfolio thumbnail card for Synergy Tutorials student lead capture engine. |
| `soni_classes_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "Soni Classes coaching institute website thumbnail" | Portfolio thumbnail card showing Soni Classes coaching institute portal. |
| `vm_graphite_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "VM Graphite industrial machinery catalog thumbnail" | Portfolio thumbnail card for VM Graphite industrial manufacturing product catalog. |
| `iota_anpr_thumbnail` | WebP (800x500px) | 16:10 | Historical Case Studies, Portfolio Grid | "IOTA ANPR automatic number plate recognition software thumbnail" | Portfolio thumbnail card for IOTA ANPR security software portal. |

---

## Technical Performance & Optimization Rules

1. **Cloudinary WebP Auto-Formatting**: All images must be requested with `f_auto,q_auto` to serve WebP or AVIF dynamically based on client browser capability.
2. **Explicit Width/Height Attributes**: Every rendered Next.js `<Image>` component must declare `width` and `height` matching the dimensions in this spec to eliminate layout shifts (CLS = 0.00).
3. **Lazy Loading Strategy**:
   * Hero assets (`vraj_headshot_square`, `geo_ai_overviews_hero`, `banner_*`) use `priority={true}` for rapid LCP < 1.2s execution.
   * All secondary portfolio thumbnails and case study screenshots use native `loading="lazy"`.
4. **Accessibility Enforcement**: Zero images are permitted without a descriptive `alt` string matching the exact mandatory alt text defined in this inventory.
