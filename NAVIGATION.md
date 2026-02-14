# Sistema Integrado - Estructura de Navegación

## Estructura de Directorios

```
/Users/edzzn/dev/aguas/
├── index.html                           # Hub principal
├── curso/                               # Módulos de capacitación
│   ├── modulo-1.html                   # Módulo I (Ciencias del Agua)
│   ├── modulo-2-filtros.html           # Módulo II.2 (Filtración)
│   ├── modulo-2-desinfeccion.html      # Módulo II.3 (Desinfección)
│   ├── modulo-3.html                   # Módulo III (Operaciones)
│   └── modulo-4.html                   # Módulo IV (Gestión Avanzada)
└── operaciones/                         # Sistemas operacionales
    ├── siatm.html                      # Asistencia Técnica
    └── operacion-diaria.html           # Operación Diaria
```

## URLs de Acceso

### Hub Principal
- **Inicio**: `/index.html`

### Curso de Operadores (Secuencial)
- **Módulo I**: `/curso/modulo-1.html`
- **Módulo II.2**: `/curso/modulo-2-filtros.html`
- **Módulo II.3**: `/curso/modulo-2-desinfeccion.html`
- **Módulo III**: `/curso/modulo-3.html`
- **Módulo IV**: `/curso/modulo-4.html`

### Sistemas de Operación (Paralelo)
- **SIATM**: `/operaciones/siatm.html`
- **Operación Diaria**: `/operaciones/operacion-diaria.html`

## Flujos de Navegación

### 1. CURSO - Navegación Secuencial
```
Index → Módulo I → Módulo II.2 → Módulo II.3 → Módulo III → Módulo IV
  ↑__________|___________|___________|___________|___________|
             ← Botón "Inicio" en cada módulo
```

**Características:**
- Barra de navegación fija superior en todos los módulos
- Botón "← Inicio" (izquierda) - regresa a index.html
- Indicador de módulo actual (centro)
- Botón "Siguiente →" (derecha) - va al siguiente módulo
- Módulo IV muestra badge "Completado" en lugar de botón siguiente
- Clase `.no-print` para ocultar navegación al imprimir

### 2. OPERACIONES - Navegación Paralela

#### SIATM
```
Index → SIATM → [Tabs internos: Recolección / Comercial / Historial]
  ↑______|
    "Volver al Inicio"
```

**Características:**
- Barra superior sticky con botón "Volver al Inicio"
- Mantiene navegación por tabs interna
- Firebase backend integrado

#### Operación Diaria
```
Index → Landing → [6 vistas: Console / Dosis / Docs / Alerts / Stats / Form]
  ↑______|______|
    "Inicio"  "← Volver"
```

**Características:**
- Botón "INICIO" en landing page (esquina superior izquierda)
- Navegación de dos niveles:
  - Vistas internas → Landing (botón "← Volver")
  - Landing → Index (botón "INICIO")

## Componentes de Navegación

### Barra de Navegación CURSO
```html
<nav class="fixed top-0 left-0 right-0 bg-gradient-to-r from-slate-900 to-slate-800 border-b border-slate-700 z-50 no-print shadow-xl">
    <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
        <!-- Inicio -->
        <a href="/index.html" class="flex items-center gap-3 text-white hover:text-sky-400 transition-colors group">
            <i class="fas fa-home text-xl group-hover:scale-110 transition-transform"></i>
            <span class="font-black uppercase text-sm tracking-wider">← Inicio</span>
        </a>
        
        <!-- Indicador -->
        <div class="flex items-center gap-2 text-xs font-bold text-slate-400">
            <i class="fas fa-book text-sky-500"></i>
            <span>MÓDULO [X]</span>
        </div>
        
        <!-- Siguiente -->
        <a href="/curso/[next].html" class="flex items-center gap-3 text-white hover:text-sky-400 transition-colors group">
            <span class="font-black uppercase text-sm tracking-wider">Siguiente</span>
            <i class="fas fa-arrow-right text-xl group-hover:scale-110 transition-transform"></i>
        </a>
    </div>
</nav>

<div class="pt-20">
    <!-- Contenido del módulo -->
</div>
```

### Barra de Navegación SIATM
```html
<div class="bg-white border-b border-slate-200 shadow-sm no-print sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-6 py-3 flex justify-between items-center">
        <a href="/index.html" class="flex items-center gap-2 text-slate-600 hover:text-navy font-bold text-sm transition-colors">
            <i class="fas fa-arrow-left"></i>
            <span class="uppercase tracking-wide">Volver al Inicio</span>
        </a>
        <div class="text-xs font-black text-slate-400 uppercase tracking-widest">SIATM</div>
    </div>
</div>
```

### Botón Home en Operación Diaria Landing
```html
<a href="/index.html" class="bg-white/10 hover:bg-white/20 px-4 py-2 rounded-lg font-black text-xs text-white transition-all flex items-center gap-2">
    <i class="fas fa-home"></i>
    <span>INICIO</span>
</a>
```

## Estilos y Consistencia

### Paleta de Colores
- **Navy Blue**: `#0a1128`, `#0a192f`, `#000040`
- **Sky Blue**: `#0ea5e9`, `#06b6d4`
- **Slate Gray**: `#1e293b`, `#475569`
- **Emerald**: `#10b981`

### Tipografía
- **Fuentes**: Inter, IBM Plex Sans
- **Pesos**: 300 (light), 400 (regular), 700 (bold), 900 (black)
- **Estilo**: Uppercase con tracking amplio para headings

### Interactividad
- Hover: `translateY(-2px)` + aumento de shadow
- Transiciones: `0.3s ease`
- Iconos: Font Awesome 6.5.1
- `z-index: 50` para navegación fija

## Testing Checklist

### Navegación Funcional
- [ ] Index → Módulo 1 → Módulo 2 Filtros → Módulo 2 Desinfección → Módulo 3 → Módulo 4
- [ ] Botón "Inicio" desde cualquier módulo → Index
- [ ] Index → SIATM → Inicio
- [ ] Index → Operación Diaria → Landing → Inicio
- [ ] Operación Diaria: Subvistas → Landing → Index

### Preservación de Funcionalidad
- [ ] Módulos CURSO: Tabs/sidebar internos funcionan
- [ ] SIATM: Tabs (recolección, comercial, historial) funcionan
- [ ] SIATM: Firebase backend conectado
- [ ] Operación Diaria: switchView() funciona
- [ ] Todos los formularios e inputs funcionan
- [ ] AI assistants (Gemini) funcionan

### Responsive Design
- [ ] Mobile (320px): Navegación apilada correctamente
- [ ] Tablet (768px): Layout adaptado
- [ ] Desktop (1920px): Layout completo

### Impresión
- [ ] Clase `.no-print` oculta navegación al imprimir
- [ ] Contenido se imprime correctamente sin barra de navegación

### Estética
- [ ] Colores navy/sky consistentes
- [ ] Hover states funcionan
- [ ] Iconos Font Awesome cargan correctamente
- [ ] Transiciones suaves
- [ ] No hay layout shifts
