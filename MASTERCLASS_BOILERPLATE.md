# Masterclass Course Boilerplate

This document describes how to replicate the AI Masterclass course structure, styling, and interactivity for new masterclasses. Treat the course as a **collection of concepts** (modules); you can organize them in one list or group them into sections (e.g., "Part 1", "Part 2") in the sidebar.

---

## 1. File & Folder Structure

```
course/
├── index.html              # Main shell: sidebar + content area, loads welcome + modules in iframes
├── modules/
│   ├── welcome.html        # Landing page with course overview and "Start Learning" CTA
│   ├── module-1.html       # First concept/module
│   ├── module-2.html
│   └── ...                 # module-N.html (or section-module-N.html if you use sections)
```

- **Single entry point:** `index.html` (or e.g. `your-masterclass.html`).
- **One HTML file per module** (and one for welcome). Each module is loaded in an iframe.
- Module count and sidebar labels are configured in the main shell’s HTML and JS.

---

## 2. Main Shell (index.html) – Structure

### 2.1 HTML Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Masterclass Title</title>
    <style>/* see Section 4 – Main Shell CSS */</style>
</head>
<body>
    <button class="mobile-menu-toggle" id="mobile-menu-toggle" aria-label="Toggle menu">☰</button>
    <div class="sidebar-overlay" id="sidebar-overlay"></div>
    <div class="container">
        <div class="main-wrapper">
            <div class="sidebar" id="sidebar">
                <div class="sidebar-title" onclick="showWelcome()">Course Title</div>
                <div class="sidebar-tabs">
                    <!-- Optional: section subheadings -->
                    <div class="sidebar-subheading">Section A</div>
                    <button class="sidebar-tab active" onclick="showModule(1)">Module 1: Name</button>
                    <button class="sidebar-tab" onclick="showModule(2)">Module 2: Name</button>
                    <!-- ... -->
                    <div class="sidebar-subheading">Section B</div>
                    <button class="sidebar-tab" onclick="showModule(10)">Module 10: Name</button>
                </div>
            </div>
            <div class="content">
                <div id="welcome-container" class="module-content active">
                    <iframe id="welcome-iframe" class="module-iframe" src="modules/welcome.html"></iframe>
                </div>
                <div id="module-container" class="module-content" style="display: none;">
                    <!-- Iframes created by JS -->
                </div>
            </div>
        </div>
    </div>
    <script>/* see Section 6 – Main Shell JS */</script>
</body>
</html>
```

- **Sidebar:** `.sidebar` with `.sidebar-title` (click → welcome) and `.sidebar-tabs` (buttons with `onclick="showModule(N)"`).
- **Content:** Two zones—`#welcome-container` (welcome iframe) and `#module-container` (module iframes). Only one is visible at a time.
- **Overlay:** `.sidebar-overlay` for mobile: dims content and closes sidebar on click.

### 2.2 Sidebar Conventions

- **Single list of concepts:** Only `.sidebar-tab` buttons, no `.sidebar-subheading`.
- **Grouped concepts:** Use `<div class="sidebar-subheading">Label</div>` between groups; tabs still use `showModule(1)`, `showModule(2)`, etc. Tab index = module number (1-based).
- First tab is typically `active` in the initial markup (for when user skips welcome via URL/localStorage).

---

## 3. Content Structure (Modules & Welcome)

### 3.1 Welcome Page (modules/welcome.html)

- **Purpose:** Introduce the course and provide a single CTA to enter the course (hides welcome, shows first module).
- **Layout:**
  - **Title:** `<h1>` + optional `<p class="subtitle">`.
  - **Stats (optional):** `<div class="stats">` with several `<div class="stat-item">`, each containing `.stat-value` and `.stat-label` (e.g. module count, duration).
  - **Sections:** `<h2>`, `<p>`, `<ul>` as needed.
  - **Callout boxes:** `.info-box` (blue accent), `.highlight-box` (yellow/warning accent).
  - **Course structure (optional):** `<div class="course-structure">` with grid of `.section-card` (each with `.section-title` and content like `.module-list`).
  - **CTA:** One button that triggers start:
    ```html
    <button class="start-button" onclick="window.parent.postMessage({type: 'startCourse'}, '*')">
        Start Learning → Begin with Module 1
    </button>
    ```
