# Plan de Optimización Móvil - Sistema Integrado de Agua Potable

**Fecha de creación:** 15 de febrero de 2026
**Versión:** 1.0
**Objetivo:** Mejorar la experiencia móvil del proyecto con enfoque mobile-first

---

## 📊 Resumen Ejecutivo

**Archivos a modificar:** 8 archivos HTML
**Problemas identificados:** 47 problemas de responsividad
**Severidad:** 12 críticos, 18 altos, 17 medios
**Tiempo estimado:** 6-8 horas de trabajo

### Impacto esperado
- ✅ Eliminación de scroll horizontal no deseado
- ✅ Mejor legibilidad en pantallas pequeñas (320px - 768px)
- ✅ Inputs y botones más fáciles de tocar (mínimo 44px altura)
- ✅ Navegación fluida sin superposiciones
- ✅ Tablas con scroll horizontal explícito

---

## 🎯 Estrategia de Implementación

### Principios Mobile-First
1. **Base móvil primero**: Estilos sin prefijo para móvil (<640px)
2. **Progresión incremental**: `sm:` (640px) → `md:` (768px) → `lg:` (1024px)
3. **Touch-friendly**: Mínimo 44×44px para elementos interactivos
4. **Legibilidad**: Texto mínimo 14px, líneas de 45-75 caracteres
5. **Performance**: Evitar animaciones complejas en móvil

---

## 📋 FASE 1: Correcciones Críticas (Prioridad URGENTE)

**Duración estimada:** 2-3 horas
**Archivos:** Todos los HTML

### PASO 1.1: Tablas con Overflow Horizontal
**Problema:** Tablas con `min-w-[XXXpx]` sin contenedor scrollable

#### Archivos a modificar:

**A) `operacion-diaria.html` - Línea ~325**
```html
<!-- ANTES -->
<div class="bg-white rounded-3rem shadow-xl overflow-hidden">
    <table id="opTable" class="w-full text-left min-w-[800px]">

<!-- DESPUÉS -->
<div class="bg-white rounded-3rem shadow-xl overflow-hidden overflow-x-auto">
    <table id="opTable" class="w-full text-left min-w-[600px] md:min-w-[800px]">
```

**B) `siatm.html` - Línea ~381**
```html
<!-- ANTES -->
<table class="w-full text-left min-w-[900px]">

<!-- DESPUÉS -->
<div class="overflow-x-auto">
    <table class="w-full text-left min-w-[600px] md:min-w-[900px]">
    </table>
</div>
```

**C) `modulo-3.html` - Línea ~325**
```html
<!-- ANTES -->
<table id="opTable" class="w-full text-left min-w-[800px]">

<!-- DESPUÉS -->
<div class="overflow-x-auto rounded-2xl">
    <table id="opTable" class="w-full text-left min-w-[600px] md:min-w-[800px]">
```

**✅ Checklist:**
- [ ] Envolver tablas en contenedor con `overflow-x-auto`
- [ ] Cambiar `min-w-[XXXpx]` a `min-w-[YYYpx] md:min-w-[XXXpx]`
- [ ] Agregar `rounded-2xl` o `rounded-xl` al contenedor para mantener diseño
- [ ] Probar en Chrome DevTools con viewport 375px (iPhone SE)

---

### PASO 1.2: Modales y Ventanas Flotantes
**Problema:** Modales con ancho fijo (`w-96`) que no caben en móvil

#### `modulo-1.html` - Línea ~441

```html
<!-- ANTES -->
<div id="ai-assistant-window"
     class="fixed bottom-28 right-8 w-96 max-w-[90vw] h-[500px] glass-morphism rounded-[2rem] shadow-2xl z-50 flex flex-col overflow-hidden border border-white/10 no-print">

<!-- DESPUÉS -->
<div id="ai-assistant-window"
     class="fixed bottom-20 sm:bottom-28 right-4 sm:right-8 w-[calc(100vw-2rem)] sm:w-96 max-h-[70vh] sm:h-[500px] glass-morphism rounded-2xl sm:rounded-[2rem] shadow-2xl z-50 flex flex-col overflow-hidden border border-white/10 no-print">
```

**Cambios explicados:**
- `bottom-20 sm:bottom-28`: Menos espacio en móvil
- `right-4 sm:right-8`: Pegado al borde en móvil
- `w-[calc(100vw-2rem)] sm:w-96`: Ancho completo menos margen en móvil
- `max-h-[70vh] sm:h-[500px]`: Altura dinámica en móvil
- `rounded-2xl sm:rounded-[2rem]`: Bordes menos redondeados en móvil

