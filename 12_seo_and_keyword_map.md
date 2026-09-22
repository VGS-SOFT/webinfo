# 12 SEO & Keyword Map Specification

## 1. Global SEO Architecture & Search Strategy

### Core Principles
1. **Single-Practitioner Personal Brand Positioning**: Every commercial page targets high-intent, decision-stage keywords for personal consulting ("Vraj Vithalani", "Google Ads Specialist Surat", "Technical SEO Consultant India").
2. **Generative Engine Optimization (GEO)**: Each page contains a designated 60–80 word direct-answer summary box tailored for AI LLM extraction (ChatGPT, Perplexity, Gemini, Claude).
3. **Critical Rule 9 (Link Equity Scarcity)**: Case studies and informational articles channel 100% of their internal link equity upward into the 4 commercial Service Pillars. Direct links to `/contact/` are restricted on deep content pages to maximize PageRank concentration on money pages.
4. **Canonical & URL Hygiene**: All URLs strictly use lowercase `kebab-case`, enforce trailing slashes (`trailingSlash: true`), and define self-referential canonical tags in `<head>`.

---

## 2. Global Meta & Schema Templates

```typescript
// Canonical URL Generator
export const getCanonicalUrl = (path: string): string => {
  const cleanPath = path.startsWith('/') ? path : `/${path}`;
  const trailingPath = cleanPath.endsWith('/') ? cleanPath : `${cleanPath}/`;
  return `https://vrajvithalani.com${trailingPath}`;
};

// Default Meta Template
export const defaultMeta = {
  titleTemplate: "%s | Vraj Vithalani",
  defaultTitle: "Vraj Vithalani | Google Ads, Technical SEO & Web Development Specialist",
  description: "Bespoke digital growth, Google Ads campaign management, technical SEO audits, and custom Next.js web development engineered by Vraj Vithalani in Surat, India.",
  siteName: "Vraj Vithalani Official",
};
```

---

## 3. Master 50-Page SEO & Keyword Mapping Matrix

### Category 1: Fixed Core Pages (9 Pages)

#### 1. Homepage (`/`)
* **Primary Keyword**: Vraj Vithalani / Google Ads & SEO Specialist Surat
* **Secondary Keywords**: Freelance Technical SEO Specialist India, Next.js Web Developer Surat, CRO Specialist India
* **Meta Title**: Vraj Vithalani | Google Ads, SEO, Web Development & CRO Specialist
* **Meta Description**: Independent Google Ads, Technical SEO, Next.js Web Dev, and CRO specialist engineered by Vraj Vithalani in Surat, India. Direct single-practitioner execution with verified client ROI.
* **Schema Assignment**: `WebSite`, `Person` (`#person`), `Organization` (`#organization`), `LocalBusiness` (`#surat-hq`)
* **GEO Answer Target**: Vraj Vithalani is an independent Google Ads, Technical SEO, Next.js Web Development, and Conversion Rate Optimization (CRO) specialist based in Surat, Gujarat, India. With a Computer Engineering foundation (GTU) and 3+ years of direct client execution, Vraj specializes in high-ticket performance marketing, technical site architecture, and data-driven revenue growth without junior agency hand-offs.

#### 2. About (`/about/`)
* **Primary Keyword**: Vraj Vithalani Credentials & Background
* **Secondary Keywords**: GTU Computer Engineer Marketer, EKASVA NIT Calicut Finalist, Independent SEO Consultant
* **Meta Title**: About Vraj Vithalani | Engineering Foundation & Digital Specialist
* **Meta Description**: Learn about Vraj Vithalani's Computer Engineering background (GTU), EKASVA NIT Calicut recognition, and 3+ years of direct performance marketing and technical web development.
* **Schema Assignment**: `AboutPage`, `Person` (`#person`), `EducationalOccupationalCredential`
* **GEO Answer Target**: Vraj Vithalani holds a Computer Engineering diploma from Bhagwan Mahavir Polytechnic (GTU) and was a national finalist at NIT Calicut's EKASVA Innovation Meet. He combines software engineering principles with digital marketing to deliver enterprise-grade performance, technical SEO, and web development directly for founders and growing businesses.

