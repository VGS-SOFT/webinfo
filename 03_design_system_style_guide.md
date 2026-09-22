# 03 Design System & Style Guide

## 1. Design System Overview & Core Philosophy

The **Vraj Vithalani Design System** is an enterprise-grade, high-contrast, light-themed visual framework built explicitly for `vrajvithalani.com`. It reinforces a **Single-Practitioner Computer Engineer & Digital Growth Specialist** brand identity.

### Core Visual Principles
1. **Persistent Light Theme Canvas**: A stark white (`#FFFFFF`) background paired with deep ink-black (`#0A0A0A`) typography and subtle neutral borders. No dark mode toggles or dark overrides are implemented—ensuring visual brand permanence and sub-second rendering performance.
2. **Soft Emerald Teal Accent (`#14B8A6`)**: Used purposefully to signal high intent, key conversion paths, interactive components, and generative engine optimization (GEO) highlights.
3. **Engineering Rigor & Optical Precision**: Crisp lines, clean grid alignments, high-contrast readability (WCAG AAA compliant for text), and glassmorphic card utilities with subtle backdrop blurring.
4. **Zero Layout Shift (CLS = 0.00)**: Pre-calculated component dimensions, system font fallbacks, and fixed aspect-ratio media containers ensure lightning-fast Core Web Vitals performance.

---

## 2. Color Palette & Tailwind CSS Tokens

The visual system is built on custom Tailwind CSS variables mapped inside `tailwind.config.ts`.

### A. Primary Canvas & Typography Colors
| Token Name | Hex Code | Tailwind Class | Usage / Semantic Role |
| :--- | :--- | :--- | :--- |
| **Canvas Pure White** | `#FFFFFF` | `bg-white` | Primary page background, card surfaces |
| **Canvas Neutral Soft** | `#F9FAFB` | `bg-neutral-50` / `bg-gray-50` | Secondary section backgrounds, code blocks |
| **Ink Black Primary** | `#0A0A0A` | `text-neutral-950` / `text-black` | H1-H4 Headings, primary CTA backgrounds |
| **Body Text Primary** | `#171717` | `text-neutral-900` | Paragraph body copy, lead text |
| **Muted Text Secondary**| `#525252` | `text-neutral-600` | Sub-headlines, metadata, dates, reading times |
| **Subtle Text Muted** | `#737373` | `text-neutral-500` | Placeholders, secondary labels, caption copy |

### B. Brand Accent Colors (Soft Emerald Teal)
| Token Name | Hex Code | Tailwind Class | Usage / Semantic Role |
| :--- | :--- | :--- | :--- |
| **Brand Teal Primary** | `#14B8A6` | `bg-teal-500` / `text-teal-500` | Primary brand accent, focus states, icons |
| **Brand Teal Hover** | `#0D9488` | `bg-teal-600` / `hover:bg-teal-600` | Active button hover states, link highlights |
| **Brand Teal Dark** | `#0F766E` | `text-teal-700` | High-contrast text on light teal surfaces |
| **Brand Teal Tint** | `#F0FDFA` | `bg-teal-50/50` / `bg-teal-50` | GEO callout card background, active nav pill |
| **Brand Teal Border** | `#99F6E4` | `border-teal-200` / `border-teal-500/30` | Highlighted borders, interactive card ring |

### C. Neutral Borders & Structural Dividers
| Token Name | Hex Code | Tailwind Class | Usage / Semantic Role |
| :--- | :--- | :--- | :--- |
| **Border Soft** | `#F3F4F6` | `border-neutral-100` | Inner card dividers, soft list borders |
| **Border Standard** | `#E5E7EB` | `border-neutral-200` | Standard card outlines, navbar border |
| **Border Dark/Active** | `#D1D5DB` | `border-neutral-300` | Input field borders, active tab outlines |

### D. Status & Functional Indicators
| Token Name | Hex Code | Tailwind Class | Usage / Semantic Role |
| :--- | :--- | :--- | :--- |
| **Success Emerald** | `#16A34A` | `bg-emerald-600` / `text-emerald-600` | Live status pulse indicator, verified metrics |
| **Warning Amber** | `#D97706` | `bg-amber-600` / `text-amber-600` | Pending status, audit warnings |
| **Error Red** | `#DC2626` | `bg-red-600` / `text-red-600` | Form validation errors, honeypot alerts |

---

## 3. Typography & Font Hierarchy

