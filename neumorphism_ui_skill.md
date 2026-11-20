---
name: neuromorphism-ui
description: Sistema de diseño experto en Neumorfismo (Soft UI) para aplicaciones web y desktop. Aplica física de luz consistente, soporte nativo Light/Dark Mode, micro-interacciones táctiles y accesibilidad WCAG AA.
triggers:
  - "neumorphism"
  - "neumorfismo"
  - "soft ui"
  - "estilo neumórfico"
  - "diseño táctil"
globs:
  - "**/*.css"
  - "**/*.scss"
  - "**/*.tsx"
  - "**/*.jsx"
  - "**/*.vue"
  - "**/*.html"
---

# Neuromorphism (Soft UI) Design System Skill

Actúa como un **Principal UI/UX Engineer** especializado en Neumorfismo moderno (Soft UI 2.0). Tu objetivo es generar interfaces táctiles, limpias y funcionales, corrigiendo las debilidades históricas del neumorfismo clásico (bajo contraste y falta de jerarquía visual).

---

## 1. Reglas Físicas Fundamentales (Innegociables)

1. **Fuente de luz única (145°):** La luz siempre proviene de la esquina **superior izquierda**.
   - Sombra clara (luz): desplazamiento negativo (`-X -Y`).
   - Sombra oscura (sombra): desplazamiento positivo (`+X +Y`).
2. **Cohesión de superficie:** El color de fondo del componente DEBE ser idéntico al color de fondo de su contenedor padre (excepto en superficies convexas/cóncavas que usan gradientes sutiles de ±5% de luminosidad).
3. **Prohibición de colores puros:**
   - NUNCA uses `#FFFFFF` puro como fondo base en Light Mode (no permite sombra blanca).
   - NUNCA uses `#000000` puro como fondo base en Dark Mode (no permite sombra negra).
4. **Espaciado respirable:** Los elementos extruidos requieren al menos `16px–24px` de margen entre sí para que sus sombras no colisionen ni generen ruido visual.
5. **Regla de Oro de la Jerarquía:** No apliques neumorfismo a *todo*. Úsalo en contenedores clave, controles táctiles (botones, sliders, toggles, inputs) y tarjetas principales. El texto, tablas densas e íconos secundarios deben permanecer planos y legibles.

---

## 2. Design Tokens (CSS Variables)

Siempre inyecta o verifica la existencia de estos tokens base antes de estilizar componentes:

```css
:root {
  /* Light Mode - Base fría y limpia */
  --neu-bg: #e6e9ef;
  --neu-surface: #e6e9ef;
  --neu-shadow-light: #ffffff;
  --neu-shadow-dark: #c5cbd6;
  --neu-border-subtle: rgba(255, 255, 255, 0.6);

  /* Tipografía y Acentos (WCAG AA garantizado) */
  --neu-text-primary: #2d3748;
  --neu-text-secondary: #5a677d;
  --neu-accent: #4f46e5;
  --neu-accent-glow: rgba(79, 70, 229, 0.25);

  /* Elevaciones */
  --neu-extruded-sm: 4px 4px 8px var(--neu-shadow-dark), -4px -4px 8px var(--neu-shadow-light);
  --neu-extruded-md: 8px 8px 16px var(--neu-shadow-dark), -8px -8px 16px var(--neu-shadow-light);
  --neu-extruded-lg: 14px 14px 28px var(--neu-shadow-dark), -14px -14px 28px var(--neu-shadow-light);

  /* Hundidos (Inset) */
  --neu-inset-sm: inset 3px 3px 6px var(--neu-shadow-dark), inset -3px -3px 6px var(--neu-shadow-light);
  --neu-inset-md: inset 6px 6px 12px var(--neu-shadow-dark), inset -6px -6px 12px var(--neu-shadow-light);

  /* Radios y Transiciones */
  --neu-radius-sm: 10px;
  --neu-radius-md: 16px;
  --neu-radius-lg: 24px;
  --neu-radius-pill: 9999px;
  --neu-transition: all 0.2s cubic-bezier(0.25, 0.8, 0.25, 1);
}

[data-theme="dark"], .dark {
  /* Dark Mode - Base grafito profundo */
  --neu-bg: #252a34;
  --neu-surface: #252a34;
  --neu-shadow-light: #303744;
  --neu-shadow-dark: #1a1e25;
  --neu-border-subtle: rgba(255, 255, 255, 0.04);

  --neu-text-primary: #edf2f7;
  --neu-text-secondary: #94a3b8;
  --neu-accent: #6366f1;
  --neu-accent-glow: rgba(99, 102, 241, 0.35);
}
```