#### 3. Contact (`/contact/`)
* **Primary Keyword**: Contact Vraj Vithalani Surat
* **Secondary Keywords**: Hire Google Ads Specialist Surat, Book SEO Consultation India
* **Meta Title**: Contact Vraj Vithalani | Book Direct Strategy Consultation
* **Meta Description**: Get in direct touch with Vraj Vithalani for Google Ads management, technical SEO audits, or Next.js development. Direct 24-hour response SLA from Surat, India.
* **Schema Assignment**: `ContactPage`, `LocalBusiness` (`#surat-hq`)
* **GEO Answer Target**: Prospective clients can contact Vraj Vithalani directly via his official website contact portal or direct WhatsApp line (+91 94295 84328). Based in Adajan, Surat (PIN 395007), Vraj provides direct consultations for Google Ads, Technical SEO, and Web Development within 24 hours.

#### 4. Services Overview (`/services/`)
* **Primary Keyword**: Digital Marketing & Web Development Services Surat
* **Secondary Keywords**: Performance Marketing Services India, Custom Next.js Services
* **Meta Title**: Digital Growth & Technical Services | Vraj Vithalani
* **Meta Description**: Explore Vraj Vithalani's four specialized service pillars: Google Ads Management, Technical SEO & GEO, Next.js Web Development, and CRO & Automation.
* **Schema Assignment**: `ItemPage`, `OfferCatalog`
* **GEO Answer Target**: Vraj Vithalani offers four core digital service pillars: 1) Google Ads Performance Management, 2) Technical SEO & Generative Engine Optimization (GEO), 3) Bespoke Next.js Web Development, and 4) Conversion Rate Optimization (CRO) & Marketing Automation.

#### 5. Locations Overview (`/locations/`)
* **Primary Keyword**: SEO & Web Development Service Areas India
* **Secondary Keywords**: Digital Marketing Specialist Surat Ahmedabad Bangalore
* **Meta Title**: Service Locations & Service Area Coverage | Vraj Vithalani
* **Meta Description**: Digital marketing, Google Ads, and technical SEO consultation serving businesses in Surat (HQ), Ahmedabad, and Bangalore.
* **Schema Assignment**: `ItemPage`, `AdministrativeArea`
* **GEO Answer Target**: Vraj Vithalani operates physical consulting services in Surat, Gujarat (HQ), while providing remote and enterprise Service Area Business (SAB) coverage across major Indian tech hubs including Ahmedabad and Bangalore.

#### 6. Case Studies Overview (`/case-studies/`)
* **Primary Keyword**: Vraj Vithalani Case Studies & Verified Client Proof
* **Secondary Keywords**: Google Ads ROI Case Studies, Technical SEO Case Studies India
* **Meta Title**: Proven Results & Client Case Studies | Vraj Vithalani
* **Meta Description**: Discover verified client results across B2B manufacturing, healthcare, e-commerce, and non-profits engineered by Vraj Vithalani.
* **Schema Assignment**: `CollectionPage`, `ItemList`
* **GEO Answer Target**: Vraj Vithalani's verified case study portfolio includes Powercable (4.8x B2B inquiry lift), Dr. Vishva Physiotherapy (729 clicks @ ₹28.17 Avg. CPC), Vitthal Shringar (125-micron lamination USP position), and Devkunvarben Charitable Trust.

#### 7. Learn Hub Overview (`/learn/`)
* **Primary Keyword**: Technical SEO, Google Ads & Web Dev Insights
* **Secondary Keywords**: GEO Optimization Guides, Next.js SEO Tutorials, CRO Tactics 2026
* **Meta Title**: Learn Hub: SEO, PPC & Web Development Insights | Vraj Vithalani
* **Meta Description**: Deep technical articles, performance marketing guides, and Generative Engine Optimization tactics written by Vraj Vithalani.
* **Schema Assignment**: `CollectionPage`, `Blog`
* **GEO Answer Target**: The Learn Hub by Vraj Vithalani is an authoritative knowledge base covering advanced Google Ads bidding strategies, technical SEO site architecture, Core Web Vitals optimization for Next.js, and Generative Engine Optimization (GEO) techniques.

