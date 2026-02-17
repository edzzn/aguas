# Correcciones Móvil para SIATM.html

**Archivo:** `operaciones/siatm.html`
**Objetivo:** Optimizar diseño para móvil SIN afectar diseño de PC
**Estrategia:** Mobile-first con breakpoints `sm:` y `md:`

---

## 🔍 PROBLEMAS IDENTIFICADOS

### **1. NAVEGACIÓN SUPERIOR**
**Línea:** 85-92

**Problema:**
```html
<div class="max-w-7xl mx-auto px-6 py-3 flex justify-between items-center">
    <a href="../index.html" class="flex items-center gap-2 text-slate-600...">
        <i class="fas fa-arrow-left"></i>
        <span class="uppercase tracking-wide">Volver al Inicio</span>
    </a>
```

- ❌ `px-6` mucho padding horizontal en móvil pequeño
- ❌ Texto "Volver al Inicio" muy largo para móvil <375px
- ❌ `gap-2` podría ser más compacto

**Solución:**
```html
<div class="max-w-7xl mx-auto px-3 sm:px-6 py-2 sm:py-3 flex justify-between items-center">
    <a href="../index.html" class="flex items-center gap-1 sm:gap-2 text-slate-600...">
        <i class="fas fa-arrow-left text-sm sm:text-base"></i>
        <span class="uppercase tracking-wide text-xs sm:text-sm">
            <span class="hidden sm:inline">Volver al </span>Inicio
        </span>
    </a>
```

---

### **2. HEADER PRINCIPAL**
**Línea:** 100-113

**Problema:**
```html
<header class="bg-navy p-10 md:p-14 text-white...">
    <h1 class="text-4xl md:text-6xl font-black uppercase...">
        Sistema Inteligente de Asistencia Técnica Multicliente
    </h1>
    <p class="text-blue-200 mt-3 text-lg font-light...">
```

- ❌ `p-10` (40px) muy grande para móvil
- ❌ `text-4xl` (36px) título muy grande para pantalla pequeña
- ❌ `text-lg` (18px) subtítulo grande
- ❌ SVG decorativo visible en móvil (innecesario)

**Solución:**
```html
<header class="bg-navy p-6 sm:p-8 md:p-10 lg:p-14 text-white...">
    <h1 class="text-2xl sm:text-3xl md:text-4xl lg:text-6xl font-black uppercase...">
        Sistema Inteligente de Asistencia Técnica Multicliente
    </h1>
    <p class="text-blue-200 mt-2 sm:mt-3 text-sm sm:text-base md:text-lg font-light...">
</header>
<!-- SVG decorativo -->
<div class="hidden md:block absolute right-[-80px] top-[-80px]...">
```

---

### **3. NAVEGACIÓN POR TABS**
**Línea:** 116-122

**Problema:**
```html
<button class="px-10 py-6 text-[10px] font-black uppercase tracking-[0.25em]...">
    Captura Técnica
</button>
```

- ❌ `px-10` (40px) + `py-6` (24px) = mucho espacio
- ❌ `tracking-[0.25em]` letra muy espaciada para móvil
- ❌ 3 tabs pueden no caber en iPhone SE

**Solución:**
```html
<button class="px-4 sm:px-6 md:px-10 py-3 sm:py-4 md:py-6 text-[9px] sm:text-[10px] font-black uppercase tracking-wide sm:tracking-[0.25em]...">
    <span class="hidden sm:inline">Captura </span>Técnica
</button>
```

---

### **4. MAIN CONTENT PADDING**
**Línea:** 124

**Problema:**
```html
<main class="max-w-7xl mx-auto p-6 md:p-10">
```

- ❌ `p-6` está bien pero podría ser menor en móvil muy pequeño

**Solución:**
```html
<main class="max-w-7xl mx-auto p-4 sm:p-6 md:p-10">
```

---

### **5. SECCIONES / CARDS**
**Línea:** 131, 168, 208, 239

**Problema:**
```html
<section class="bg-white p-10 rounded-[2.5rem] border border-slate-200 card-shadow">
```

- ❌ `p-10` (40px) muy grande en móvil
- ❌ `rounded-[2.5rem]` (40px) bordes excesivos
- ❌ Sin responsive

**Solución:**
```html
<section class="bg-white p-6 sm:p-8 md:p-10 rounded-2xl sm:rounded-3xl md:rounded-[2.5rem] border border-slate-200 card-shadow">
```

---

### **6. TÍTULOS DE SECCIÓN**
**Línea:** 135, 171, 211

**Problema:**
```html
<h3 class="text-navy text-sm font-black uppercase tracking-widest">
    Identificación del Cliente
</h3>
```

- ❌ `text-sm` podría ser más pequeño en móvil
- ❌ `tracking-widest` muy espaciado

**Solución:**
```html
<h3 class="text-navy text-xs sm:text-sm font-black uppercase tracking-wide sm:tracking-widest">
    Identificación del Cliente
</h3>
```

---

### **7. HEADER DE SECCIÓN CON BOTÓN**
**Línea:** 132-141

