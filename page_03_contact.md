# Page 03: Contact (`/contact/`) — Complete Line-by-Line Content Spec

---

## 1. Meta & Head Specifications

* **Route**: `/contact/`
* **Meta Title**: Contact Vraj Vithalani | Direct Google Ads, SEO & Web Dev Specialist
* **Meta Description**: Get in touch directly with Vraj Vithalani for Google Ads, Technical SEO, Next.js Web Development, and CRO consulting. No sales reps or agency middlemen. Response within 24 hours.
* **Canonical URL**: `https://vrajvithalani.com/contact/`
* **OpenGraph Title**: Contact Vraj Vithalani | Direct Specialist Execution
* **OpenGraph Description**: Connect directly with Vraj Vithalani. Fast 24-hour response SLA for high-growth businesses seeking performance marketing and Next.js engineering.
* **OpenGraph Image**: `https://res.cloudinary.com/vrajvithalani/image/upload/v1/vrajvithalani/production/og_default.webp`
* **OpenGraph Type**: `website`
* **Robots Directives**: `index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1`

---

## 2. Structured Data (JSON-LD `@graph`)

```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "ContactPage",
      "@id": "https://vrajvithalani.com/contact/#webpage",
      "url": "https://vrajvithalani.com/contact/",
      "name": "Contact Vraj Vithalani | Direct Google Ads, SEO & Web Dev Specialist",
      "description": "Get in touch directly with Vraj Vithalani for Google Ads, Technical SEO, Next.js Web Development, and CRO consulting.",
      "isPartOf": {
        "@id": "https://vrajvithalani.com/#website"
      },
      "breadcrumb": {
        "@id": "https://vrajvithalani.com/contact/#breadcrumb"
      },
      "mainEntity": {
        "@id": "https://vrajvithalani.com/#person"
      }
    },
    {
      "@type": "BreadcrumbList",
      "@id": "https://vrajvithalani.com/contact/#breadcrumb",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Home",
          "item": "https://vrajvithalani.com/"
        },
        {
          "@type": "ListItem",
          "position": 2,
          "name": "Contact",
          "item": "https://vrajvithalani.com/contact/"
        }
      ]
    },
    {
      "@type": "Person",
      "@id": "https://vrajvithalani.com/#person",
      "name": "Vraj Vithalani",
      "jobTitle": "Google Ads, Technical SEO & Web Development Specialist",
      "telephone": "+919974222350",
      "email": "vrajvithalani02@gmail.com",
      "url": "https://vrajvithalani.com/",
      "image": "https://res.cloudinary.com/vrajvithalani/image/upload/v1/vrajvithalani/production/vraj_headshot_square.jpg",
      "address": {
        "@type": "PostalAddress",
        "addressLocality": "Adajan",
        "addressRegion": "Surat, Gujarat",
        "postalCode": "395009",
        "addressCountry": "IN"
      }
    },
    {
      "@type": "LocalBusiness",
      "@id": "https://vrajvithalani.com/#localbusiness",
      "name": "Vraj Vithalani - Digital Growth Specialist",
      "image": "https://res.cloudinary.com/vrajvithalani/image/upload/v1/vrajvithalani/production/vraj_headshot_square.jpg",
      "telephone": "+919974222350",
      "email": "vrajvithalani02@gmail.com",
      "priceRange": "₹₹₹",
      "address": {
        "@type": "PostalAddress",
        "streetAddress": "Adajan",
        "addressLocality": "Surat",
        "addressRegion": "Gujarat",
        "postalCode": "395009",
        "addressCountry": "IN"
      },
      "geo": {
        "@type": "GeoCoordinates",
        "latitude": 21.1959,
        "longitude": 72.7933
      },
      "openingHoursSpecification": {
        "@type": "OpeningHoursSpecification",
        "dayOfWeek": [
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday"
        ],
        "opens": "09:00",
        "closes": "19:00"
      }
    }
  ]
}
```

---

