# VKS/Tanzu TAM Field Guide - Agent Handoff Document

This document enables any AI coding agent (OpenAI Codex, Claude Code, Cursor, etc.) or human developer to understand and continue work on this project.

---

## 1. Project Overview

This is a **single-page GitHub Pages static site** that serves as a VKS (vSphere Kubernetes Service) / Tanzu TAM study guide. The target audience is VMware TAMs (Technical Account Managers) working Lenovo enterprise accounts.

All content lives in a single `index.html` file with inline CSS and JS. There is no build step, no bundler, no package.json, and no framework. You edit one file and push.

---

## 2. Tech Stack

| Layer       | Technology                              |
|-------------|----------------------------------------|
| Markup      | Plain HTML5                            |
| Styling     | Inline CSS (inside `<style>` tag)      |
| Scripting   | Inline vanilla JS (inside `<script>`)  |
| Fonts       | Google Fonts: JetBrains Mono, DM Sans  |
| Hosting     | GitHub Pages (main branch, root `/`)   |
| Build step  | None                                   |
| Frameworks  | None - intentionally framework-free    |

---

## 3. Architecture

The entire site is a single `index.html` file with three logical regions:

```
index.html
├── <head>
│   ├── Meta tags, title, favicon
│   └── Google Fonts imports
├── <style>
│   ├── CSS custom properties (--color-*, --spacing-*, etc.)
│   ├── Dark mode defaults (prefers-color-scheme: dark)
│   ├── Light mode overrides (.light-mode class on <body>)
│   ├── Layout (sidebar nav + main content grid)
│   ├── Component styles (cards, accordions, code blocks, tables)
│   ├── Section color-coding classes
│   ├── Responsive breakpoints
│   └── Print stylesheet (@media print)
├── <body>
│   ├── Sidebar nav (sticky, with scroll spy highlighting)
│   ├── Hero / header section
│   ├── Search bar
│   ├── 11 content sections (see Section 5 below)
│   └── Footer
└── <script>
    ├── Dark/light mode toggle (reads/writes localStorage)
    ├── Search filtering (filters sections by text match)
    ├── Collapsible section expand/collapse with animation
    ├── Copy-to-clipboard for code blocks
    ├── localStorage-backed study plan checklist
    └── Scroll spy (highlights active nav link on scroll)
```

---

## 4. Design System

### Color Palette

| Token                | Dark Mode Value | Purpose                    |
|----------------------|-----------------|----------------------------|
| Base background      | `#0f172a`       | Dark navy/slate background |
| Surface              | `#1e293b`       | Cards, sidebar             |
| Electric teal accent | `#06b6d4`       | Links, highlights, CTA     |
| Text primary         | `#f1f5f9`       | Main body text (dark mode) |
| Text secondary       | `#94a3b8`       | Muted/supporting text      |

### Section Color Coding

| Color  | Hex       | Meaning                          |
|--------|-----------|----------------------------------|
| Purple | varies    | Architecture / conceptual topics |
| Teal   | `#06b6d4` | Hands-on / technical workflows   |
| Coral  | varies    | Customer-facing / FAQ content    |
| Amber  | varies    | Warnings, caveats, callouts      |

### Typography

- **Headings / UI**: DM Sans (Google Fonts)
- **Code / monospace**: JetBrains Mono (Google Fonts)

### Modes

- **Dark mode** is the default.
- **Light mode** is toggled via a button that adds/removes a `.light-mode` class on `<body>`.
- User preference is persisted to `localStorage`.

---

## 5. Content Sections

The page contains 11 sections, in order:

| #  | Section                              | Description                                                    |
|----|--------------------------------------|----------------------------------------------------------------|
| 1  | Quick Reference Card                 | At-a-glance VKS facts, versions, key URLs                     |
| 2  | "WTF is VKS?" Explainer             | Plain-language overview of vSphere Kubernetes Service          |
| 3  | VKS 3.6 What's New                  | Feature highlights for the latest release                      |
| 4  | Version History & Upgrade Lifecycle  | Table of VKS versions, dates, EOL status                       |
| 5  | Lenovo Stack                         | ThinkAgile VX + VCF + VKS integration details                 |
| 6  | Hands-On Workflows                   | Step-by-step CLI/UI procedures with code blocks                |
| 7  | Competitive Positioning              | VKS vs. EKS Anywhere, Rancher, OpenShift, etc.                |
| 8  | Customer FAQ                         | Accordion-style Q&A for common TAM conversations               |
| 9  | Learning Resources                   | Links to docs, labs, videos, organized by category             |
| 10 | 3-Day Study Plan                     | Checklist with localStorage persistence                        |
| 11 | Glossary                             | Definitions table for VKS/Tanzu/Kubernetes terminology         |