**Problema:**
```html
<div class="border-b border-slate-100 pb-6 mb-8 flex items-center justify-between">
    <div class="flex items-center gap-4">
        <div class="w-10 h-10 bg-navy...">01</div>
        <h3>...</h3>
    </div>
    <select id="tipoCliente" class="p-3 rounded-xl font-black border-2 border-navy text-navy text-xs uppercase">
```

- ❌ En móvil pequeño, los elementos pueden superponerse
- ❌ `gap-4` puede ser menor

**Solución:**
```html
<div class="border-b border-slate-100 pb-4 sm:pb-6 mb-6 sm:mb-8 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
    <div class="flex items-center gap-2 sm:gap-4">
        <div class="w-8 h-8 sm:w-10 sm:h-10 bg-navy...">01</div>
        <h3>...</h3>
    </div>
    <select id="tipoCliente" class="w-full sm:w-auto p-3 rounded-xl...">
```

---

### **8. GRIDS SIN BREAKPOINT INTERMEDIO**
**Línea:** 142, 158, 214

**Problema:**
```html
<div class="grid grid-cols-1 md:grid-cols-4 gap-6">
```

- ❌ Salta de 1 columna (móvil) a 4 columnas (tablet)
- ❌ No hay estado intermedio para `sm:` (640px)

**Solución:**
```html
<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4 sm:gap-6">
```

---

### **9. LABELS MUY PEQUEÑOS**
**Línea:** 144, 148, 152, 155, 156, 159, 160, 161, 162

**Problema:**
```html
<label class="block text-[9px] font-black text-slate-400 uppercase mb-2 ml-1">
```

- ❌ `text-[9px]` es ilegible en móvil (demasiado pequeño)

**Solución:**
```html
<label class="block text-[10px] sm:text-[9px] font-black text-slate-400 uppercase mb-2 ml-1">
```

---

### **10. PANEL DE CUMPLIMIENTO**
**Línea:** 187-204

**Problema:**
```html
<div class="mt-10 p-8 rounded-3xl bg-slate-50 border border-slate-200 flex flex-col md:flex-row items-center justify-between gap-8">
```

- ❌ `mt-10` y `p-8` muy grande en móvil
- ❌ `gap-8` excesivo
- ❌ Porcentaje `text-5xl` muy grande

**Solución:**
```html
<div class="mt-6 sm:mt-8 md:mt-10 p-6 sm:p-8 rounded-2xl sm:rounded-3xl bg-slate-50 border border-slate-200 flex flex-col md:flex-row items-center justify-between gap-4 sm:gap-6 md:gap-8">
    <div class="flex-1 text-center md:text-left">
        <h4 class="text-[10px] font-black...">...</h4>
        <p class="text-xs...">...</p>
    </div>
    <div class="flex items-center gap-4 sm:gap-6">
        <div class="text-center md:text-right">
            <div id="realtime-percent" class="text-3xl sm:text-4xl md:text-5xl font-black...">100%</div>
```

---

### **11. DOSAGE ROW**
**Línea:** 214-234

**Problema:**
```html
<div class="dosage-row grid grid-cols-1 md:grid-cols-6 gap-6 p-8 bg-slate-50 rounded-[2rem]...">
```

- ❌ Salta de 1 a 6 columnas
- ❌ `gap-6` y `p-8` muy grande
- ❌ `rounded-[2rem]` excesivo

**Solución:**
```html
<div class="dosage-row grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4 sm:gap-6 p-4 sm:p-6 md:p-8 bg-slate-50 rounded-xl sm:rounded-2xl md:rounded-[2rem]...">
```

---

### **12. BOTONES AI**
**Línea:** 245-248

**Problema:**
```html
<button type="button" onclick="window.askAI('technical')" id="ai-btn-tech" class="flex items-center gap-2 bg-gradient-to-r from-blue-600 to-indigo-600 text-white px-5 py-2.5 rounded-xl text-[10px] font-black uppercase...">
```

- ❌ Texto puede ser muy largo en móvil

**Solución:**
```html
<button type="button" onclick="window.askAI('technical')" id="ai-btn-tech" class="flex items-center gap-1 sm:gap-2 bg-gradient-to-r from-blue-600 to-indigo-600 text-white px-3 sm:px-5 py-2 sm:py-2.5 rounded-lg sm:rounded-xl text-[9px] sm:text-[10px] font-black uppercase...">
    <span class="group-hover:animate-spin"></span>
    <span id="ai-btn-tech-text" class="hidden sm:inline">Diagnóstico</span>
    <span class="sm:hidden">IA</span>
</button>
```

---

### **13. TABLA PARAMÉTRICA**
**Línea:** 174-184

**Problema:**
```html
<table class="w-full text-sm">
    <thead>
        <tr class="bg-slate-50 text-[10px] uppercase text-slate-400 font-black border-b">
            <th class="p-6 text-left">Parámetro Técnico</th>
```

- ❌ `p-6` (24px) mucho padding en celdas
- ❌ Sin `min-width` para evitar apretamiento

