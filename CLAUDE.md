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

Dark mode with cyan/purple gradient accents.

- **Colors**: `--bg-primary: #0a0a0a`, `--bg-card: #1a1a1a`, `--accent-primary: #00d4ff` (cyan), `--accent-secondary: #7c3aed` (purple), `--text-primary: #ffffff`, `--text-secondary: #a0a0a0`
- **Fonts**: System font stack (`-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, ...`)
- **Gradients**: Primary accent gradient is `linear-gradient(135deg, var(--accent-primary), var(--accent-secondary))` — used on badges, buttons, gradient text, top bars.
- **Gradient text**: Apply `background: linear-gradient(...)`, `-webkit-background-clip: text`, `-webkit-text-fill-color: transparent` on titles.
- **Cards**: `background: linear-gradient(135deg, var(--bg-card), var(--bg-secondary))`, `border-radius: 20px`, `border: 1px solid var(--border-color)`. Gradient top bar via `::before` pseudo-element (4px height).
- **Hover effects**: `translateY(-8px)` with cyan glow `box-shadow: 0 20px 40px rgba(0, 212, 255, 0.15)`.
- **Code blocks**: Background `#0d1117`, font `SF Mono`, border-radius 12px.

## Blog System

Blog posts are static HTML files. Workflow:
1. Use `admin.html` to write in Markdown and generate themed HTML
2. Save the `.html` file in project root
3. Add a `<a class="blog-card">` entry to the `#blog` section in `index.html`
4. Push to deploy

Admin password is base64-encoded in `admin.html` (variable `ADMIN_KEY`). Default: `ramadmin2025`.