#### `modulo-1.html` - Línea ~436 (Botón toggle)

```html
<!-- ANTES -->
<div id="ai-assistant-toggle"
     class="fixed bottom-8 right-8 w-16 h-16 bg-sky-600 rounded-full">

<!-- DESPUÉS -->
<div id="ai-assistant-toggle"
     class="fixed bottom-4 right-4 sm:bottom-8 sm:right-8 w-14 h-14 sm:w-16 sm:h-16 bg-sky-600 rounded-full">
```

**✅ Checklist:**
- [ ] Modificar modal AI assistant en todos los módulos que lo tengan
- [ ] Ajustar posición de botones flotantes
- [ ] Probar apertura y cierre en viewport 375px y 414px
- [ ] Verificar que no tape contenido importante

---

### PASO 1.3: Inputs Diminutos (Touch Targets)
**Problema:** Inputs con `p-1` (4px) difíciles de tocar

#### `modulo-3.html` - Líneas ~246-289 (Jarras de validación)

```html
<!-- ANTES -->
<div><label class="input-label text-[7px]">Turb.</label>
<input type="number" id="jar_turb_1" class="text-center p-1 text-xs"></div>

<!-- DESPUÉS -->
<div><label class="input-label text-[8px] sm:text-[7px]">Turb.</label>
<input type="number" id="jar_turb_1" class="text-center p-2 sm:p-1 text-sm sm:text-xs min-h-[44px] sm:min-h-0"></div>
```

**Principio WCAG:** Elementos táctiles deben tener mínimo 44×44px

#### `operacion-diaria.html` - CSS global (Línea ~63-72)

```css
/* ANTES */
input, select, textarea {
    padding: 10px;
    font-size: 14px;
}

/* DESPUÉS */
input, select, textarea {
    padding: 12px;
    font-size: 14px;
    min-height: 44px;
}

/* Para inputs pequeños específicos en desktop */
@media (min-width: 768px) {
    .compact-input {
        padding: 8px;
        min-height: auto;
    }
}
```

**✅ Checklist:**
- [ ] Actualizar padding mínimo de inputs a `p-3` (12px) en móvil
- [ ] Agregar `min-h-[44px]` a todos los inputs, selects, buttons
- [ ] En desktop permitir inputs más compactos con clase `.compact-input`
- [ ] Probar con dedos reales en dispositivo físico

---

### PASO 1.4: Navegación con Superposición
**Problema:** Nav bars con 3 elementos que se superponen en móviles pequeños

#### `modulo-1.html` (y todos los módulos) - Línea ~44-64

```html
<!-- ANTES -->
<nav class="fixed top-0 left-0 right-0 bg-gradient-to-r from-slate-900 to-slate-800 border-b border-slate-700 z-50 no-print shadow-xl">
    <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
        <a href="../index.html" class="flex items-center gap-3 text-white">
            <i class="fas fa-home text-xl"></i>
            <span class="font-black uppercase text-sm tracking-wider">← Inicio</span>
        </a>
        <div class="flex items-center gap-2 text-xs font-bold text-slate-400">
            <i class="fas fa-book text-sky-500"></i>
            <span>MÓDULO I</span>
        </div>
        <a href="modulo-2-filtros.html" class="flex items-center gap-3 text-white">
            <span class="font-black uppercase text-sm tracking-wider">Siguiente</span>
            <i class="fas fa-arrow-right text-xl"></i>
        </a>
    </div>
</nav>

<!-- DESPUÉS -->
<nav class="fixed top-0 left-0 right-0 bg-gradient-to-r from-slate-900 to-slate-800 border-b border-slate-700 z-50 no-print shadow-xl">
    <div class="max-w-7xl mx-auto px-3 sm:px-6 py-2 sm:py-4 flex justify-between items-center gap-2 sm:gap-4">
        <!-- Botón Inicio: Solo icono en móvil -->
        <a href="../index.html" class="flex items-center gap-2 text-white shrink-0">
            <i class="fas fa-home text-lg sm:text-xl"></i>
            <span class="hidden sm:inline font-black uppercase text-xs sm:text-sm tracking-wider">Inicio</span>
        </a>

        <!-- Indicador módulo: Compacto en móvil -->
        <div class="flex items-center gap-1 sm:gap-2 text-[10px] sm:text-xs font-bold text-slate-400 flex-1 justify-center">
            <i class="fas fa-book text-sky-500 text-sm sm:text-base"></i>
            <span class="truncate">MÓD. I</span>
        </div>

        <!-- Botón Siguiente: Solo icono en móvil -->
        <a href="modulo-2-filtros.html" class="flex items-center gap-2 text-white shrink-0">
            <span class="hidden sm:inline font-black uppercase text-xs sm:text-sm tracking-wider">Siguiente</span>
            <i class="fas fa-arrow-right text-lg sm:text-xl"></i>
        </a>
    </div>
</nav>
```

