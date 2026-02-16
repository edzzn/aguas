# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static client-side water treatment training platform ("Sistema Integrado de Gestión de Agua Potable") for Ecuador. All content is in Spanish. No build system, no package manager, no server — pure HTML/CSS/JS served as static files.

This is a professional education and operational management platform for water treatment plant operators, combining theoretical training with practical operational tools.

## Architecture

**Landing Page** (`index.html`):
- Dual-path entry point with gradient dark theme (`#0a1128` to `#001f54`)
- Two main cards: "Curso de Operadores" and "Sistemas de Operación"
- Responsive grid layout with hover animations (translateY + shadow effects)

**Section 1: Curso de Operadores** (`curso/`) — 40-hour certification program:
- **5 sequential modules** with fixed navigation flow:
  - `modulo-1.html` — Fundamentos de Ingeniería de Potabilización (8h)
  - `modulo-2-filtros.html` — Filtración (8h)
  - `modulo-2-desinfeccion.html` — Desinfección (8h)
  - `modulo-3.html` — Operación y Mantenimiento (8h)
  - `modulo-4.html` — Gestión y Calidad (8h)
- **Fixed top navbar** on all modules: Home button (left) | Module indicator (center) | Next button (right)
- **MathJax 3** for rendering chemical/hydraulic formulas in LaTeX notation
- **IBM Plex Sans/Mono** typography for technical content
- Print-optimized styles (`.no-print` class)
- Animated scroll reveals and interactive elements

**Section 2: Sistemas de Operación** (`operaciones/`) — Dual operational tools:

### 2A. SIATM (`siatm.html`) — Sistema Inteligente de Asistencia Técnica Multicliente
**3-tab architecture** with `switchTab()` navigation:
1. **Captura Técnica** (Technical data capture):
   - Client identification (CARTERA/POTENCIAL toggle)
   - INEN 1108 parameter table (7 parameters: Turbiedad, Color, pH, Alcalinidad, Cloro, Fe, Mn)
   - Real-time compliance calculator with circular progress indicator
   - Dosage engineering calculator (PAC, Sulfato, Hipoclorito, etc.)
   - Chemical dose formulas: `Dosis (mg/L) = (Caudal Bomba mL/min * % Conc * 10) / (Caudal Planta L/s * 60)`
   - Autonomy calculator: `Días = Stock / ((Caudal * % * Horas * 60) / 100000)`
   - AI diagnostics button (Gemini API integration)

2. **Gestión Comercial** (Commercial management):
   - Product consumption checklist (Coagulantes, Desinfectantes, Floculantes, pH regulators)
   - Competitor analysis fields (current supplier, procurement method, contract type)
   - Quote generator with automatic subtotal/IVA(15%)/total calculations
   - AI strategy suggestions

3. **Archivo Central** (Central archive):
   - Two separate tables: "Cartera de Clientes" (technical follow-up) and "Pipeline Comercial" (sales prospects)
   - Excel export functionality by client type
   - Real-time sync with Firebase Firestore

**Firebase Integration**:
- Project ID: `app-visitas-tecnicas`
- Collections: `visitas` (stores client data, fecha, caudal, tipo)
- Real-time listeners with `onSnapshot()`
- API key hardcoded in module script

**Google Gemini AI**:
- Model: `gemini-2.5-flash-preview-09-2025`
- Used for technical diagnostics and commercial strategy suggestions
- Client-side fetch calls with hardcoded API key
- Response injected into `observaciones` or `com_comentarios` textareas

### 2B. Operación Diaria (`operacion-diaria.html`) — Daily Operations Console
**6-view single-page app** with `switchView()` navigation:

1. **Console** (landing): Quick access buttons to all 5 operational views
2. **Dosage/Aforo**:
   - Multi-product dosing calculator
   - Real-time dose calculation per product row
   - Stock autonomy tracking
   - Formula: `Dosis (mg/L) = (ml/min * % conc) / (L/s * 6)`