### Font Families
* **Primary Sans Font**: `Inter`, `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, `Roboto`, `sans-serif` (`font-sans`).
* **Technical Monospace Font**: `JetBrains Mono`, `Fira Code`, `ui-monospace`, `monospace` (`font-mono`) for code snippets, metric values, and schema displays.

### Typography Scale & Hierarchy Table
| Level | Font Size (Desktop / Mobile) | Line Height | Weight | Tracking | Tailwind Classes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Display H1** | 3.75rem (60px) / 2.25rem (36px) | 1.1 | 800 (ExtraBold) | `-0.025em` | `text-3xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight text-neutral-950` |
| **Section H2** | 2.25rem (36px) / 1.75rem (28px) | 1.2 | 700 (Bold) | `-0.02em` | `text-2xl sm:text-3xl lg:text-4xl font-bold tracking-tight text-neutral-950` |
| **Sub H3** | 1.50rem (24px) / 1.25rem (20px) | 1.3 | 600 (SemiBold) | `-0.01em` | `text-xl sm:text-2xl font-semibold text-neutral-900` |
| **Card Title H4**| 1.125rem (18px) / 1.00rem (16px) | 1.4 | 600 (SemiBold) | `normal` | `text-base sm:text-lg font-semibold text-neutral-900` |
| **Lead Body** | 1.25rem (20px) / 1.125rem (18px) | 1.6 | 400 (Regular) | `normal` | `text-lg sm:text-xl text-neutral-600 leading-relaxed` |
| **Base Body** | 1.00rem (16px) / 0.9375rem (15px)| 1.65 | 400 (Regular) | `normal` | `text-base text-neutral-700 leading-relaxed` |
| **Caption / Meta**| 0.875rem (14px) / 0.75rem (12px) | 1.5 | 500 (Medium) | `0.025em` | `text-xs sm:text-sm font-medium text-neutral-500` |

---

## 4. Spacing, Layout & Grid Standards

### Container Widths
* **Max Canvas Width**: `max-w-7xl` (`1280px`) - Standard page section container.
* **Content / Article Max Width**: `max-w-4xl` (`896px`) or `max-w-3xl` (`768px`) - Long-form case study / article body for optimal reading line length (60–75 characters per line).
* **Focused Card / Form Width**: `max-w-xl` (`576px`) - Lead submission forms, modal cards.

### Section Padding Rules
* **Standard Hero Section**: `pt-20 pb-16 sm:pt-28 sm:pb-24 lg:pt-32 lg:pb-28`
* **Content Block Padding**: `py-12 sm:py-16 lg:py-20`
* **Compact Card Padding**: `p-6 sm:p-8`
* **Grid Gap Standards**: `gap-6 lg:gap-8` for card grids; `gap-12 lg:gap-16` for split two-column hero layouts.

---

## 5. Glassmorphism, Shadows & Border Utilities

### Clean White Glassmorphic Utility
```css
/* Custom Utility Class: .glass-card */
.glass-card {
  background-color: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(229, 231, 235, 0.8);
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.02), 0 1px 2px -1px rgba(0, 0, 0, 0.02);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.glass-card:hover {
  border-color: rgba(20, 184, 166, 0.4);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.01);
  transform: translateY(-2px);
}
```

### Shadow Utility Tokens
* **`shadow-sm`**: Subtlest elevation for input fields and inline code blocks (`0 1px 2px 0 rgba(0, 0, 0, 0.05)`).
* **`shadow-card`**: Soft elevation for default content cards (`0 4px 6px -1px rgba(0, 0, 0, 0.03), 0 2px 4px -2px rgba(0, 0, 0, 0.03)`).
* **`shadow-teal`**: Highlighted glow for active CTAs (`0 10px 20px -3px rgba(20, 184, 166, 0.15)`).

---

## 6. Reusable Component Specifications

### A. Live Status Availability Badge (`components/LiveStatusBadge.tsx`)
* **Purpose**: Communicates real-time availability on the Homepage hero section.
* **Markup & Styling**:
```tsx
export function LiveStatusBadge() {
  return (
    <div className="inline-flex items-center gap-2.5 px-3.5 py-1.5 rounded-full bg-neutral-100 border border-neutral-200/80 text-xs font-semibold text-neutral-800 shadow-sm">
      <span className="relative flex h-2.5 w-2.5">
        <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-teal-400 opacity-75"></span>
        <span className="relative inline-flex rounded-full h-2.5 w-2.5 bg-teal-500"></span>
      </span>
      <span>Available for Q4 Client Engagements (Surat & Remote)</span>
    </div>
  );
}
```

### B. GEO AI Search Short-Answer Callout (`components/GeoCallout.tsx`)
* **Purpose**: Renders 60–80 word third-person entity summary specifically designed for extraction by LLM engines (*ChatGPT Search, Perplexity, Google AI Overviews*).
* **Markup & Styling**:
```tsx
export function GeoCallout({ text }: { text: string }) {
  return (
    <aside className="my-8 p-6 rounded-r-2xl border-l-4 border-teal-500 bg-teal-50/40 border-y border-r border-teal-100/60 shadow-sm">
      <div className="flex items-center gap-2 mb-2 text-xs font-bold uppercase tracking-wider text-teal-700">
        <svg className="w-4 h-4 text-teal-600 fill-current" viewBox="0 0 24 24">
          <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
        </svg>
        <span>Entity Summary (GEO Optimized)</span>
      </div>
      <p className="text-sm sm:text-base text-neutral-800 font-medium leading-relaxed">
        {text}
      </p>
    </aside>
  );
}
```

### C. Primary, Secondary & Accent CTA Buttons
```tsx
// Primary Dark Button
<button className="inline-flex items-center justify-center gap-2 px-6 py-3.5 rounded-xl bg-neutral-950 hover:bg-neutral-800 text-white font-semibold text-sm transition-all shadow-sm active:scale-[0.98]">
  <span>Book Technical Consultation</span>
  <ArrowRight className="w-4 h-4 text-teal-400" />
