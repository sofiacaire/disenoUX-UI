# Coffea — Landing page (HTML + CSS)

Landing page estática de una sola página para la cafetería ficticia **Coffea**, construida según el PRD del TP 4 (Diseño UX-UI 2026) con HTML5 semántico y CSS3, sin JavaScript, frameworks ni preprocesadores.

## Estructura

```
coffea-landing/
├── index.html
├── 404.html            # usa rutas absolutas (/css, /assets) para funcionar en cualquier URL
├── netlify.toml        # publish = "." + headers de caché y seguridad
├── css/
│   ├── main.css        # @layer + @import de las capas en orden
│   ├── tokens.css      # custom properties (colores, tipografía, radios, espacios)
│   ├── base.css        # reset, tipografía base, :focus-visible, prefers-reduced-motion
│   ├── layout.css      # .container, franjas, página 404
│   ├── components/     # header, hero, categories, product-card, carousel,
│   │                   # promo, testimonial-card, newsletter, footer
│   └── utilities.css   # .visually-hidden, .sr-only, .skip-link
└── assets/
    ├── img/            # AVIF/WebP + JPG de respaldo (PLACEHOLDERS, ver abajo)
    ├── icons/          # SVG de íconos
    └── fonts/          # vacío: las fuentes se cargan desde Google Fonts
```

## Convenciones

- **BEM**: `.product-card`, `.product-card__title`, `.testimonial-card--featured`.
- **Sin valores sueltos**: colores, fuentes, radios y espacios salen de `css/tokens.css`.
- **Mobile-first**: estilos base para 360 px y `@media (min-width: …)` hacia arriba (md 48rem, lg 64rem, xl 90rem).
- **Capas**: `@layer tokens, base, layout, components, utilities`.
- **Sin JavaScript**: carrusel con `scroll-snap` + anclas, menú mobile con `<details>/<summary>`, favoritos con `checkbox` + `label`, validación con `type="email" required` y `:user-invalid`.

## Levantar en local

No hay build. Desde la raíz del proyecto:

```bash
python3 -m http.server 8000
# abrir http://localhost:8000
```

Conviene usar un servidor (y no abrir el archivo con `file://`) porque el logo y el 404 usan rutas absolutas.

## Despliegue en Netlify

1. Subir el repositorio a GitHub o GitLab.
2. En Netlify: **Add new site → Import an existing project** y elegir el repositorio.
3. Build command: *(vacío)*. Publish directory: `.` (ya declarado en `netlify.toml`).
4. La rama `main` publica a producción; cada Pull/Merge Request genera un Deploy Preview.
5. HTTPS y la redirección HTTP → HTTPS los gestiona Netlify automáticamente.
6. Medir Lighthouse sobre la URL de producción, no en local.

Como `css/` y `assets/` se sirven con caché `immutable`, al cambiar un archivo hay que renombrarlo con versión (por ejemplo `main.v2.css`) y actualizar la referencia.

## Pendientes y decisiones abiertas

- **Imágenes**: Se encuentran generadas y optimizadas en `assets/img/` en formatos WebP, AVIF y JPG (a 1x y 2x para pantallas de alta densidad), cumpliendo con el límite de peso de página ≤ 1,5 MB.
- **Íconos**: Se encuentran completos y optimizados en `assets/icons/`, incluyendo variantes para categorías, estrellas amarillas de rating y variantes activas/rellenas (favoritos).
- **Responsive**: tablet y mobile son la propuesta de la sección 7 del PRD, a validar con diseño.
- **Idioma**: el copy está en inglés (`lang="en"`); los textos de accesibilidad están en español como indica el PRD. Confirmar idioma final.
- **Copy**: los textos de cards, testimonios y links del footer son placeholders. Los typos del diseño original ya están corregidos.
- **Moneda**: se mantiene "Rs." del diseño; confirmar si se cambia.
- **Open Graph**: `og:image` usa una ruta relativa; cambiarla por URL absoluta cuando se conozca el dominio.
- **Fuentes**: se cargan desde Google Fonts; evaluar autohospedar en `assets/fonts/` (woff2).
- **Flechas del carrusel**: son anclas a `#coffee-5` / `#coffee-1` (y `#dessert-…`); sin JS no paginan por bloques.
- **Validación**: pasar W3C HTML/CSS, Lighthouse y pruebas cross-browser antes de cerrar la Fase 5.
