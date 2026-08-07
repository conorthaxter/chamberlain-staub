# Chamberlain Staub — Site Rebuild Prompt for Claude Code

## Context

I'm rebuilding a portfolio website for my client Chamberlain Staub, a Director & Producer based in LA. The project lives at `~/Desktop/chambo/`. There's an existing single-page HTML mockup (`chamberlain-staub.html`) that needs to become a multi-page site with her actual image assets swapped in for the current Squarespace CDN placeholder URLs. She's provided a folder of image files organized by project category, and has specific notes on changes she wants.

The design language is already established: dark background (`#0c0c0b`), cream text (`#e8e0d0`), green accents (`#3a9a4a`), Playfair Display (italic, 900) for display type, Hanken Grotesk (300/400/500) for body. Diagonal grid layout, editorial/cinematic feel. Custom cursor on desktop. Keep this aesthetic consistent across all pages.

---

## Step 1: Set Up Directory Structure

Create this directory structure inside `~/Desktop/chambo/`:

```
~/Desktop/chambo/
├── index.html                    (converted from chamberlain-staub.html)
├── work.html                     (new full portfolio page)
├── assets/
│   ├── images/
│   │   ├── headshot/
│   │   ├── documentary/
│   │   ├── music-videos/
│   │   ├── branded/
│   │   ├── narrative/
│   │   ├── accolades/
│   │   └── logos/
│   └── video/                    (empty for now, future use)
```

## Step 2: Move & Organize Image Assets

Move files from `~/Desktop/chambo/client files/` into the new `assets/images/` structure:

- `client files/Chamberlain Headshot.jpg` → `assets/images/headshot/chamberlain-headshot.jpg`
- `client files/Accolades/PVF_Savethedate_Still_0.webp` → `assets/images/accolades/pvf-savethedate.webp`
- `client files/Accolades/images.jpg` → `assets/images/accolades/emmy.jpg`

For each project subfolder inside `client files/Documentary/`, `client files/Music Videos/`, `client files/Branded/`, and `client files/Narrative/`:
- Move ALL image/video files from each project subfolder into the corresponding `assets/images/{category}/` directory
- Rename files to be web-safe: lowercase, hyphens instead of spaces, no special characters or quotes
- Preserve the project name in the filename (e.g., files from `"Doom" - Great Grandpa (Director)/` become `doom-great-grandpa-01.jpg`, `doom-great-grandpa-02.jpg`, etc.)
- For the `Branded/LOGOS/` folder, move all logo files into `assets/images/logos/`

For project subfolders in `client files/Branded/` that are brand names (House of Rohl, Lightbio, Manscaped, Speedo, TikTok), move their contents into `assets/images/branded/` with the brand name as prefix (e.g., `manscaped-01.jpg`).

**Important**: Some project folders may contain multiple images. Keep ALL of them — they'll be used for Ken Burns crossfade animations on the work page.

After moving everything, list what you found in each folder so I can verify the mapping is correct.

## Step 3: Edit index.html (Homepage)

Copy `~/Desktop/chambo/chamberlain-staub.html` to `~/Desktop/chambo/index.html`, then make these changes:

### 3a. Change subtitle text
Change:
```html
<div class="hero-subtitle">Director &nbsp;·&nbsp; Cinematographer &nbsp;·&nbsp; Storyteller</div>
```
To:
```html
<div class="hero-subtitle">Director &nbsp;·&nbsp; Producer</div>
```

Also update the page `<title>` to: `Chamberlain Staub — Director & Producer`

Also update the about section label from "Director & Cinematographer" to "Director & Producer" (the `.about-img-label` and `.footer-line` references).

### 3b. Remove the shatter/scatter scroll effect

The client doesn't like the interactive name or the "ghosty" disappearing scroll behavior. Make these changes:

1. **Remove all `<span class="letter">` wrapping** from the hero name. Replace the letter-by-letter spans with just plain text:
```html
<div class="hero-name" id="heroName">
  <div>Chamberlain</div>
  <div>Staub</div>
</div>
```

