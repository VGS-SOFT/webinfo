# 01 Database Schema & Entity Relationship Diagram (ERD)

**Document Version:** 2.0.0  
**Target Platform:** Next.js 15 App Router + MongoDB Atlas + Mongoose 8.x  
**Database Name:** `vrajvithalani_prod`  
**Scope:** V2 Production Launch (50-Page Architecture)  
**Date:** September 2026  

---

## 1. Database Architecture & Singleton Connection Pattern

The database layer for `vrajvithalani.com` is hosted on **MongoDB Atlas** (M0/M10 Cluster) using **Mongoose 8.x**. Because Next.js 15 uses serverless/edge route handlers, database connections must use a **cached singleton pattern** to prevent connection exhaustion during traffic spikes.

### Connection Singleton Strategy (`lib/db.ts`)
* **Connection Reuse:** Caches `mongoose.connection` on the global object in development and serverless instances.
* **Timeout Configurations:** `bufferCommands: false`, `serverSelectionTimeoutMS: 5000`.
* **URI Environment Variable:** `MONGODB_URI` (Stored securely in `.env.local`).

---

## 2. Entity Relationship Diagram (ERD)

```
+-------------------------------------------------------------------------+
|                              MONGODB ATLAS                              |
+-------------------------------------------------------------------------+
                                     |
    +--------------------------------+--------------------------------+
    |                                |                                |
    v                                v                                v
+-----------------------+  +-----------------------+  +-----------------------+
|        Contact        |  |       Settings        |  |         Media         |
+-----------------------+  +-----------------------+  +-----------------------+
| _id: ObjectId         |  | _id: ObjectId         |  | _id: ObjectId         |
| name: String          |  | gtmContainerId: Str   |  | publicId: String      |
| email: String         |  | ga4MeasurementId: Str |  | secureUrl: String     |
| phone: String         |  | metaPixelId: String   |  | format: String        |
| service: String       |  | customHeadScripts: Str|  | width: Number         |
| budget: String        |  | customBodyScripts: Str|  | height: Number        |
| message: String       |  | updatedBy: ObjectId   |  | bytes: Number         |
| honeypot: String      |  | updatedAt: Date       |  | folder: String        |
| status: Enum          |  +-----------------------+  | altText: String       |
| notes: [NoteSchema]   |            ^                | caption: String       |
| ipAddress: String     |            |                | tags: [String]        |
| userAgent: String     |            |                | uploadedBy: ObjectId  |
| createdAt: Date       |            |                | createdAt: Date       |
| updatedAt: Date       |            |                +-----------------------+
+-----------------------+            |                            ^
            ^                        |                            |
            |                        |                            |
            +------------------------+----------------------------+
                                     |
                          +--------------------+
                          |     AdminUser      |
                          +--------------------+
                          | _id: ObjectId      |
                          | username: String   |
                          | email: String      |
                          | passwordHash: Str  |
                          | role: Enum         |
                          | tokenVersion: Num  |
                          | lastLogin: Date    |
                          +--------------------+
```

---

## 3. Detailed Mongoose Schema Definitions

### 3.1 Contact / Lead Schema (`models/Contact.ts`)

Stores all inbound inquiries submitted through the contact form, floating WhatsApp triggers, or audit widgets.

