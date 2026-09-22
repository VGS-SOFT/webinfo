# 04 Security & Compliance Specification

## 1. Executive Summary & Security Philosophy
This document establishes the security architecture, threat model, input sanitization protocols, authentication mechanics, and data privacy compliance framework for **vrajvithalani.com** (Next.js 15 App Router on Render/Vercel with MongoDB Atlas and Cloudinary).

As a single-practitioner personal brand platform, security is built around three core principles:
1. **Zero External Friction Anti-Spam**: Block automated form bots using invisible client-side traps (honeypot) without subjecting human prospects to CAPTCHA puzzles.
2. **Hardened Administrative Vault**: Protect the `/iamadmin/` management interface via HTTP-Only, `SameSite=Strict` JWT session cookies, rate-limited login endpoints, and instant cryptographic session revocation.
3. **Data Privacy & Statutory Compliance**: Align with India's **Digital Personal Data Protection (DPDP) Act 2023** and global standards via cookie-less analytics, transparent lead collection disclosure, and automated data retention hygiene.

---

## 2. Anti-Spam Architecture: Invisible Honeypot Pattern

To maintain a frictionless conversion funnel while blocking automated spam bots, `vrajvithalani.com` uses an **Invisible Client-Side Honeypot** backed by silent server-side rejection.

```
+-----------------------------------------------------------------------------------+
|                                 CLIENT / BROWSER                                  |
|                                                                                   |
|  [ Visible Form Inputs ]                                                          |
|  - Full Name, Email, Phone, Service, Budget, Message                             |
|                                                                                   |
|  [ Invisible Trap Input ] (Off-screen / SR-only)                                 |
|  - Name: `website_url`                                                           |
|  - CSS: `opacity: 0; position: absolute; top: -9999px; left: -9999px;`           |
|  - TabIndex: -1, aria-hidden="true", autocomplete="off"                           |
+-----------------------------------------------------------------------------------+
                                          |
                                          | POST /api/contact
                                          v
+-----------------------------------------------------------------------------------+
|                                SERVER (API ROUTE)                                 |
|                                                                                   |
|  IF payload.website_url IS NOT EMPTY:                                            |
|    1. Log bot attempt silently (IP, User-Agent, Timestamp)                       |
|    2. RETURN HTTP 200 OK {"success": true, "message": "Thank you!"}              |
|    3. DO NOT SAVE TO MONGODB ATLAS                                               |
|    4. DO NOT TRIGGER NOTIFICATION EMAILS / WEBHOOKS                              |
|                                                                                   |
|  ELSE:                                                                            |
|    1. Validate required visible fields                                            |
|    2. Sanitize HTML/Script tags                                                   |
|    3. Save lead to MongoDB Atlas                                                 |
|    4. Return HTTP 201 Created                                                     |
+-----------------------------------------------------------------------------------+
```

### 2.1 Honeypot Implementation Details
* **Trap Input Identifier**: `website_url` (chosen to attract spambots searching for link-building input fields).
* **Styling Defense**: Styled via inline styles and Tailwind utilities (`absolute -top-[9999px] -left-[9999px] opacity-0 pointer-events-none -z-50`) rather than `display: none` to trick sophisticated bots that ignore hidden inputs.
* **Silent Rejection Mechanics**: When `website_url` contains any character string, the server returns an immediate `200 OK` response identical to a successful submission. This tricks bot scripts into logging a success, preventing retry loops or adaptive payload testing.

---

## 3. Administrative Authentication & Session Hardening (`/iamadmin/`)

The administrative portal (`/iamadmin/`) provides complete control over lead status, Cloudinary media uploads, and dynamic tracking script injection. It is protected by multi-layered cryptographic controls.

### 3.1 Authentication Flow & Token Storage
* **Cryptographic Signing**: JWT tokens are signed using `HS256` with a high-entropy secret (`JWT_SECRET`, min 64 random characters).
* **Cookie Flags**: Tokens are delivered exclusively via HTTP-Only, Secure cookies (`auth_token`) with `SameSite=Strict` attributes.
  * **`HttpOnly`**: Blocks XSS script access (`document.cookie` cannot read the token).
  * **`Secure`**: Enforces HTTPS transport only.
  * **`SameSite=Strict`**: Completely blocks Cross-Site Request Forgery (CSRF) by withholding cookies on cross-origin requests.
  * **`Path=/`**: Restricts cookie scope to site routing.
  * **`Max-Age`**: Fixed 8-hour expiration (`28,800` seconds).

```typescript
// Cookie issuance header specification
Set-Cookie: auth_token=<JWT_STRING>; HttpOnly; Secure; SameSite=Strict; Path=/; Max-Age=28800
```

### 3.2 Instant Session Revocation (`tokenVersion`)
To allow instant revocation of all active admin sessions in the event of credential leakage:
1. The `AdminUser` document in MongoDB maintains an integer field: `tokenVersion: number`.
2. The JWT payload encodes `{ userId, email, role, tokenVersion }`.
3. The server-side API middleware (`middleware.ts`) verifies that `payload.tokenVersion === user.tokenVersion` on every `/api/admin/*` request.
4. Incrementing `tokenVersion` in the database immediately invalidates all issued JWTs across all active browsers.

### 3.3 Admin Brute-Force & Rate Limiting
* **Failed Attempt Penalty**: `/api/admin/auth/login` tracks failed attempts per IP address using a memory store / Redis cache.
* **Thresholds**:
  * 5 consecutive failed attempts within 15 minutes triggers a 15-minute IP lockout (`429 Too Many Requests`).
  * Admin login responses do not distinguish between invalid username vs. invalid password (`"Invalid admin credentials"`).

