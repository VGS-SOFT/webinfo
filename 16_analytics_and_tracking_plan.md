# 16 Analytics, Tracking & Tag Management Plan

## 1. Analytics & Tracking Architecture Overview

The Vraj Vithalani website platform employs a unified, privacy-compliant, zero-latency-impact tracking architecture. Tracking scripts are dynamically injected at runtime via Next.js `Script` components (`strategy="afterInteractive"`) reading from the administrative `Settings` collection in MongoDBAtlas.

### Core Tracking Infrastructure

| Platform | Primary Function | Identifier / Container Format | Injection Strategy |
| :--- | :--- | :--- | :--- |
| **Google Tag Manager (GTM)** | Central tag orchestration & event dispatch | `GTM-XXXXXXX` | Head script + Body noscript iframe |
| **Google Analytics 4 (GA4)** | Server-side & client-side behavioral telemetry | `G-XXXXXXXXXX` | GTM container tag or direct `gtag.js` |
| **Meta Pixel (Facebook)** | Campaign conversion tracking & audience remarketing | `123456789012345` | GTM custom HTML / Meta pixel snippet |
| **Microsoft Clarity** | Free visual heatmaps, scroll maps & session recording | `cl_id_xxxxxx` | Dynamic `<script>` injection |
| **Custom Head / Body Scripts** | Custom tracking scripts (LinkedIn Insight, Twitter Pixel, etc.) | Raw HTML / JS strings | Database singleton string parsing |

---

## 2. GA4 Event Taxonomy & Custom Dimensions

All custom conversion and engagement interactions push structured data objects to `window.dataLayer`.

### Event Taxonomy Matrix

| Event Name | Trigger Condition | Data Layer Parameters | Conversion Flag |
| :--- | :--- | :--- | :--- |
| `page_view` | Next.js route change | `page_title`, `page_location`, `page_path` | No |
| `lead_form_start` | First user focus in lead form | `form_id="contact_form"`, `service_preselected` | No |
| `lead_form_submit` | Valid `/api/contact` 200 response | `form_id="contact_form"`, `service_category`, `budget_bracket`, `lead_id` | **Yes (Primary)** |
| `roi_calculator_interact` | Slider adjustment on Ads Calculator | `monthly_budget_inr`, `estimated_leads`, `calculated_roas` | No |
| `roi_calculator_cta` | Click "Claim This Campaign Setup" | `monthly_budget_inr`, `estimated_leads`, `target_service="google-ads"` | **Yes (Secondary)** |
| `whatsapp_click` | Floating or inline WhatsApp click | `click_location` (`floating_badge`, `hero_cta`, `footer`), `page_path` | **Yes (Secondary)** |
| `geo_callout_copy` | Text selection & copy in GEO box | `geo_topic`, `copied_text_snippet` | No |
| `case_study_click` | Card click in portfolio | `case_study_slug`, `client_name` | No |
| `phone_click` | Direct `tel:` link click | `phone_number`, `page_path` | **Yes (Secondary)** |
| `email_click` | Direct `mailto:` link click | `email_address`, `page_path` | **Yes (Secondary)** |

### Custom Dimensions & User Properties Matrix

```typescript
// Registered Custom Dimensions in GA4 Admin Interface
interface CustomDimensions {
  dimension1: 'service_category';    // e.g., 'google-ads', 'seo', 'web-dev'
  dimension2: 'budget_bracket';      // e.g., '50k_1L', '1L_3L', '3L_plus'
  dimension3: 'page_type';           // e.g., 'pillar', 'location', 'case_study'
  dimension4: 'geo_location_target'; // e.g., 'surat-adajan', 'ahmedabad-sg-highway'
  dimension5: 'lead_status';          // e.g., 'new', 'qualified'
}
```

---

## 3. DataLayer Ingestion Specifications

### A. Inbound Lead Form Submission (`lead_form_submit`)

```javascript
// Pushed upon receiving HTTP 200 from /api/contact
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: 'lead_form_submit',
  event_id: 'lead_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9),
  form_id: 'public_contact_form',
  service_category: formData.service, // 'google-ads' | 'seo' | 'web-development' | 'cro-and-automation'
  budget_bracket: formData.monthlyBudget, // 'below_25k' | '25k_50k' | '50k_100k' | '100k_plus'
  page_source: window.location.pathname,
  user_data: {
    sha256_email_address: hashEmail(formData.email), // Encrypted for Meta / GA4 Enhanced Conversions
    sha256_phone_number: hashPhone(formData.phone)
  }
});
```

### B. Google Ads ROI Simulator Interaction (`roi_calculator_interact`)

```javascript
// Debounced push (500ms) during slider adjustment
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: 'roi_calculator_interact',
  monthly_budget_inr: budgetValue,
  estimated_clicks: calculatedClicks,
  estimated_leads: calculatedLeads,
  estimated_revenue_inr: calculatedRevenue
});
```