3. **Operational Log**:
   - 6-hour interval table (06:00, 10:00, 14:00, 18:00, 22:00, 02:00)
   - Barrier validation (pH 6.5-8.5, Cloro ≥1.0)
   - Chart.js doughnut chart for compliance visualization
   - Average flow calculation and daily/monthly projections
4. **Inventory/Kardex**:
   - 7 chemical products (PAC, SULFATO, RPH, CAL, HIPO, GAS, AYUDANTE)
   - Toggle active/inactive products
   - Real-time stock updates with manual input
   - Autonomy alerts (critical if <30 days)
5. **Finance/Economics**:
   - Monthly cost projection per chemical
   - Unit cost calculation (USD/m³)
   - Dynamic pricing inputs
6. **Master Log/Bitácora**:
   - Two tabs: Design history and Operations history
   - PDF report generation for technical memos
   - CSV export functionality

**Data Persistence**:
- `productStock` object in memory (7 products with amount/update/used flags)
- `savedDesignRecords` array for engineering designs
- `savedOpsRecords` array for daily operations
- `bestJarInfo` object storing winning jar test results
- `latestCaracterization` object for raw water parameters

## Technology Stack

- **Tailwind CSS** via CDN (no local install, dynamic config via `tailwind.config`)
- **Chart.js** 4.x for doughnut charts and data visualization
- **MathJax 3** for LaTeX math formulas (`tex-mml-chtml.js`)
- **Font Awesome 6.5.1** for icons
- **Google Fonts**: Inter (300-900), IBM Plex Sans (300-700), IBM Plex Mono (500-700), JetBrains Mono
- **Firebase 11.0.1** (ES modules from CDN): Firestore for data persistence
- **Google Gemini API** (`gemini-2.5-flash-preview-09-2025`) — called directly via fetch, API key client-side

## Key Conventions

### Language & Content
- **Spanish-only**: All UI text, comments, variable names, and documentation in Spanish
- **Technical terminology**: Ecuador-specific (GAD, SERCOP, Ínfima Cuantía, etc.)
- **INEN 1108** compliance: Ecuador's potable water quality standard (referenced throughout)

### Styling & Design
- **Color palette**:
  - Navy primary: `#0a1128`, `#0a192f`, `#000040` (headers, emphasis)
  - Sky/cyan accents: `#0ea5e9`, `#00d4ff` (CTAs, highlights)
  - Success green: `#10b981`, `#dcfce7` (compliance, OK states)
  - Danger red: `#ef4444`, `#fee2e2` (failures, critical alerts)
  - Gold commercial: `#d4af37` (commercial section accent)
  - Pink/amber: `#ec4899`, `#f59e0b` (finance, inventory)
- **Typography**:
  - Uppercase headings with `letter-spacing: 0.2em-0.4em` (tracking-widest)
  - Font weights: 300 (light), 400 (normal), 700 (bold), 800-900 (black/extrabold)
  - Monospace for technical values and costs
- **Borders & Shadows**:
  - Thick left borders (`border-l-[15px]`) for section emphasis
  - Rounded corners: `rounded-xl` (0.75rem), `rounded-[2rem]` (2rem), `rounded-[2.5rem]` (2.5rem)
  - Card shadows: `shadow-xl`, `shadow-2xl` with color variants
- **Animations**:
  - Hover effects: `translateY(-4px)`, `translateY(-8px)` with shadow changes
  - Active states: `active:scale-95`
  - Loading animations: `pulse`, custom `ai-loading` gradient pulse
- **Print styles**: `.no-print` class hides navigation and action bars

### Navigation & UX
- **Fixed/sticky navbars**: `z-index: 50-100`
- **Tab systems**: Active state marked with `.tab-active` class (bold, underlined, or colored border)
- **View switching**: JavaScript `switchView(viewName)` or `switchTab(tabName)` functions
- **Toast notifications**: Fixed bottom position, fade in/out animations, 3-second auto-hide
- **Action bars**: Fixed bottom with backdrop blur, shadows, and prominent CTA buttons

