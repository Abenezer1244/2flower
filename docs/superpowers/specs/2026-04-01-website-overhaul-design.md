# Flower AFH Website Overhaul — Design Spec

## Overview

Production-ready overhaul of the Flower Adult Family Home LLC website. Five existing standalone HTML pages get renamed, linked together, filled with real business content, and enhanced with scroll-driven parallax and storytelling effects. No new pages, no build tools, no redesign — elevate what exists.

## File Structure

| Current | New | Page |
|---------|-----|------|
| `generated-page (5).html` | `index.html` | Home |
| `generated-page (4).html` | `about.html` | About Us |
| `generated-page (3).html` | `services.html` | Services |
| `generated-page (2).html` | `gallery.html` | Gallery |
| `generated-page (1).html` | `contact.html` | Contact |

## Navigation

- Consistent nav bar across all 5 pages: Home / About / Services / Gallery / Contact
- Current page visually highlighted
- Maintains existing minimal style: uppercase, wide letter-spacing, slash-separated links
- Mobile: hamburger icon triggers a slide-in panel with nav links
- Sticky on scroll with subtle backdrop blur (`backdrop-blur-sm`)

## Business Info

Replace all placeholder contact details with:

- **Phone 1:** (206) 504-0037
- **Phone 2:** (206) 313-1516
- **Email:** abeba@flowerafh.com
- **Address:** 605 74th St SW, Everett, WA 98203
- **Hours:** Available 24/7 for inquiries

### Where it appears:
- Contact page: both phone numbers, email, address, Google Maps embed updated to Everett address
- Footer on all pages: address, primary phone, email
- Copyright year: 2025

## Services Page Content

Eight core services, each with an icon and 2-3 sentence description:

1. **Personal Care** — bathing, grooming, dressing, mobility assistance
2. **Medication Management** — administration, tracking, coordination with physicians
3. **Nutritious Meals** — home-cooked meals tailored to dietary needs, 3 meals + snacks daily
4. **24/7 Supervision** — round-the-clock staff presence and emergency response
5. **Memory & Dementia Care** — specialized support for Alzheimer's and related conditions
6. **Housekeeping & Laundry** — clean, comfortable living environment maintained daily
7. **Social Activities** — recreational programs, outings, companionship
8. **Hospice & End-of-Life Support** — compassionate coordination with hospice providers

Existing page layout (cards/sections with GSAP reveals) stays. Content swapped in.

## About Page Content

Replace fake team member profiles with general content:

1. **Hero** — "Our Story" with parallax background image
2. **Mission** — why Flower AFH exists, emphasis on family-oriented care in Everett
3. **Values** — compassion, dignity, community, safety
4. **Our Team** — general description of experienced, compassionate caregiving team. No fake names or bios. Stock photos reframed as lifestyle/care imagery.
5. **Why Families Choose Us** — trust signals (licensed, 24/7 care, home environment)

## Scroll Effects & Parallax

Build on existing GSAP + ScrollTrigger foundation.

### Parallax layers
- Hero images scroll at slower rate than content (parallax depth)
- Background decorative elements (vertical grid lines) shift subtly on scroll

### Scroll-driven storytelling (Home page)
- Sections transition like chapters: Welcome > Why Us > Our Care > Contact
- Text elements stagger in word-by-word (refine existing behavior, make consistent)
- Numbers/stats animate up with counter effect on viewport entry

### Page-wide enhancements
- Smooth scroll-linked opacity transitions between sections
- Images scale subtly (1.0 to 1.05) within containers on scroll
- Services page: cards reveal in staggered cascade
- Gallery page: subtle parallax offset within grid cells
- About page: team section fades in with horizontal slide

### Preserved effects
- WebGL image effects on Gallery page
- 3D map animation on Contact page

### Accessibility
- All animations respect `prefers-reduced-motion` media query (disabled when set)

## Technical Constraints

- Pure static HTML — no build tools, no framework
- Tailwind CSS via CDN
- GSAP + ScrollTrigger via CDN
- Three.js via CDN (Gallery page WebGL)
- Iconify via CDN
- Google Fonts: Playfair Display + Inter
- Existing color palette: `#2A3324`, `#5C715E`, `#E3E7E0`, `#C4CFC0`
- All existing image sources (ibb.co, Unsplash, Supabase) preserved

## Out of Scope

- New pages
- Build system or component framework
- Color palette or typography changes
- Replacing existing images
- Backend functionality (form submissions go nowhere — no change)
- SEO meta tags or analytics (can be a follow-up)
