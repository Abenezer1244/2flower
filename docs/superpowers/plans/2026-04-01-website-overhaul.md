# Flower AFH Website Overhaul — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform 5 standalone HTML pages into a production-ready, interlinked website with real business content, consistent navigation, and enhanced parallax scroll effects.

**Architecture:** Pure static HTML site — no build tools. Each page is a self-contained `.html` file with inline Tailwind (CDN), GSAP animations, and shared visual patterns. Changes are made directly to each file. Navigation and footer are duplicated across pages (no shared partials).

**Tech Stack:** HTML, Tailwind CSS (CDN), GSAP 3.12 + ScrollTrigger (CDN), Three.js r128 (CDN), Iconify icons (CDN), Google Fonts (Playfair Display + Inter)

---

## File Structure

| Action | File | Responsibility |
|--------|------|---------------|
| Rename | `generated-page (5).html` → `index.html` | Home page |
| Rename | `generated-page (4).html` → `about.html` | About Us page |
| Rename | `generated-page (3).html` → `services.html` | Services page |
| Rename | `generated-page (2).html` → `gallery.html` | Gallery page |
| Rename | `generated-page (1).html` → `contact.html` | Contact page |

---

### Task 1: Rename Files

**Files:**
- Rename: `generated-page (5).html` → `index.html`
- Rename: `generated-page (4).html` → `about.html`
- Rename: `generated-page (3).html` → `services.html`
- Rename: `generated-page (2).html` → `gallery.html`
- Rename: `generated-page (1).html` → `contact.html`

- [ ] **Step 1: Rename all files**

```bash
cd "C:/Users/Windows/OneDrive - Seattle Colleges/Desktop/2flower"
mv "generated-page (5).html" index.html
mv "generated-page (4).html" about.html
mv "generated-page (3).html" services.html
mv "generated-page (2).html" gallery.html
mv "generated-page (1).html" contact.html
```

- [ ] **Step 2: Verify files exist**

```bash
ls *.html
```

Expected: `about.html  contact.html  gallery.html  index.html  services.html`

- [ ] **Step 3: Commit**

```bash
git add -A
git commit -m "rename generated pages to proper filenames"
```

---

### Task 2: Add Consistent Navigation to All Pages

Replace each page's `<header>` block with a unified navigation bar that links all 5 pages, highlights the current page, includes a mobile hamburger menu, and is sticky on scroll.

**Files:**
- Modify: `index.html` (header, lines ~36-51)
- Modify: `about.html` (header, lines ~36-51)
- Modify: `services.html` (header, lines ~36-51)
- Modify: `gallery.html` (header, lines ~33-46)
- Modify: `contact.html` (header, lines ~37-46)

- [ ] **Step 1: Define the shared nav HTML**

The new header for every page. The `{CURRENT}` page link gets `text-[#2A3324]` (active), others get `text-[#2A3324]/70 hover:text-[#2A3324]`. Replace `{CURRENT_TITLE}` with the page name in the title attribute.