### Data & State Management
- **In-memory state**: No backend, all data stored in JS variables
- **localStorage**: Used for persistence across sessions (not yet implemented but referenced)
- **Firebase Firestore**: Real-time sync for SIATM client records
- **Formulas** (all calculations done in JavaScript):
  - Dosage: `(ml/min * conc% * factor) / (flow * time)`
  - Autonomy: `stock_kg / daily_consumption_kg`
  - Compliance: `(parameters_met / total_parameters) * 100`
  - Monthly volume: `flow_L/s * 86.4 * 30`
  - Unit cost: `monthly_cost / monthly_volume_m3`

### Standards & Compliance
- **INEN 1108 limits** (hardcoded):
  - Turbiedad: ≤5 NTU (ideal <1)
  - Color: ≤15 UPC (ideal <1)
  - pH: 6.5-8.5
  - Alcalinidad: ≤200 mg/L
  - Cloro Residual: 0.3-1.5 mg/L (meta: 1.0)
  - Hierro: ≤0.3 mg/L
  - Manganeso: ≤0.1 mg/L
- **Currency**: USD with `$` symbol, comma thousands separator, 2 decimal places
- **Dates**: DD/MM/YYYY format, `es-ES` locale

## Development Workflow

**No build steps required**:
1. Edit HTML/CSS/JS directly in any file
2. Refresh browser to see changes
3. No compilation, transpilation, or bundling

**Testing**:
- Manual testing only (no automated tests)
- Check print preview for `.no-print` behavior
- Test responsive breakpoints (sm, md, lg)
- Verify formulas with sample data

**Deployment**:
- Serve as static files from any HTTP server
- No server-side logic required
- Firebase/Gemini API calls are client-side

## File Structure Summary
```
/
├── index.html              # Landing page (dual-path entry)
├── curso/
│   ├── modulo-1.html       # Module 1 (8h, foundations)
│   ├── modulo-2-filtros.html
│   ├── modulo-2-desinfeccion.html
│   ├── modulo-3.html
│   └── modulo-4.html       # Module 4 (8h, quality management)
├── operaciones/
│   ├── siatm.html          # 3-tab client management + AI (Firebase + Gemini)
│   └── operacion-diaria.html  # 6-view daily ops console (Chart.js)
├── CLAUDE.md               # This file
└── README.md               # Project readme (minimal)
```

## Common Patterns

**Adding a new parameter to SIATM**:
1. Add to `PARAM_LIST` array with `{name, limit, unit, min?}`
2. Table auto-generates via `renderParamTable()`
3. Compliance recalculates on input via `updateCompliance()`

**Adding a new chemical product**:
1. Add to `productStock` object with `{amount, update, used}` structure
2. Update inventory selection checkboxes
3. Add to dosage calculator options

**Creating a new operational view**:
1. Create new `<div id="view_name" class="view-container">` section
2. Add navigation button with `onclick="switchView('name')"`
3. Implement view-specific logic in `<script>` section

**Generating a PDF report**:
1. Collect data into structured object
2. Build HTML string with inline styles
3. Open new window, write HTML, trigger `window.print()`

## Important Notes for AI Assistance

- **Never use npm/yarn/build tools**: This is a pure static site
- **Preserve Spanish**: All text must remain in Spanish
- **Maintain formula accuracy**: Chemical dosing formulas are critical for safety
- **Keep API keys client-side**: Firebase and Gemini keys are intentionally exposed (public demo)
- **Respect INEN 1108**: Water quality limits are regulatory requirements
- **Print-friendly**: Always test that new components respect `.no-print` class
- **Mobile-first**: Use Tailwind responsive classes (sm:, md:, lg:)
- **Accessibility**: While not explicitly implemented, consider basic a11y in suggestions
