# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static client-side water treatment training platform ("Sistema Integrado de Agua Potable") for Ecuador. All content is in Spanish. No build system, no package manager, no server — pure HTML/CSS/JS served as static files.

## Architecture

**Two main sections** accessed from `index.html`:

1. **Curso de Operadores** (`curso/`) — 5 sequential training modules for a 40-hour certification program:
   - `modulo-1.html` → `modulo-2-filtros.html` → `modulo-2-desinfeccion.html` → `modulo-3.html` → `modulo-4.html`
   - Each module has a fixed top nav bar with Home/Module indicator/Next buttons

2. **Sistemas de Operación** (`OPERACIONES/`) — parallel operational tools:
   - `SIATM.HTML` — 3-tab technical assistance system (Technical Capture, Commercial Management, Central Archive) with Gemini AI integration
   - `operacion-diaria.html` — 6-view daily operations system (Console, Dosage, Docs, Alerts, Stats, Log) using `switchView()` for internal navigation

## Technology Stack

- **Tailwind CSS** via CDN (no local install)
- **Chart.js** for data visualization
- **MathJax 3** for LaTeX math formulas (chemical/hydraulic calculations)
- **Font Awesome 6.5.1** + **Lucide** for icons
- **Google Fonts**: Inter, IBM Plex Sans, IBM Plex Mono
- **Google Gemini API** (`gemini-2.5-flash-preview-09-2025`) — called directly via fetch in SIATM, API key is hardcoded client-side

## Key Conventions

- **Language**: All UI text, comments, and documentation are in Spanish
- **Styling**: Navy/sky-blue color palette (`#0a1128`, `#0ea5e9`); uppercase headings with wide letter-spacing; `font-weight: 900` (black) for emphasis
- **Navigation**: Fixed/sticky nav bars with `z-index: 50`; `.no-print` class hides nav when printing
- **State**: In-memory JS variables and `localStorage`; no backend database
- **Standards**: Ecuador INEN 1108 water quality standard is referenced throughout
- **Currency/locale**: USD, DD/MM/YYYY dates, `es-ES` locale

## Development

No build or install steps required. Open HTML files directly in a browser or serve with any static file server. There are no tests, linters, or CI pipelines configured.

## Reference

See `NAVIGATION.md` for detailed navigation flows, component templates, color palette, and a manual testing checklist.