- **Important:** Parent listens for `event.data.type === 'startCourse'` and calls `startCourse()` (which hides welcome, shows `#module-container`, creates iframes if needed, shows module 1).

### 3.2 Module Pages (modules/module-N.html)

Each module is a full HTML document (with its own `<head>` and `<style>`) loaded in an iframe. Structure:

1. **Module title:** One `<h2>` at the top (e.g. "Module 1: Concept Name").
2. **Concept blocks:** Use a consistent set of content blocks (see below).
3. **Optional quiz:** A single `.quiz-section` at the end (see Quizzes).

**Content blocks (use as needed):**

| Block | Class | Use |
|-------|--------|-----|
| Concept / example card | `.example-box` | White card, border, padding; wrap a concept or example with `<h3>`, `<p>`, lists. |
| Interactive demo | `.interactive-container` | Border in primary color; contains canvas, sliders, buttons, or other controls. |
| Info / tip | `.info-box` | Blue left border, light blue background. |
| Warning / tip | `.highlight-box` or `.warning-box` | Yellow left border. |
| Success | `.success-box` | Green left border. |
| Inline highlight | `<span class="highlight">` | Yellow background for terms. |
| Tables | `.truth-table` or generic `table` | Styled in module CSS (header #667eea, stripes). |
| Step-by-step | `.step-box` | For procedures; optional `<h4>` inside. |
| Calculation / code | `.calculation-box` | Monospace, blue accent. |
| Sliders | `.slider-container` → `.slider-group` → `.slider-wrapper` with `<input type="range" class="slider">` and `.slider-value`. |
| Buttons | `<button>` | Primary style in module CSS (purple, hover darker). |

**Heading hierarchy:**

- **h2** – Module title (once per page).
- **h3** – Major sections (concept, example, quiz title).
- **h4** – Subsections within an example or question.

### 3.3 Quizzes (inside a module)

- **Container:** `<div class="quiz-section">` with `<h3>📝 Module Quiz</h3>` (or similar).
- **Per question:**
  ```html
  <div class="quiz-question">
      <h4>Question N: Question text?</h4>
      <ul class="quiz-options">
          <li>A) Option A</li>
          <li>B) Option B</li>
          <!-- C, D ... -->
      </ul>
      <button class="show-answer-btn" id="btn-N" onclick="showAnswer(N)">Show Answer</button>
      <div class="quiz-answer" id="answer-N">
          <strong>Correct Answer: X</strong><br>
          Explanation text.
      </div>
  </div>
  ```
- **Behavior:** `showAnswer(questionNum)` toggles the corresponding `.quiz-answer` to visible (e.g. add class `show`), disables the button, and optionally changes button text to "Answer Revealed". No automatic scoring; answers are reveal-on-click only.

---

## 4. Main Shell CSS (Inline in index.html)

These styles power the layout, sidebar, and responsiveness. Keep them in a single `<style>` in `<head>`.

### 4.1 Reset & Base

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
}
html { -webkit-text-size-adjust: 100%; -ms-text-size-adjust: 100%; }
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    margin: 0;
    padding: 0;
    height: 100vh;
    overflow: hidden;
}
```

### 4.2 Layout

```css
.container {
    width: 100%;
    min-height: 100vh;
    background: white;
    display: flex;
    flex-direction: column;
    position: relative;
}
.main-wrapper {
    display: flex;
    flex: 1;
    position: relative;
    min-height: 0;
}
.sidebar {
    width: 280px;
    background: #f8f9fa;
    border-right: 2px solid #e0e0e0;
    padding: 25px 20px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    overflow-y: auto;
    overflow-x: hidden;
    height: 100vh;
    max-height: 100vh;
    transition: transform 0.3s ease;
    -webkit-overflow-scrolling: touch;
}
.content {
    flex: 1;
    padding: 0;
    position: relative;
    overflow-y: auto;
    overflow-x: hidden;
    width: 100%;
    height: 100vh;
    -webkit-overflow-scrolling: touch;
}
```

### 4.3 Sidebar Typography & Tabs

```css
.sidebar-title {
    font-size: 1.8em;
    font-weight: 700;
    color: #667eea;
    margin-bottom: 5px;
    padding-bottom: 15px;
    border-bottom: 2px solid #e0e0e0;
    cursor: pointer;
    transition: color 0.3s ease;
}
.sidebar-title:hover { color: #764ba2; }
.sidebar-subheading {
    font-size: 0.85em;
    font-weight: 700;
    color: #764ba2;
    margin-top: 20px;
    margin-bottom: 10px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}
.sidebar-tabs { display: flex; flex-direction: column; gap: 8px; }
.sidebar-tab {
    padding: 12px 15px;
    cursor: pointer;
    border: none;
    background: transparent;
    font-size: 0.9em;
    font-weight: 500;
    color: #666;
    text-align: left;
    transition: all 0.3s ease;
    border-radius: 6px;
    border-left: 3px solid transparent;
    touch-action: manipulation;
    -webkit-tap-highlight-color: transparent;
}
.sidebar-tab:hover { background: #e8e8e8; color: #333; }
.sidebar-tab:active { transform: scale(0.98); }
.sidebar-tab.active {
    color: #667eea;
    background: #e8f0ff;
    border-left-color: #667eea;
    font-weight: 600;
}
```

### 4.4 Mobile Menu Button & Overlay

```css
.mobile-menu-toggle {
    display: none;
    position: fixed;
    top: 15px;
    left: 15px;
    z-index: 1001;
    background: #667eea;
    color: white;
    border: none;
    padding: 12px 15px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 1.2em;
    box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    transition: all 0.3s ease;
}
.mobile-menu-toggle:hover { background: #764ba2; transform: scale(1.05); }
.mobile-menu-toggle.active { background: #764ba2; }
.sidebar-overlay {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 999;
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
}
.sidebar-overlay.active { opacity: 1; pointer-events: auto; }
@media (min-width: 769px) {
    .sidebar-overlay {
        display: none !important;
        pointer-events: none !important;
        opacity: 0 !important;
    }
}
```

### 4.5 Module Content & Iframes

```css
.module-content { display: block; animation: fadeIn 0.3s ease; width: 100%; }
.module-iframe {
    width: 100%;
    border: none;
    display: none;
    background: white;
    margin: 0;
    padding: 0;
    vertical-align: top;
}
.module-iframe.active { display: block; }
#welcome-iframe {
    display: block;
    width: 100%;
    border: none;
    margin: 0;
    padding: 0;
}
#module-container { width: 100%; position: relative; }
@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}
```

### 4.6 Responsive Breakpoints (Main Shell)

- **≤1024px:** Slightly narrower sidebar (260px), smaller title.
- **≤768px:** Show mobile menu button and overlay; sidebar becomes fixed, off-screen by default (transform: translateX(-100%)), and gets `.open` to slide in; content full width; touch-friendly tab height (min-height: 44px).
- **≤480px:** Smaller padding and font sizes; sidebar can go full width.
- **≤360px:** Even tighter padding/fonts.
- **Landscape (max-height: 500px):** Reduced sidebar padding and tab size.

Always include the overlay and mobile menu rules so the sidebar works on small screens.

---

## 5. Module & Welcome CSS (Inline in Each HTML)

Each of `welcome.html` and `module-N.html` includes its own `<style>` in `<head>`. Shared conventions:

### 5.1 Color Palette (Theming)

| Role | Hex | Usage |
|------|-----|--------|
| Primary | `#667eea` | Headings, primary buttons, borders, active tab |
| Secondary | `#764ba2` | H3, secondary accents, gradient end |
| Background light | `#f9f9f9`, `#f8f9fa` | Body, cards |
| Border | `#e0e0e0`, `#ddd` | Borders, tables |
| Text | `#444`, `#333` | Body, headings |
| Success | `#27ae60`, `#28a745` | Correct answer, success box |
| Error / no | `#e74c3c` | Wrong, alerts |
| Warning / highlight | `#ffc107`, `#fff3cd` | Highlight box, step box |

### 5.2 Welcome-Specific

- **Body:** Gradient background, white text outside card; padding.
- **.welcome-container:** Max-width 900px, centered, white background with opacity, large border-radius, box-shadow.
- **.stats:** Flex/grid of stat items; `.stat-value` (large, primary color), `.stat-label` (smaller, gray).
- **.course-structure:** CSS Grid (e.g. 1fr 1fr); gap; on small screens switch to 1 column.
- **.section-card:** Light gray bg, border, border-radius, padding.
- **.start-button:** Full-width, gradient bg, white text, rounded, hover lift + shadow.
- **.info-box / .highlight-box:** Same semantics as in modules (left border + padding).

### 5.3 Module-Specific (Shared Across Modules)

- **body:** `background: #f9f9f9`, padding 20px (smaller on mobile).
- **h2:** `#667eea`, bottom border, margin.
- **h3:** `#764ba2`, section spacing.
- **h4:** `#667eea`, smaller than h3.
- **.example-box:** White, 2px border #e0e0e0, border-radius 8px, padding 25px, box-shadow.
- **.interactive-container:** White, 2px solid #667eea, border-radius 8px, padding 20px.
- **.info-box:** Background #e8f4f8, border-left 4px #667eea, padding 15px, border-radius 4px.
- **.highlight-box / .warning-box:** Background #fff3cd, border-left 4px #ffc107.
- **.success-box:** Background #d4edda, border-left 4px #28a745.
- **.highlight:** Inline yellow background, padding, border-radius.
- **button:** Purple bg, white text, rounded, hover darker + translateY(-2px); disabled: opacity 0.5.
- **.slider:** Range input; custom thumb (e.g. 18px circle #667eea); `.slider-value` for numeric display.
- **Tables:** border-collapse, th #667eea white text, td padding, tr:nth-child(even) background.
- **.quiz-section:** White, 2px solid #667eea, border-radius 8px, padding 30px, box-shadow.
- **.quiz-question:** Light gray bg, left border #764ba2, padding 20px.
- **.quiz-options li:** White bg, border, rounded, padding, cursor pointer; hover border #667eea.
- **.quiz-answer:** Initially `display: none`; with `.show` → `display: block`; green left border, padding.
- **.show-answer-btn:** Green button; disabled state gray.

### 5.4 Responsive (Modules & Welcome)

- **768px:** Reduce padding, font sizes for h2/h3/h4/p; stack slider label and value; full-width buttons; canvas `max-width: 100%`; quiz padding reduced.
- **480px:** Further reduce padding and font sizes; smaller tables.
- **Landscape + short height:** Limit canvas max-height so layout stays usable.

---

## 6. Main Shell JavaScript (index.html)

### 6.1 Configuration

- `totalModules` – number of module iframes to create (e.g. 18).
- Module URLs: either one pattern (e.g. `modules/module-${i}.html`) or two (e.g. 1–9 → `module-${i}.html`, 10–18 → `business-module-${i - 9}.html`). Adjust in `initializeIframes()`.

### 6.2 Core Behaviors

1. **Mobile menu**
   - Toggle `.open` on `.sidebar`, `.active` on overlay and `.mobile-menu-toggle`.
   - On overlay click or tab/title click (when width ≤ 768): close sidebar and overlay.
   - On resize to > 768: close sidebar.
   - Optional: when sidebar is open, set `body` overflow hidden to prevent background scroll.

2. **Welcome vs modules**
   - Initially show `#welcome-container`, hide `#module-container`.
   - On `postMessage` with `type: 'startCourse'`: call `startCourse()`.
   - `startCourse()`: set `courseStarted = true`, persist in `localStorage.setItem('courseStarted', 'true')`, hide welcome, show `#module-container`, call `initializeIframes()` if not yet done, then `showModule(1)`.

3. **Iframe creation**
   - `initializeIframes()`: for each `i` from 1 to `totalModules`, create an iframe with id `module-iframe-${i}`, class `module-iframe`, `scrolling="no"`, and `src` set from your URL pattern. Append to `#module-container`. Store in object `iframes[i]`. Set initial height (e.g. 3000px) so content is visible before measurement.

4. **Iframe height**
   - When an iframe loads (and on window resize), read `contentDocument.body.scrollHeight` (and documentElement heights) and set iframe height to `contentHeight + 40` (or similar) so the parent `.content` scrolls instead of the iframe. Retry after 100, 500, 1000, 2000 ms if height is still small (content may load async).
   - Welcome iframe: same idea; adjust height on load and after short delays.

5. **Showing a module**
   - `showModule(moduleNumber)`: if `!courseStarted`, call `startCourse()` first. Set `currentModule = moduleNumber`. Remove `.active` from all module iframes, add `.active` to `iframes[moduleNumber]`. Remove `.active` from all `.sidebar-tab`, add to the tab that corresponds to `moduleNumber` (by index: tab at index `moduleNumber - 1`). Call `adjustIframeHeight` for the active iframe. Optionally close mobile sidebar and scroll `.content` to top.

6. **Showing welcome**
   - `showWelcome()`: show `#welcome-container`, hide `#module-container`, clear active state from all tabs, close mobile menu if open, scroll content to top.

7. **Skip welcome**
   - On DOMContentLoaded: if `localStorage.getItem('courseStarted') === 'true'` or URL param (e.g. `?skipWelcome=true`), call `startCourse()`; otherwise leave welcome visible.

8. **Keyboard**
   - Arrow Left: `showModule(currentModule - 1)` if ≥ 1.
   - Arrow Right: `showModule(currentModule + 1)` if ≤ totalModules.

9. **Scrolling**
   - Ensure `.content` has `overflow-y: auto` and a fixed height (e.g. `100vh` or `window.innerHeight`) so the main scroll happens in the shell, not inside the iframe (iframe height is set to content height).

---

## 7. Interactivity Inside Modules

- **Canvas:** Use `<canvas id="...">` and JS to draw (e.g. charts, diagrams). On window resize, recompute dimensions and redraw; use `max-width: 100%` and proportional height in CSS so it’s responsive.
- **Sliders:** `<input type="range" class="slider">` with `input` listener; update a `.slider-value` span and any visualization (e.g. redraw canvas).
- **Buttons:** `onclick="someFunction()"` to trigger demo steps (e.g. "Show separating line", "Clear", "Run").
- **Quizzes:** Only "Show Answer" pattern: button calls `showAnswer(N)`, which shows `#answer-N` and disables the button. No submission or scoring.
- **postMessage:** Only from welcome page: `window.parent.postMessage({ type: 'startCourse' }, '*')` to enter the course.

---

## 8. Checklist for a New Masterclass

1. **Copy** the main shell HTML (with embedded CSS and JS) and rename (e.g. `data-masterclass.html`).
2. **Set** `<title>` and `.sidebar-title` to the course name.
3. **Set** `totalModules` and the iframe `src` pattern(s) in `initializeIframes()`.
4. **Build** sidebar: one or more `.sidebar-subheading` + `.sidebar-tab` with correct `onclick="showModule(N)"` (N from 1 to totalModules).
5. **Create** `modules/welcome.html`: same structure and classes as above; CTA must post `startCourse`.
6. **Create** each `modules/module-N.html` (or section-module-N): same CSS patterns, one h2, then example-box / interactive-container / info-box / tables / quiz-section as needed.
7. **Optional:** Change primary/secondary colors by replacing `#667eea` and `#764ba2` across the main shell and module CSS for a different theme.
8. **Test:** Desktop, 768px, 480px; mobile menu open/close; welcome → start → module 1; keyboard arrows; iframe height and scrolling.

---

## 9. Quick Reference – Key Classes

| Context | Class | Purpose |
|---------|--------|--------|
| Shell | `.container`, `.main-wrapper` | Layout wrapper |
| Shell | `.sidebar`, `.sidebar-title`, `.sidebar-tabs`, `.sidebar-tab`, `.sidebar-subheading` | Navigation |
| Shell | `.content`, `.module-content`, `.module-iframe`, `.module-iframe.active` | Content area and iframes |
| Shell | `.mobile-menu-toggle`, `.sidebar-overlay`, `.sidebar.open`, `.sidebar-overlay.active` | Mobile menu |
| Welcome | `.welcome-container`, `.stats`, `.stat-item`, `.course-structure`, `.section-card`, `.start-button` | Welcome layout and CTA |
| Module | `.example-box`, `.interactive-container`, `.info-box`, `.highlight-box`, `.success-box`, `.warning-box` | Content blocks |
| Module | `.slider-container`, `.slider-group`, `.slider`, `.slider-value` | Sliders |
| Module | `.quiz-section`, `.quiz-question`, `.quiz-options`, `.quiz-answer`, `.show-answer-btn` | Quiz |
| Module | `table`, `.truth-table` | Tables |

Using this boilerplate, you can replicate the same responsive layout, navigation, welcome flow, and module structure (including quizzes and interactive blocks) for any new masterclass.