2. **Remove the `.letter` CSS class** and any related styles.

3. **Remove the credits crawl** (the `<div class="credits-crawl">` and its contents) — she called it "the little scrolly thing". Remove the `.credits-crawl` and `.credits-inner` and `.credit-item` CSS as well.

4. **Remove the shatter/scatter JavaScript entirely** — delete the `shatterText()` function, the `resetText()` function, the scroll listener that triggers them, and all the `letters` variable references.

5. **Make the nav logo always visible** — remove the `.nav-logo` opacity:0 default and the `.nav-logo.visible` toggle. Just set it to `opacity: 1` always. Remove the JS that toggles the `visible` class.

6. **Remove the transition overlay** div and its CSS.

7. The hero name should simply fade in on load (keep the `heroReveal` animation) and stay put. No interactivity, no scroll response.

### 3c. Add Accolades section to the About area

Insert an accolades section within or just after the existing `#about` section. Use the data from her current site's about page. The accolades should have:

- A section eyebrow label "OFFICIAL SELECTIONS / AWARDS" in the same green uppercase style
- The PhotoVogue Festival image (`assets/images/accolades/pvf-savethedate.webp`) displayed to the left, similar to how she has it on her current site (see the attached screenshot for reference)
- Each award as a row with:
  - The award name in green, linked to the press article URL (opens in new tab)
  - A pipe separator
  - The project name in cream
  - The year right-aligned
- Links and URLs for each award (from her current site):
  - **PhotoVogue Festival** | 'Companion of the Setting Sun' | 2026 → https://www.vogue.com/article/women-by-women-the-shortlist
  - **SXSW Official Selection** | 'Doom' | 2026 → https://schedule.sxsw.com/films/2241673
  - **Emmy** | Best Directing Team for a Single Camera Daytime Non-Fiction Program | Blue Zones | 2024 → https://www.bluezones.com/news/live-to-100-wins-3-daytime-emmy-awards/
  - **Mountainfilm Micro Grant** | 'Companion of the Setting Sun' | 2025 → https://www.mountainfilm.org/press-releases/mountainfilm-spotlights-global-stories-with-2025-commitment-grants/

Style this to match the existing credit-row aesthetic but with the green linked names.

### 3d. Update "More Work" link

Change the "More Work" `<a href="#">` to point to `work.html`.

### 3e. Replace all Squarespace CDN image URLs

Replace every `https://images.squarespace-cdn.com/...` URL in the homepage with the correct local path from `assets/images/`. Match each project image to the file you moved in Step 2. For the headshot in the about section, use `assets/images/headshot/chamberlain-headshot.jpg`.

If you can't find a local file match for a specific project card, leave a `<!-- TODO: need local image for [project name] -->` comment.

### 3f. Update nav links

The nav should have: Work (links to `#work`), About (links to `#about`), Contact (links to `#contact`), and the Instagram icon (already there). Add a "More Work" or "Full Portfolio" link that goes to `work.html`.

---

## Step 4: Build work.html (Full Portfolio Page)

Create a new `work.html` page that serves as the full portfolio. It should:

### 4a. Share the same design system
- Same CSS variables, fonts, cursor, nav, and footer as index.html
- Extract shared CSS into a comment block at the top so I can later move it to a shared stylesheet if I want
- Nav should link back to `index.html` for the logo and Work link

### 4b. Page structure

The page should have:
1. **Nav bar** (same as homepage, logo links to index.html)
2. **Page header** with "Selected Work" eyebrow and a large "Portfolio" or "Work" title in the Playfair italic style
3. **Category filter/tabs** — horizontal row of category labels (All, Documentary, Music Videos, Branded, Narrative) that filter which projects are visible. Use CSS classes + minimal JS to toggle visibility. The "All" tab shows everything. Style tabs in the green uppercase Hanken Grotesk style, with the active tab getting a bottom border or full green color.
4. **Project grid** — projects displayed in a grid (2 or 3 columns on desktop, 1-2 on mobile)
5. **Footer** (same as homepage)

### 4c. Project card design