**Cambios clave:**
- `px-3 sm:px-6`: Menos padding horizontal
- `py-2 sm:py-4`: Menos padding vertical
- `gap-2 sm:gap-4`: Menos espacio entre elementos
- `hidden sm:inline`: Ocultar texto en móvil, solo iconos
- `truncate`: Truncar texto largo si es necesario
- `shrink-0`: Evitar que botones se compriman
- `flex-1 justify-center`: Centrar indicador de módulo

**✅ Checklist:**
- [ ] Modificar navbar en todos los módulos (5 archivos)
- [ ] Ocultar texto "Inicio" y "Siguiente" en móvil
- [ ] Probar en viewport 320px (iPhone SE más pequeño)
- [ ] Verificar que los 3 elementos no se superpongan

---

## 📋 FASE 2: Tipografía Responsive (Prioridad ALTA)

**Duración estimada:** 1-2 horas
**Archivos:** index.html, todos los módulos

### PASO 2.1: Títulos Principales (h1)

#### Jerarquía de tamaños recomendada:

| Viewport | Tailwind Class | Tamaño (rem) | Tamaño (px) |
|----------|----------------|--------------|-------------|
| Móvil (<640px) | `text-3xl` | 1.875rem | 30px |
| SM (640px+) | `sm:text-4xl` | 2.25rem | 36px |
| MD (768px+) | `md:text-5xl` | 3rem | 48px |
| LG (1024px+) | `lg:text-6xl` | 3.75rem | 60px |
| XL (1280px+) | `xl:text-7xl` | 4.5rem | 72px |

#### Implementación por archivo:

**A) `index.html` - Línea ~75**
```html
<!-- ANTES -->
<h1 class="text-5xl md:text-6xl font-black text-white mb-4 tracking-tight">
    Gestión de Agua Potable
</h1>

<!-- DESPUÉS -->
<h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-black text-white mb-4 tracking-tight">
    Gestión de Agua Potable
</h1>
```

**B) `modulo-1.html` - Línea ~85**
```html
<!-- ANTES -->
<h1 class="text-6xl md:text-8xl font-black text-white tracking-tighter uppercase leading-none mb-6">
    Ingeniería de <br><span class="text-transparent bg-clip-text bg-gradient-to-r from-sky-400 to-blue-500">Potabilización</span>
</h1>

<!-- DESPUÉS -->
<h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl 2xl:text-8xl font-black text-white tracking-tighter uppercase leading-none mb-6">
    Ingeniería de <br><span class="text-transparent bg-clip-text bg-gradient-to-r from-sky-400 to-blue-500">Potabilización</span>
</h1>
```

**C) `modulo-4.html` - Línea ~156**
```html
<!-- ANTES -->
<h1 class="text-6xl md:text-8xl font-black tracking-tighter uppercase mb-6 leading-none">

<!-- DESPUÉS -->
<h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl 2xl:text-8xl font-black tracking-tighter uppercase mb-6 leading-none">
```

**✅ Checklist:**
- [ ] Actualizar todos los h1 en portadas de módulos
- [ ] Actualizar h1 en index.html
- [ ] Actualizar h1 en siatm.html y operacion-diaria.html
- [ ] Probar legibilidad en 375px, 414px, 768px
- [ ] Verificar que no haya overflow horizontal

---

### PASO 2.2: Subtítulos y Texto de Soporte

#### `index.html` - Línea ~80**
```html
<!-- ANTES -->
<p class="text-xl text-slate-300 max-w-3xl mx-auto font-light">
    Plataforma integrada para capacitación operativa y gestión de plantas de tratamiento
</p>

<!-- DESPUÉS -->
<p class="text-base sm:text-lg md:text-xl text-slate-300 max-w-3xl mx-auto font-light leading-relaxed">
    Plataforma integrada para capacitación operativa y gestión de plantas de tratamiento
</p>
```

