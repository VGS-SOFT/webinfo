# 02 API Documentation & Integration Specification

This document provides the OpenAPI-style REST API specifications for all public and administrative endpoints on **vrajvithalani.com**, built on the Next.js 15 App Router (`app/api/`).

---

## 1. Global API Standards

* **Base URL**: `https://vrajvithalani.com/api` (Production) / `http://localhost:3000/api` (Local)
* **Content Type**: `application/json` (except media upload endpoints using `multipart/form-data`)
* **Authentication**: HTTP-Only Secure Cookies (`vraj_admin_token`) signed with JWT (`HS256`)
* **Standard Error Response Format**:
  ```json
  {
    "success": false,
    "error": {
      "code": "INVALID_INPUT",
      "message": "Detailed human-readable error explanation",
      "details": []
    }
  }
  ```

---

## 2. Public Endpoints

### 2.1 Lead Capture & Contact Form
* **Endpoint**: `POST /api/contact`
* **Purpose**: Ingests prospect inquiries from `/contact/` and contextual CTA blocks across service/case study pages.
* **Authentication**: None (Public)
* **Rate Limit**: 5 requests per IP per 15 minutes (Memory/Redis backed)
* **Security Constraints**: Honeypot field validation (`website_url` must remain empty). If populated, returns `200 OK` with a fake success response to mute bot retries without storing data.

#### Request Headers
```http
POST /api/contact HTTP/1.1
Host: vrajvithalani.com
Content-Type: application/json
```

#### Request Payload
```json
{
  "fullName": "Bhavik Patel",
  "email": "bhavik@powercable.in",
  "phone": "+91 98765 43210",
  "service": "Google Ads Management",
  "monthlyBudget": "₹15,000 - ₹30,000",
  "message": "We need to scale our B2B electrical cable export inquiries.",
  "pageSource": "/services/google-ads/",
  "website_url": ""
}
```

#### Field Schema & Validation Rules
| Field | Type | Required | Validation / Sanitization |
| :--- | :--- | :---: | :--- |
| `fullName` | String | **Yes** | Trimmed, 2–100 chars, XSS sanitized. |
| `email` | String | **Yes** | Valid RFC 5322 email regex, lowercased. |
| `phone` | String | No | Trimmed, 10–15 digits optional format. |
| `service` | String | **Yes** | Enum: `Google Ads Management`, `Technical SEO & GEO`, `Custom Next.js & Hardened WordPress`, `CRO & Conversion Rate Optimization`, `Other / Advisory`. |
| `monthlyBudget` | String | No | Enum: `< ₹15,000`, `₹15,000 - ₹30,000`, `₹30,000 - ₹75,000`, `> ₹75,000`. |
| `message` | String | **Yes** | Trimmed, 10–2000 chars, HTML/script tags stripped. |
| `pageSource` | String | No | Originating URL route. |
| `website_url` | String | **Yes** | **Honeypot field.** Must be empty. |

#### Success Response (`201 Created`)
```json
{
  "success": true,
  "message": "Your message has been received. Vraj Vithalani will respond within 24 hours.",
  "leadId": "65f2c8d1e4b0a1a2b3c4d5e6"
}
```

#### Honeypot Bot Trap Response (`200 OK`)
```json
{
  "success": true,
  "message": "Your message has been received. Vraj Vithalani will respond within 24 hours."
}
```

---

## 3. Administrative Authentication Endpoints

### 3.1 Admin Login
* **Endpoint**: `POST /api/admin/auth/login`
* **Purpose**: Authenticates superadmin into `/iamadmin/` and sets HTTP-Only JWT cookie.
* **Authentication**: None (Public authentication endpoint)

#### Request Payload
```json
{
  "username": "admin",
  "password": "SuperSecretPassword123!"
}
```

#### Success Response (`200 OK`)
* **Headers**: `Set-Cookie: vraj_admin_token=<JWT>; Path=/; HttpOnly; Secure; SameSite=Strict; Max-Age=86400`
```json
{
  "success": true,
  "user": {
    "id": "65f1a2b3c4d5e6f7a8b9c0d1",
    "username": "vraj_admin",
    "role": "superadmin"
  }
}
```

### 3.2 Admin Logout
* **Endpoint**: `POST /api/admin/auth/logout`
* **Purpose**: Invalidates session cookie and clears local JWT cookie.

#### Success Response (`200 OK`)
* **Headers**: `Set-Cookie: vraj_admin_token=; Path=/; HttpOnly; Secure; SameSite=Strict; Max-Age=0`
```json
{
  "success": true,
  "message": "Successfully logged out."
}
```

---

## 4. Administrative Management Endpoints (Protected)

All endpoints in Section 4 require a valid `vraj_admin_token` cookie. Unauthorized requests return `401 Unauthorized`.

### 4.1 Get Lead Submissions
* **Endpoint**: `GET /api/admin/leads`
* **Query Parameters**:
  * `status`: `new` | `contacted` | `in_pipeline` | `closed` | `archived` | `all` (default: `all`)
  * `page`: Number (default: `1`)
  * `limit`: Number (default: `20`)
  * `search`: String (searches `fullName`, `email`, `message`)

