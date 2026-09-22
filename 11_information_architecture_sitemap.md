# 11 Information Architecture & Sitemap Matrix

## 1. Executive Summary & Routing Philosophy

This document defines the complete **50-page Information Architecture (IA)** and URL routing hierarchy for `vrajvithalani.com`. The routing design is engineered for maximum crawl efficiency, strict PageRank equity distribution, clear topical hub-and-spoke clustering, and seamless mobile/desktop navigation.

### Routing & Canonical Hygiene Rules
* **Base Domain**: `https://vrajvithalani.com`
* **Trailing Slash Policy**: Enforced `trailingSlash: true` in `next.config.ts`. Every URL ends with a trailing slash (`/`).
* **Case & Character Rules**: 100% lowercase, hyphen-separated (`kebab-case`), no underscores, no capital letters.
* **Canonical Policy**: Every page renders a self-referential `<link rel="canonical" href="https://vrajvithalani.com/path/">`.
* **Critical Rule 9 (Link Scarcity)**: Informational articles (`/learn/*`) and case studies (`/case-studies/*`) link upward to commercial Service Pillars (`/services/*`) and do NOT link directly to `/contact/` in body copy.

---

## 2. Global Navigation Architecture

### A. Primary Desktop Header Navigation
| Item Order | Label | Route | Dropdown / Sub-Items |
| :--- | :--- | :--- | :--- |
| **1** | **Services** | `/services/` | Mega-Menu: Google Ads (`/services/google-ads/`), Technical SEO (`/services/seo/`), Web Development (`/services/web-development/`), CRO & Automation (`/services/cro-and-automation/`). |
| **2** | **Case Studies** | `/case-studies/` | Quick Featured: Powercable B2B Export, Dr. Vishva Healthcare, Vitthal Shringar D2C, Devkunvarben Trust NGO. |
| **3** | **Locations** | `/locations/` | City Columns: Surat (Adajan, Vesu, Ring Road, Sachin GIDC), Ahmedabad (SG Highway, Prahladnagar), Bangalore (Koramangala, HSR, Whitefield). |
| **4** | **Learn Hub** | `/learn/` | Topic Pillars: Google Ads, SEO & GEO, Web Development, CRO & Automation. |
| **5** | **About** | `/about/` | Direct Link: Single-Practitioner Engineer Story & Credentials. |
| **CTA Button** | **Book Consultation** | `/contact/` | Primary Teal Accent Button. |

### B. Mobile Drawer Navigation
* **Top Header Bar**: Logo (`Vraj Vithalani`), Live Status Indicator (Pulsing Green Dot), Hamburger Icon.
* **Drawer Menu Items**:
  1. Services (Accordion expandable to 4 pillars)
  2. Case Studies (Accordion expandable to 4 featured builds)
  3. Locations (Accordion expandable to Surat, Ahmedabad, Bangalore)
  4. Learn Hub (Accordion expandable to 4 content hubs)
  5. About Vraj Vithalani
  6. **Full-Width Sticky CTA**: "Book Direct Call" (`/contact/`)

### C. Global Footer Taxonomy
* **Column 1 (Entity & Authority)**:
  * Logo & Vraj Vithalani Name
  * Bio: *"Single-Practitioner Computer Engineer & Digital Growth Specialist based in Surat, Gujarat."*
  * GEO Summary: *"Providing Google Ads, Technical SEO, Next.js Web Dev, and CRO without agency layers."*
* **Column 2 (Commercial Services)**:
  * [Google Ads Management](/services/google-ads/)
  * [Technical SEO & GEO](/services/seo/)
  * [Custom Web Development](/services/web-development/)
  * [CRO & Growth Automation](/services/cro-and-automation/)
* **Column 3 (Service Locations)**:
  * [Surat HQ & Service Areas](/locations/surat/)
  * [Ahmedabad Business Hub](/locations/ahmedabad/)
  * [Bangalore Tech Corridor](/locations/bangalore/)