---

## 3. Reglas por Tipo de Componente

### Botones (Buttons)
- **Estado normal:** `box-shadow: var(--neu-extruded-sm)` + `border: 1px solid var(--neu-border-subtle)`.
- **Hover:** Incrementa ligeramente la elevación o aplica un tinte sutil al texto/ícono con `--neu-accent`.
- **Active (Presionado):** Transición inmediata a `box-shadow: var(--neu-inset-sm)` y `transform: scale(0.98)` para simular resistencia física.
- **CTA Primario:** Si requiere alta visibilidad, usa fondo `--neu-accent` con texto blanco y sombra externa adaptada (`--neu-extruded-sm`), sin perder el radio redondeado.

### Campos de Texto (Inputs / Textareas / Selects)
- **SIEMPRE hundidos (`inset`):** `box-shadow: var(--neu-inset-sm)`. Esto comunica visualmente que el campo "recibe" contenido.
- **Focus:** NUNCA dependas solo de la sombra. Agrega un anillo de enfoque claro:
  `outline: none; box-shadow: var(--neu-inset-sm), 0 0 0 2px var(--neu-accent);`

### Tarjetas y Contenedores (Cards / Modals)
- Usa `var(--neu-extruded-md)` o `var(--neu-extruded-lg)` con `border-radius: var(--neu-radius-lg)` y padding generoso (`24px+`).
- Evita anidar más de **2 niveles** de elementos extruidos (ej. Tarjeta extruida → Botón extruido es válido; Tarjeta → Sub-tarjeta → Panel → Botón rompe la ilusión visual).

### Toggles, Switches y Sliders
- **Track (carril):** Hundido (`var(--neu-inset-sm)`).
- **Thumb (perilla):** Extruido (`var(--neu-extruded-sm)`), circular (`var(--neu-radius-pill)`).
- **Estado activo:** Relleno del track o indicador LED central con `--neu-accent` y un sutil `box-shadow: 0 0 8px var(--neu-accent-glow)`.

---

## 4. Integración con Tailwind CSS (si el proyecto usa Tailwind)

Si detectas `tailwind.config.*` o Tailwind v4 (`@import "tailwindcss";`), define utilidades semánticas en lugar de repetir valores arbitrarios largos:
- `.neu-card` → superficie extruida media + borde sutil + radio grande.
- `.neu-btn` → superficie extruida pequeña + transición + estado `:active` hundido.
- `.neu-input` → superficie hundida + focus ring accesible.
- `.neu-concave` → `background: linear-gradient(145deg, var(--neu-shadow-dark), var(--neu-shadow-light))` (al 15% de mezcla).
- `.neu-convex` → `background: linear-gradient(145deg, var(--neu-shadow-light), var(--neu-shadow-dark))` (al 15% de mezcla).

---

## 5. Checklist de Auditoría del Agente (Ejecutar antes de responder)

Antes de entregar código al usuario, verifica internamente:
1. ¿El fondo del `body` o contenedor padre coincide exactamente con `--neu-bg`?
2. ¿Todos los elementos interactivos tienen un estado `:hover`, `:active` (hundido) y `:focus-visible` accesible?
3. ¿El contraste de texto cumple mínimo **4.5:1** tanto en Light Mode como en Dark Mode?
4. ¿Las sombras tienen suficiente margen externo para no cortarse por un `overflow: hidden` accidental del padre? (Si un contenedor padre tiene `overflow: hidden`, agrega padding suficiente para las sombras).