```html
<!-- Sticky Header Navigation -->
<header id="site-header" class="py-6 flex justify-between items-center border-b border-[#C4CFC0] sticky top-0 bg-[#E3E7E0]/90 backdrop-blur-sm z-50 transition-all">
    <nav class="hidden md:flex flex-wrap gap-x-4 gap-y-2 text-sm uppercase tracking-widest text-[#2A3324]/70">
        <a href="index.html" class="{home_class} transition-colors">Home</a>
        <span class="text-[#C4CFC0]">/</span>
        <a href="about.html" class="{about_class} transition-colors">About</a>
        <span class="text-[#C4CFC0]">/</span>
        <a href="services.html" class="{services_class} transition-colors">Services</a>
        <span class="text-[#C4CFC0]">/</span>
        <a href="gallery.html" class="{gallery_class} transition-colors">Gallery</a>
        <span class="text-[#C4CFC0]">/</span>
        <a href="contact.html" class="{contact_class} transition-colors">Contact</a>
    </nav>

    <!-- Mobile Hamburger -->
    <button id="menu-toggle" class="md:hidden flex flex-col gap-1.5 p-2 z-50" aria-label="Toggle menu">
        <span class="block w-6 h-[1.5px] bg-[#2A3324] transition-all menu-line-1"></span>
        <span class="block w-6 h-[1.5px] bg-[#2A3324] transition-all menu-line-2"></span>
        <span class="block w-4 h-[1.5px] bg-[#2A3324] transition-all menu-line-3"></span>
    </button>

    <div class="hidden sm:block text-[#5C715E]">
        <iconify-icon icon="solar:leaf-linear" class="text-3xl" style="stroke-width: 1.5;"></iconify-icon>
    </div>
</header>

<!-- Mobile Menu Overlay -->
<div id="mobile-menu" class="fixed inset-0 bg-[#E3E7E0] z-40 flex flex-col items-center justify-center gap-8 translate-x-full transition-transform duration-500 ease-in-out md:hidden">
    <a href="index.html" class="font-serif-custom text-4xl tracking-tight hover:text-[#5C715E] transition-colors">Home</a>
    <a href="about.html" class="font-serif-custom text-4xl tracking-tight hover:text-[#5C715E] transition-colors">About</a>
    <a href="services.html" class="font-serif-custom text-4xl tracking-tight hover:text-[#5C715E] transition-colors">Services</a>
    <a href="gallery.html" class="font-serif-custom text-4xl tracking-tight hover:text-[#5C715E] transition-colors">Gallery</a>
    <a href="contact.html" class="font-serif-custom text-4xl tracking-tight hover:text-[#5C715E] transition-colors">Contact</a>
    <div class="mt-8 text-sm uppercase tracking-widest text-[#2A3324]/50">
        <a href="tel:+12065040037">(206) 504-0037</a>
    </div>
</div>
```

For each page, set the current page link class to `text-[#2A3324]` and all others to `hover:text-[#2A3324]`.

- [ ] **Step 2: Add mobile menu JS to each page**

Add this script block just before the closing `</body>` tag (before the existing animation scripts) on every page:

```html
<script>
    // Mobile menu toggle
    const menuToggle = document.getElementById('menu-toggle');
    const mobileMenu = document.getElementById('mobile-menu');
    let menuOpen = false;

    menuToggle.addEventListener('click', () => {
        menuOpen = !menuOpen;
        if (menuOpen) {
            mobileMenu.classList.remove('translate-x-full');
            mobileMenu.classList.add('translate-x-0');
            menuToggle.querySelector('.menu-line-1').style.transform = 'rotate(45deg) translate(4px, 4px)';
            menuToggle.querySelector('.menu-line-2').style.opacity = '0';
            menuToggle.querySelector('.menu-line-3').style.transform = 'rotate(-45deg) translate(2px, -2px)';
            menuToggle.querySelector('.menu-line-3').style.width = '1.5rem';
        } else {
            mobileMenu.classList.add('translate-x-full');
            mobileMenu.classList.remove('translate-x-0');
            menuToggle.querySelector('.menu-line-1').style.transform = '';
            menuToggle.querySelector('.menu-line-2').style.opacity = '1';
            menuToggle.querySelector('.menu-line-3').style.transform = '';
            menuToggle.querySelector('.menu-line-3').style.width = '1rem';
        }
    });
</script>
```

- [ ] **Step 3: Apply nav to index.html**

Replace the existing `<header>...</header>` block. Set Home link class to `text-[#2A3324]`, all others to `hover:text-[#2A3324]`. Add the mobile menu overlay div right after the header. Add the mobile JS script.

- [ ] **Step 4: Apply nav to about.html**

Same as above but About link gets `text-[#2A3324]`.

- [ ] **Step 5: Apply nav to services.html**

Same but Services link gets `text-[#2A3324]`.

- [ ] **Step 6: Apply nav to gallery.html**

Same but Gallery link gets `text-[#2A3324]`.

- [ ] **Step 7: Apply nav to contact.html**