```typescript
import mongoose, { Schema, Document, Model } from 'mongoose';

export type LeadStatus = 'new' | 'contacted' | 'in_pipeline' | 'closed' | 'archived';

export interface INote {
  author: string;
  text: string;
  createdAt: Date;
}

export interface IContact extends Document {
  name: string;
  email: string;
  phone: string;
  service: string;
  budget?: string;
  message: string;
  honeypot?: string; // Must remain empty; filled = spam
  status: LeadStatus;
  notes: INote[];
  ipAddress?: string;
  userAgent?: string;
  createdAt: Date;
  updatedAt: Date;
}

const NoteSchema = new Schema<INote>(
  {
    author: { type: String, required: true, trim: true },
    text: { type: String, required: true, trim: true },
    createdAt: { type: Date, default: Date.now },
  },
  { _id: true }
);

const ContactSchema = new Schema<IContact>(
  {
    name: {
      type: String,
      required: [true, 'Name is required'],
      trim: true,
      maxlength: [100, 'Name cannot exceed 100 characters'],
    },
    email: {
      type: String,
      required: [true, 'Email is required'],
      trim: true,
      lowercase: true,
      match: [/^\S+@\S+\.\S+$/, 'Invalid email address'],
    },
    phone: {
      type: String,
      required: [true, 'Phone number is required'],
      trim: true,
      maxlength: [20, 'Phone number too long'],
    },
    service: {
      type: String,
      required: [true, 'Service selection is required'],
      enum: [
        'Google Ads Management',
        'Technical SEO & GEO',
        'Next.js Web Development',
        'CRO & Automation',
        'Full Digital Strategy',
        'Other Inquiry',
      ],
    },
    budget: {
      type: String,
      enum: ['< ₹15,000/mo', '₹15,000 - ₹30,000/mo', '₹30,000 - ₹75,000/mo', '₹75,000+/mo', 'Undisclosed'],
      default: 'Undisclosed',
    },
    message: {
      type: String,
      required: [true, 'Message body is required'],
      trim: true,
      maxlength: [2000, 'Message cannot exceed 2000 characters'],
    },
    honeypot: {
      type: String,
      default: '',
      select: false, // Hidden from standard queries
    },
    status: {
      type: String,
      enum: ['new', 'contacted', 'in_pipeline', 'closed', 'archived'],
      default: 'new',
      index: true,
    },
    notes: [NoteSchema],
    ipAddress: { type: String, trim: true },
    userAgent: { type: String, trim: true },
  },
  {
    timestamps: true,
  }
);

// Indexes for fast searching in Admin Panel (/iamadmin/)
ContactSchema.index({ createdAt: -1 });
ContactSchema.index({ email: 1 });
ContactSchema.index({ name: 'text', message: 'text' });

export const Contact: Model<IContact> =
  mongoose.models.Contact || mongoose.model<IContact>('Contact', ContactSchema);
```

---

### 3.2 Settings Schema (`models/Settings.ts`)

Controls site-wide tracking scripts, analytics IDs, and custom header/body tags injected via the Admin Control Center (`/iamadmin/settings/`).

```typescript
import mongoose, { Schema, Document, Model } from 'mongoose';

export interface ISettings extends Document {
  gtmContainerId?: string;     // e.g. "GTM-XXXXXXX"
  ga4MeasurementId?: string;   // e.g. "G-XXXXXXXXXX"
  metaPixelId?: string;        // e.g. "123456789012345"
  customHeadScripts?: string;  // Custom JS/HTML tags for <head>
  customBodyScripts?: string;  // Custom JS/HTML tags for <body>
  updatedBy?: mongoose.Types.ObjectId;
  updatedAt: Date;
}

const SettingsSchema = new Schema<ISettings>(
  {
    gtmContainerId: { type: String, trim: true, default: '' },
    ga4MeasurementId: { type: String, trim: true, default: '' },
    metaPixelId: { type: String, trim: true, default: '' },
    customHeadScripts: { type: String, default: '' },
    customBodyScripts: { type: String, default: '' },
    updatedBy: { type: Schema.Types.ObjectId, ref: 'AdminUser' },
  },
  {
    timestamps: true,
  }
);

// Singleton Pattern Guard: Ensures only ONE settings document ever exists
SettingsSchema.statics.getSingleton = async function () {
  let settings = await this.findOne();
  if (!settings) {
    settings = await this.create({});
  }
  return settings;
};

export const Settings: Model<ISettings> =
  mongoose.models.Settings || mongoose.model<ISettings>('Settings', SettingsSchema);
```

---

### 3.3 Media Schema (`models/Media.ts`)

Tracks image files uploaded to Cloudinary through the `/iamadmin/media/` dashboard for blog posts (`/learn/`) and case studies.

