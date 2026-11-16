---
name: landing-html-css
description: Reglas para crear o modificar la landing page del artista de este proyecto (4Geeks "html-hello") usando SOLO HTML y CSS. Úsala siempre que se pida crear, editar, ampliar, revisar o corregir index.html o styles.css, añadir secciones, estilos, animaciones, accesibilidad, SEO o datos estructurados Schema.org en este proyecto.
---

# Landing page de artista – solo HTML y CSS

Proyecto del curso de 4Geeks: una landing page de una sola página para un artista
que necesita presencia en internet. Esta skill resume los requisitos acordados y
los criterios de evaluación. Síguelos en cada cambio.

## Restricción principal (no negociable)

- **Solo HTML y CSS.** No añadas JavaScript (ni `<script>` ejecutable, ni
  atributos `onclick`, etc.), frameworks (Bootstrap, Tailwind, React…),
  preprocesadores ni herramientas de build.
- La **única** excepción es `<script type="application/ld+json">` para los datos
  estructurados de Schema.org, porque es un dato y no código que se ejecute.
- Todo lo interactivo se resuelve con CSS: `:hover`, `:focus-visible`, anclas
  `#id`, `scroll-behavior`, `@keyframes`, `animation-timeline: view()` dentro de
  `@supports`.
- Si alguien pide algo que solo se puede hacer con JS (formulario que envía
  datos, carrusel automático, menú hamburguesa con estado…), dilo y ofrece una
  alternativa con solo CSS.

## Organización de archivos

```
index.html   ← estructura y contenido
styles.css   ← todos los estilos (sin estilos inline ni <style> en el HTML)
```

Solo puede haber un archivo CSS, enlazado en el `<head>`. Si se añaden imágenes,
van en `assets/` con nombres en minúsculas y con guiones.

## Estructura acordada con el artista

1. **Barra de navegación** (`<header>` > `<nav aria-label="Navegación principal">`)
   con enlaces a: **Sobre mí**, **Trayectoria** y **Próximas presentaciones**.
2. **Portada / bienvenida** (`#inicio`): presentación del artista con un único `<h1>`.
3. **Sobre mí** (`#sobre-mi`).
4. **Trayectoria** (`#trayectoria`).
5. **Próximas presentaciones** (`#presentaciones`).
6. **Footer** con contacto (`<address>`) y redes sociales.

Cada sección ocupa **aproximadamente el alto de una pantalla**: usa
`min-height: 100vh; min-height: 100svh;`. Compensa la barra fija con
`scroll-padding-top`.

## HTML semántico (uso estricto)

- Usa `header`, `nav`, `main`, `section`, `article`, `aside`, `footer`, `figure`
  y `figcaption`, `time datetime="…"`, `address`, `cite`, listas `ul` y `ol`.
- Usa `div` o `span` solo cuando sean para estilos y no tengan significado.
- Jerarquía de títulos: un solo `h1` en la portada, un `h2` por sección y `h3`
  dentro de cada sección, sin saltarse niveles.
- `<html lang="es">`, `meta charset` y `meta viewport`.
- Indentación de 2 espacios y etiquetas siempre cerradas y bien anidadas.

## Accesibilidad (lectores de pantalla)

- Pon un enlace "Saltar al contenido principal" como primer elemento del
  `<body>` y haz que apunte a `<main id="contenido" tabindex="-1">`.
- Da a cada `section` un nombre accesible con `aria-labelledby` que apunte al id
  de su `h2` (o al `h1` en la portada).
- Si hay más de un `nav`, cada uno lleva su `aria-label` ("Navegación
  principal", "Redes sociales").
- Las imágenes llevan un `alt` que las describa. Las decorativas llevan
  `alt=""` o `aria-hidden="true"`.
- Un SVG con significado lleva `role="img"` más `<title>` y `<desc>` enlazados
  con `aria-labelledby`.
- Los enlaces o botones con texto ambiguo ("Entradas") llevan un `aria-label`
  con contexto, por ejemplo "Comprar entradas para Madrid, 23 de enero".
- Los enlaces con `target="_blank"` llevan `rel="noopener"` y avisan de que se
  abren en una pestaña nueva.
- El foco debe verse siempre (`:focus-visible` con outline). Nunca uses
  `outline: none` sin un sustituto.
- El contraste del texto debe ser de al menos 4.5:1.
- Respeta `@media (prefers-reduced-motion: reduce)` desactivando las
  animaciones y el scroll suave.

## SEO

En el `<head>`:
- Un `<title>` único y descriptivo (≈ 50–60 caracteres).
- `<meta name="description">` (≈ 150–160 caracteres).
- `<link rel="canonical">`, `meta robots`, `meta theme-color`.
- Open Graph (`og:type`, `og:title`, `og:description`, `og:url`, `og:locale`)
  y `twitter:card`.

## Datos estructurados Schema.org (JSON-LD)

Un único bloque `application/ld+json` en el `<head>` con un `@graph` que incluya:
- `WebSite` con un `about` que apunte al artista.
- El artista: `Person` (más `MusicGroup` si es músico) con `@id`, `name`,
  `jobTitle`, `description`, `genre`, `url`, `sameAs` (redes) y `award` o
  `album` si existen.
- Un `MusicEvent` o `Event` por cada presentación de la página, con
  `startDate` en ISO 8601 con zona horaria, `location` (`Place` o
  `MusicVenue` con `PostalAddress`), `performer` apuntando al `@id` del
  artista, `eventStatus`, `eventAttendanceMode` y `offers`.

**Los datos del JSON-LD deben coincidir con lo que se ve en la página**
(fechas, lugares, nombres). Si cambias una fecha en el HTML, cámbiala también
en el JSON-LD.

## Estilos (presentación atractiva y moderna)

- Define los colores, tipografías y medidas como variables CSS en `:root`.
- Ordena el CSS por bloques comentados: variables → reset → utilidades →
  componentes → secciones → animaciones → responsive → preferencias del usuario.
- Diseño responsive pensado primero para móvil, sin scroll horizontal a 390 px.
  Usa `clamp()` en las tipografías grandes.
- Usa Flexbox y Grid para el layout. No uses floats ni posiciones absolutas
  para la estructura.
- Las animaciones son opcionales, suaves y siempre con alternativa en
  `prefers-reduced-motion`.
- Las tipografías pueden venir de Google Fonts mediante `<link>`.

## Verificación antes de dar el trabajo por terminado

1. Comprueba que el HTML esté bien formado (etiquetas cerradas y anidadas) y
   que el JSON-LD sea JSON válido. Por ejemplo, extrae el bloque y pásalo por
   `python3 -c "import json,sys; json.load(sys.stdin)"`.
2. Comprueba que no haya JavaScript: busca `<script` y verifica que la única
   coincidencia sea la de `application/ld+json`.
3. Revisa que haya un solo `h1` y que los títulos sigan el orden.
4. Comprueba que cada enlace del menú apunte a un `id` que exista.
5. Abre la página en el navegador (`open index.html`) y revísala en escritorio
   y en móvil (390 px).
6. Opcional: valida con https://validator.w3.org y con
   https://validator.schema.org.

## Contenido del artista

Mientras no haya datos reales, se usa una artista ficticia (Lía Montero,
cantautora y pianista) y el dominio `liamontero.example.com`. Cuando el usuario
dé los datos reales, reemplázalos en el HTML, en el JSON-LD y en las metaetiquetas
a la vez.
