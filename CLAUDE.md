# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio website for Ram Harikrishnan (ramharikrishnan.dev). Fully static site — no build tools, no frameworks, no bundlers. Deployed on Railway via git push.

## Deployment

- **Hosting**: Railway (project: `ram-portfolio`, service: `ram-portfolio`)
- **Deploy**: `git push origin main` triggers auto-deploy, or `railway up -d -s ram-portfolio`
- **Domain**: ramharikrishnan.dev
- **Repo**: github.com/shankerram3/portfolio-website

There is no build step. Railway serves the static files directly.

## Architecture

Single-page static HTML site with inline CSS and minimal inline JS.

- `index.html` — Main portfolio page with all styles/scripts inline. Sections: home, experience, projects, skills, blog, education, achievements, contact.
- `blog-*.html` — Individual blog post pages. Each is standalone with its own inline styles matching the site design system.
- `admin.html` — Password-protected blog post editor. Client-side only (sessionStorage auth, localStorage drafts). Generates blog HTML matching the theme. Hidden from search engines (noindex).
- `images/` — Static assets.
- `resume.pdf` — Downloadable resume.

## Design System

Editorial-technical: near-black canvas with a single electric-chartreuse signal accent. (Redesigned 2026 — replaced the older cyan/purple gradient theme.) Positioned for freelance/contract clients as well as employers.

- **Colors**: `--bg: #0a0b0c`, `--bg-2: #101214`, `--panel: #131619`, `--line: #22272b`, `--line-soft: #1a1e22`, `--ink: #f2f4ef`, `--ink-dim: #9aa3a0`, `--ink-faint: #5f6a66`, `--accent: #c8f135` (chartreuse), `--accent-deep: #9bbd1f`. Chartreuse RGB for glows/tints is `200, 241, 53`.
- **Fonts** (Google Fonts): `Instrument Serif` (display/headings, often italic for emphasis), `JetBrains Mono` (labels, eyebrows, nav, tags, dates), `Hanken Grotesk` (body). All three loaded via one `<link>` in each HTML file.
- **Accent usage**: solid chartreuse, NOT gradients. Section titles and big display text use `--serif`; small labels/kickers use `--mono` uppercase with letter-spacing.
- **Cards**: `background: var(--panel)`, `border: 1px solid var(--line-soft)`, `border-radius: 4px`. Hover lifts `translateY(-3px)` and reveals a 3px chartreuse left bar via `::after` (`scaleY`).
- **Background atmosphere**: fixed grid (`64px` lines, radial-masked) via `body::before` and a subtle SVG noise grain via `body::after`.
- **Reveal animation**: elements with class `rv` fade/translate in via an IntersectionObserver (respects `prefers-reduced-motion`).
- **Blog pages**: share the same CSS variables and font stack so they stay visually consistent with `index.html`.
- **Code blocks** (blog): background `#0d1117`, mono font, border-radius 12px.

> Note: `admin.html` still generates blog HTML in the OLD cyan/purple theme. If you use it, run new posts through the same palette/font swap applied to the existing blog pages (swap `:root` vars, replace `0, 212, 255`→`200, 241, 53` and `124, 58, 237`→`155, 189, 31`, add the Google Fonts link).

## Sections (index.html)

Order is intentional — leads with credentials, de-emphasizes side projects:
`home` (hero + headshot) → stats band → `achievements` (Awards) → `experience` (4 roles, current = Mesa Associates) → `services` (freelance offerings) → `projects` (featured hackathon + side-project grid) → `skills` → `education` → `blog` → `contact`.

## Blog System

Blog posts are static HTML files. Workflow:
1. Use `admin.html` to write in Markdown and generate themed HTML
2. Save the `.html` file in project root
3. Add a `<a class="blog-card">` entry to the `#blog` section in `index.html`
4. Push to deploy

Admin password is base64-encoded in `admin.html` (variable `ADMIN_KEY`). Default: `ramadmin2025`.