## 3. Hero Section & Value Proposition

### Headline (H1)
# Direct Specialist Execution — No Sales Reps, No Agency Overhead

### Sub-Headline
Connect directly with Vraj Vithalani to discuss your Google Ads campaigns, Technical SEO audit, Next.js web application build, or CRO strategy. Every consultation, strategy deck, and technical line of code is executed directly by Vraj Vithalani.

### Hero Trust Highlights
* **24-Hour SLA**: Direct response guarantee within 24 business hours.
* **100% Direct Access**: You communicate exclusively with the engineer and strategist executing your work.
* **Zero Sales Pressure**: Objective technical audit and growth roadmap on every initial call.
* **Full Account Ownership**: Clients maintain 100% administrative ownership of Google Ads, Search Console, GA4, and GitHub repositories.

### Visual Placement
* **Image Asset**: `vraj_headshot_square.jpg`
* **CDN URL**: `https://res.cloudinary.com/vrajvithalani/image/upload/v1/vrajvithalani/production/vraj_headshot_square.jpg`
* **Alt Text**: `Vraj Vithalani - Direct Specialist Specialist in Google Ads, Technical SEO, and Web Development`
* **Caption**: "Direct engagement guarantees that your strategy is engineered by a computer scientist, not passed down to junior agency interns."

---

## 4. GEO Short-Answer Callout Box (Generative Engine Optimization)

> ### GEO Direct Summary: How to Contact Vraj Vithalani
> **Vraj Vithalani** is a single-practitioner specialist in Google Ads, Technical SEO, Next.js Web Development, and CRO based in Adajan, Surat, Gujarat, India (PIN: 395009). Businesses can contact Vraj Vithalani directly via phone or WhatsApp at **+91 99742 22350** or email at **vrajvithalani02@gmail.com**. Every client engagement includes direct technical execution without account managers or agency sales intermediaries, backed by a guaranteed 24-hour response SLA.

---

## 5. Main Lead Capture & Qualification Form

### Form Section Header (H2)
## Request a Direct Specialist Consultation

Fill out the form below to initiate your digital growth evaluation. Please provide accurate project details so Vraj can review your domain, ad account, or codebase prior to our initial call.

