## PORTFOLIO GUIDE  

---

### WHY A PORTFOLIO?  
A polished one-pager instantly shows  
1. you can ship working software,  
2. you can explain what you built and why, and  
3. you’re someone teammates enjoy working with.  

You’ll have roughly **5–6 in-class hours (today + tomorrow)** to build the foundation. After that, refine, add, or totally redesign whenever you like—your site is a living résumé.

---

## 1 PREP WORK  *(finish before class)*  

Place all of the items below in **one folder** on your laptop:

* Friendly head-and-shoulders photo  
* 2–3-sentence **About Me** paragraph  
* Two fun-fact bullets (show personality)  
* Skills list – for example: “Ruby • Rails • JavaScript • React • PostgreSQL”  
* Screenshots or GIFs for each project (1280 × 720 works well)  
* Contact links – email, LinkedIn, GitHub (résumé PDF if you have one)  
* **Project objects** for Bolt (plain-text file):  
  * **Capstone** – title, paragraph description, tech-stack list, image file-name, **live URL OR leave blank**, repo URL  
  * **Mini-capstone** – same fields  
  * Any **other projects** you’d like to show – same fields  

*Note → If you haven’t deployed a project yet, leave its live URL blank (“”). The generated site automatically hides the **Live Demo** button.*

---

## 2 — Generate the scaffold with Bolt.new  

A. Sign in to **bolt.new** with GitHub.  

B. **Choose your portfolio style** from the prompts below. Each creates a different look and feel:
   - **Prompt A**: Modern Animated (clean, professional with smooth animations)
   - **Prompt B**: Glass & Neon (dark cyberpunk theme with glass-morphism effects)  
   - **Prompt C**: Magazine (editorial multi-page with parallax scrolling)
   - **Prompt D**: Dashboard Cards (material design with sidebar navigation)
   - **Prompt E**: Retro Terminal (nostalgic green-phosphor CRT terminal vibe)

Pick the one that matches your personality and style, then copy the entire prompt into Bolt. **Replace every ALL-CAPS placeholder** with your own info.

