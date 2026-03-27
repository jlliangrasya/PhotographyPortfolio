# 🎯 A.F. Photography — Full Project Masterplan
> Feed this entire document to Claude Code in VS Code to scaffold the project.

---

## 📌 Project Overview

- **Brand Name:** A.F. Photography
- **Photographers:** Aldrian Francis & Jillian Grace
- **Location:** Southern Leyte (Service: Nationwide)
- **Email:** jlliangracia.snaps@gmail.com
- **Tagline:** "We shoot first, ask questions later."
- **Style:** Bold & Modern
- **Color Scheme:** Dark Navy & Silver
- **Email Service:** EmailJS (no backend, no database)

---

## 🛠️ Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Framework | **Astro 4** | Perfect for portfolio/landing pages, blazing fast |
| Styling | **Tailwind CSS** | Utility-first, easy to customize |
| Animations | **AOS (Animate On Scroll)** | Lightweight scroll animations, no overhead |
| Gallery Lightbox | **GLightbox** | Vanilla JS lightbox, no framework needed |
| Email | **EmailJS** (`@emailjs/browser`) | Send booking emails, no backend needed |
| Icons | **Lucide Icons** (SVG) | Lightweight, copy-paste SVG icons |
| Fonts | **Google Fonts** — `Playfair Display` + `Inter` | Elegant headings + clean body |
| Image Optimization | **Astro built-in** `<Image />` | Auto-compress & lazy-load photos |
| Deployment | **Netlify** (free, drag & drop) | Simplest deployment possible |

---

## 📁 Project Structure

```
af-photography/
├── src/
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── About.astro
│   │   ├── Services.astro
│   │   ├── Gallery.astro
│   │   ├── Testimonials.astro
│   │   ├── Booking.astro
│   │   └── Footer.astro
│   ├── data/
│   │   ├── gallery.js       ← Add/remove photos here easily
│   │   └── testimonials.js  ← Add/remove testimonials here
│   ├── layouts/
│   │   └── Layout.astro     ← Base HTML, fonts, meta tags
│   └── pages/
│       └── index.astro      ← Main page (imports all components)
├── public/
│   └── images/              ← Put local images here
├── .env                     ← EmailJS keys go here
├── astro.config.mjs
├── tailwind.config.mjs
└── package.json
```

---

## 🎨 Design Tokens (Tailwind Config)

```js
// tailwind.config.mjs
colors: {
  navy: {
    900: '#0a0f1e',  // darkest background
    800: '#0d1530',  // section background
    700: '#111d42',  // card background
  },
  silver: {
    100: '#f0f0f0',  // light text
    300: '#c0c0c0',  // muted text
    500: '#9e9e9e',  // placeholder text
  },
  accent: '#c9d6e3', // soft silver-blue for highlights
}

fontFamily: {
  heading: ['Playfair Display', 'serif'],
  body: ['Inter', 'sans-serif'],
}
```

---

## 📄 Section-by-Section Instructions

---

### 1. 🔝 Navbar — `Navbar.astro`

- Fixed top, transparent → solid `navy-900` background on scroll (JS scroll listener)
- Logo: **"A.F."** in Playfair Display, bold, silver-white
- Nav links: Home · About · Services · Gallery · Testimonials · Book Now
- "Book Now" → silver outlined pill button
- Mobile: hamburger icon → slide-down drawer menu
- Smooth scroll via `scroll-behavior: smooth` in CSS + anchor links (`#about`, `#services`, etc.)

---

### 2. 🦸 Hero — `Hero.astro`

- Full viewport height (`min-h-screen`)
- Background: deep `navy-900` with subtle radial gradient from center
- Large semi-transparent **"AF"** monogram watermark in background (CSS, low opacity)
- Content (centered, vertically):
  - Badge: `"📍 Southern Leyte · Serving Nationwide"`
  - Heading: `"We Shoot First, Ask Questions Later."` — Playfair Display, large, white
  - Subheading: `"Wedding · Portrait · Events · Commercial Photography"`
  - CTA buttons: `"Book a Session"` (silver filled) + `"View Our Work"` (outlined)
- AOS animation: `data-aos="fade-up"` on heading and buttons
- Animated scroll-down arrow at the bottom

---

### 3. 👥 About — `About.astro`

- Section ID: `id="about"`
- Background: `navy-800`
- Heading: `"The People Behind the Lens"`
- Layout: two cards side by side (stacked on mobile)

**Card — Aldrian Francis**
- Square placeholder image (use `/public/images/aldrian.jpg`)
- Title: `"Lead Photographer"`
- Bio: `"With an eye for dramatic light and bold compositions, Aldrian brings stories to life through every frame."`

**Card — Jillian Grace**
- Square placeholder image (use `/public/images/jillian.jpg`)
- Title: `"Creative Photographer"`
- Bio: `"Jillian's warm and candid style captures the genuine emotions that make every moment unforgettable."`