#### `modulo-1.html` - Línea ~87
```html
<!-- ANTES -->
<p class="text-slate-400 text-xl md:text-2xl font-light mb-12 max-w-2xl mx-auto italic">

<!-- DESPUÉS -->
<p class="text-slate-400 text-base sm:text-lg md:text-xl lg:text-2xl font-light mb-8 sm:mb-12 max-w-2xl mx-auto italic leading-relaxed">
```

**Regla general:**
- Texto base móvil: `text-sm` o `text-base` (14-16px)
- Desktop: `md:text-lg` o `md:text-xl` (18-20px)
- Agregar `leading-relaxed` para mejor legibilidad

**✅ Checklist:**
- [ ] Actualizar subtítulos en portadas
- [ ] Agregar `leading-relaxed` o `leading-loose`
- [ ] Verificar longitud de línea (45-75 caracteres)

---

### PASO 2.3: Etiquetas de Formulario

#### `siatm.html` y `operacion-diaria.html`
```html
<!-- ANTES -->
<label class="block text-[9px] font-black text-slate-400 uppercase mb-2 ml-1">
    Provincia
</label>

<!-- DESPUÉS -->
<label class="block text-[10px] sm:text-[9px] font-black text-slate-400 uppercase mb-2 ml-1">
    Provincia
</label>
```

**Regla:** Etiquetas deben tener mínimo 10px en móvil para legibilidad

**✅ Checklist:**
- [ ] Aumentar `text-[9px]` a `text-[10px] sm:text-[9px]`
- [ ] Aumentar `text-[7px]` a `text-[8px] sm:text-[7px]`
- [ ] Probar legibilidad en dispositivo real

---

## 📋 FASE 3: Espaciados y Layout (Prioridad ALTA)

**Duración estimada:** 1-2 horas

### PASO 3.1: Padding y Margin Responsive

#### Principio general:
```
Móvil: p-4 (16px)   → Desktop: md:p-8 (32px)
Móvil: py-8 (32px)  → Desktop: md:py-16 (64px)
Móvil: gap-4 (16px) → Desktop: md:gap-6 (24px)
```

#### `modulo-2-filtros.html` - Línea ~176
```html
<!-- ANTES -->
<main class="max-w-7xl mx-auto py-16 px-6 space-y-16">

<!-- DESPUÉS -->
<main class="max-w-7xl mx-auto py-8 md:py-16 px-4 sm:px-6 space-y-8 md:space-y-16">
```

#### Todas las páginas - Secciones principales
```html
<!-- PATRÓN GENERAL -->

<!-- ANTES -->
<section class="p-10 rounded-[2.5rem]">

<!-- DESPUÉS -->
<section class="p-6 sm:p-8 md:p-10 rounded-2xl md:rounded-[2.5rem]">
```

**✅ Checklist:**
- [ ] Reducir padding vertical de secciones principales: `py-16` → `py-8 md:py-16`
- [ ] Reducir padding horizontal de containers: `px-6` → `px-4 sm:px-6`
- [ ] Reducir spacing entre elementos: `space-y-16` → `space-y-8 md:space-y-16`
- [ ] Reducir bordes redondeados en móvil: `rounded-[2.5rem]` → `rounded-2xl md:rounded-[2.5rem]`
- [ ] Probar en viewport 375px para verificar uso eficiente del espacio

---

### PASO 3.2: Grids Responsive

#### Patrón recomendado:
```html
<!-- 4 columnas en desktop -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6">

<!-- 3 columnas en desktop -->
<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 md:gap-6">

<!-- 2 columnas en desktop -->
<div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6">
```

#### `modulo-4.html` - Línea ~202
```html
<!-- ANTES -->
<div class="grid md:grid-cols-3 gap-8 text-sm">

<!-- DESPUÉS -->
<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 md:gap-6 lg:gap-8 text-sm">
```

#### `index.html` - Línea ~88
```html
<!-- ANTES -->
<div class="grid lg:grid-cols-2 gap-8">

<!-- DESPUÉS -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-6 md:gap-8">
```

**✅ Checklist:**
- [ ] Agregar `grid-cols-1` explícitamente como base
- [ ] Agregar breakpoint intermedio `sm:grid-cols-2`
- [ ] Reducir gaps: `gap-8` → `gap-4 md:gap-6 lg:gap-8`
- [ ] Verificar que cards no se vean apretadas en 320px

---

### PASO 3.3: Navegación por Tabs