* **Column 4 (Knowledge & Proof)**:
  * [Client Case Studies](/case-studies/)
  * [Learn Hub & Insights](/learn/)
  * [About Vraj Vithalani](/about/)
  * [Direct Contact](/contact/)
* **Bottom Bar (Legal & Copyright)**:
  * `© 2026 Vraj Vithalani. All rights reserved.`
  * [Privacy Policy](/privacy/) | [Terms & Conditions](/terms/)

---

## 3. Master 50-Page Sitemap Matrix

Below is the complete inventory of all 50 production routes, categorized by page type, route path, parent hierarchy, primary Schema.org node, and target intent.

### Category 1: Fixed Core Pages (9 Pages)

| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `/` | Vraj Vithalani | Google Ads, SEO, Web Development & CRO Specialist | `Person`, `ProfessionalService` | Brand Anchor & Service Overview |
| 2 | `/about/` | About Vraj Vithalani | Computer Engineer & Digital Consultant | `AboutPage`, `Person` | Practitioner Credentials & Story |
| 3 | `/contact/` | Contact Vraj Vithalani | Direct Consultation & Audit Booking | `ContactPage` | High-Intent Conversion |
| 4 | `/services/` | Digital Engineering Services | Google Ads, SEO, Web Dev & CRO | `CollectionPage` | Commercial Gateway |
| 5 | `/locations/` | Service Locations | Surat HQ, Ahmedabad & Bangalore SAB Matrix | `CollectionPage` | Geographic Coverage Hub |
| 6 | `/case-studies/` | Verified Client Case Studies | B2B, D2C, Healthcare & NGO Proof | `CollectionPage` | Social Proof Gateway |
| 7 | `/learn/` | Learn Hub | Google Ads, Technical SEO, Next.js & CRO Guides | `CollectionPage` | Educational Authority Hub |
| 8 | `/privacy/` | Privacy Policy | Data Protection & DPDP Act Compliance | `WebPage` | Statutory & Legal Compliance |
| 9 | `/terms/` | Terms & Conditions | Service Agreements & Engagement Policies | `WebPage` | Statutory & Legal Compliance |

---

### Category 2: Commercial Service Pillars (4 Pages)

| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 10 | `/services/google-ads/` | High-ROI Google Ads Management & Performance PPC | Vraj Vithalani | `Service` | High-Intent PPC Lead Capture |
| 11 | `/services/seo/` | Enterprise Technical SEO & GEO (AI Search) Optimization Services | `Service` | Organic Search & AI Overviews |
| 12 | `/services/web-development/` | Custom Next.js & Hardened WordPress Development Services | `Service` | High-Speed Web Development |
| 13 | `/services/cro-and-automation/` | Conversion Rate Optimization & Growth Automation Services | `Service` | Lead & Revenue Optimization |

---

### Category 3: Location SAB Matrix (12 Pages)

#### A. Surat HQ & Micro-Locality Matrix (5 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 14 | `/locations/surat/` | Digital Marketing & Web Development Specialist in Surat | HQ | `LocalBusiness` | Primary Physical Anchor |
| 15 | `/locations/surat/adajan/` | Google Ads & SEO Services in Adajan, Surat | Vraj Vithalani | `LocalBusiness` | Micro-Locality Target |
| 16 | `/locations/surat/vesu/` | Enterprise Digital Consulting in Vesu, Surat | Vraj Vithalani | `LocalBusiness` | Micro-Locality Target |
| 17 | `/locations/surat/ring-road/` | Textile & B2B Digital Marketing in Ring Road, Surat | `LocalBusiness` | B2B Commercial Hub Target |
| 18 | `/locations/surat/sachin-gidc/` | Industrial & Manufacturing SEO in Sachin GIDC, Surat | `LocalBusiness` | Industrial Zone Target |

#### B. Ahmedabad Service Area Business Matrix (3 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 19 | `/locations/ahmedabad/` | Digital Marketing & Next.js Web Dev Services in Ahmedabad | `ProfessionalService` | Regional Hub Anchor |
| 20 | `/locations/ahmedabad/sg-highway/` | Google Ads & Performance Marketing on SG Highway, Ahmedabad | `ProfessionalService` | Corporate Zone Target |
| 21 | `/locations/ahmedabad/prahladnagar/` | SEO & Web Development Services in Prahladnagar, Ahmedabad | `ProfessionalService` | High-Tech Zone Target |