Same but Contact link gets `text-[#2A3324]`.

- [ ] **Step 8: Verify by opening each page in browser**

Open each HTML file in a browser. Confirm:
- Nav links appear on desktop with correct highlighting
- Clicking links navigates between pages
- Hamburger menu appears on mobile viewport (resize window to <768px)
- Menu slides in/out on toggle

- [ ] **Step 9: Commit**

```bash
git add index.html about.html services.html gallery.html contact.html
git commit -m "add consistent sticky navigation with mobile menu to all pages"
```

---

### Task 3: Update Footer on All Pages

Replace the footer nav links (currently `#` anchors and section anchors) with real page links, update copyright year, and add contact info.

**Files:**
- Modify: `index.html` (footer, near end of file)
- Modify: `about.html` (footer)
- Modify: `services.html` (footer)
- Modify: `gallery.html` (footer)
- Modify: `contact.html` (footer)

- [ ] **Step 1: Define the updated footer**

Replace the footer `<nav>` links and copyright line on every page with:

```html
<footer class="bg-[#2A3324] text-[#E3E7E0] pt-16 md:pt-24 pb-8 relative z-20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-12 mb-16">
            <div>
                <h3 class="font-serif-custom text-3xl md:text-4xl tracking-tight font-normal mb-6">Flower Adult Family Home</h3>
                <p class="text-base text-[#E3E7E0]/70 max-w-sm font-light">
                    Dedicated to providing an exceptional standard of living and compassionate care in a warm, family-oriented environment.
                </p>
            </div>
            <div class="flex flex-col gap-4">
                <p class="text-xs uppercase tracking-widest text-[#E3E7E0]/40 mb-2">Contact</p>
                <a href="tel:+12065040037" class="text-base text-[#E3E7E0]/70 hover:text-[#E3E7E0] transition-colors">(206) 504-0037</a>
                <a href="tel:+12063131516" class="text-base text-[#E3E7E0]/70 hover:text-[#E3E7E0] transition-colors">(206) 313-1516</a>
                <a href="mailto:abeba@flowerafh.com" class="text-base text-[#E3E7E0]/70 hover:text-[#E3E7E0] transition-colors">abeba@flowerafh.com</a>
                <p class="text-sm text-[#E3E7E0]/50 mt-2">605 74th St SW, Everett, WA 98203</p>
                <p class="text-sm text-[#E3E7E0]/50">Available 24/7 for inquiries</p>
            </div>
            <div class="flex md:justify-end items-end opacity-5 pointer-events-none select-none overflow-hidden">
                <h3 class="font-serif-custom text-7xl md:text-9xl leading-none tracking-tight whitespace-nowrap">Flower AFH</h3>
            </div>
        </div>

        <div class="flex flex-col md:flex-row justify-between items-center pt-8 border-t border-[#E3E7E0]/20 text-xs uppercase tracking-widest text-[#E3E7E0]/60">
            <nav class="flex flex-wrap justify-center gap-x-6 gap-y-4 mb-6 md:mb-0">
                <a href="index.html" class="hover:text-[#E3E7E0] transition-colors">Home</a>
                <a href="about.html" class="hover:text-[#E3E7E0] transition-colors">About</a>
                <a href="services.html" class="hover:text-[#E3E7E0] transition-colors">Services</a>
                <a href="gallery.html" class="hover:text-[#E3E7E0] transition-colors">Gallery</a>
                <a href="contact.html" class="hover:text-[#E3E7E0] transition-colors">Contact</a>
            </nav>
            <p>&copy; 2025 Flower Adult Family Home LLC. All rights reserved.</p>
        </div>
    </div>
</footer>
```

- [ ] **Step 2: Replace footer in index.html**

Replace the entire `<footer>...</footer>` block with the updated footer above.

- [ ] **Step 3: Replace footer in about.html**

Same replacement.

- [ ] **Step 4: Replace footer in services.html**

Same replacement.

- [ ] **Step 5: Replace footer in gallery.html**

Same replacement.