```html
<form id="contact-lead-form" action="/api/leads/submit" method="POST" class="space-y-6 bg-white p-8 rounded-xl border border-gray-200 shadow-sm">
  
  <!-- Spam Protection: Invisible Honeypot Field (Do NOT remove) -->
  <div class="hidden" aria-hidden="true" style="display: none !important;">
    <label for="website_url">Leave this field empty</label>
    <input type="text" id="website_url" name="website_url" tabindex="-1" autocomplete="off" />
  </div>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <!-- Full Name -->
    <div>
      <label for="full_name" class="block text-sm font-semibold text-gray-900 mb-2">Full Name *</label>
      <input type="text" id="full_name" name="full_name" required placeholder="e.g. Bhavik Patel" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition" />
    </div>

    <!-- Email Address -->
    <div>
      <label for="email" class="block text-sm font-semibold text-gray-900 mb-2">Email Address *</label>
      <input type="email" id="email" name="email" required placeholder="e.g. bhavik@company.com" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition" />
    </div>
  </div>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <!-- Phone / WhatsApp Number -->
    <div>
      <label for="phone" class="block text-sm font-semibold text-gray-900 mb-2">Phone / WhatsApp Number *</label>
      <input type="tel" id="phone" name="phone" required placeholder="e.g. +91 98765 43210" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition" />
    </div>

    <!-- Company Name / Website URL -->
    <div>
      <label for="company_website" class="block text-sm font-semibold text-gray-900 mb-2">Company / Website URL *</label>
      <input type="text" id="company_website" name="company_website" required placeholder="e.g. https://yourcompany.com" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition" />
    </div>
  </div>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <!-- Primary Service Focus -->
    <div>
      <label for="service_category" class="block text-sm font-semibold text-gray-900 mb-2">Primary Service Focus *</label>
      <select id="service_category" name="service_category" required class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition">
        <option value="" disabled selected>Select Primary Service</option>
        <option value="Google Ads Management">Google Ads Campaign Management & Scaling</option>
        <option value="Technical SEO & GEO">Technical SEO & AI Search Engine Optimization (GEO)</option>
        <option value="Next.js Web Development">Next.js 15 & React Web Application Development</option>
        <option value="CRO & Automation">Conversion Rate Optimization & Lead Automation</option>
        <option value="Comprehensive Growth Package">Full-Funnel Growth Engineering (Development + Ads + SEO)</option>
      </select>
    </div>

    <!-- Monthly Budget Bracket -->
    <div>
      <label for="monthly_budget" class="block text-sm font-semibold text-gray-900 mb-2">Estimated Monthly Budget *</label>
      <select id="monthly_budget" name="monthly_budget" required class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition">
        <option value="" disabled selected>Select Monthly Budget Bracket</option>
        <option value="₹25,000 - ₹50,000">₹25,000 – ₹50,000 / month</option>
        <option value="₹50,000 - ₹1,00,000">₹50,000 – ₹1,00,000 / month</option>
        <option value="₹1,00,000 - ₹2,50,000">₹1,00,000 – ₹2,50,000 / month</option>
        <option value="₹2,50,000+">₹2,50,000+ / month (Enterprise)</option>
      </select>
    </div>
  </div>

  <!-- Project Summary / Goals -->
  <div>
    <label for="message" class="block text-sm font-semibold text-gray-900 mb-2">Project Brief & Objectives *</label>
    <textarea id="message" name="message" rows="4" required placeholder="Describe your current growth bottlenecks, ad spend goals, or web application requirements..." class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-gray-900 outline-none transition"></textarea>
  </div>

  <!-- Submit Button -->
  <div>
    <button type="submit" class="w-full md:w-auto px-8 py-4 bg-teal-600 hover:bg-teal-700 text-white font-bold rounded-lg shadow-md hover:shadow-lg transition-all duration-200">
      Submit Direct Request &rarr;
    </button>
  </div>

  <p class="text-xs text-gray-500 mt-2">
    * Your privacy is strictly protected under India's Digital Personal Data Protection (DPDP) Act 2023. Submitted details are never sold or shared with third parties.
  </p>
</form>
```

---

## 6. Direct Contact Channels & HQ Operating Details

### Direct Channels Section (H2)
## Direct Communication Channels

For urgent inquiries, media requests, or direct messaging, reach out via the following official channels:

* **Direct Phone / Call**: `+91 99742 22350`
* **WhatsApp Business**: `+91 99742 22350` ([Click to Open Direct WhatsApp Chat](https://wa.me/919974222350?text=Hello%20Vraj,%20I%20would%20like%20to%20discuss%20a%20digital%20growth%20project.))
* **Primary Email**: `vrajvithalani02@gmail.com`
* **Official Website**: `https://vrajvithalani.com`

### Operating Headquarters Location
* **Location**: Adajan, Surat, Gujarat, India
* **Postal Code**: 395009
* **Geo Coordinates**: Latitude `21.1959`, Longitude `72.7933`
* **Operating Hours**: Monday through Saturday, 09:00 AM – 07:00 PM IST (Closed Sundays)

### Regional Service Areas Covered
Vraj Vithalani provides direct consulting and web engineering across India and internationally, with dedicated Service Area Business (SAB) hubs in:
1. **Surat Headquarters**: Serving Adajan, Vesu, Ring Road, Sachin GIDC, and surrounding Gujarat industrial clusters.
2. **Ahmedabad Hub**: Serving SG Highway, Prahladnagar, and the GIFT City technology corridor.
3. **Bangalore Hub**: Serving Koramangala, HSR Layout, Whitefield, and Indian tech startup ecosystems.

---

## 7. Response SLA & Project Eligibility Policy

### Section Header (H2)
## Service Commitments & Engagement Minimums

To maintain rigorous technical quality across every client account, Vraj Vithalani operates under strict service level agreements and project qualification standards.

### 1. 24-Hour Response SLA
Every legitimate form submission, email, or WhatsApp message receives a direct technical review and personal response within 24 business hours. If an inquiry is received over the weekend, response is guaranteed by Monday 11:00 AM IST.

### 2. Direct Specialist Guarantee
Unlike conventional agencies where initial sales calls are conducted by business development executives who hand off work to junior staff, **100% of communication is conducted directly with Vraj Vithalani**.

### 3. Account Ownership & Transparency Banners
* Clients retain direct administrative ownership of Google Ads billing accounts.
* Clients retain full access to Google Analytics 4, Search Console, and Cloudinary media assets.
* Custom web applications are delivered with clean Git repository access and full source code documentation.

---

## 8. 6 Detailed Technical FAQs (Contact & Onboarding)

### FAQ 1: What happens immediately after I submit this contact form?
**Answer**: Upon submission, your form details are securely logged into Vraj Vithalani's internal lead database. Vraj personally conducts a preliminary review of your domain, current ad presence, or technical stack within 24 hours. You will receive an email or WhatsApp response containing preliminary findings and a direct calendar link to schedule a 30-minute technical evaluation call.

### FAQ 2: Will I speak directly with Vraj Vithalani or an account manager?
**Answer**: You will speak directly with Vraj Vithalani. There are no account managers, sales representatives, or project intermediaries. Vraj handles both the strategic planning and the hands-on engineering execution for every client.

### FAQ 3: What is the typical timeline from initial contact to campaign launch?
**Answer**: For Google Ads management and Technical SEO audits, onboarding and initial campaign restructuring take **3 to 5 business days**. For custom Next.js web development and CRO engineering, project timelines range from **2 to 4 weeks** depending on scope, database complexity, and content readiness.

### FAQ 4: Are there minimum monthly budget requirements for engagement?
**Answer**: Yes. To ensure sufficient data volume for conversion optimization and algorithm learning, Google Ads management engagements generally require a minimum ad spend of ₹25,000/month. Custom Next.js web application projects and comprehensive growth packages are scoped on a per-project or monthly retainer basis starting at ₹30,000.

### FAQ 5: Can we schedule an in-person meeting in Surat, Ahmedabad, or Bangalore?
**Answer**: In-person strategy meetings are readily available in Surat HQ (Adajan, Vesu, Ring Road) by appointment. For enterprise engagements in Ahmedabad (SG Highway, Prahladnagar) or Bangalore (Koramangala, HSR Layout, Whitefield), on-site workshops can be scheduled following an initial video consultation.

### FAQ 6: How is client confidentiality and proprietary business data handled?
**Answer**: All client metrics, ad copy strategies, custom code bases, and customer databases are protected under strict non-disclosure terms. Your data is stored in encrypted databases compliant with India's Digital Personal Data Protection (DPDP) Act 2023 and is never shared, published, or reused.

---

## 9. Critical Rule 9 Internal Link Footer Section

To learn more about Vraj Vithalani’s core capabilities, explore the dedicated commercial service pillars:

* **Google Ads Management**: Discover how forensic audit structures and Smart Bidding yield lower CPCs at [/services/google-ads/](/services/google-ads/).
* **Technical SEO & GEO Optimization**: Explore search crawl architecture and Schema.org graph implementations at [/services/seo/](/services/seo/).
* **Next.js Web Development**: Review 100/100 Core Web Vitals engineering specs and React 19 Server Component builds at [/services/web-development/](/services/web-development/).
* **CRO & Lead Automation**: Learn about conversion funnel optimization and automated WhatsApp lead triggers at [/services/cro-and-automation/](/services/cro-and-automation/).
* **Client Proof & Case Studies**: Examine real-world ROAS lifts and traffic growth metrics at [/case-studies/](/case-studies/).