Each project card should have:
- **Image area** with Ken Burns crossfade animation (details below)
- **Project title** in Playfair italic
- **Role** in small green uppercase text (e.g., "DIRECTOR", "PRODUCER", "STORY PRODUCER")
- **Platform/network badge** where applicable (Netflix, Disney+, etc.) using the gold badge style from the homepage
- The entire card links to the project's external URL (YouTube, Vimeo, Instagram, or website)

### 4d. Ken Burns crossfade animation

For projects with multiple images:
- Stack all images in the same card using `position: absolute`
- Each image gets a CSS animation that slowly scales from `scale(1.0)` to `scale(1.08)` while fading in/out
- Stagger the animations so images crossfade on a ~5-6 second cycle per image
- Total animation duration = (number of images × 5-6 seconds)
- Animation is `infinite` loop
- On hover, `animation-play-state: paused` so the user can examine the current frame
- Use CSS `@keyframes` only — no JavaScript for the animation itself

For projects with a single image:
- Just apply the slow Ken Burns zoom (scale 1.0 → 1.05 over ~8 seconds, infinite alternate)
- Same hover pause behavior

### 4e. Project data

Here is every project organized by category, with titles, roles, and external links pulled from her current Squarespace site. Use the local image files you moved in Step 2. If a project folder had multiple images, use them all for the Ken Burns crossfade.

**DOCUMENTARY:**

| Project | Role | Link |
|---------|------|------|
| Live to 100: Secrets of the Blue Zones | Producer | https://youtu.be/it-8MIm29bI?si=wAv_CW40i2xF7mMv |
| Chef's Table - S7 | Story Producer | https://www.youtube.com/watch?v=wk5hRoJKxB0 |
| First to the Finish | Supervising Producer | https://youtu.be/bWzKk5FheEg?si=ov3OuLJd3UcDFAcF |
| Companion of the Setting Sun | Producer · Upcoming | https://www.instagram.com/companionofthesettingsunfilm |
| Shark Whisperer | Story Producer | https://youtu.be/EzFguo4vEAQ?si=0wPftw92WtUiRWSj |
| Grandmother of Xochimilco | Producer | https://vimeo.com/1133495645 |
| More Than Robots | Associate Producer | https://www.youtube.com/watch?v=AjIISbARc20 |
| Olivia Rodrigo: Driving Home 2 U | Production Coordinator | (no external link — view fullsize on current site) |
| Sketchbook | Production Coordinator | https://www.youtube.com/watch?v=P8stiIE-8oI |
| Being Eddie | Field Producer | https://www.youtube.com/watch?v=98xUkIDUPVg |
| The Me You Can't See | Field Coordinator | https://www.youtube.com/watch?v=dWevopoBmAE |
| The Trials of Gabriel Fernandez | Production Coordinator | https://www.youtube.com/watch?v=-T7VXlB4qUI |
| Obi-Wan Kenobi: A Jedi's Return | Field Producer | https://www.youtube.com/watch?v=4WaC8k3WVl8 |
| Gloria's Garden | DP + Director | https://youtu.be/5ALztH_eNiI?si=W07sCiQx27EQN70n |
| Melba | (check folder for role) | (no link on current site — check if one exists) |

**MUSIC VIDEOS:**

| Project | Role | Link |
|---------|------|------|
| "Doom" - Great Grandpa | Director | https://youtu.be/heXjjQLnKuc?si=dOeSlf1dAPl6gSAj |
| "Ducktales" - R.O.O.F | Producer | https://vimeo.com/1132614343 |
| "Xerox" - Thomas Aren | Producer | https://www.youtube.com/watch?v=VN2zc2-i_OM |
| "Low" (Live Cover) - Arswain | Director | https://www.youtube.com/watch?v=7THn8Wn1sSM |
| "Werewoof" - Thomas Aren | Co-Director | https://www.youtube.com/watch?v=xn4GZUxrQlk |
| "Whippoor Will" - Max Look | Producer | https://www.youtube.com/watch?v=DoYx6mL1QkU |
| "Hollywood 4ever" - Conor James | Co-Director | https://www.youtube.com/watch?v=_bQMuJ8iXkY |
| "2 Months" - Jessica Vines | Director | https://www.youtube.com/watch?v=6aY77iqty58 |
| "Childish" - Thomas Aren | Producer | https://www.youtube.com/watch?v=f1UuWEMQCuk |
| Bride | (check folder for details) | (check for link) |
| Garden | (check folder for details) | (check for link) |
| Why Do U Say u Love Me | (check folder for details) | (check for link) |

