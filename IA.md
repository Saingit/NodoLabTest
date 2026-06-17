# IA — Registro de interacciones

Herramienta utilizada: **Claude Code** (claude-sonnet-4-6) como agente de desarrollo en la terminal.

---

## Prompt inicial

> "El objetivo de esta prueba es crear una estructura de contenido en HTML y SCSS a partir de un diseño entregado en dos imágenes .png. La estructura final será utilizada dentro de Moodle. El usuario final modificará el contenido desde el editor HTML de Moodle para reemplazar los textos [...] Se recomienda evitar depender de clases aplicadas directamente a estas etiquetas [...] El mismo bloque deberá poder adaptarse a diferentes clientes cambiando colores sin tener que reescribir todo el código SCSS."

---

## Interacciones relevantes

### 1. Análisis de los diseños
Claude Code leyó las imágenes `Design A.png` y `Design B.png` directamente (es un modelo multimodal) e identificó:
- Layout de dos columnas: texto a la izquierda, ilustración a la derecha.
- Design A: fondo lavanda, forma orgánica blob en esquina superior derecha, paleta purple.
- Design B: fondo blanco/gris, card con bordes redondeados, círculos decorativos teal.
- Misma estructura HTML para ambos — solo cambia la clase modificadora del wrapper.

### 2. Decisión de arquitectura SCSS
A partir del requerimiento de multi-cliente, se propuso un mapa `$themes` en SCSS donde cada cliente define sus valores de color, radio y tipo de decoración. Un `@each` loop genera automáticamente las clases `.content-block--{nombre}` sin duplicar código.

### 3. Compatibilidad con Moodle
Se decidió estilizar `h3`, `h4`, `p` e `img` exclusivamente mediante selectores de hijo (`.content-block__text h3 { }`, `.content-block__media img { }`) para evitar depender de clases en esos elementos, que Moodle puede eliminar al editar contenido.

### 4. Elementos decorativos
Los fondos decorativos (blob orgánico en Design A, círculos en Design B) se implementaron con pseudo-elementos CSS `::before` y `::after` en lugar de elementos HTML extra o imágenes, para que sean puramente presentacionales y no editables desde Moodle.

### 5. Ajuste manual
- Revisión y ajuste de los valores de color tomados del diseño (hexadecimales aproximados a partir de la lectura visual de las imágenes PNG).
- Afinado de los valores `border-radius` del blob orgánico para que se pareciera más al diseño A.
- Decisión de crear ilustraciones SVG custom en lugar de usar las imágenes del diseño, ya que son contenido de ejemplo reemplazable.
- Corrección del SCSS para usar `@use "sass:map"` y `map.get()` en lugar del `map-get()` global deprecado en Dart Sass.

---

## Partes resueltas directamente con IA
- Estructura BEM del componente.
- Lógica del `@each` loop para generación de temas.
- Mixins `deco-blob` y `deco-circles`.
- SVGs de las ilustraciones placeholder.
- Compilación y corrección de warnings de Sass.

## Partes ajustadas manualmente
- Valores exactos de colores y opacidades para fidelidad visual.
- Decisión de usar `clamp()` para tipografía y padding responsivos.
- Orden de propiedades y comentarios explicativos en el SCSS.

---

## Qué mejoraría con más tiempo
- Extraer las variables de tema a archivos `_theme-a.scss` y `_theme-b.scss` separados para mayor escalabilidad.
- Añadir un tercer tema de ejemplo para validar que el sistema de temas funciona con un cliente nuevo en minutos.
- Crear ilustraciones más fieles a los diseños originales o usar imágenes reales del cliente.
- Añadir un pequeño script `build.js` para compilar y copiar archivos listos para subir a Moodle.