- [ ] **Step 6: Replace footer in contact.html**

Same replacement.

- [ ] **Step 7: Verify**

Open any page and check the footer: phone numbers, email, address display correctly. Links navigate to correct pages. Copyright reads 2025.

- [ ] **Step 8: Commit**

```bash
git add index.html about.html services.html gallery.html contact.html
git commit -m "update footer with real contact info, page links, and 2025 copyright"
```

---

### Task 4: Update Contact Page with Real Business Info

**Files:**
- Modify: `contact.html`

- [ ] **Step 1: Update phone info section**

Find the "Call Us" info-item block (line ~111-121). Replace with two phone numbers:

```html
<div class="info-item flex gap-6 items-start">
    <div class="w-12 h-12 rounded-full border border-[#C4CFC0] flex items-center justify-center text-[#5C715E] shrink-0">
        <iconify-icon icon="solar:phone-linear" class="text-xl" style="stroke-width: 1.5;"></iconify-icon>
    </div>
    <div>
        <span class="text-xs uppercase tracking-widest text-[#2A3324]/60 block mb-2">Call Us</span>
        <a href="tel:+12065040037" class="text-xl font-serif-custom hover:text-[#5C715E] transition-colors block">(206) 504-0037</a>
        <a href="tel:+12063131516" class="text-xl font-serif-custom hover:text-[#5C715E] transition-colors block mt-1">(206) 313-1516</a>
        <p class="text-sm text-[#2A3324]/70 mt-2">Available 24/7 for inquiries</p>
    </div>
</div>
```

- [ ] **Step 2: Update email info section**

Find the "Email Us" info-item block. Replace email:

```html
<div class="info-item flex gap-6 items-start">
    <div class="w-12 h-12 rounded-full border border-[#C4CFC0] flex items-center justify-center text-[#5C715E] shrink-0">
        <iconify-icon icon="solar:letter-linear" class="text-xl" style="stroke-width: 1.5;"></iconify-icon>
    </div>
    <div>
        <span class="text-xs uppercase tracking-widest text-[#2A3324]/60 block mb-2">Email Us</span>
        <a href="mailto:abeba@flowerafh.com" class="text-xl font-serif-custom hover:text-[#5C715E] transition-colors">abeba@flowerafh.com</a>
        <p class="text-sm text-[#2A3324]/70 mt-2">We typically reply within 24 hours.</p>
    </div>
</div>
```

- [ ] **Step 3: Update address info section**

Find the "Visit Us" info-item block. Replace address:

```html
<div class="info-item flex gap-6 items-start">
    <div class="w-12 h-12 rounded-full border border-[#C4CFC0] flex items-center justify-center text-[#5C715E] shrink-0">
        <iconify-icon icon="solar:map-point-linear" class="text-xl" style="stroke-width: 1.5;"></iconify-icon>
    </div>
    <div>
        <span class="text-xs uppercase tracking-widest text-[#2A3324]/60 block mb-2">Visit Us</span>
        <address class="text-xl font-serif-custom not-italic hover:text-[#5C715E] transition-colors cursor-pointer">
            605 74th St SW,<br>
            Everett, WA 98203
        </address>
        <p class="text-sm text-[#2A3324]/70 mt-2">Visits by appointment only.</p>
    </div>
</div>
```

- [ ] **Step 4: Update Google Maps embed**

Find the `<iframe>` in the map section (line ~162). Replace the `src` with a URL for the Everett address:

```html
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2680.5!2d-122.2080!3d47.9450!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2s605%2074th%20St%20SW%2C%20Everett%2C%20WA%2098203!5e0!3m2!1sen!2sus!4v1700000000000!5m2!1sen!2sus" class="absolute inset-0 w-full h-full border-0 map-filter" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade">
</iframe>
```

Note: The embed coordinates are approximate. The user should generate an exact embed URL from Google Maps for the real address. For now this will show the general Everett area.

- [ ] **Step 5: Verify contact page**