#### `modulo-2-desinfeccion.html` - Línea ~133-160
```html
<!-- ANTES -->
<button class="flex-1 min-w-[140px] md:min-w-[180px] flex items-center justify-center gap-2 md:gap-3 px-3 py-6">

<!-- DESPUÉS -->
<button class="flex-1 min-w-[90px] sm:min-w-[120px] md:min-w-[140px] lg:min-w-[180px] flex items-center justify-center gap-1 sm:gap-2 md:gap-3 px-2 sm:px-3 py-4 sm:py-5 md:py-6">
```

**Contenedor de tabs:**
```html
<!-- ANTES -->
<div class="bg-white rounded-2xl shadow-2xl border border-slate-200 overflow-hidden flex flex-wrap lg:flex-nowrap">

<!-- DESPUÉS -->
<div class="bg-white rounded-2xl shadow-2xl border border-slate-200 overflow-hidden overflow-x-auto">
    <div class="flex flex-nowrap min-w-max">
        <!-- tabs aquí -->
    </div>
</div>
```

**✅ Checklist:**
- [ ] Reducir `min-w-[140px]` a `min-w-[90px] sm:min-w-[140px]`
- [ ] Agregar `overflow-x-auto` al contenedor de tabs
- [ ] Usar `flex-nowrap` con `min-w-max` para permitir scroll horizontal suave
- [ ] Reducir padding vertical: `py-6` → `py-4 sm:py-6`

---

## 📋 FASE 4: Elementos Específicos (Prioridad MEDIA)

**Duración estimada:** 1-2 horas

### PASO 4.1: Layout de Aplicación (h-screen w-screen)

#### `operacion-diaria.html` - Línea ~78
```html
<!-- ANTES -->
<div id="app" class="flex flex-col md:flex-row h-screen w-screen relative overflow-hidden">

<!-- DESPUÉS -->
<div id="app" class="flex flex-col md:flex-row min-h-screen w-full relative overflow-hidden">
```

**Razón:** `h-screen` y `w-screen` pueden causar problemas con la barra de navegación del navegador móvil.

**Agregar CSS adicional:**
```css
/* Manejo robusto de viewport height en móvil */
#app {
    min-height: 100vh;
    min-height: 100dvh; /* Dynamic viewport height */
}

@supports not (height: 100dvh) {
    #app {
        min-height: -webkit-fill-available;
    }
}
```

**✅ Checklist:**
- [ ] Cambiar `h-screen` a `min-h-screen`
- [ ] Cambiar `w-screen` a `w-full`
- [ ] Agregar CSS con `100dvh` para mejor soporte móvil
- [ ] Probar en Safari iOS y Chrome Android

---

### PASO 4.2: Botones de Acción Flotantes

#### Patrón general para FABs (Floating Action Buttons):
```html
<!-- Móvil: Más cerca de los bordes, más pequeño -->
<button class="fixed bottom-4 right-4 sm:bottom-6 sm:right-6 md:bottom-8 md:right-8
               w-12 h-12 sm:w-14 sm:h-14 md:w-16 md:h-16
               bg-sky-600 rounded-full shadow-xl z-50">
```

#### Barra de acción inferior (Action Bar):
```html
<!-- ANTES -->
<div class="action-bar no-print">
    <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-6">

<!-- DESPUÉS -->
<div class="action-bar no-print">
    <div class="max-w-7xl mx-auto flex flex-col gap-3 sm:gap-4 md:flex-row md:justify-between items-stretch sm:items-center px-4 py-4 sm:py-6">
```

**✅ Checklist:**
- [ ] Reducir tamaño de FABs en móvil
- [ ] Acercar a bordes en móvil (más fácil de alcanzar)
- [ ] En action bars, usar `flex-col` en móvil y `md:flex-row` en desktop
- [ ] Botones principales ocupar todo el ancho en móvil: `w-full md:w-auto`

---

### PASO 4.3: Cartas y Componentes de Contenido

#### Patrón para cards:
```html
<!-- ANTES -->
<div class="bg-white p-10 rounded-[2.5rem] border border-slate-200 card-shadow">

<!-- DESPUÉS -->
<div class="bg-white p-6 sm:p-8 md:p-10 rounded-2xl sm:rounded-3xl md:rounded-[2.5rem] border border-slate-200 card-shadow">
```