---

## 6. Key Features to Maintain

These features are implemented in the inline `<script>` block and corresponding CSS. Do not remove or break them:

- **Client-side search** - Filters all sections by text content; hides non-matching sections.
- **Copy-to-clipboard** - Every `<div class="code-block">` has a `<button class="copy-btn">` that copies the code content.
- **Collapsible sections** - Sections expand/collapse with smooth CSS transitions.
- **Sticky sidebar nav with scroll spy** - Left nav highlights the currently visible section on scroll using `IntersectionObserver` or scroll position checks.
- **Dark/light mode toggle** - Persisted to `localStorage` key (check the JS for the exact key name).
- **Study plan checkboxes** - Checkbox state for the 3-Day Study Plan is saved to `localStorage`.
- **Print stylesheet** - `@media print` rules strip nav, dark background, and non-essential UI for clean printing.
- **Responsive layout** - Works on laptop and tablet viewports. Sidebar collapses on smaller screens.
- **Accessibility** - ARIA labels on interactive elements, keyboard-navigable accordions, logical heading hierarchy (`h1` > `h2` > `h3`).

---

## 7. Common Tasks

### Update VKS version info
Search `index.html` for version numbers (e.g., `3.6`, `3.5`). Update in two places:
1. **Quick Reference Card** (Section 1) - the version number shown at a glance.
2. **Version History table** (Section 4) - add or modify rows in the `<table>`.

### Add a new FAQ entry
In Section 8 (Customer FAQ), copy an existing accordion item. The pattern is:
```html
<div class="accordion-item">
  <button class="accordion-header" aria-expanded="false">
    <span>Question text here?</span>
    <span class="accordion-icon">+</span>
  </button>
  <div class="accordion-content">
    <p>Answer text here.</p>
  </div>
</div>
```

### Add a new glossary term
In Section 11 (Glossary), add a new row to the `<table>`:
```html
<tr>
  <td><strong>Term</strong></td>
  <td>Definition text here.</td>
</tr>
```

### Update the "last updated" date
Search for `Last updated` in the hero/header section near the top of `<body>`. Update the date string.

### Add a new code block
Use this pattern anywhere a CLI command or code snippet is needed:
```html
<div class="code-block">
  <button class="copy-btn" aria-label="Copy to clipboard">Copy</button>
  <pre><code>your command or code here</code></pre>
</div>
```
The inline JS already attaches click handlers to all `.copy-btn` elements.

### Add new learning resources
In Section 9 (Learning Resources), find the relevant category (docs, labs, videos, etc.) and add a new list item or link entry following the existing pattern.

---

## 8. Deployment

1. Push changes to the `main` branch of the GitHub repository.
2. GitHub Pages serves the site from the repository root (`/`).
3. No build step, CI pipeline, or artifact generation is required.
4. The site is live as soon as GitHub Pages rebuilds (typically under 1 minute).

To set up GitHub Pages for the first time:
- Go to **Settings > Pages** in the GitHub repo.
- Set source to **Deploy from a branch**.
- Select **main** branch, root (`/`) folder.
- Save.

---

## 9. Known Considerations

- **External links**: All outbound links point to real Broadcom, VMware, and Lenovo documentation pages. If a link breaks, find the updated URL on the vendor's current docs site.
- **Content freshness**: Content is based on publicly available documentation as of April 2026. VKS releases and Lenovo hardware models may change; update accordingly.
- **Not official**: This is NOT an official Broadcom, VMware, or Lenovo resource. It is a personal study aid. Do not represent it as vendor-endorsed.
- **Framework-free by design**: The project is intentionally kept as a single vanilla HTML file. Do not introduce React, Vue, Tailwind, or any framework/build tooling. The simplicity is a feature.
- **No server-side logic**: Everything runs in the browser. There is no backend, no API calls, no database. All persistence uses `localStorage`.
- **File size**: Since everything is in one file, be mindful of file size. Avoid embedding large images inline; use external URLs or keep assets minimal.

---

## 10. File Inventory

```
vks-tanzu-field-guide/
├── index.html    # The entire site (HTML + CSS + JS)
├── agents.md     # This handoff document
└── README.md     # GitHub repo description (if present)
```

There are no other source files. If you see additional files, they were added after this document was written.