Open `contact.html` in browser. Check:
- Both phone numbers appear and are clickable
- Email is correct
- Address shows Everett
- Map shows approximate Everett area

- [ ] **Step 6: Commit**

```bash
git add contact.html
git commit -m "update contact page with real phone numbers, email, address, and map"
```

---

### Task 5: Update Home Page Contact Section & Business Info

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Update the "Take Action Today" contact info**

Find the contact section (id="contact", line ~330). Replace the Location and Direct Contact blocks:

```html
<div class="flex flex-col gap-8 mt-auto border-t border-[#C4CFC0] pt-8">
    <div>
        <p class="text-xs uppercase tracking-widest text-[#2A3324]/50 mb-2">Location</p>
        <p class="text-lg font-normal">Flower Adult Family Home LLC<br>605 74th St SW<br>Everett, WA 98203</p>
    </div>
    <div>
        <p class="text-xs uppercase tracking-widest text-[#2A3324]/50 mb-2">Direct Contact</p>
        <a href="mailto:abeba@flowerafh.com" class="text-lg font-normal block hover:opacity-70 transition-opacity">abeba@flowerafh.com</a>
        <a href="tel:+12065040037" class="text-lg font-normal block hover:opacity-70 transition-opacity mt-1">(206) 504-0037</a>
        <a href="tel:+12063131516" class="text-lg font-normal block hover:opacity-70 transition-opacity mt-1">(206) 313-1516</a>
    </div>
</div>
```

- [ ] **Step 2: Verify**

Open `index.html` in browser, scroll to contact section. Confirm real info displays.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "update home page contact section with real business info"
```

---

### Task 6: Update About Page Contact Section & Team Content

**Files:**
- Modify: `about.html`

- [ ] **Step 1: Update "Take Action Today" contact info**

Find the contact section (id="contact", lines ~207-276). Replace the Location and Direct Contact blocks with the same real info as Task 5 Step 1.

- [ ] **Step 2: Replace team member section with general team description**

Find the "Our People" section (lines ~152-203). Replace the three individual team member cards with:

```html
<!-- Our People Section -->
<section class="py-16 md:py-24 border-t border-[#C4CFC0]">
    <div class="mb-16 flex flex-col justify-center items-center text-center">
        <span class="text-sm uppercase tracking-widest text-[#2A3324]/70 block mb-6">Our People</span>
        <h2 class="reveal-text font-serif-custom text-4xl sm:text-5xl md:text-6xl tracking-tight font-normal max-w-4xl">
            Dedicated professionals who feel like family.
        </h2>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 md:gap-12">
        <!-- Care Team -->
        <div class="group">
            <div class="webgl-measure relative w-full h-[400px] mb-6" data-grayscale="true">
                <div class="absolute inset-0 w-full h-full bg-transparent">
                    <img src="https://images.unsplash.com/photo-1576765608535-5f04d1e3f289?q=80&w=600&auto=format&fit=crop" alt="Compassionate caregiving" class="webgl-img w-full h-full object-cover opacity-0">
                </div>
            </div>
            <div class="border-t border-[#C4CFC0] pt-4 flex flex-col">
                <h3 class="font-serif-custom text-2xl tracking-tight font-normal mb-1">Licensed Caregivers</h3>
                <span class="text-xs uppercase tracking-widest text-[#5C715E] block mb-4">Round-the-Clock Care</span>
                <p class="text-sm text-[#2A3324]/80 font-light">Our team of trained, licensed caregivers provides attentive 24/7 support. Each team member is selected not just for their clinical skills, but for their genuine compassion and warmth.</p>
            </div>
        </div>

        <!-- Health Management -->
        <div class="group mt-0 md:mt-12">
            <div class="webgl-measure relative w-full h-[400px] mb-6" data-grayscale="true">
                <div class="absolute inset-0 w-full h-full bg-transparent">
                    <img src="https://images.unsplash.com/photo-1537368910025-700350fe46c7?q=80&w=600&auto=format&fit=crop" alt="Health and wellness coordination" class="webgl-img w-full h-full object-cover opacity-0">
                </div>
            </div>
            <div class="border-t border-[#C4CFC0] pt-4 flex flex-col">
                <h3 class="font-serif-custom text-2xl tracking-tight font-normal mb-1">Health Coordination</h3>
                <span class="text-xs uppercase tracking-widest text-[#5C715E] block mb-4">Medical Oversight</span>
                <p class="text-sm text-[#2A3324]/80 font-light">Our care coordinators work closely with physicians, pharmacists, and specialists to ensure each resident's health plan is followed with precision and adjusted as needs evolve.</p>
            </div>
        </div>

        <!-- Nutrition & Wellness -->
        <div class="group mt-0 md:mt-24">
            <div class="webgl-measure relative w-full h-[400px] mb-6" data-grayscale="true">
                <div class="absolute inset-0 w-full h-full bg-transparent">
                    <img src="https://images.unsplash.com/photo-1581056771107-24ca5f033842?q=80&w=600&auto=format&fit=crop" alt="Nutrition and daily wellness" class="webgl-img w-full h-full object-cover opacity-0">
                </div>
            </div>
            <div class="border-t border-[#C4CFC0] pt-4 flex flex-col">
                <h3 class="font-serif-custom text-2xl tracking-tight font-normal mb-1">Nutrition & Wellness</h3>
                <span class="text-xs uppercase tracking-widest text-[#5C715E] block mb-4">Daily Nourishment</span>
                <p class="text-sm text-[#2A3324]/80 font-light">Delicious, health-conscious meals are prepared from scratch daily, accommodating dietary needs and preferences. We believe shared mealtimes are the heart of a happy home.</p>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 3: Verify**