#### Badges y tags:
```html
<!-- ANTES -->
<span class="bg-sky-500/20 text-sky-400 px-4 py-2 rounded-full text-xs font-black uppercase tracking-widest">

<!-- DESPUÉS -->
<span class="bg-sky-500/20 text-sky-400 px-3 py-1.5 sm:px-4 sm:py-2 rounded-full text-[10px] sm:text-xs font-black uppercase tracking-wide sm:tracking-widest">
```

**✅ Checklist:**
- [ ] Reducir padding de cards en móvil
- [ ] Reducir border-radius en móvil
- [ ] Reducir tamaño de badges y tags
- [ ] Reducir letter-spacing en móvil

---

## 📋 FASE 5: Optimización Final (Prioridad MEDIA-BAJA)

**Duración estimada:** 1 hora

### PASO 5.1: Imágenes y SVG Decorativos

#### Ocultar decoraciones complejas en móvil:
```html
<!-- ANTES -->
<div class="absolute right-[-80px] top-[-80px] opacity-10 pointer-events-none scale-150">
    <svg width="400" height="400" viewBox="0 0 24 24" fill="white">

<!-- DESPUÉS -->
<div class="hidden sm:block absolute right-[-80px] top-[-80px] opacity-10 pointer-events-none scale-150">
    <svg width="400" height="400" viewBox="0 0 24 24" fill="white">
```

**✅ Checklist:**
- [ ] Identificar SVG y elementos decorativos grandes
- [ ] Agregar `hidden sm:block` para ocultar en móvil
- [ ] Reducir tamaño de imágenes decorativas: `scale-150` → `scale-100 md:scale-150`

---

### PASO 5.2: Animaciones y Transiciones

#### CSS para reducir animaciones en móvil:
```css
/* Reducir animaciones en móvil para mejor performance */
@media (max-width: 768px) {
    * {
        animation-duration: 0.3s !important;
        transition-duration: 0.2s !important;
    }

    /* Deshabilitar animaciones complejas */
    .pulse-animation {
        animation: none;
    }
}

/* Respetar preferencias de usuario */
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

**✅ Checklist:**
- [ ] Reducir duración de animaciones en móvil
- [ ] Agregar soporte para `prefers-reduced-motion`
- [ ] Deshabilitar `pulse`, `bounce` complejos en móvil

---

### PASO 5.3: Fuentes y Tipografía Optimizada

#### Agregar font-display swap para mejor performance:
```html
<!-- En todos los archivos HTML -->

<!-- ANTES -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&display=swap" rel="stylesheet">

<!-- DESPUÉS -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
```

**Optimizaciones:**
- Eliminar peso 300 (light) que se usa poco
- Usar `font-display: swap` (ya incluido con `&display=swap`)
- Considerar cargar solo Latin characters si no se usan otros idiomas

**✅ Checklist:**
- [ ] Reducir pesos de fuente a solo los necesarios
- [ ] Verificar que `display=swap` esté presente
- [ ] Considerar preload de fuentes críticas

---

## 📋 FASE 6: Testing y Validación

**Duración estimada:** 1 hora

### PASO 6.1: Checklist de Viewports

Probar en estos tamaños (usar Chrome DevTools):

| Dispositivo | Viewport | Orientación | Checklist |
|-------------|----------|-------------|-----------|
| iPhone SE (2016) | 320×568 | Portrait | [ ] Sin scroll horizontal<br>[ ] Textos legibles<br>[ ] Botones tocables |
| iPhone SE (2022) | 375×667 | Portrait | [ ] Layout correcto<br>[ ] Modales caben |
| iPhone 12/13/14 | 390×844 | Portrait | [ ] Navegación fluida<br>[ ] Tablas scrolleables |
| iPhone 14 Plus | 414×896 | Portrait | [ ] Espaciado adecuado |
| Samsung Galaxy S21 | 360×800 | Portrait | [ ] Chrome Android OK |
| iPad Mini | 768×1024 | Portrait | [ ] Breakpoint md: activo |
| iPad Pro 11" | 834×1194 | Portrait | [ ] Layout intermedio |
| Desktop | 1280×720 | Landscape | [ ] Breakpoint lg: activo |

**Herramientas:**
- Chrome DevTools (F12) → Device Toolbar (Ctrl+Shift+M)
- Firefox Responsive Design Mode
- BrowserStack (si disponible)
- Dispositivos físicos (ideal)

---

### PASO 6.2: Checklist Funcional

#### Navegación:
- [ ] Botones de navegación principales visibles y funcionales
- [ ] Tabs/pestañas con scroll horizontal suave
- [ ] Menús desplegables accesibles
- [ ] Breadcrumbs (si hay) no se superponen

#### Formularios:
- [ ] Inputs tienen altura mínima 44px
- [ ] Labels son legibles (mínimo 10px)
- [ ] Selects se abren correctamente
- [ ] Teclado móvil no tapa campos activos

#### Tablas:
- [ ] Scroll horizontal explícito con indicador visual
- [ ] Columnas importantes visibles sin scroll
- [ ] Headers sticky (opcional pero recomendado)

#### Modales y Popups:
- [ ] No exceden ancho de viewport
- [ ] Botón de cerrar accesible
- [ ] Contenido scrolleable si es largo
- [ ] No tapan navegación crítica

#### Performance:
- [ ] Carga inicial < 3 segundos en 3G
- [ ] Animaciones fluidas (60fps)
- [ ] No hay layout shifts evidentes

---

### PASO 6.3: Accesibilidad Básica

```html
<!-- Agregar meta viewport si no existe -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
```

**Nota:** `maximum-scale=5.0` permite zoom pero previene zoom excesivo accidental

#### Checklist A11y:
- [ ] Contraste de color mínimo 4.5:1 (texto normal)
- [ ] Contraste de color mínimo 3:1 (texto grande)
- [ ] Touch targets mínimo 44×44px
- [ ] Navegación por teclado funcional
- [ ] Atributos `aria-label` en iconos sin texto

**Herramienta:** Lighthouse (Chrome DevTools → Lighthouse → Mobile)

---

## 📋 FASE 7: Documentación y Refactorización

### PASO 7.1: Crear Clases Utility Reutilizables

En el `<style>` de cada página o en un archivo CSS compartido:

```css
/* Mobile-first utilities */