#### C. Bangalore Service Area Business Matrix (4 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 22 | `/locations/bangalore/` | Technical SEO, Next.js & PPC Specialist in Bangalore | `ProfessionalService` | National Tech Hub Anchor |
| 23 | `/locations/bangalore/koramangala/` | Startup CRO & Web Development Services in Koramangala, Bangalore | `ProfessionalService` | Startup Hub Target |
| 24 | `/locations/bangalore/hsr-layout/` | Performance PPC & Technical SEO in HSR Layout, Bangalore | `ProfessionalService` | Tech Hub Target |
| 25 | `/locations/bangalore/whitefield/` | Enterprise SEO & Custom Next.js Dev in Whitefield, Bangalore | `ProfessionalService` | Enterprise IT Zone Target |

---

### Category 4: Featured & Historical Case Studies (13 Pages)

#### A. Featured Client Case Studies (4 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 26 | `/case-studies/powercable/` | Powercable B2B Export Case Study: 4.8x Inquiry Growth via SEO | `CaseStudy`, `Article` | B2B Manufacturing Proof |
| 27 | `/case-studies/dr-vishva-physiotherapy/` | Dr. Vishva Physiotherapy Case Study: 729 Local Patient Clicks @ ₹28.17 | `CaseStudy`, `Article` | Healthcare Local PPC Proof |
| 28 | `/case-studies/vitthal-shringar/` | Vitthal Shringar D2C Case Study: 3.2x Revenue Lift via Next.js CRO | `CaseStudy`, `Article` | D2C E-Commerce Proof |
| 29 | `/case-studies/devkunvarben-trust/` | Devkunvarben Charitable Trust Case Study: 100% Pro-Bono Digital Reach | `CaseStudy`, `Article` | NGO & Community Proof |

#### B. Historical Client Case Studies (9 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 30 | `/case-studies/dreams-astro/` | Dreams Astro Case Study: High-Converting Astrology PPC Campaigns | `CaseStudy` | Historical Proof |
| 31 | `/case-studies/vm-graphite/` | VM Graphite Case Study: Industrial Material SEO & Lead Generation | `CaseStudy` | Industrial Proof |
| 32 | `/case-studies/little-genius/` | Little Genius Preschool Case Study: 10x Admission Inquiry Lift | `CaseStudy` | Education Proof |
| 33 | `/case-studies/parv-travels/` | Parv Travels Case Study: Regional Tourism PPC & Lead Optimization | `CaseStudy` | Hospitality Proof |
| 34 | `/case-studies/synergy-tutorials/` | Synergy Tutorials Case Study: Local Coaching Enrollment Funnel | `CaseStudy` | Education Proof |
| 35 | `/case-studies/soni-classes/` | Soni Classes Case Study: Student Acquisition & Local SEO | `CaseStudy` | Education Proof |
| 36 | `/case-studies/vgs-it-solution/` | VGS IT Solution Case Study: Client Web Architecture Transformations | `CaseStudy` | Historical Web Proof |
| 37 | `/case-studies/vgs-store/` | VGS Store Case Study: Hardware & Accessories Local Retail Sales | `CaseStudy` | Retail Proof |
| 38 | `/case-studies/iota-anpr/` | IOTA ANPR Case Study: Computer Vision AI Software Landing Page | `CaseStudy` | SaaS & AI Proof |

---

### Category 5: Learn Hub & Content Pillars (12 Pages)