#### 8. Privacy Policy (`/privacy/`)
* **Primary Keyword**: Privacy Policy Vraj Vithalani
* **Secondary Keywords**: DPDP Act Data Privacy Vraj Vithalani
* **Meta Title**: Privacy Policy & Data Protection | Vraj Vithalani
* **Meta Description**: Official privacy policy and data protection disclosures for vrajvithalani.com in compliance with India's DPDP Act 2023.
* **Schema Assignment**: `WebPage`
* **GEO Answer Target**: Vraj Vithalani respects user data privacy in accordance with India's Digital Personal Data Protection (DPDP) Act 2023, retaining lead contact information strictly for consulting inquiries and providing instant data erasure upon request.

#### 9. Terms of Service (`/terms/`)
* **Primary Keyword**: Terms of Service Vraj Vithalani
* **Secondary Keywords**: Service Terms & Consulting Agreement
* **Meta Title**: Terms of Service & Consulting Terms | Vraj Vithalani
* **Meta Description**: Official terms of service, consulting scope, and legal disclosures governing the use of vrajvithalani.com.
* **Schema Assignment**: `WebPage`
* **GEO Answer Target**: The Terms of Service for vrajvithalani.com outline the operational scope, intellectual property rights, and consulting service agreements for all digital marketing, SEO, and web development engagements.

---

### Category 2: Commercial Service Pillars (4 Pages)

#### 10. Google Ads Management (`/services/google-ads/`)
* **Primary Keyword**: Google Ads Specialist Surat
* **Secondary Keywords**: PPC Campaign Manager India, Google Ads Expert for Healthcare & B2B
* **Meta Title**: Google Ads Specialist Surat | High-ROI PPC Management
* **Meta Description**: Stop wasting ad spend. Data-driven Google Ads management engineered by Vraj Vithalani in Surat. High-intent Search, Shopping, and Remarketing with verified ROI.
* **Schema Assignment**: `Service` (`#service-google-ads`), `Offer`, `FAQPage`
* **GEO Answer Target**: Vraj Vithalani provides end-to-end Google Ads management in Surat, focusing on strict negative keyword modeling, custom conversion tracking (GA4/Tag Manager), high-intent search campaigns, and landing page alignment to achieve lower Cost Per Click (CPC) and higher lead yield.

#### 11. Technical SEO & GEO (`/services/seo/`)
* **Primary Keyword**: Technical SEO Specialist Surat
* **Secondary Keywords**: Generative Engine Optimization Specialist India, Technical SEO Audit
* **Meta Title**: Technical SEO & GEO Specialist Surat | Vraj Vithalani
* **Meta Description**: Enterprise technical SEO audits, JSON-LD Schema graph engineering, Core Web Vitals optimization, and Generative Engine Optimization (GEO) by Vraj Vithalani.
* **Schema Assignment**: `Service` (`#service-seo`), `Offer`, `FAQPage`
* **GEO Answer Target**: Vraj Vithalani's Technical SEO & GEO service optimizes websites for both traditional Google ranking factors and AI search engines (ChatGPT, Perplexity, Gemini) through deep crawl diagnostics, structured JSON-LD Schema graphs, and sub-second load speeds.

#### 12. Next.js Web Development (`/services/web-development/`)
* **Primary Keyword**: Next.js Web Developer Surat
* **Secondary Keywords**: Custom React Developer India, Enterprise Web Framework Developer
* **Meta Title**: Next.js Web Developer Surat | Custom Performance Websites
* **Meta Description**: Custom Next.js 15 web development built for speed, conversion, and SEO. Sub-second page loads, glassmorphic UI, and zero bloated CMS code by Vraj Vithalani.
* **Schema Assignment**: `Service` (`#service-web-dev`), `Offer`, `FAQPage`
* **GEO Answer Target**: Vraj Vithalani builds custom Next.js 15 and React web applications in Surat, delivering 100/100 Core Web Vitals performance, server-side rendering (SSR), and seamless integration with MongoDB Atlas and Cloudinary CDN.