/* Cards responsive */
.card-mobile {
    @apply p-6 sm:p-8 md:p-10
           rounded-2xl sm:rounded-3xl md:rounded-[2.5rem]
           border border-slate-200
           bg-white
           shadow-md md:shadow-xl;
}

/* Container responsive */
.container-mobile {
    @apply px-4 sm:px-6 md:px-8
           py-8 md:py-12 lg:py-16
           max-w-7xl mx-auto;
}

/* Touch-friendly button */
.btn-touch {
    @apply min-h-[44px] min-w-[44px]
           px-4 py-2
           text-sm sm:text-base
           font-bold
           rounded-xl
           transition-all
           active:scale-95;
}

/* Responsive heading */
.heading-1 {
    @apply text-3xl sm:text-4xl md:text-5xl lg:text-6xl
           font-black
           tracking-tight
           leading-tight;
}

.heading-2 {
    @apply text-2xl sm:text-3xl md:text-4xl
           font-bold
           tracking-tight;
}

.heading-3 {
    @apply text-xl sm:text-2xl md:text-3xl
           font-bold;
}

/* Responsive table container */
.table-responsive {
    @apply overflow-x-auto
           rounded-xl md:rounded-2xl
           border border-slate-200
           bg-white;
}

.table-responsive table {
    @apply min-w-[600px] md:min-w-[800px];
}
```

**Nota:** Si no se usa Tailwind con `@apply`, convertir a CSS vanilla:
```css
.card-mobile {
    padding: 1.5rem;
    border-radius: 1rem;
    border: 1px solid #e2e8f0;
    background-color: white;
}

@media (min-width: 640px) {
    .card-mobile {
        padding: 2rem;
        border-radius: 1.5rem;
    }
}

@media (min-width: 768px) {
    .card-mobile {
        padding: 2.5rem;
        border-radius: 2.5rem;
    }
}
```

---

### PASO 7.2: Actualizar CLAUDE.md

Agregar sección de Mobile-First Guidelines:

```markdown
## Mobile-First Design Guidelines

### Breakpoints Standard
- **Base (móvil)**: <640px - estilos sin prefijo
- **sm**: 640px+ - teléfonos grandes, tablets pequeñas
- **md**: 768px+ - tablets
- **lg**: 1024px+ - laptops
- **xl**: 1280px+ - desktop
- **2xl**: 1536px+ - pantallas grandes

### Tamaños Mínimos
- Touch targets: 44×44px mínimo
- Texto cuerpo: 14px mínimo (16px ideal)
- Texto pequeño: 12px mínimo
- Input padding: 12px mínimo
- Button padding: 12px vertical, 16px horizontal mínimo