Open `about.html`. Confirm:
- No fake names (Sarah Jenkins, David Chen, Maria Rossi) appear
- Team section shows three general role cards
- Contact section shows real business info

- [ ] **Step 4: Commit**

```bash
git add about.html
git commit -m "replace fake team bios with general team descriptions, update contact info"
```

---

### Task 7: Update Services Page Content

The Services page already has good, detailed service descriptions. The main changes needed are updating any contact info and ensuring the content aligns with the spec's eight services. The current page has 4 core services + 2 specialties + 4 amenities — this is actually more detailed than the spec's list, so we keep it as-is since it covers all 8 spec items across its sections.

**Files:**
- Modify: `services.html`

- [ ] **Step 1: Verify services content alignment**

Read through the Services page sections. The current page covers:
1. 24/7 Attentive Care ✓
2. Medication Management ✓
3. Personal Hygiene (Personal Care) ✓
4. Nutritional Diet (Nutritious Meals) ✓
5. Memory & Dementia Care (specialty) ✓
6. Mobility & Rehab Support (relates to Housekeeping/Social Activities) ✓
7. Private Suites, Accessible Design, Gardens, Lounges (amenities) ✓

The content is comprehensive and well-written. No content changes needed — the existing descriptions exceed the spec's requirements.

- [ ] **Step 2: Verify no placeholder contact info remains**

The Services page has no contact section or placeholder info — it only has the footer (already updated in Task 3). No changes needed.

- [ ] **Step 3: Commit (if any changes were made)**

If no changes beyond footer (Task 3), skip this commit.

---

### Task 8: Enhanced Parallax & Scroll Effects on Home Page

Add deeper parallax layers, section-level storytelling transitions, and counter animations to `index.html`.

**Files:**
- Modify: `index.html` (script section at bottom)

- [ ] **Step 1: Add prefers-reduced-motion CSS**

Add to the `<style>` block in `<head>`:

```css
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
    }
    .reveal-word { transform: none !important; }
}
```

- [ ] **Step 2: Add section storytelling transitions**

Add after the existing `gsap.to('.parallax-img', ...)` block in the `<script>` section:

```javascript
// Storytelling section transitions
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!prefersReduced) {
    // Fade-in sections on scroll
    gsap.utils.toArray('section').forEach((section) => {
        gsap.fromTo(section,
            { opacity: 0.3, y: 60 },
            {
                opacity: 1,
                y: 0,
                duration: 1,
                ease: 'power2.out',
                scrollTrigger: {
                    trigger: section,
                    start: 'top 85%',
                    end: 'top 40%',
                    scrub: 1
                }
            }
        );
    });

    // Image zoom-on-scroll effect for WebGL images
    gsap.utils.toArray('.webgl-measure').forEach((img) => {
        gsap.fromTo(img,
            { scale: 1 },
            {
                scale: 1.05,
                ease: 'none',
                scrollTrigger: {
                    trigger: img,
                    start: 'top bottom',
                    end: 'bottom top',
                    scrub: true
                }
            }
        );
    });

    // Expertise cards staggered cascade
    gsap.fromTo('.bg-\\[\\#D5DBD1\\]',
        { y: 80, opacity: 0 },
        {
            y: 0,
            opacity: 1,
            duration: 0.8,
            stagger: 0.15,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '#expertise',
                start: 'top 75%'
            }
        }
    );
}
```

- [ ] **Step 3: Verify animations**

Open `index.html` in browser. Scroll through the page and confirm:
- Sections fade in smoothly as you scroll
- Images have a subtle zoom effect
- Expertise cards cascade in
- Parallax image section still works

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "enhance home page with scroll storytelling, parallax depth, and reduced-motion support"
```

---

### Task 9: Enhanced Scroll Effects on Services Page

**Files:**
- Modify: `services.html` (style and script sections)

- [ ] **Step 1: Add prefers-reduced-motion CSS**

Add the same reduced-motion CSS from Task 8 Step 1 to the services page `<style>` block.

- [ ] **Step 2: Add scroll animations**

Add after the existing reveal-text animation in the `<script>` section:

```javascript
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!prefersReduced) {
    // Service items stagger reveal
    gsap.utils.toArray('#services-list .group').forEach((item, i) => {
        gsap.fromTo(item,
            { y: 60, opacity: 0 },
            {
                y: 0,
                opacity: 1,
                duration: 0.8,
                ease: 'power2.out',
                scrollTrigger: {
                    trigger: item,
                    start: 'top 85%',
                    toggleActions: 'play none none none'
                }
            }
        );
    });

    // Care approach cards
    gsap.fromTo('.grid > .flex.flex-col',
        { y: 40, opacity: 0 },
        {
            y: 0,
            opacity: 1,
            stagger: 0.12,
            duration: 0.8,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid > .flex.flex-col',
                start: 'top 80%'
            }
        }
    );

    // Amenity cards cascade
    gsap.fromTo('.grid.grid-cols-1.sm\\:grid-cols-2 > div',
        { y: 50, opacity: 0, scale: 0.95 },
        {
            y: 0,
            opacity: 1,
            scale: 1,
            stagger: 0.1,
            duration: 0.6,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid.grid-cols-1.sm\\:grid-cols-2',
                start: 'top 80%'
            }
        }
    );
}
```

- [ ] **Step 3: Verify**

Open `services.html`, scroll through. Service items should stagger in, amenity cards cascade.

- [ ] **Step 4: Commit**

```bash
git add services.html
git commit -m "add staggered scroll animations to services page"
```

---

### Task 10: Enhanced Scroll Effects on About Page

**Files:**
- Modify: `about.html` (style and script sections)

- [ ] **Step 1: Add prefers-reduced-motion CSS**

Same as Task 8 Step 1.

- [ ] **Step 2: Add scroll animations**

Add after existing reveal-text animation in script:

```javascript
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!prefersReduced) {
    // Purpose/Vision section - columns slide in from sides
    gsap.fromTo('.grid.grid-cols-1.md\\:grid-cols-2 > .flex:first-child',
        { x: -60, opacity: 0 },
        {
            x: 0,
            opacity: 1,
            duration: 1,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid.grid-cols-1.md\\:grid-cols-2',
                start: 'top 80%'
            }
        }
    );

    gsap.fromTo('.grid.grid-cols-1.md\\:grid-cols-2 > .flex:last-child',
        { x: 60, opacity: 0 },
        {
            x: 0,
            opacity: 1,
            duration: 1,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid.grid-cols-1.md\\:grid-cols-2',
                start: 'top 80%'
            }
        }
    );

    // Timeline items stagger
    gsap.fromTo('.grid.grid-cols-1.md\\:grid-cols-4 > div',
        { y: 60, opacity: 0 },
        {
            y: 0,
            opacity: 1,
            stagger: 0.2,
            duration: 0.8,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid.grid-cols-1.md\\:grid-cols-4',
                start: 'top 80%'
            }
        }
    );

    // Team cards slide in from right
    gsap.fromTo('.grid.grid-cols-1.md\\:grid-cols-3 > .group',
        { x: 80, opacity: 0 },
        {
            x: 0,
            opacity: 1,
            stagger: 0.15,
            duration: 0.8,
            ease: 'power2.out',
            scrollTrigger: {
                trigger: '.grid.grid-cols-1.md\\:grid-cols-3',
                start: 'top 80%'
            }
        }
    );
}
```

- [ ] **Step 3: Verify**

Open `about.html`, scroll through. Purpose/Vision slide in from sides, timeline staggers, team cards slide from right.

- [ ] **Step 4: Commit**

```bash
git add about.html
git commit -m "add parallax and scroll storytelling animations to about page"
```

---

### Task 11: Enhanced Scroll Effects on Gallery & Contact Pages

**Files:**
- Modify: `gallery.html` (style and script sections)
- Modify: `contact.html` (style section)

- [ ] **Step 1: Add prefers-reduced-motion CSS to gallery.html**

Same CSS as Task 8 Step 1.

- [ ] **Step 2: Add parallax offset to gallery items**

The gallery already has rotation/scale/parallax animations on `.gallery-item`. Enhance by adding a subtle vertical parallax offset within each image:

Add after the existing gallery-item animation in the script:

```javascript
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!prefersReduced) {
    // Subtle parallax within gallery grid cells
    gsap.utils.toArray('.webgl-measure').forEach((measure) => {
        gsap.fromTo(measure,
            { yPercent: -5 },
            {
                yPercent: 5,
                ease: 'none',
                scrollTrigger: {
                    trigger: measure,
                    start: 'top bottom',
                    end: 'bottom top',
                    scrub: true
                }
            }
        );
    });
}
```

- [ ] **Step 3: Add prefers-reduced-motion CSS to contact.html**

Same CSS as Task 8 Step 1.

- [ ] **Step 4: Verify**

Open both pages. Gallery images should have subtle vertical movement within their cells on scroll. Contact page animations should still work, and both should disable gracefully with reduced-motion preference.

- [ ] **Step 5: Commit**

```bash
git add gallery.html contact.html
git commit -m "add parallax depth to gallery, reduced-motion support to gallery and contact"
```

---

### Task 12: Final Verification & Cleanup

- [ ] **Step 1: Open each page and verify navigation**

Test all 5 pages:
- `index.html` → About, Services, Gallery, Contact links work
- Each page highlights the correct nav item
- Mobile menu works on all pages (resize to <768px)

- [ ] **Step 2: Verify no placeholder info remains**

Search all files for placeholder text:

```bash
grep -rn "555" *.html
grep -rn "flowerhome" *.html
grep -rn "Serenity Lane" *.html
grep -rn "Care District" *.html
grep -rn "info@flowerhome" *.html
grep -rn "hello@flowerhome" *.html
grep -rn "Sarah Jenkins\|David Chen\|Maria Rossi" *.html
grep -rn "2024" *.html
```

Expected: No matches for any placeholder content. Copyright should show 2025.

- [ ] **Step 3: Fix any remaining placeholders found**

If grep finds any leftover placeholder text, replace it with the correct real info.

- [ ] **Step 4: Final commit**

```bash
git add -A
git commit -m "final cleanup: verify no placeholder content remains"
```