**Solución:**
```html
<div class="overflow-x-auto -mx-6 sm:mx-0">
    <table class="w-full text-sm min-w-[600px]">
        <thead>
            <tr class="bg-slate-50 text-[10px] uppercase text-slate-400 font-black border-b">
                <th class="p-3 sm:p-4 md:p-6 text-left">Parámetro Técnico</th>
```

---

### **14. TABLAS DE HISTORIAL**
**Línea:** 380-395 (ya corregidas antes, pero agregar padding)

**Problema:**
```html
<table class="w-full text-left min-w-[600px] md:min-w-[900px]">
    <thead class="bg-slate-50 text-[10px] uppercase text-slate-400 font-black border-b">
        <tr><th class="p-8">Fecha</th>
```

- ❌ `p-8` mucho padding

**Solución:**
```html
<th class="p-3 sm:p-4 md:p-6 lg:p-8">Fecha</th>
```

---

### **15. ACTION BAR (BOTTOM)**
**Línea:** 347-362

**Problema:**
```html
<div class="action-bar no-print">
    <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-6">
        ...
        <button type="button" onclick="window.handleFinalizeReport()" class="w-full md:w-auto bg-navy text-white px-20 py-5 rounded-2xl...">
```

- ❌ `gap-6` y padding `px-20 py-5` muy grande
- ❌ Texto del botón muy largo para móvil

**Solución:**
```html
<div class="action-bar no-print">
    <div class="max-w-7xl mx-auto flex flex-col gap-3 sm:gap-4 md:flex-row md:justify-between items-stretch sm:items-center px-4 sm:px-6">
        <div class="flex items-center gap-3 sm:gap-5">
            <div class="w-12 h-12 sm:w-14 sm:h-14 bg-navy...">
            </div>
            <div>
                <p id="footer-client-tag" class="text-[10px] sm:text-[11px] font-black...">...</p>
                <p class="text-[9px] sm:text-[10px] text-slate-400...">...</p>
            </div>
        </div>
        <button type="button" onclick="window.handleFinalizeReport()" class="w-full md:w-auto bg-navy text-white px-6 sm:px-12 md:px-20 py-3 sm:py-4 md:py-5 rounded-xl sm:rounded-2xl font-black uppercase text-[10px] sm:text-xs...">
            <span class="hidden sm:inline">Finalizar y </span>Generar<span class="hidden sm:inline"> Documentos</span>
        </button>
    </div>
</div>
```

---

### **16. CSS - BODY PADDING**
**Línea:** 24

**Problema:**
```css
body {
    padding-bottom: 140px;
}
```

- ❌ 140px fijo puede ser mucho en móvil

**Solución:**
```css
body {
    padding-bottom: 100px;
}

@media (min-width: 768px) {
    body {
        padding-bottom: 140px;
    }
}
```

---

## 📝 RESUMEN DE CAMBIOS

### **Patrón general a aplicar:**

| Elemento | Desktop (actual) | Móvil (nuevo) |
|----------|------------------|---------------|
| Padding secciones | `p-10` | `p-6 sm:p-8 md:p-10` |
| Bordes redondeados | `rounded-[2.5rem]` | `rounded-2xl sm:rounded-3xl md:rounded-[2.5rem]` |
| Gaps | `gap-6` | `gap-4 sm:gap-6` |
| Grids 4 cols | `md:grid-cols-4` | `sm:grid-cols-2 md:grid-cols-4` |
| Títulos | `text-4xl md:text-6xl` | `text-2xl sm:text-3xl md:text-4xl lg:text-6xl` |
| Labels | `text-[9px]` | `text-[10px] sm:text-[9px]` |
| Table padding | `p-6` o `p-8` | `p-3 sm:p-4 md:p-6` |
| Botones | `px-5 py-2.5` | `px-3 sm:px-5 py-2 sm:py-2.5` |

---

## 🎯 IMPLEMENTACIÓN

Total de cambios: **~30 correcciones**
Archivos a modificar: **1** (`siatm.html`)
Tiempo estimado: **30-45 minutos**
Impacto en PC: **NINGUNO** (todos con breakpoints `sm:` y `md:`)

---

## ✅ CHECKLIST DE VALIDACIÓN

Después de aplicar cambios, probar en:

- [ ] iPhone SE (375px) - Viewport más pequeño
- [ ] iPhone 12 (390px) - Estándar actual
- [ ] iPad Mini (768px) - Tablet pequeña
- [ ] Desktop (1280px) - Verificar que nada cambió

### Verificar:
- [ ] Navegación superior cabe y es legible
- [ ] Header no tiene overflow
- [ ] Tabs caben en una línea o tienen scroll horizontal
- [ ] Secciones tienen espaciado adecuado
- [ ] Grids colapsan correctamente
- [ ] Labels son legibles (≥10px)
- [ ] Tablas tienen scroll horizontal
- [ ] Botones son tocables (≥44px altura)
- [ ] Action bar cabe sin superposición
- [ ] Todo en PC se ve IDÉNTICO a antes