- Shared line below cards: `"Together, we are A.F. Photography — two siblings with one lens, chasing the perfect shot across the Philippines."`
- AOS: left card `data-aos="fade-right"`, right card `data-aos="fade-left"`

---

### 4. 🎛️ Services — `Services.astro`

- Section ID: `id="services"`
- Background: `navy-900`
- Heading: `"What We Do"`
- 4 cards in a 2×2 grid (1 column on mobile):

| Service | Icon (SVG) | Description |
|---------|-----------|-------------|
| Weddings | Heart | "From vows to reception, we capture every tear, laugh, and dance." |
| Portraits | User | "Personal or professional, we make sure you look your absolute best." |
| Events | Calendar | "Birthdays, debuts, corporate — if it matters to you, it matters to us." |
| Commercial / Brand | Briefcase | "Elevate your brand with imagery that speaks louder than words." |

- Each card: `navy-700` background, silver top-border, hover → silver border glow (`box-shadow`)
- AOS: `data-aos="fade-up"` with staggered `data-aos-delay` (0, 100, 200, 300)

---

### 5. 🖼️ Gallery — `Gallery.astro`

- Section ID: `id="gallery"`
- Background: `navy-800`
- Heading: `"Our Work"`

**Filter Tabs** (above grid):
- Buttons: `All | Weddings | Portraits | Events | Commercial`
- Active tab: silver background, dark text
- Filtering via vanilla JS — toggle `hidden` class based on `data-category` attribute on each image

**Masonry Grid:**
- CSS `columns` approach: 3 cols (desktop), 2 cols (tablet), 1 col (mobile)
- Each image wrapped in `<a>` tag for GLightbox
- Use Astro's `<Image />` component with `loading="lazy"` and `decoding="async"`

**Load More:**
- Show first 12 images on load
- "Load More" button appends next 8 images
- Implemented in vanilla JS

**Image Data — `src/data/gallery.js`:**
```js
export const galleryImages = [
  { src: "/images/gallery/photo1.jpg", alt: "Wedding shot", category: "weddings" },
  { src: "/images/gallery/photo2.jpg", alt: "Portrait session", category: "portraits" },
  { src: "/images/gallery/photo3.jpg", alt: "Corporate event", category: "events" },
  { src: "/images/gallery/photo4.jpg", alt: "Brand shoot", category: "commercial" },
  // Add more entries here — no code changes needed
]
```

**GLightbox Setup (in `<script>` tag inside Gallery.astro):**
```js
import GLightbox from 'glightbox'
const lightbox = GLightbox({ selector: '.glightbox' })
```

---

### 6. 💬 Testimonials — `Testimonials.astro`

- Section ID: `id="testimonials"`
- Background: `navy-900`
- Heading: `"What Our Clients Say"`
- Auto-scrolling horizontal carousel using CSS `animation: scroll` (infinite marquee)
- Pause on hover via CSS `animation-play-state: paused`
- Each card: quote, client name, service type, ⭐⭐⭐⭐⭐ stars (gold)

**Testimonial Data — `src/data/testimonials.js`:**
```js
export const testimonials = [
  {
    quote: "A.F. Photography captured our wedding perfectly. Every photo tells a story!",
    name: "Maria Santos",
    service: "Wedding",
    rating: 5
  },
  {
    quote: "Professional, creative, and so easy to work with. Highly recommend!",
    name: "Carlo Reyes",
    service: "Portrait",
    rating: 5
  },
  {
    quote: "They made our corporate event look absolutely stunning.",
    name: "Ana Villanueva",
    service: "Commercial",
    rating: 5
  },
  {
    quote: "Our debut photos were beyond what we imagined. Pure magic!",
    name: "Sofia Lim",
    service: "Event",
    rating: 5
  },
  {
    quote: "Worth every peso. The photos speak for themselves.",
    name: "Renz Bautista",
    service: "Wedding",
    rating: 5
  },
]
```

---

### 7. 📅 Booking Form — `Booking.astro`

- Section ID: `id="booking"`
- Background: `navy-800`
- Heading: `"Book a Session"`
- Subtitle: `"Fill out the form and we'll get back to you within 24 hours."`

**Form Fields:**

| Field | Type | Required |
|-------|------|----------|
| Full Name | text | ✅ |
| Email Address | email | ✅ |
| Phone Number | tel | ✅ |
| Service Type | dropdown (Wedding · Portrait · Event · Commercial) | ✅ |
| Preferred Date | date | ✅ |
| Event Location | text | ✅ |
| Message / Details | textarea | ❌ |