### Espaciados Responsive
```
Móvil   → Desktop
p-4     → md:p-8
py-8    → md:py-16
gap-4   → md:gap-6
space-y-8 → md:space-y-16
```

### Tablas
- Siempre usar `overflow-x-auto` en contenedor
- Min-width responsive: `min-w-[600px] md:min-w-[800px]`
- Headers sticky: `sticky top-0 z-10`

### Modales
- Ancho: `w-full sm:w-96` o `w-[calc(100vw-2rem)] sm:w-96`
- Altura: `max-h-[70vh]` para evitar overflow
- Posición: `bottom-4 sm:bottom-8`, `right-4 sm:right-8`

### Performance
- Reducir animaciones en móvil
- Lazy load imágenes grandes
- Ocultar decoraciones complejas: `hidden sm:block`
```

---

## 🎯 Resumen y Próximos Pasos

### Orden de Implementación Recomendado

1. **DÍA 1 (2-3 horas)**: FASE 1 - Correcciones Críticas
   - Tablas con overflow
   - Modales responsive
   - Inputs tocables
   - Navegación sin superposición

2. **DÍA 2 (1-2 horas)**: FASE 2 - Tipografía
   - Títulos responsive
   - Subtítulos y texto
   - Etiquetas de formulario

3. **DÍA 3 (1-2 horas)**: FASE 3 - Espaciados y Layout
   - Padding/margin responsive
   - Grids responsive
   - Tabs con scroll

4. **DÍA 4 (1-2 horas)**: FASE 4 - Elementos Específicos
   - h-screen/w-screen fixes
   - Botones flotantes
   - Cards y componentes

5. **DÍA 5 (1 hora)**: FASE 5 - Optimización Final
   - Decoraciones
   - Animaciones
   - Fuentes

6. **DÍA 6 (1 hora)**: FASE 6 - Testing
   - Probar en múltiples viewports
   - Validar funcionalidad
   - Accesibilidad básica

7. **DÍA 7 (30 mins)**: FASE 7 - Documentación
   - Crear utilities
   - Actualizar docs

---

## 📊 Métricas de Éxito

Al finalizar, el proyecto debe cumplir:

- [ ] **0 errores** de scroll horizontal en viewport 320px
- [ ] **100%** de elementos interactivos con mínimo 44px
- [ ] **Lighthouse Mobile Score**: >85
- [ ] **Tiempo de carga en 3G**: <3 segundos
- [ ] **Tipografía legible**: Texto mínimo 14px
- [ ] **Touch targets accesibles**: 100% pasan test manual
- [ ] **Tablas funcionales**: Scroll horizontal explícito en todas

---

## 🛠 Herramientas Útiles

### Chrome DevTools
```
F12 → Device Toolbar (Ctrl+Shift+M)
- Preset: iPhone SE, iPhone 12, iPad
- Custom: 320px, 375px, 414px
- Network throttling: Fast 3G
```

### Firefox Responsive Design
```
Ctrl+Shift+M
- Touch simulation
- Screenshot de viewport
```

### Lighthouse
```
Chrome DevTools → Lighthouse
- Mode: Mobile
- Categories: Performance, Accessibility, Best Practices
```

### Online Tools
- [Responsively App](https://responsively.app/) - Preview múltiples dispositivos
- [BrowserStack](https://www.browserstack.com/) - Testing en dispositivos reales
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

---

## 📝 Notas Finales

### Principios a mantener:
1. **Mobile-first siempre**: Estilos base sin prefijo para móvil
2. **Progressive enhancement**: Agregar complejidad en viewports mayores
3. **Touch-first**: Elementos táctiles grandes y espaciados
4. **Performance**: Menos animaciones, menos assets en móvil
5. **Accessibility**: Contraste, tamaños, navegación por teclado

### No romper:
- ❌ No eliminar estilos desktop existentes
- ❌ No cambiar estructura HTML si no es necesario
- ❌ No modificar funcionalidad JavaScript
- ❌ No cambiar colores de marca (navy, sky-blue)
- ❌ No eliminar `.no-print` classes

### Sí mejorar:
- ✅ Agregar breakpoints intermedios
- ✅ Reducir tamaños base para móvil
- ✅ Agregar overflow-x-auto donde falte
- ✅ Mejorar touch targets
- ✅ Optimizar espaciados

---

**Versión:** 1.0
**Última actualización:** 15 de febrero de 2026
**Próxima revisión:** Post-implementación FASE 1