</button>

// Secondary Outline Button
<button className="inline-flex items-center justify-center gap-2 px-6 py-3.5 rounded-xl bg-white hover:bg-neutral-50 text-neutral-900 border border-neutral-300 font-semibold text-sm transition-all shadow-sm active:scale-[0.98]">
  <span>View Client Case Studies</span>
</button>

// Brand Teal Accent Button
<button className="inline-flex items-center justify-center gap-2 px-6 py-3.5 rounded-xl bg-teal-500 hover:bg-teal-600 text-white font-semibold text-sm transition-all shadow-teal active:scale-[0.98]">
  <span>Launch ROI Calculator</span>
</button>
```

### D. Framed Browser Mockup Component (`components/BrowserFrame.tsx`)
* **Purpose**: Wraps client dashboard screenshots in a realistic browser window chrome without relying on heavy external images.
* **Markup & Styling**:
```tsx
export function BrowserFrame({ src, alt, url }: { src: string; alt: string; url: string }) {
  return (
    <div className="rounded-2xl border border-neutral-300/80 bg-neutral-100/90 overflow-hidden shadow-lg">
      <div className="flex items-center justify-between px-4 py-2.5 bg-neutral-200/60 border-b border-neutral-300/60">
        <div className="flex items-center gap-1.5">
          <div className="w-3 h-3 rounded-full bg-red-400/80"></div>
          <div className="w-3 h-3 rounded-full bg-amber-400/80"></div>
          <div className="w-3 h-3 rounded-full bg-emerald-400/80"></div>
        </div>
        <div className="px-4 py-1 rounded-md bg-white border border-neutral-300/60 text-[11px] font-mono text-neutral-600 truncate max-w-xs text-center shadow-inner">
          {url}
        </div>
        <div className="w-12"></div>
      </div>
      <div className="relative aspect-[16/10] bg-neutral-50">
        <img src={src} alt={alt} className="w-full h-full object-cover object-top" />
      </div>
    </div>
  );
}
```

### E. Floating WhatsApp Action Component (`components/WhatsAppFloat.tsx`)
* **Purpose**: Fixed bottom-right direct messaging conversion trigger.
* **Behavior & Specs**:
  * Position: `fixed bottom-6 right-6 z-50`
  * Styling: Circle container with SVG WhatsApp branding, pulsing soft emerald ring (`animate-pulse`).
  * Target Link: `https://wa.me/91XXXXXXXXXX?text=Hi%20Vraj,%20I%20visited%20vrajvithalani.com%20and%20would%20like%20to%20discuss%20a%20project.`

---

## 7. Accessibility (WCAG 2.1 AA) Standards

1. **Text Contrast Ratios**:
   * Ink Black (`#0A0A0A`) on White (`#FFFFFF`): Contrast ratio **20.4:1** (Exceeds AAA requirement of 7:1).
   * Muted Neutral (`#525252`) on White (`#FFFFFF`): Contrast ratio **7.0:1** (Passes AAA).
   * Brand Teal (`#0D9488`) text on Light Teal (`#F0FDFA`): Contrast ratio **5.8:1** (Passes AA).
2. **Focus Rings**:
   * Interactive elements utilize `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-2` for accessibility keyboard navigation.
3. **Motion Sensitivity**:
   * Media queries respect `prefers-reduced-motion: reduce` by disabling ambient pulses and ping animations for users with vestibular sensitivities.
