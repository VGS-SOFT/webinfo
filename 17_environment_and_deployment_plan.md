# 17 Environment and Deployment Plan

## 1. Executive Summary & Infrastructure Overview

This document outlines the complete environment setup, Git branching strategy, continuous integration/continuous deployment (CI/CD) pipeline, and production deployment execution for the **Vraj Vithalani Platform**.

The infrastructure leverages a dual-cloud strategy:
* **Production Engine**: Hosted on **Hostinger Cloud Startup (Managed Node.js Hosting)** via direct GitHub CI/CD integration, delivering dedicated serverless/container performance for `vrajvithalani.com`.
* **Staging & QA Engine**: Hosted on **Vercel**, providing instant preview builds, edge function testing, and client QA validation prior to production release.
* **DNS & Edge Protection**: Proxied through **Cloudflare** for SSL/TLS termination, DDoS protection, edge caching, and instant DNS cutover.

---

## 2. Git Branching Strategy & Promotion Workflow

To ensure 100% stability, zero downtime, and strict quality control, code moves through a structured **3-branch workflow**:

```
+-----------------------------------------------------------------------------------+
|                                  GIT WORKFLOW                                     |
|                                                                                   |
|  [Local Dev] --(Commit/Push)--> [main]                                            |
|                                    |                                              |
|                                 (Merge)                                           |
|                                    v                                              |
|                                [staging] ----(Auto CI/CD)----> [Vercel Staging]   |
|                                    |                           (Live Preview QA)  |
|                             (QA Pass / ~1 Wk)                                     |
|                                    v                                              |
|                              [production] --(Auto CI/CD)----> [Hostinger Prod]    |
|                                                                (Live Website)     |
+-----------------------------------------------------------------------------------+
```

### Branch Roles & Policies

| Branch Name | Primary Purpose | Connected Target Platform | Deployment Trigger | Access & Merge Policy |
| :--- | :--- | :--- | :--- | :--- |
| **`main`** | Core development synchronization & version record baseline. | None (Repository Source) | Manual Push | Developer local pushes. Clean feature commits. |
| **`staging`** | Online testing, multi-device verification, and QA validation. | **Vercel** | Automatic on Push / PR Merge | Merged from `main` when features pass local tests. |
| **`production`** | Live public production environment (`vrajvithalani.com`). | **Hostinger Cloud** | Automatic on Push / PR Merge | Merged from `staging` after 1-week baking/QA period. |

---

## 3. Deployment Pipeline Step-by-Step

### Phase A: Local Development & Feature Isolation (`main`)
1. Developer builds features and tests locally on WSL2 / Node.js 22 (`http://localhost:3000`).
2. Run mandatory pre-commit verification:
   ```bash
   npm run lint
   npx tsc --noEmit
   npm run build
   ```
3. Commit clean code and push directly to `main` branch:
   ```bash
   git add .
   git commit -m "feat: complete section architecture for service pillars"
   git push origin main
   ```

### Phase B: Staging Deployment & Quality Assurance (`staging`)
1. Merge `main` into `staging`:
   ```bash
   git checkout staging
   git merge main
   git push origin staging
   ```
2. **Vercel Auto CI/CD Trigger**: Vercel detects the push on `staging`, runs `npm run build`, and deploys the build to `https://staging.vrajvithalani.com` (or Vercel preview URL).
3. **Staging QA Protocol (1-Week Baking Period)**:
   * Verify all 50 static pages load with HTTP 200 OK.
   * Verify lead submission and honeypot filtering on live server.
   * Check interactive ROI calculator on mobile devices.
   * Audit GA4 / GTM network requests in Developer Tools.

### Phase C: Production Release (`production`)
1. Upon successful QA bake period, merge `staging` into `production`:
   ```bash
   git checkout production
   git merge staging
   git push origin production
   ```
2. **Hostinger Cloud Auto CI/CD Trigger**:
   * Hostinger's GitHub integration receives the webhook trigger for the `production` branch.
   * Hostinger pulls the latest commit, executes `npm install`, runs `npm run build`, and restarts the Node.js application process (`npm run start` or PM2 process manager).
3. Live production changes immediately reflect on `https://vrajvithalani.com`.

---

## 4. Environment Variables & Secret Management Matrix

Secrets are strictly isolated across environments. Never commit `.env` files to Git.

| Variable Name | Local (`.env.local`) | Staging (Vercel) | Production (Hostinger) | Description |
| :--- | :--- | :--- | :--- | :--- |
| `NODE_ENV` | `development` | `staging` | `production` | Node execution environment |
| `NEXT_PUBLIC_SITE_URL` | `http://localhost:3000` | `https://staging.vrajvithalani.com` | `https://vrajvithalani.com` | Canonical site domain |
| `MONGODB_URI` | `mongodb://localhost:27017/vraj_dev` | `mongodb+srv://.../vraj_staging` | `mongodb+srv://.../vraj_prod` | MongoDB Atlas database string |
| `JWT_SECRET` | `dev_secret_key_123` | `staging_jwt_secret_456` | `PROD_CRYPTO_JWT_KEY_999!` | Administrative session signing key |
| `CLOUDINARY_CLOUD_NAME` | `vraj-dev` | `vraj-staging` | `vrajvithalani` | Cloudinary storage tenant |
| `CLOUDINARY_API_KEY` | `dev_key` | `staging_key` | `prod_api_key` | Cloudinary access API key |
| `CLOUDINARY_API_SECRET` | `dev_secret` | `staging_secret` | `prod_api_secret` | Cloudinary cryptographic secret |

---

## 5. Hostinger Cloud Node.js Server Configuration

### Application Settings (Hostinger Panel)
* **Application Framework**: Node.js 22.x LTS
* **Deployment Directory**: `/public_html` or `/app`
* **Build Command**: `npm run build`
* **Start Command**: `npm run start` (or `next start -p $PORT`)
* **Auto-Deploy**: Enabled for branch `production` via GitHub Webhook Integration.

### Next.js Production Configuration (`next.config.ts`)
```typescript
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  reactStrictMode: true,
  trailingSlash: true,
  images: {
    loader: 'default',
    domains: ['res.cloudinary.com'],
    formats: ['image/webp', 'image/avif'],
  },
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
        ],
      },
    ];
  },
};

export default nextConfig;
```

---

## 6. Post-Deployment Smoke Test & Verification Protocol

Immediately following any merge to `production`:
1. **Health Verification**: Query `GET https://vrajvithalani.com/api/contact` to verify API route responsiveness.
2. **Database Verification**: Test lead form submission on `/contact/` and verify record creation in MongoDB Atlas.
3. **Crawl Hygiene Verification**:
   * Inspect `https://vrajvithalani.com/sitemap.xml`
   * Inspect `https://vrajvithalani.com/robots.txt`
4. **Cloudflare Cache Management**:
   * If CSS or JS static assets were updated, perform a targeted purge or Purge Everything via Cloudflare API / Dashboard.