#### Success Response (`200 OK`)
```json
{
  "success": true,
  "data": {
    "leads": [
      {
        "_id": "65f2c8d1e4b0a1a2b3c4d5e6",
        "fullName": "Bhavik Patel",
        "email": "bhavik@powercable.in",
        "phone": "+91 98765 43210",
        "service": "Google Ads Management",
        "monthlyBudget": "₹15,000 - ₹30,000",
        "message": "We need to scale our B2B electrical cable export inquiries.",
        "pageSource": "/services/google-ads/",
        "status": "new",
        "notes": [],
        "createdAt": "2026-09-21T10:15:30.000Z"
      }
    ],
    "pagination": {
      "total": 42,
      "page": 1,
      "pages": 3,
      "limit": 20
    }
  }
}
```

### 4.2 Update Lead Status & Add Notes
* **Endpoint**: `PATCH /api/admin/leads/[id]`

#### Request Payload
```json
{
  "status": "in_pipeline",
  "note": "Spoke on phone. Sending proposal for ₹25K/mo management fee."
}
```

#### Success Response (`200 OK`)
```json
{
  "success": true,
  "data": {
    "_id": "65f2c8d1e4b0a1a2b3c4d5e6",
    "status": "in_pipeline",
    "notes": [
      {
        "text": "Spoke on phone. Sending proposal for ₹25K/mo management fee.",
        "createdAt": "2026-09-21T11:00:00.000Z"
      }
    ],
    "updatedAt": "2026-09-21T11:00:00.000Z"
  }
}
```

---

### 4.3 Analytics & Script Manager Settings

#### Get Current Settings
* **Endpoint**: `GET /api/admin/settings`

#### Success Response (`200 OK`)
```json
{
  "success": true,
  "data": {
    "gtmContainerId": "GTM-XXXXXXX",
    "ga4MeasurementId": "G-XXXXXXXXXX",
    "metaPixelId": "123456789012345",
    "customHeadScripts": "<!-- Hotjar Tracking Code -->",
    "customBodyScripts": "<!-- Custom Conversion Tag -->",
    "updatedAt": "2026-09-21T09:00:00.000Z"
  }
}
```

#### Update Analytics Settings
* **Endpoint**: `PUT /api/admin/settings`

#### Request Payload
```json
{
  "gtmContainerId": "GTM-VRAJ123",
  "ga4MeasurementId": "G-VRAJ45678",
  "metaPixelId": "987654321098765",
  "customHeadScripts": "<script>console.log('Tracking Active');</script>",
  "customBodyScripts": ""
}
```

#### Success Response (`200 OK`)
```json
{
  "success": true,
  "message": "Analytics tracking settings updated successfully.",
  "data": {
    "gtmContainerId": "GTM-VRAJ123",
    "ga4MeasurementId": "G-VRAJ45678",
    "metaPixelId": "987654321098765",
    "customHeadScripts": "<script>console.log('Tracking Active');</script>",
    "customBodyScripts": "",
    "updatedAt": "2026-09-21T12:00:00.000Z"
  }
}
```

---

### 4.4 Cloudinary Media Manager API

#### Upload Media File
* **Endpoint**: `POST /api/admin/media`
* **Content-Type**: `multipart/form-data`

#### Form Data Parameters
* `file`: Binary image/document file (PNG, JPG, WEBP, SVG, PDF up to 10MB)
* `folder`: Target Cloudinary directory (`blog`, `case-studies`, `branding`, `general`)
* `altText`: Mandatory accessibility description string

#### Success Response (`201 Created`)
```json
{
  "success": true,
  "data": {
    "publicId": "vrajvithalani/blog/seo-audit-dashboard",
    "secureUrl": "https://res.cloudinary.com/vrajvithalani/image/upload/v1726912345/vrajvithalani/blog/seo-audit-dashboard.webp",
    "format": "webp",
    "width": 1200,
    "height": 630,
    "bytes": 84210,
    "altText": "Technical SEO audit dashboard showing 100/100 Core Web Vitals",
    "markdownCode": "![Technical SEO audit dashboard showing 100/100 Core Web Vitals](https://res.cloudinary.com/vrajvithalani/image/upload/v1726912345/vrajvithalani/blog/seo-audit-dashboard.webp)"
  }
}
```

#### Fetch Media Gallery
* **Endpoint**: `GET /api/admin/media`
* **Query Parameters**: `folder` (optional), `page`, `limit`

#### Delete Media Asset
* **Endpoint**: `DELETE /api/admin/media/[publicId]`

#### Success Response (`200 OK`)
```json
{
  "success": true,
  "message": "Asset deleted successfully from Cloudinary and media registry."
}
```

---

## 5. HTTP Response Code Summary

| Code | Status | Meaning |
| :--- | :--- | :--- |
| **200** | OK | Request succeeded. Returns requested payload. |
| **201** | Created | Resource successfully created (Lead or Media upload). |
| **400** | Bad Request | Validation failure or malformed JSON payload. |
| **401** | Unauthorized | Missing or invalid JWT session cookie. |
| **403** | Forbidden | Insufficient user role permissions. |
| **429** | Too Many Requests | Rate limit threshold exceeded. |
| **500** | Internal Server Error | Unhandled server exception or database error. |
