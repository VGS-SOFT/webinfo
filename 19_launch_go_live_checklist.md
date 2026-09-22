# 19 Launch & Go-Live Checklist

## Executive Summary & Launch Governance
This document specifies the exact, step-by-step **Launch & Go-Live Protocol** for the **Vraj Vithalani Website Platform**. It governs the complete transition from staging preview (`staging` branch on Vercel) to live production (`production` branch on Hostinger Cloud Node.js), ensuring zero downtime, 100% data integrity, valid SSL encryption, and immediate search engine indexing.

---

## 1. T-7 Days: Pre-Launch Technical & Content Verification
Before initiating any deployment code promotion, all 18 preceding architectural blueprints must be signed off and verified.

- [ ] **Data Model & DB Verification**: `MONGODB_URI` connection strings verified for Atlas Production Cluster (`vrajvithalani-prod`). Mongoose indexes (`Contact`, `Settings`, `Media`, `AdminUser`) built and validated.
- [ ] **Content & Placeholder Sanitization**: 100% of temporary string placeholders (`[PIN]`, `[COORDINATES]`, `[HEADSHOT_URL]`) verified against `03_design_system_style_guide.md` and actual production values.
- [ ] **Media Assets Integrity**: All 31 image assets uploaded to Cloudinary (`/vrajvithalani/production/`) with WebP auto-formatting and mandatory accessibility `alt` text.
- [ ] **Legal & Privacy Verification**: Privacy Policy (`/privacy/`) and Terms of Service (`/terms/`) audited for alignment with India's **DPDP Act 2023**.
- [ ] **Build Verification**: Local `main` branch builds cleanly via `npm run build` with zero TypeScript errors (`tsc --noEmit`) or ESLint warnings.

---

## 2. T-3 Days: Staging Environment (`staging.vrajvithalani.com`) QA Baking
The staging environment hosted on Vercel (`staging` branch) undergoes a mandatory 3-day stability bake.

- [ ] **Functional Form Testing**: Submit live test leads through `/contact/` and service forms. Confirm honeypot spambot trap functions silently (fake `200 OK`).
- [ ] **Admin Vault Audit**: Test login at `/iamadmin/login` using encrypted Bcrypt credentials. Verify session security, lead status transitions (*New, Contacted, In Pipeline, Closed, Archived*), internal notes logging, and hard record deletion.
- [ ] **Dynamic Analytics Injection**: Test GA4, GTM, Meta Pixel, and Microsoft Clarity script injection via `/iamadmin/settings/`. Confirm zero main-thread render blocking.
- [ ] **Interactive Calculators**: Verify Google Ads ROI Simulator math (\(CPC = ₹28.17\), \(CR = 5.0\%\), \(SCR = 20.0\%\)) across mobile and desktop devices.
- [ ] **Cross-Device & Browser Audits**: Test responsive layouts on iOS Safari, Android Chrome, Desktop Chrome, Firefox, and Edge. Ensure touch targets are $\ge 44\times 44$px.

---

## 3. T-1 Day: Production Hostinger Cloud Node.js Infrastructure Prep
Prepare the Hostinger Cloud Startup Node.js container for live production traffic.

- [ ] **Hostinger Node Environment Setup**: Ensure Node.js 22 LTS is configured in the Hostinger Cloud panel. Set root directory to `/vrajvithalani-nextjs`.
- [ ] **Environment Variables Configuration**: Inject all production secrets into the Hostinger environment manager:
  - `NODE_ENV=production`
  - `NEXT_PUBLIC_SITE_URL=https://vrajvithalani.com`
  - `MONGODB_URI=mongodb+srv://...`
  - `JWT_SECRET=...`
  - `CLOUDINARY_CLOUD_NAME=...`, `CLOUDINARY_API_KEY=...`, `CLOUDINARY_API_SECRET=...`