---

## 4. Input Sanitization & Injection Defense

### 4.1 NoSQL Injection Protection
* **MongoDB Operator Stripping**: Mongoose schemas enforce strict type validation.
* **Query Object Sanitization**: User-supplied input string values in JSON bodies are sanitized to strip MongoDB query operators (`$gt`, `$ne`, `$where`, `$regex`).
* **Mongoose Schema Strictness**: All schemas enforce `strict: true`, dropping unrecognized payload fields automatically.

### 4.2 Cross-Site Scripting (XSS) Prevention
* **React Auto-Escaping**: React 19 / Next.js JSX automatically escapes all string variables rendered in the DOM.
* **Raw HTML Disallowance**: `dangerouslySetInnerHTML` is banned across all public components. The single exception is `<ScriptManager />` which renders authorized tracking scripts stored in the encrypted admin database.
* **Form Text Sanitization**: All incoming contact form text fields (`fullName`, `message`) pass through HTML entity encoding before database insertion.

### 4.3 HTTP Security Headers Policy
Next.js `next.config.ts` applies mandatory security headers to all HTTP responses:

```typescript
// Security Header Configuration Specification
const securityHeaders = [
  { key: 'X-DNS-Prefetch-Control', value: 'on' },
  { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' },
  { key: 'X-Frame-Options', value: 'SAMEORIGIN' },
  { key: 'X-Content-Type-Options', value: 'nosniff' },
  { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
  { key: 'Permissions-Policy', value: 'camera=(), microphone=(), geolocation=(), interest-cohort=()' },
  {
    key: 'Content-Security-Policy',
    value: [
      "default-src 'self'",
      "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.googletagmanager.com https://www.google-analytics.com https://connect.facebook.net",
      "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
      "img-src 'self' data: blob: https://res.cloudinary.com https://www.google-analytics.com https://www.facebook.com",
      "font-src 'self' https://fonts.gstatic.com",
      "connect-src 'self' https://www.google-analytics.com https://stats.g.doubleclick.net https://connect.facebook.net https://api.cloudinary.com",
      "frame-ancestors 'self'",
    ].join('; ')
  }
];
```

---

## 5. Data Privacy & Statutory Compliance (DPDP Act 2023 / GDPR)

### 5.1 Digital Personal Data Protection (DPDP) Act 2023 Alignment
As a consultant operating from Surat, Gujarat, India, data processing complies with the DPDP Act 2023 requirements for Data Fiduciaries:

1. **Specified Purpose Notice**: The contact form explicitly states:
   > *"By submitting this form, you consent to Vraj Vithalani storing your details to respond to your project inquiry. Your data is never sold, shared, or used for third-party marketing."*
2. **Data Minimization**: Only essential project fields (`fullName`, `email`, `phone`, `service`, `monthlyBudget`, `message`) are collected.
3. **Right to Erasure / Summary**: Prospects can request full erasure of their stored lead record by emailing `privacy@vrajvithalani.com` or via WhatsApp. The `/iamadmin/leads` panel supports instant hard deletion of lead records upon request.

### 5.2 Cookie-Less & Privacy-First Analytics Strategy
* **Zero First-Party Tracking Cookies Required**: Core site functionality, navigation, and local performance operate without dropping tracking cookies on human visitors.
* **Dynamic Script Injection Controls**: Analytics pixels (GA4, GTM, Meta Pixel) injected via `/iamadmin/settings/` respect client browser `Do Not Track` (DNT) headers and execute strictly after interactive hydration (`strategy="afterInteractive"`).

### 5.3 Automated Data Retention Policy
* **Active Pipeline Leads**: Stored in MongoDB Atlas with encrypted TLS 1.3 connections in-transit and AES-256 at-rest.
* **Archived Lead Purge**: Inbound contact records marked as `archived` are automatically purged from the MongoDB database after **180 days** via a scheduled cron job / Mongoose TTL index option.

---

## 6. Environment & Secret Key Security

| Secret Key Name | Storage Location | Scope | Security Policy |
| :--- | :--- | :--- | :--- |
| `MONGODB_URI` | Server Environment (`.env.local`) | Server Only | TLS 1.3 connection string with IP whitelist restricted to Render/Vercel static outbound IPs. |
| `JWT_SECRET` | Server Environment (`.env.local`) | Server Only | Cryptographic secret (64+ chars). Never exposed to client-side JS. |
| `CLOUDINARY_API_SECRET` | Server Environment (`.env.local`) | Server Only | Write access key for Cloudinary API. Kept strictly on server endpoints. |
| `NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME` | Client Environment (`.env.local`) | Client & Server | Public Cloudinary cloud identifier for rendering CDN image URLs. |
| `NEXT_PUBLIC_SITE_URL` | Client Environment (`.env.local`) | Client & Server | Canonical site URL (`https://vrajvithalani.com`). |

---

## 7. Security Implementation Verification Checklist

- [x] Invisible Honeypot field (`website_url`) implemented with silent HTTP 200 rejection.
- [x] Administrative authentication secured via HTTP-Only, `SameSite=Strict`, `Secure` cookies.
- [x] `tokenVersion` counter added to `AdminUser` model for instant session invalidation.
- [x] Strict CORS policy applied to `/api/*` endpoints.
- [x] Mongoose schemas configured with `strict: true` and NoSQL injection defenses.
- [x] Full suite of Security Headers (`CSP`, `HSTS`, `X-Frame-Options`) configured in `next.config.ts`.
- [x] Lead submission form DPDP Act 2023 purpose notice integrated.
- [x] Admin hard-delete functionality operational for user data removal requests.
- [x] Database credentials and API secrets stored exclusively in server environment variables.