*(Feel free to customize any prompt to match your style! You can rewrite sections, change colors, swap animations, add new features, or remove parts you don't want. These prompts are just starting points—make them yours. You'll be able to customize the code even more once it's on your machine.)*

### Prompt A — "Modern Animated" (clean, professional)
```
Create a responsive, animated portfolio using React 18, Vite, Tailwind CSS, and Framer-Motion v10.

Sections + features:

1. Hero
   – Full-width banner with a subtle animated gradient background.  
   – Name line: "I'M YOUR NAME — SOFTWARE ENGINEER." (bold, all-caps, slides up on load).  
   – Sub-line: "I solve real-world problems with code." (fades in 300 ms after the name).  
   – Circular profile photo on the right that scales from 0.8 → 1 on hover.

2. About
   – One full-width paragraph containing: "ABOUT_PARAGRAPH_GOES_HERE".  
   – Below it, a two-column list of funFacts[]; each item fades in on scroll (25 px upward motion, 0.4 s).

3. Skills
   – Responsive pill grid generated from skills[].  
   – Each pill bumps up by 3 px and changes background to sky-600 on hover.

4. Projects
   4.1 Capstone – layout: large image left, text right; image zooms 1.05× on hover.  
   4.2 Mini-capstone – same layout but image on the right for a zig-zag pattern.  
   4.3 Other Projects – iterate projects[] into cards, 2-up on desktop, 1-up on mobile.  
        Cards lift (translate-y -8) and shadow intensifies on hover.  
   – If an object's liveUrl is empty, omit the **Live Demo** button and show only GitHub.

5. Contact
   – Email link, LinkedIn icon, and a "Download Résumé" button linking to resumeURL.  
   – Section heading performs a gentle wiggle when it enters the viewport.

Global features
– Dark/light mode toggle in the navbar (Tailwind class strategy).  
– Smooth-scroll navigation.  
– Sticky navbar that slides down with a spring animation after first scroll.  
– Mobile hamburger menu animated with Framer-Motion (menu ✕ → menu ✓ morph).  
– Alt text on all images (imageAlt field).  
– SEO meta tags from siteMeta{ title, description }.  
– TODO comments wherever the student should customise text or replace images.

Put all data objects (below) in `src/data/portfolioData.js` and import where needed.
```

### Prompt B — "Glass & Neon" (dark cyberpunk theme)
```
Create a dark-mode–first, glass-morphism portfolio using React 18, Vite, Tailwind CSS, and Framer-Motion v10.

Overall vibe: sleek cyber-punk (neon accents, blurred glass cards, animated glow on hover).

Sections + features:

1. Hero
   – Full-viewport section with animated particle background (small, subtle).  
   – Name line: "YOUR NAME // SOFTWARE ENGINEER" (monospace, gradient-text, slides in from left).  
   – Tagline: "Turning ideas into shipped products." (fades in 400 ms later).  
   – Circular profile image inside a blurred glass card that tilts with mouse movement (Framer-Motion).

2. Stats Bar
   – Three stat blocks (Years coding, Projects shipped, Cups of coffee) inside a semi-transparent bar that sticks to top after first scroll.

3. Skills
   – Horizontal scrollable chips generated from skills[]; each chip glows on hover (Tailwind ring-offset-sky-500).

4. Projects
   – Iterate over capstone, miniCapstone, projects[] into glass cards laid out in a Masonry grid (2-up desktop, 1-up mobile).  
   – Card expands (scale 1.03) and neon ring appears on hover.  
   – If liveUrl is blank, only show GitHub button.

5. Contact
   – Centered social icons (LinkedIn, GitHub, Email) with neon hover glow.  
   – "Download Résumé" button pulses slowly.

Global features
– Dark/light toggle (but default dark, with vivid neon in dark mode, muted pastel in light mode).  
– Smooth-scroll, section scroll-spy.  
– SEO meta tags from siteMeta{ title, description }.  
– Alt text on all images.  
– TODO comments wherever customisation is required.

Place all data objects in `src/data/portfolioData.js`.
```

### Prompt C — "Magazine" (editorial multi-page with parallax)
```
Build a minimalist, editorial-style portfolio using Next.js 14 (App Router), Tailwind CSS, and GSAP ScrollTrigger for parallax.

Create three pages: Home (/), Projects (/projects), Contact (/contact).

/ === Home
1. Hero
   – Full-height hero with high-resolution background photo (IMAGE_HOME) and subtle parallax.  
   – Big serif headline: "HELLO, I'M YOUR NAME" (fade up 0.5 s).  
   – Kicker line: "Software engineer focused on HUMAN-CENTERED solutions." (fade-in 0.8 s).

2. About
   – Two-column layout: profile photo left, aboutParagraph right.  
   – Fun facts list fades in staggered as each item enters viewport.

3. Featured Projects (capstone, miniCapstone)
   – Large edge-to-edge images with parallax (image scrolls slower than content).  
   – If liveUrl empty, hide Live Demo button.

4. Skills strip
   – Horizontal marquee of skills[] (infinite scroll).

/projects
   – Grid of project cards from projects[].  
   – Filter buttons by tech (derived from techStack entries).  
   – Cards lift on hover; quick-view modal opens on click.

/contact
   – Simple form (Name, Email, Message) that POSTs to Formspree.  
   – Social links + résumé button.

Global features
– Light mode, lots of whitespace, max-width-prose.  
– SEO meta tags & Open Graph images.  
– Smooth page-transition fade between routes (Next.js Layout.js + framer-motion).  
– Images served via next-image for optimisation.  
– TODO comments for every placeholder.

Data objects live in `lib/portfolioData.js`.
```

### Prompt D — "Dashboard Cards" (material design with sidebar)
```
Generate a responsive, card-based portfolio using React 18, Vite, Material-UI v5 (MUI), and Framer-Motion.

Layout
– Persistent left sidebar (80 px collapsed, 240 px expanded) with icon links to each section.  
– Main content scrolls; sidebar highlights current section (scroll-spy).

Sections + features:

1. Hero
   – Centered introduction card with avatar, name ("YOUR NAME"), headline ("Full-Stack Engineer") and action buttons (LinkedIn, Email, Résumé).  
   – Card lifts (z-depth 8) on hover.

2. About
   – Two-column accordion: summary paragraph always visible; click to expand funFacts list with slide-down motion.

3. Skills
   – Responsive grid of MUI Chips from skills[]; each chip shakes slightly on hover (Framer-Motion).

4. Projects (capstone, miniCapstone, projects[])
   – Each project rendered as a MUI Card inside a responsive grid (3-up desktop, 1-up mobile).  
   – Image top, content block, actions at bottom.  
   – Card flips (rotateY) on hover to reveal techStack list.  
   – Live Demo button hidden if liveUrl is blank.

5. Timeline
   – Vertical timeline of milestones (Education, Internship, Capstone) pulled from timeline[] object (students can add or delete).

6. Contact
   – Simple MUI contact form (emailjs integration stubbed) + social icons.

Global features
– MUI dark/light theme toggle in sidebar footer.  
– Alt text on images.  
– SEO meta from siteMeta.  
– TODO comments guiding customisation.

All data lives in `src/data/portfolioData.js`.
```

### Prompt E — "Retro Terminal" (nostalgic green-phosphor CRT)
```
Build a single-page portfolio that emulates an old-school CRT terminal using React 18, Vite, Tailwind CSS, and Typewriter-effect animations (use TypeIt.js or Framer-Motion variants).

Overall vibe: black background, phosphor-green monospace text (#00FF9F), subtle scan-lines and glow.  
Navigation is keyboard-friendly: users press ↑ / ↓ or click "prompt" links to jump between sections.

Sections + features:

1. Boot Screen
   – Simulated BIOS boot log ("BOOTING YOUR NAME PORTFOLIO… OK") with 1 s delays.  
   – Ends with blinking cursor → auto-scrolls into Hero.

2. Hero
   – Big ASCII-style banner:  
     ```
     ███╗   ██╗YOUR NAME
     ████╗  ██║SOFTWARE  ENGINEER
     ████╗ ██║< tagline >
     ```
   – Typewriter effect prints tagline: "Coding the future, one commit at a time_".  
   – Press any key / click to continue hint.

3. About
   – Text appears line-by-line like terminal output:  
     `> about_me --name "YOUR NAME" --bio "ABOUT_PARAGRAPH"`  
   – Fun facts printed via faux-command `> cat funfacts.txt`.

4. Skills
   – Grid of "chip" divs styled as terminal windows (`┌ Ruby ┐`, `└───┘`).  
   – Chips glow brighter on hover; responsive wrap.

5. Projects
   – Each project rendered as a collapsible terminal block:  
     ```
     > open capstone_project
     | Title: CAPSTONE TITLE
     | Stack: Rails API, React, Tailwind
     | Desc : Paragraph description...
     | Demo : https://live-url.com  (omitted if blank)
     | Repo : https://github.com/...
     > _
     ```  
   – Blocks slide down when opened; close on second click.

6. Contact
   – Faux-command prompt:  
     `> connect --email EMAIL --linkedin LINKEDIN_URL --resume RESUME_URL`  
   – Icons rendered with phosphor glow; résumé downloads on click.

Global features
– Dark/light toggle switches from CRT green to modern light theme (white background, navy text).  
– Scan-line overlay (CSS repeating-linear-gradient) toggled via checkbox "CRT mode".  
– Smooth keyboard navigation (keydown listeners) plus traditional scroll-spy.  
– Alt text on all images; minimal images to keep the retro feel.  
– SEO meta tags from siteMeta{ title, description }.  
– TODO comments wherever student customisation is needed.

Put data objects (siteMeta, funFacts, skills, capstone, miniCapstone, projects, resumeURL) in `src/data/portfolioData.js`.
```

**Don't see a style you like?** Feel free to create your own prompt! Mix and match elements from different prompts, or come up with a completely unique design that represents your personality. The prompts above are just starting points—the best portfolios are the ones that feel authentically *you*.

C. Paste the example data block underneath the prompt, then overwrite each field with your own values.

```
// Note: You'll replace the image strings with imported images later in Step 2

const siteMeta = {
  title: "YOUR NAME | Software Engineer",
  description: "Portfolio and project showcase of YOUR NAME"
};

const funFacts = [
  "🏀 Guam National Team basketball player",
  "🍜 Can cook perfect tonkotsu ramen"
];

const skills = [
  "Ruby", "Rails", "JavaScript", "React", "PostgreSQL",
  "HTML", "CSS", "Git", "Docker"
];

const capstone = {
  title: "Capstone Project Title",
  description: "One-paragraph summary of the problem you solved and what you built.",
  techStack: ["Rails API", "React", "Tailwind", "PostgreSQL"],
  image: "capstone.png",    // You'll replace this with imported image
  imageAlt: "Screenshot of Capstone Project",
  liveUrl: "",             // leave blank if not deployed yet
  repoUrl: "https://github.com/yourname/capstone"
};

const miniCapstone = {
  title: "Mini-Capstone Title",
  description: "Brief description of your role and the main feature you built.",
  techStack: ["Node", "Express", "MongoDB"],
  image: "mini-capstone.png",    // You'll replace this with imported image
  imageAlt: "Screenshot of Mini-Capstone Project",
  liveUrl: "",
  repoUrl: "https://github.com/yourname/mini-capstone"
};

const projects = [
  {
    title: "CLI Budget Tracker",
    description: "A command-line app that helps users track monthly expenses.",
    techStack: ["Ruby"],
    image: "budget-cli.png",    // You'll replace this with imported image
    imageAlt: "Screenshot of Budget Tracker CLI",
    liveUrl: "",
    repoUrl: "https://github.com/yourname/budget-cli"
  }
];

const resumeURL = "https://drive.google.com/your-resume.pdf";
```

D. When Bolt finishes, click **Download** to save the ZIP archive.  
E. Unzip, open the folder in VS Code, then run:

```
npm install
npm run dev
```

F. Preview at `http://localhost:5173`.  
G. Create a new GitHub repo, then:

```
git init
git add .
git commit -m "scaffold: initial animated portfolio"
git branch -M main
git remote add origin https://github.com/YOURNAME/portfolio.git
git push -u origin main
```

---

## 3 — Personalise, polish, commit, push  

1. Replace every TODO (Hero, About, project objects).  
2. **Add your images correctly** (see detailed steps below).  
3. Tweak Tailwind colors, spacing, or Framer-Motion timings to match your style.  
4. Commit and push after each milestone (hero done, projects done, etc.).  
5. Mobile check—DevTools ► Responsive mode ► iPhone 15 & iPad widths.  
6. Accessibility pass:  
   * Alt text on images  
   * Color contrast meets WCAG AA  
7. Pair-swap for a five-minute peer review; fix at least one suggestion.

### IMPORTANT: How to Add Images in Vite React Apps for Netlify

**Step 1:** Place all images in the `src/assets/` directory  
```
src/
  assets/
    capstone.png
    mini-capstone.png
    budget-cli.png
    profile-photo.jpg
```

**Step 2:** Import each image in your data file using ES6 imports  
```javascript
// In src/data/portfolioData.js
import capstoneImage from '../assets/capstone.png';
import miniCapstoneImage from '../assets/mini-capstone.png';
import budgetCliImage from '../assets/budget-cli.png';
import profilePhoto from '../assets/profile-photo.jpg';

const capstone = {
  title: "Capstone Project Title",
  description: "One-paragraph summary...",
  techStack: ["Rails API", "React", "Tailwind", "PostgreSQL"],
  image: capstoneImage,  // Use the imported variable
  imageAlt: "Screenshot of Capstone Project",
  liveUrl: "",
  repoUrl: "https://github.com/yourname/capstone"
};
```

**Step 3:** Use the imported images in your components  
```javascript
// In your component
<img src={capstone.image} alt={capstone.imageAlt} />
```

**Why this approach works:**
- Vite processes images in `src/assets/` and gives them hashed names in production
- This ensures images load correctly on Netlify after deployment
- The bundler optimizes images and handles caching automatically  
- Broken image links are caught at build time, not in production

**❌ Don't do this:**
```javascript
// Wrong - string paths won't work reliably in production
image: "/assets/capstone.png"
image: "./assets/capstone.png"
```

**✅ Do this:**
```javascript
// Correct - imported images work everywhere
import capstoneImage from '../assets/capstone.png';
image: capstoneImage
```

*(Remember—this is **your** site. Feel free to redesign components, swap animations, or restructure pages. The scaffold is just a head-start.)*

---

## 4 DEPLOY TO NETLIFY  *(when you’re ready)*  

A. Log in to Netlify.  
B. Add new site → **Import from Git** → select your repo.  
C. If Netlify doesn’t auto-detect, set  
   *Build command* `npm run build`  *Publish directory* `dist`  
D. Click **Deploy site** (≈ 30 s).  
E. Optional: connect a custom domain and verify the HTTPS lock icon.

---

## 5 ONGOING IMPROVEMENTS  *(iterate forever)*  

* Swap in new projects as you build them.  
* Add a blog post (“What I learned building X”).  
* Experiment with more animations—keep them snappy (< 400 ms) and respect prefers-reduced-motion.  
* Run Chrome Lighthouse; aim for scores ≥ 90 in Performance & Accessibility.  
* Keep commit messages meaningful (`feat: add blog section`, `fix: mobile nav overlap`).  
* Total redesign later? Create a new branch or scaffold again—the portfolio grows as you do.

---

## 6 TROUBLESHOOTING QUICK REF  

* **Netlify build fails with “command not found: vite”**  
  ➜ Site settings ► Build & Deploy → Build command `npm run build`, Publish directory `dist`.  

* **Images show locally but 404 on Netlify**  
  ➜ Make sure you're importing images from `src/assets/` with ES6 imports (not string paths)  
  ➜ Paths are case-sensitive in production; check filename casing matches exactly  
  ➜ If using string paths, switch to imported images: `import img from '../assets/image.png'`  

* **Tailwind classes don’t apply**  
  ➜ `tailwind.config.js` needs `./src/**/*.{js,jsx,ts,tsx}` in the `content` array.

---

Build it, refine it, and keep showing the world what you can do—this animated site is the start of your professional story. **Feel free to make the prompt and styling completely your own—what you generate in Bolt is just a launching pad for your creativity.** Have fun and good luck!