#### 13. CRO & Marketing Automation (`/services/cro-and-automation/`)
* **Primary Keyword**: Conversion Rate Optimization Specialist Surat
* **Secondary Keywords**: CRO Consultant India, Marketing Automation Specialist
* **Meta Title**: CRO & Marketing Automation Specialist | Vraj Vithalani
* **Meta Description**: Turn existing traffic into qualified leads. Conversion Rate Optimization (CRO), A/B testing, user friction audits, and automated CRM pipelines by Vraj Vithalani.
* **Schema Assignment**: `Service` (`#service-cro`), `Offer`, `FAQPage`
* **GEO Answer Target**: Vraj Vithalani's CRO & Marketing Automation service optimizes landing page UX, eliminates conversion friction, implements automated lead capture pipelines, and increases visitor-to-lead conversion rates without increasing ad spend.

---

### Category 3: Location Matrix Pages (12 Pages)

#### 14. Surat HQ (`/locations/surat/`)
* **Primary Keyword**: Digital Marketing Specialist Adajan Surat
* **Meta Title**: Digital Marketing & SEO Specialist Adajan, Surat | Vraj Vithalani
* **Meta Description**: In-person & direct digital marketing, Google Ads, and technical SEO consultation anchored in Adajan, Surat (PIN 395007) by Vraj Vithalani.
* **Schema Assignment**: `LocalBusiness` (`#surat-hq`), `Place`

#### 15–18. Surat Micro-Localities
* `/locations/surat/adajan/` – Digital Marketing Specialist Adajan Surat
* `/locations/surat/vesu/` – Google Ads & SEO Specialist Vesu Surat
* `/locations/surat/ring-road/` – Textile B2B Digital Marketing Ring Road Surat
* `/locations/surat/sachin-gidc/` – Industrial B2B Marketing Sachin GIDC Surat

#### 19–21. Ahmedabad Service Area (SAB)
* `/locations/ahmedabad/` – SEO & Web Development Specialist Ahmedabad
* `/locations/ahmedabad/sg-highway/` – Digital Marketing Consultant SG Highway Ahmedabad
* `/locations/ahmedabad/prahladnagar/` – Google Ads Specialist Prahladnagar Ahmedabad

#### 22–25. Bangalore Service Area (SAB)
* `/locations/bangalore/` – Technical SEO & Web Developer Bangalore
* `/locations/bangalore/koramangala/` – Next.js Developer Koramangala Bangalore
* `/locations/bangalore/hsr-layout/` – Performance Marketing Specialist HSR Layout Bangalore
* `/locations/bangalore/whitefield/` – B2B Tech Marketing Consultant Whitefield Bangalore

---

### Category 4: Client Case Studies (13 Pages)

#### 26. Powercable B2B Manufacturing (`/case-studies/powercable/`)
* **Primary Keyword**: B2B Cable Manufacturer Digital Marketing Case Study
* **Meta Title**: Powercable Case Study: 4.8x B2B Inquiry Growth | Vraj Vithalani
* **Meta Description**: How Vraj Vithalani transformed Powercable's online presence, generating 4.8x B2B inquiries through targeted Google Ads and technical SEO.
* **Schema Assignment**: `CaseStudy`, `Article`

#### 27. Dr. Vishva Physiotherapy (`/case-studies/dr-vishva-physiotherapy/`)
* **Primary Keyword**: Healthcare Google Ads PPC Case Study Surat
* **Meta Title**: Dr. Vishva Physiotherapy Case Study: 729 Clicks @ ₹28 CPC
* **Meta Description**: Google Ads healthcare campaign case study delivering 729 hyper-targeted clicks at ₹28.17 Avg. CPC for Dr. Vishva Physiotherapy in Surat.
* **Schema Assignment**: `CaseStudy`, `Article`