```typescript
import mongoose, { Schema, Document, Model } from 'mongoose';

export interface IMedia extends Document {
  publicId: string;      // Cloudinary Public ID (e.g., "vrajvithalani/blog/seo-audit-2026")
  secureUrl: string;     // Full HTTPS Cloudinary URL
  format: string;        // webp, png, jpg, svg
  width: number;
  height: number;
  bytes: number;
  resourceType: string;  // image, raw, video
  folder: string;        // /public/images/learn, /public/images/case-studies
  altText: string;       // SEO Alt text string
  caption?: string;      // Optional image caption
  tags: string[];        // Category tags (e.g., ["seo", "google-ads", "case-study"])
  uploadedBy?: mongoose.Types.ObjectId;
  createdAt: Date;
}

const MediaSchema = new Schema<IMedia>(
  {
    publicId: { type: String, required: true, unique: true, index: true },
    secureUrl: { type: String, required: true },
    format: { type: String, required: true },
    width: { type: Number, required: true },
    height: { type: Number, required: true },
    bytes: { type: Number, required: true },
    resourceType: { type: String, default: 'image' },
    folder: { type: String, default: 'vrajvithalani' },
    altText: { type: String, required: true, trim: true },
    caption: { type: String, trim: true, default: '' },
    tags: [{ type: String, trim: true }],
    uploadedBy: { type: Schema.Types.ObjectId, ref: 'AdminUser' },
  },
  {
    timestamps: { createdAt: true, updatedAt: false },
  }
);

MediaSchema.index({ createdAt: -1 });
MediaSchema.index({ tags: 1 });
MediaSchema.index({ folder: 1 });

export const Media: Model<IMedia> =
  mongoose.models.Media || mongoose.model<IMedia>('Media', MediaSchema);
```

---

### 3.4 AdminUser Schema (`models/AdminUser.ts`)

Manages access to the encrypted Admin Control Center (`/iamadmin/`).

```typescript
import mongoose, { Schema, Document, Model } from 'mongoose';

export type AdminRole = 'superadmin' | 'editor';

export interface IAdminUser extends Document {
  username: string;
  email: string;
  passwordHash: string;
  role: AdminRole;
  tokenVersion: number; // Incremented on logout to invalidate active JWTs
  lastLogin?: Date;
  createdAt: Date;
  updatedAt: Date;
}

const AdminUserSchema = new Schema<IAdminUser>(
  {
    username: { type: String, required: true, unique: true, trim: true, lowercase: true },
    email: { type: String, required: true, unique: true, trim: true, lowercase: true },
    passwordHash: { type: String, required: true, select: false },
    role: { type: String, enum: ['superadmin', 'editor'], default: 'superadmin' },
    tokenVersion: { type: Number, default: 0 },
    lastLogin: { type: Date },
  },
  {
    timestamps: true,
  }
);

export const AdminUser: Model<IAdminUser> =
  mongoose.models.AdminUser || mongoose.model<IAdminUser>('AdminUser', AdminUserSchema);
```

---

## 4. Field-Level Validation & Security Rules

| Collection | Field | Validation Constraint | Security / Business Rule |
| :--- | :--- | :--- | :--- |
| `contacts` | `honeypot` | Must be empty string (`""`) | If populated, request is dropped as bot spam with HTTP 200 (silent rejection). |
| `contacts` | `email` | Standard Regex match | Sanitized & lowercased before store. |
| `contacts` | `message` | Max length 2,000 chars | XSS escaped before rendering in `/iamadmin/`. |
| `settings` | `customHeadScripts` | Raw HTML/JS string | Injected via `next/script` (`strategy="afterInteractive"`) inside server boundary. |
| `media` | `altText` | Required string | Enforces web accessibility (WCAG AA) for all uploaded media assets. |
| `adminusers`| `tokenVersion` | Integer | Used for instant session revocation without stateful server sessions. |

---

## 5. Summary Matrix & Indexing Strategy

1. **Compound Index (`contacts`)**: `{ createdAt: -1, status: 1 }` — Drives fast pipeline filtering in `/iamadmin/leads`.
2. **Text Index (`contacts`)**: `{ name: "text", message: "text" }` — Enables instant search across leads.
3. **Unique Index (`media`)**: `{ publicId: 1 }` — Prevents duplicate Cloudinary asset entries.
4. **Unique Indexes (`adminusers`)**: `{ username: 1 }`, `{ email: 1 }` — Prevents duplicate account creation.
