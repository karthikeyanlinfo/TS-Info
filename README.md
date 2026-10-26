# Toxin Solutions — "Crimson Flight" (heavy edition)

A cinematic, effects-heavy landing page for **toxinsolutions.in**. It's plain HTML, CSS and JavaScript with no build step and no external JS libraries. All the 3D and shader work is hand-written WebGL.
Enquiries from the form go straight into **Google Sheets** through **Apps Script**. The site is hosted on **Vercel** and deployed from **GitHub**.

## What's inside the page

| Effect | Where |
|---|---|
| Boot-sequence **preloader** (counter, log, curtain split) | top |
| **16,000 GPU particles** form your bird. It flaps, the particles scatter away from your cursor, and it explodes into a vortex as you scroll | hero |
| Live **crimson nebula shader** (domain-warped noise) + film grain | hero background |
| Giant title that **scales up as you scroll**, plus HUD corners (live FPS, particle count, cursor X/Y, IST clock) | hero |
| **Velocity marquee**: speeds up, skews and reverses with your scroll | after hero |
| **Scroll-lit manifesto**: words light up one by one | Beyond the screen |
| **Horizontal pinned scroll** with animated SVG wireframes, a CSS-3D cube and an automation flow with moving packets | Services |
| **Bento "Capabilities, live"**: streaming chart, typing terminal, variable-font specimen, draggable 3D dot sphere, audio visualiser, heatmap, toolkit rows, cursor spotlight + 3D tilt on every card | Lab |
| **Raymarched obsidian sculpture** rendered live, following your cursor, with difference-blend type | Reel |
| **Stacking sticky cards** for the Orbit / Forma / Pulse concepts | Work |
| **Scroll-drawn glowing path** that lights up each step | Process |
| Counters, rotating halo, floating bird | About |
| Magnetic buttons, animated fields, **particle burst + chime** on submit | Contact |
| Giant wordmark with a **cursor wave** | Footer |
| **Generative sound engine** (Web Audio): drone that opens up when you scroll fast, hover ticks, clicks, chime. **Off by default** | Sound toggle |
| Custom cursor, inertia smooth scroll, scroll progress bar, header hides on scroll down | everywhere |

### It's heavy, but it won't break
- A **performance governor** watches the frame rate and lowers resolution and particle count on slower devices.
- WebGL **pauses when off-screen**, and everything pauses when the tab is hidden.
- Phones get a lighter particle budget and native touch scrolling.
- Visitors with **"Reduce motion"** turned on get a calm, static version.
- No WebGL? The hero falls back to your bird image with a glow.
- Sound always starts **off**, because browsers block autoplay.

---

## Files

```
index.html
privacy.html
404.html
favicon.ico   robots.txt   sitemap.xml   vercel.json
css/
  main.css
js/
  config.js        ← THE ONLY FILE YOU NEED TO EDIT
  bird-points.js   ← particle data made from your logo (don't edit)
  core.js          ← engine: loop, smooth scroll, perf governor, WebGL helpers
  gl-hero.js       ← nebula shader + particle bird
  gl-reel.js       ← raymarched 3D sculpture
  fx.js            ← text effects, cursor, marquee, pinned/horizontal scroll, cards…
  lab.js           ← bento widgets
  audio.js         ← sound engine
  form.js          ← form → Google Sheets
  main.js          ← preloader + boot
assets/            ← bird mark, icons, share image
apps-script/
  Code.gs          ← paste into Google Apps Script (not used by Vercel)
```

---

## Step 1: Preview
Unzip, then double-click `index.html`. Chrome, Edge, Safari and Firefox all work.

## Step 2: Google Sheet + Apps Script
1. Go to <https://sheets.new> and name the sheet **Toxin Solutions — Leads**.
2. Open **Extensions → Apps Script**. Delete everything there and paste in `apps-script/Code.gs`. Click **Save**.
3. Pick **setup** from the function dropdown and click **▶ Run**. Allow the permissions:
   **Advanced → Go to (project) (unsafe) → Allow**. (That warning is normal for your own script.)
4. Go to **Deploy → New deployment → ⚙️ Web app**, with:
   - **Execute as: Me**
   - **Who has access: Anyone**

   Click **Deploy** and copy the URL that ends in `/exec`.
5. Open **`js/config.js`** and paste the URL:
   ```js
   FORM_ENDPOINT: "https://script.google.com/macros/s/AKfy..../exec",
   ```
6. Submit a test enquiry. A row should appear in the **Leads** tab, and you'll get an email alert.

> If you edit Code.gs later: **Deploy → Manage deployments → ✏️ → Version: New version → Deploy**. This keeps the same URL.

## Step 3: GitHub
1. Create a new repository on github.com.
2. Click **uploading an existing file**.
3. Open the unzipped folder, select **everything inside it** (including the `css`, `js`, `assets` and `apps-script` folders) and drag it in. Then **Commit changes**.
4. Check that `index.html` sits at the top level of the repo, next to the `css/` and `js/` folders.

## Step 4: Vercel
Go to **Add New → Project → Import** your repo. Set Framework Preset to **Other**, leave the build command empty, and click **Deploy**.

## Step 5: Your domain
`toxinsolutions.in` is already served by Vercel, so you don't need to change DNS. Either:
- **A.** Put these files into the GitHub repo your current Vercel project uses. It redeploys automatically.
- **B.** Remove the domain from the old project (**Settings → Domains → Remove**) and add it to the new one (**Settings → Domains → Add**).

---

## Tuning (`js/config.js`)

| Setting | What it does |
|---|---|
| `SMOOTH_SCROLL` | Inertia scrolling for mouse wheels (`false` = native) |
| `PRELOADER` | Boot intro (first visit about 2.3s, repeat visits about 0.9s) |
| `SOUND` | Show or hide the sound toggle |
| `CUSTOM_CURSOR` | Crimson cursor on desktop |
| `PARTICLES_DESKTOP` / `PARTICLES_MOBILE` | Particle budget (max 16,000) |

Other easy edits:
- **Colours:** `css/main.css` → `:root` (`--red`, `--void`, `--text`)
- **Text:** `index.html` (every section is labelled)
- **Form options:** `index.html` contact section, plus `points` in `js/form.js`

## Adding a real showreel video (optional)
If you have a showreel, export it as **MP4 (H.264) or WebM at 1080p, under ~8 MB**. Put it in `assets/`, then replace `<canvas class="reel-gl" …>` with:
```html
<video class="reel-gl" src="assets/reel.mp4" autoplay muted loop playsinline style="object-fit:cover"></video>
```
Avoid uncompressed 4K. GitHub rejects files over 100 MB, and huge videos stall on phones.

## Troubleshooting
| Problem | Fix |
|---|---|
| "The form isn't connected yet" | Paste your URL into `js/config.js` |
| "Something went wrong…" | Web app access must be **Anyone**, the URL must end in `/exec`, and you must deploy a **New version** after editing Code.gs |
| Site looks unstyled on Vercel | The `css` and `js` folders weren't uploaded. Upload the folders, not just the files |
| Laggy on an old laptop | Lower `PARTICLES_DESKTOP` (e.g. 9000) |
| No sound | Click the **Sound** button. Browsers never autoplay sound |