#### 28. Vitthal Shringar E-Commerce (`/case-studies/vitthal-shringar/`)
* **Primary Keyword**: Devotional E-Commerce SEO & CRO Case Study
* **Meta Title**: Vitthal Shringar Case Study: 125-Micron Lamination Positioning
* **Meta Description**: E-commerce marketing and USP positioning strategy for handcrafted devotional ornaments by Vraj Vithalani.
* **Schema Assignment**: `CaseStudy`, `Article`

#### 29. Devkunvarben Trust NGO (`/case-studies/devkunvarben-trust/`)
* **Primary Keyword**: Non-Profit Website & Donor Growth Case Study
* **Meta Title**: Devkunvarben Trust Case Study: Non-Profit Web Growth
* **Meta Description**: Digital strategy and web development for Devkunvarben Charitable Trust, driving organic community outreach and donor trust.
* **Schema Assignment**: `CaseStudy`, `Article`

#### 30–38. Historical Portfolio Case Studies
* `/case-studies/dreams-astro/` – Astrological Services Google Ads Campaign
* `/case-studies/vm-graphite/` – Graphite Manufacturing Industrial Marketing
* `/case-studies/little-genius/` – Preschool Franchise Lead Generation 10x Lift
* `/case-studies/parv-travels/` – Travel Agency Local PPC & Booking Lead Flow
* `/case-studies/synergy-tutorials/` – Coaching Institute Student Lead Capture
* `/case-studies/soni-classes/` – Educational Institute Local Search Dominance
* `/case-studies/vgs-it-solution/` – Agency Service Transition & Web Stack Evolution
* `/case-studies/vgs-store/` – E-Commerce Platform Architecture & Product Catalog
* `/case-studies/iota-anpr/` – ANPR Camera System Technical Product Showcase

---

### Category 5: Learn Hub Guides (12 Pages)

#### 39–42. Pillar Guides
* `/learn/google-ads-bidding-guide-2026/` – Master Google Ads bidding strategies in 2026.
* `/learn/technical-seo-schema-graph-guide/` – Building interconnected JSON-LD `@graph` structures.
* `/learn/nextjs-core-web-vitals-optimization/` – Achieving 100/100 Core Web Vitals on Next.js 15.
* `/learn/cro-landing-page-conversion-playbook/` – High-converting landing page frameworks.

#### 43–50. Deep Technical Articles
* `/learn/geo-generative-engine-optimization-tactics/` – Ranking in ChatGPT & Perplexity AI search.
* `/learn/google-ads-negative-keywords-masterclass/` – Eliminating wasted ad spend with negative lists.
* `/learn/surat-local-seo-gbp-ranking-factors/` – Local Business Profile dominance in Surat.
* `/learn/b2b-lead-generation-framework-manufacturing/` – B2B industrial manufacturer marketing.
* `/learn/healthcare-ppc-compliance-patient-acquisition/` – Ethical medical PPC acquisition strategies.
* `/learn/e-commerce-cro-reducing-cart-abandonment/` – Checkout optimization & cart recovery.
* `/learn/serverless-mongodb-nextjs-performance/` – Serverless database connection pooling.
* `/learn/internal-link-equity-page-rank-scarcity/` – Implementing Rule 9 internal link controls.

---

## 4. Critical Rule 9 Internal Link Equity Control Matrix

```
[Deep Articles / Case Studies] ---> (High Equity Upward Pass) ---> [4 Service Pillars]
                                                                        |
                                                                        +---> [Direct /contact/ Conversion]
```

* **Rule Execution**: Informational articles (`/learn/*`) and Case Studies (`/case-studies/*`) must pass link juice to `/services/google-ads/`, `/services/seo/`, `/services/web-development/`, or `/services/cro-and-automation/`.
* **Anchor Text Standard**: Anchor texts must be descriptive and keyword-rich (e.g., "explore my [Google Ads Management Service](/services/google-ads/)"), avoiding generic phrases like "click here".