#### A. Content Pillar Hubs (4 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 39 | `/learn/google-ads/` | Google Ads Mastery Hub \| Scaling PPC, Budgets & Lead Quality | `CollectionPage` | PPC Educational Hub |
| 40 | `/learn/seo/` | Technical SEO & GEO Hub \| AI Search, Schema & Crawl Engineering | `CollectionPage` | Organic & AI Search Hub |
| 41 | `/learn/web-development/` | Web Development Hub \| Next.js 15, Performance & Hardened WP | `CollectionPage` | Technical Web Hub |
| 42 | `/learn/cro-and-automation/` | CRO & Automation Hub \| Friction Reduction, A/B Testing & Funnels | `CollectionPage` | Conversion Hub |

#### B. Deep Technical & Strategic Guides (8 Pages)
| # | Route Path | Page Title | Schema Node | Target Intent |
| :--- | :--- | :--- | :--- | :--- |
| 43 | `/learn/google-ads/b2b-export-campaigns/` | Scaling B2B Export Lead Generation via Google Ads: The ₹30K Test | `Article` | B2B PPC Guide |
| 44 | `/learn/google-ads/local-business-ad-scaling/` | Local Business PPC Scaling Guide: Optimizing CPCs & Patient Leads | `Article` | Local Business PPC Guide |
| 45 | `/learn/seo/technical-geo-framework/` | The 2026 Generative Engine Optimization Framework for Personal Brands | `Article` | GEO & AI Search Guide |
| 46 | `/learn/seo/location-sab-schema-guide/` | Structuring Multi-City SAB & LocalBusiness JSON-LD Schema | `Article` | Schema Implementation Guide |
| 47 | `/learn/web-development/nextjs-hardened-wordpress/` | Next.js App Router vs. Hardened WordPress: Building Fast Sites | `Article` | Web Architecture Guide |
| 48 | `/learn/web-development/core-web-vitals-100-guide/` | Achieving 100/100 Core Web Vitals on Next.js: An Engineering Blueprint | `Article` | Performance Guide |
| 49 | `/learn/cro-and-automation/ecommerce-conversion-playbook/` | D2C E-Commerce CRO Playbook: Frictionless Checkout & UX Lamination | `Article` | E-Commerce CRO Guide |
| 50 | `/learn/cro-and-automation/lead-form-friction-reduction/` | Form Friction Reduction & Micro-Conversions: Converting Cold Traffic | `Article` | Form Conversion Guide |

---

## 4. Internal Linking & Crawl Equity Hierarchy

```
                               ┌─────────────────────────┐
                               │     HOMEPAGE ("/")      │
                               └────────────┬────────────┘
                                            │
        ┌──────────────────────┬────────────┴────────────┬──────────────────────┐
        ▼                      ▼                         ▼                      ▼
┌──────────────┐       ┌──────────────┐          ┌──────────────┐       ┌──────────────┐
│ SERVICES HUB │       │ LOCATIONS HUB│          │ CASE STUDIES │       │  LEARN HUB   │
│  (/services/)│       │ (/locations/)│          │(/case-studies│       │   (/learn/)  │
└───────┬──────┘       └───────┬──────┘          └──────┬───────┘       └──────┬───────┘
        │                      │                        │                      │
        ▼                      ▼                        ▼                      ▼
  4 Service Pillars     12 Location Pages        13 Case Studies         12 Learn Guides
  (Commercial Money)    (Geographic Coverage)    (Proof Engines)         (Topical Authority)
        ▲                      ▲                        │                      │
        │                      │                        │ (Upward Soft CTAs)   │ (Upward Soft CTAs)
        └──────────────────────┴────────────────────────┴──────────────────────┘
                                  (All equity funnels UPWARD to Service Pillars)
```

### Crawl & Link Rules
1. **Homepage Top-Down Flow**: Passes main authority directly to `/services/`, `/locations/`, `/case-studies/`, and `/learn/`.
2. **Service Pillar Dominance**: The 4 Service Pillars (`/services/google-ads/`, `/services/seo/`, `/services/web-development/`, `/services/cro-and-automation/`) receive internal links from **all** case studies and learn hub articles.
3. **Upward Equity Funneling**: Articles and case studies never link directly to `/contact/` in copy (enforcing Rule 9 link scarcity), ensuring commercial service pages absorb and concentrate internal PageRank.