### C. Floating WhatsApp Badge Click (`whatsapp_click`)

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: 'whatsapp_click',
  click_location: 'floating_action_badge',
  page_path: window.location.pathname,
  destination_url: whatsappUrl
});
```

---

## 4. Meta Pixel & Conversion API (CAPI) Mapping

### Event Mapping Rules

| Website Action | Meta Pixel Standard Event | Custom Parameters |
| :--- | :--- | :--- |
| Any Page Visit | `fbq('track', 'PageView');` | None |
| Valid Lead Form Submit | `fbq('track', 'Lead', {...});` | `content_name: service`, `value: estimated_value`, `currency: 'INR'` |
| WhatsApp Conversion | `fbq('track', 'Contact', {...});` | `content_category: 'whatsapp'`, `page_path: url` |
| ROI Calculator CTA | `fbq('trackCustom', 'RoiCalculatorConversion', {...});` | `monthly_budget: budget` |

---

## 5. Administrative Dynamic Script Injection Engine

Tracking scripts are governed dynamically without site redeployments via the `/iamadmin/settings/` portal.

### Implementation Pattern (`components/analytics/DynamicScriptInjector.tsx`)

```tsx
import Script from 'next/script';

interface AnalyticsSettings {
  gtmContainerId?: string;
  ga4MeasurementId?: string;
  metaPixelId?: string;
  customHeadScripts?: string;
  customBodyScripts?: string;
}

export function DynamicScriptInjector({ settings }: { settings: AnalyticsSettings }) {
  return (
    <>
      {/* Google Tag Manager - Primary */}
      {settings.gtmContainerId && (
        <Script
          id="gtm-script"
          strategy="afterInteractive"
          dangerouslySetInnerHTML={{
            __html: `
              (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
              new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
              j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
              'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
              })(window,document,'script','dataLayer','${settings.gtmContainerId}');
            `,
          }}
        />
      )}

      {/* GA4 Direct Snippet (Fallback if GTM isn't used) */}
      {settings.ga4MeasurementId && !settings.gtmContainerId && (
        <>
          <Script
            src={`https://www.googletagmanager.com/gtag/js?id=${settings.ga4MeasurementId}`}
            strategy="afterInteractive"
          />
          <Script
            id="ga4-script"
            strategy="afterInteractive"
            dangerouslySetInnerHTML={{
              __html: `
                window.dataLayer = window.dataLayer || [];
                function gtag(){dataLayer.push(arguments);}
                gtag('js', new Date());
                gtag('config', '${settings.ga4MeasurementId}', {
                  page_path: window.location.pathname,
                });
              `,
            }}
          />
        </>
      )}

      {/* Meta Pixel Snippet */}
      {settings.metaPixelId && (
        <Script
          id="meta-pixel"
          strategy="afterInteractive"
          dangerouslySetInnerHTML={{
            __html: `
              !function(f,b,e,v,n,t,s)
              {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
              n.callMethod.apply(n,arguments):n.queue.push(arguments)};
              if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
              n.queue=[];t=b.createElement(e);t.async=!0;
              t.src=v;s=b.getElementsByTagName(e)[0];
              s.parentNode.insertBefore(t,s)}(window, document,'script',
              'https://connect.facebook.net/en_US/fbevents.js');
              fbq('init', '${settings.metaPixelId}');
              fbq('track', 'PageView');
            `,
          }}
        />
      )}

      {/* Custom Head Scripts Injection */}
      {settings.customHeadScripts && (
        <Script
          id="custom-head-scripts"
          strategy="afterInteractive"
          dangerouslySetInnerHTML={{ __html: settings.customHeadScripts }}
        />
      )}
    </>
  );
}
```

---

## 6. Anti-Double Firing & Deduplication Strategy

1. **Client-Side Event ID Generation**:
   Every custom lead event generates a unique UUID `event_id` (`lead_timestamp_random`).
2. **Session Storage Deduplication**:
   ```typescript
   if (sessionStorage.getItem('lead_submitted_' + leadId)) {
     return; // Prevent duplicate event push on page refresh
   }
   sessionStorage.setItem('lead_submitted_' + leadId, 'true');
   ```
3. **Meta Pixel & GA4 Transaction Deduplication**:
   Sending the same `event_id` in both client-side Pixel and server-side CAPI enables Meta to automatically deduplicate events within a 48-hour window.

---

## 7. QA & Analytics Verification Checklist

- [ ] **GTM Preview Mode Test**: Verify dataLayer variables populate correctly on lead form submit.
- [ ] **GA4 DebugView**: Confirm `lead_form_submit` event fires with `service_category` and `budget_bracket`.
- [ ] **Meta Pixel Helper Extension**: Verify `Lead` and `PageView` events fire cleanly without warnings.
- [ ] **Core Web Vitals Impact**: Confirm tracking script injection (`afterInteractive`) adds zero blocking time to LCP/INP metrics.
- [ ] **Do Not Track (DNT) Verification**: Confirm scripts respect user browser privacy preferences when enforced.