**BRANDED:**

| Project | Role | Link |
|---------|------|------|
| "Be Your Man" - Manscaped | Producer | https://vimeo.com/1126311540 |
| "Swim Gains Campaign" - Speedo | Producer | https://endurance.biz/2026/industry-news/speedo-spotlights-swim-fitness-benefits-in-new-swim-gains-campaign/ |
| "2026 Spring Campaign" - Lightbio | Director | (view fullsize on current site) |
| House of Rohl | (check folder for role) | (check for link) |
| TikTok | (check folder for role) | (check for link) |

Display the client logos (from `assets/images/logos/`) in a subtle logo bar at the top or bottom of the Branded section — small, desaturated, cream-colored logos on the dark background.

**NARRATIVE:**

| Project | Role | Link |
|---------|------|------|
| Likeness | Producer · Upcoming | (no external link) |
| Sisters | Producer | (no external link) |

### 4f. Badges and special callouts

- Projects that have won awards or have festival selections should get badge treatment:
  - "Doom" → `SXSW '26` badge (green border)
  - Chef's Table → `Emmy Winner` badge (gold)
  - Blue Zones → `Netflix` badge (gold/branded)
  - Companion of the Setting Sun → `PhotoVogue 2026` + `Mountainfilm` badges
- "Upcoming" projects should get a subtle `~ UPCOMING` label

### 4g. Mobile responsive

- Category tabs should horizontally scroll on mobile
- Grid collapses to 1 column on mobile
- Cards get larger touch targets
- Same mobile nav behavior as homepage

---

## Step 5: Wire Everything Together

1. Homepage "More Work →" link goes to `work.html`
2. `work.html` nav logo links back to `index.html`
3. Project cards on the homepage diagonal grid should link to the corresponding external URL (same as on work.html)
4. All relative image paths should work correctly from both `index.html` and `work.html` (both are in the root of the project)
5. Test that no broken image paths remain — flag any `<!-- TODO -->` comments for images you couldn't find

---

## Step 6: Final Cleanup

1. Remove the original `chamberlain-staub.html` from the root (it's now `index.html`)
2. Verify all links work (internal nav + external project links)
3. Add `<meta>` tags for SEO: description, Open Graph tags, etc.
4. Make sure the `<title>` on work.html is "Work — Chamberlain Staub | Director & Producer"
5. Add a favicon link (she may not have one yet — leave a TODO comment)
6. Check that the custom cursor, scroll reveals, and hover effects all work on both pages

---

## Design Notes

- Keep the brutalist editorial energy. Dark, cinematic, confident.
- The Ken Burns crossfade should feel like slowly breathing film stills — not a slideshow. Smooth, barely perceptible transitions.
- Typography hierarchy: Playfair Display italic for headings/titles, Hanken Grotesk light for body, Hanken Grotesk medium for labels/nav.
- Green (`#3a9a4a`) is used for: labels, links, badges, accents. Never for large fills.
- Cream (`#e8e0d0`) at 50% opacity for secondary text.
- Hover states should feel deliberate — scale, desaturate/saturate shifts, content reveals.
- The diagonal grid on the homepage is a signature element. The work.html page should feel like an extension of that world but use a cleaner grid for scannability.

---

## Summary of Deliverables

After running this prompt, Claude Code should produce:
- `~/Desktop/chambo/index.html` — updated homepage
- `~/Desktop/chambo/work.html` — new full portfolio page
- `~/Desktop/chambo/assets/images/` — organized image assets moved from client files
- A list of any missing assets or TODOs flagged during the process