- [ ] **PM2 Process Manager**: Configure `ecosystem.config.js` or standard PM2 process startup script (`npm run start` on port 3000).
- [ ] **Database Pre-Seeding**: Run `npm run seed:admin` on production MongoDB Atlas to create the initial encrypted Superadmin account.

---

## 4. T-0 Hour: Live Cutover & DNS Migration
Execute code promotion to the Hostinger production branch and point Cloudflare DNS.

- [ ] **Step 1: Git Promotion**: Merge verified `staging` branch into `production` branch (`git checkout production && git merge staging && git push origin production`).
- [ ] **Step 2: Hostinger CI/CD Deployment**: Verify Hostinger GitHub auto-deploy webhook triggers and successfully executes `npm install && npm run build`. Confirm container status is **Active/Running**.
- [ ] **Step 3: Cloudflare DNS Update**: Update DNS A Records in Cloudflare:
  - `vrajvithalani.com` $\rightarrow$ Point to Hostinger Production IP
  - `www.vrajvithalani.com` $\rightarrow$ CNAME to `vrajvithalani.com`
- [ ] **Step 4: SSL/TLS Proxy Verification**: Ensure Cloudflare SSL setting is set to **Full (Strict)**. Verify automatic HTTPS redirect (`http://` $\rightarrow$ `https://`).
- [ ] **Step 5: Trailing Slash Enforcement**: Confirm Cloudflare page rules enforce 301 redirects for uppercase or non-trailing-slash URLs.

---

## 5. T+1 Hour: Post-Launch Smoke Test & Live Inspection
Immediately inspect live production URL (`https://vrajvithalani.com`).

- [ ] **HTTP Response Status Check**: Verify 200 OK across all primary routes (`/`, `/about/`, `/contact/`, `/services/google-ads/`, `/services/seo/`, `/services/web-development/`, `/services/cro-and-automation/`).
- [ ] **Live Lead Capture Verification**: Submit a real production lead on `/contact/`. Verify entry appears instantly in `/iamadmin/leads/` MongoDB Atlas database.
- [ ] **Live Core Web Vitals Inspection**: Run Google PageSpeed Insights on live production URL. Target: **100/100 Desktop & Mobile**, LCP $< 1.2$s, CLS $= 0.00$.
- [ ] **Schema.org Validation**: Inspect Schema graph using Google Rich Results Test. Confirm zero errors on `Person`, `Service`, `LocalBusiness`, and `BreadcrumbList` nodes.
- [ ] **Analytics Realtime Audit**: Open GA4 Realtime Debug View and Meta Pixel Helper. Confirm `page_view` and `lead_form_start` events fire accurately.

---

## 6. T+24 Hours: Search Engine Indexing & Ecosystem Submissions
Alert search crawlers and AI bots to index the new platform.

- [ ] **Google Search Console**: Submit dynamic XML sitemap (`https://vrajvithalani.com/sitemap.xml`). Request index inspection on homepage and 4 commercial service pillars.
- [ ] **Bing Webmaster Tools**: Submit sitemap XML and trigger direct URL submission.
- [ ] **AI Crawler Access Verification**: Test `robots.txt` access for `GPTBot`, `PerplexityBot`, `ClaudeBot`, and `Google-Extended`.
- [ ] **Google Business Profile (GBP)**: Update primary website URL on Surat GBP listing to `https://vrajvithalani.com/locations/surat-adajan/`.
- [ ] **Social Media Ecosystem Profiles**: Update link-in-bio URLs on LinkedIn, X/Twitter, Instagram, and GitHub profiles.

---

## 7. Post-Launch Maintenance & Backup Protocols
Sustain system performance, security, and continuous deployment health.

- [ ] **Automated MongoDB Atlas Backups**: Verify daily automated snapshots are active with 30-day point-in-time retention.
- [ ] **Log Monitoring**: Inspect Hostinger PM2 logs (`pm2 logs`) for unexpected runtime exceptions or unhandled promise rejections.
- [ ] **1-Week Code Freeze Buffer**: Enforce strict 7-day baking window before pushing new feature releases to `production`.
