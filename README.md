# nodoLab — Prueba Técnica

Componente de contenido HTML/SCSS adaptable a múltiples clientes, pensado para usarse dentro de Moodle.

## Estructura del proyecto

```
├── index.html          # Demo con ambos temas
├── styles.scss         # Estilos fuente (editar aquí)
├── styles.css          # CSS compilado (generado automáticamente)
├── images/
│   ├── illustration-a.svg   # Ilustración tema purple
│   └── illustration-b.svg   # Ilustración tema teal
├── Design A.png        # Diseño de referencia
├── Design B.png        # Diseño de referencia
├── README.md
└── IA.md
```

## Visualizar localmente

### Opción 1 — Abrir directamente en el navegador
Abrir `index.html` en cualquier navegador. No requiere servidor.

### Opción 2 — Con servidor local (recomendado para evitar restricciones CORS)
```bash
npx serve .
# o con Python:
python -m http.server 8080
```
Luego abrir `http://localhost:8080`.

## Recompilar SCSS tras cambios

Requiere Node.js instalado.

```bash
# Una sola vez
npx sass styles.scss styles.css --no-source-map

# Modo watch (recompila automáticamente al guardar)
npx sass styles.scss styles.css --no-source-map --watch
```

## Adaptar a un nuevo cliente

Solo es necesario editar la sección `$themes` al inicio de `styles.scss`:

```scss
$themes: (
  "mi-cliente": (
    bg:          #fff3e0,      // color de fondo del bloque
    deco-color:  #ffb74d,      // color de los elementos decorativos
    title-color: #3e2723,      // color del título
    text-color:  #5d4037,      // color del texto
    card:        true,         // true = sombra tipo card
    radius:      16px,         // radio del borde
    deco-type:   "circles",    // "blob" o "circles"
  ),
);
```

Luego en el HTML, usar la clase modificadora correspondiente:
```html
<div class="content-block content-block--mi-cliente">
```

## Uso en Moodle

Copiar el bloque `.content-block` completo en el editor HTML de Moodle.
Los textos dentro de `h3`, `p` y la `img` pueden editarse libremente —
el layout se mantiene porque los estilos no dependen de clases aplicadas
directamente sobre esos elementos.