**EmailJS Script (inside `<script>` in Booking.astro):**
```js
import emailjs from '@emailjs/browser'

document.getElementById('booking-form').addEventListener('submit', async (e) => {
  e.preventDefault()
  const btn = document.getElementById('submit-btn')
  btn.textContent = 'Sending...'
  btn.disabled = true

  try {
    await emailjs.send(
      import.meta.env.PUBLIC_EMAILJS_SERVICE_ID,
      import.meta.env.PUBLIC_EMAILJS_TEMPLATE_ID,
      {
        from_name: e.target.name.value,
        from_email: e.target.email.value,
        phone: e.target.phone.value,
        service: e.target.service.value,
        date: e.target.date.value,
        location: e.target.location.value,
        message: e.target.message.value,
      },
      import.meta.env.PUBLIC_EMAILJS_PUBLIC_KEY
    )
    document.getElementById('success-msg').classList.remove('hidden')
    e.target.reset()
  } catch (err) {
    document.getElementById('error-msg').classList.remove('hidden')
  } finally {
    btn.textContent = 'Send Booking Request'
    btn.disabled = false
  }
})
```

**`.env` file (user must fill in):**
```
PUBLIC_EMAILJS_SERVICE_ID=your_service_id
PUBLIC_EMAILJS_TEMPLATE_ID=your_template_id
PUBLIC_EMAILJS_PUBLIC_KEY=your_public_key
```

- Inputs styled: `navy-700` background, silver border, focus → silver glow
- Submit button: full-width, silver background, dark text, loading state
- Success: `"🎉 Booking request sent! We'll contact you within 24 hours."`
- Error: `"❌ Something went wrong. Please email us directly."`

---

### 8. 🦶 Footer — `Footer.astro`

- Background: `navy-900` with a `1px` silver top border
- 3-column layout (stacked on mobile):
  - **Col 1:** `"A.F. Photography"` logo + tagline
  - **Col 2:** Quick nav links
  - **Col 3:** Email, location, Instagram & Facebook icons (SVG)
- Bottom bar: `"© 2025 A.F. Photography. All rights reserved. | Southern Leyte, Philippines"`

---

## ⚙️ Setup & Installation Instructions

```bash
# 1. Create Astro project
npm create astro@latest af-photography
# Choose: Empty template, TypeScript: No, Install dependencies: Yes

# 2. Enter project
cd af-photography

# 3. Add Tailwind CSS
npx astro add tailwind

# 4. Install remaining dependencies
npm install @emailjs/browser glightbox aos

# 5. Create .env file and add EmailJS keys (fill in later)

# 6. Start dev server
npm run dev
# Site runs at http://localhost:4321
```

---

## 🚀 Deployment (Netlify — Drag & Drop)

```bash
# Build the site
npm run build
# This creates a "dist/" folder

# Then go to netlify.com → drag and drop the "dist/" folder
# Your site is live instantly — no terminal needed!
```

> ⚠️ Add your EmailJS environment variables in Netlify:
> Site Settings → Environment Variables → Add the 3 PUBLIC_EMAILJS_* keys

---

## 📝 Claude Code Prompt (Copy & Paste This)

```
Build a full photography landing page for "A.F. Photography" using Astro 4 + Tailwind CSS.

Project details:
- Brand: A.F. Photography
- Photographers: Aldrian Francis (Lead) & Jillian Grace (Creative)
- Location: Southern Leyte, Nationwide service
- Email: jlliangracia.snaps@gmail.com
- Tagline: "We shoot first, ask questions later."
- Style: Bold & Modern | Colors: Dark Navy & Silver
- Fonts: Playfair Display (headings) + Inter (body) via Google Fonts

Instructions:
1. Run: npm create astro@latest af-photography, then npx astro add tailwind
2. Install: @emailjs/browser glightbox aos
3. Set up Tailwind config with navy (900/800/700) and silver (100/300/500) color tokens
4. Build all components: Navbar, Hero, About, Services, Gallery, Testimonials, Booking, Footer
5. Create src/data/gallery.js with placeholder images (at least 8) with category fields
6. Create src/data/testimonials.js with 5 placeholder testimonials
7. Gallery: CSS masonry grid, GLightbox, category filter tabs, Load More button (show 12, load 8 more)
8. Testimonials: infinite auto-scroll CSS marquee carousel, pause on hover
9. Navbar: transparent → solid on scroll, mobile hamburger menu
10. Booking form: all fields as specified, EmailJS integration using PUBLIC_ env variables
11. Add AOS scroll animations throughout (fade-up, fade-left, fade-right with stagger delays)
12. Make entire site mobile-first responsive
13. Create .env.example with the 3 EmailJS variable names
14. All section IDs must match nav anchor links: #about #services #gallery #testimonials #booking
```

---

## ✅ Pre-Launch Checklist

- [ ] Replace `/public/images/aldrian.jpg` and `jillian.jpg` with real photos
- [ ] Add real gallery photos to `/public/images/gallery/` and update `gallery.js`
- [ ] Create EmailJS account → get Service ID, Template ID, Public Key
- [ ] Fill in `.env` with real EmailJS credentials
- [ ] Create EmailJS email template using the field names in the booking form
- [ ] Replace placeholder testimonials with real client feedback
- [ ] Add real Instagram and Facebook URLs in Footer
- [ ] Run `npm run build` → drag `dist/` folder to Netlify
- [ ] Add EmailJS env variables in Netlify dashboard
- [ ] Test booking form live → confirm email arrives at jlliangracia.snaps@gmail.com