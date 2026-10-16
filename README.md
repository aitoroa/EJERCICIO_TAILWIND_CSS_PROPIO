# Ejercicio: Dale identidad a la tienda

**La Aguja y el Tornillo · Ferretería y Mercería**

Ya habéis maquetado la tienda con Tailwind. Ahora mismo funciona, pero se parece a cualquier web hecha con Tailwind: usa la paleta por defecto (`green-900`, `amber-100`, `red-700`...) y no tiene nada propio.

En este ejercicio vais a darle **identidad visual** combinando las dos cosas que usaréis en cualquier proyecto real:

- **Tailwind** para el layout, los espaciados, el responsive y los estados.
- **Vuestro CSS propio**, en un archivo externo como siempre, para todo lo que Tailwind no resuelve bien.

---

## Archivos

| Archivo | Qué es |
| --- | --- |
| `index.html` | La tienda ya maquetada con Tailwind. Ya tiene enlazado `css/styles.css` y un bloque `@theme static` vacío. |
| `css/styles.css` | Vuestro CSS propio. Viene preparado con el orden de capas y comentarios. |
| `img/tienda.jpg` | Una foto del interior de la tienda, por si queréis usarla en la portada. |

---

## Tareas

### 1. Sistema de diseño con `@theme`

En el bloque `<style type="text/tailwindcss">` del `<head>`, cread vuestros propios tokens:

- **Mínimo 4 colores** con nombres que tengan sentido para la tienda (por ejemplo `rotulo`, `crema`, `mostaza`, `granate`), no `verde-1` o `color2`.
- **Mínimo 1 tipografía** de [Google Fonts](https://fonts.google.com/) para los títulos (por ejemplo `--font-rotulo`). Pegad su `<link>` en el `<head>`, donde indica el comentario.

```html
<style type="text/tailwindcss">
    @theme static {
        --color-rotulo: #1f4d2b;
        --font-rotulo: "Nombre de la fuente", serif;
    }
</style>
```

Después **sustituid en el HTML** los colores y la tipografía por defecto por los vuestros: `bg-green-900` pasa a ser `bg-rotulo`, `font-serif` pasa a ser `font-rotulo`, etc. Al terminar no debería quedar ningún color de la paleta por defecto en las zonas principales.

Documentación: [Theme variables](https://tailwindcss.com/docs/theme) · [Personalizar colores](https://tailwindcss.com/docs/colors#customizing-your-colors) · [Font family](https://tailwindcss.com/docs/font-family)

### 2. CSS propio en `css/styles.css`

Añadid **al menos 3 cosas que Tailwind no hace bien**, usando vuestros tokens con `var(--color-...)`. Entre ellas, como mínimo:

- **Una animación con `@keyframes`.**
- **Un elemento decorativo con `::before` o `::after`.**

Ideas que encajan con una tienda de barrio:

- Un **rótulo luminoso** que brilla y parpadea de vez en cuando (`text-shadow` + `@keyframes`).
- Un **zócalo de baldosa hidráulica**: una franja decorativa hecha solo con degradados (`conic-gradient`, `radial-gradient`).
- Un **subrayado de costura** bajo los títulos (`::after` con un borde discontinuo).
- Una **etiqueta de precio** con forma de etiqueta (`clip-path`) y el agujero hecho con `::before`.
- Una **cinta de ofertas** que se desplaza sin fin (marquee).

Documentación: [@keyframes (MDN)](https://developer.mozilla.org/es/docs/Web/CSS/@keyframes) · [Pseudoelementos (MDN)](https://developer.mozilla.org/es/docs/Web/CSS/Pseudo-elements) · [clip-path (MDN)](https://developer.mozilla.org/es/docs/Web/CSS/clip-path) · [Degradados (MDN)](https://developer.mozilla.org/es/docs/Web/CSS/CSS_images/Using_CSS_gradients)

### 3. Sección nueva: "Ofertas de la semana"

Añadid una sección nueva entre "Productos" y "Horario", con su enlace en el menú. Maquetadla **mezclando** clases de Tailwind (rejilla, espaciados, colores, responsive) y vuestras clases de `styles.css` (las decoraciones).

Contenido que podéis usar:

| Producto | Precio | Descripción |
| --- | --- | --- |
| Caja de 100 tornillos | 2,50 € | Tornillo de madera de 4 × 30 mm. El de toda la vida. |
| Bobina de hilo | 1,20 € | Algodón 100 %, en más de 40 colores. |
| Bombilla LED 9 W | 3,95 € | Luz cálida, casquillo grueso E27. |
| Botones de nácar | 2,00 € | Bolsita de 6 unidades, para camisa. |

Debe verse en 1 columna en móvil, 2 en tablet (`sm:`) y 4 en escritorio (`lg:`).

---

## Las dos trampas que hay que conocer

### Trampa 1: vuestro CSS "pisa" a Tailwind

Tailwind v4 guarda sus estilos dentro de capas (`@layer`). Cualquier CSS que esté **fuera** de una capa gana siempre a las capas, aunque sea menos específico. Por ejemplo, si escribís esto en `styles.css`:

```css
h1 { color: red; }
```

el título se verá rojo aunque en el HTML tenga `class="text-green-900"`.

**Solución:** dejad la primera línea de `styles.css` como está (`@layer theme, base, components, utilities;`) y meted vuestras clases dentro de `@layer components { ... }`. Así las clases del HTML siguen teniendo la última palabra.

### Trampa 2: `var(--color-...)` vacío

Por defecto, Tailwind solo crea las variables de los colores que **usáis en alguna clase del HTML**. Si definís `--color-mostaza` pero no aparece ninguna clase `bg-mostaza`, `text-mostaza`..., entonces `var(--color-mostaza)` en `styles.css` estará vacío.

**Solución:** escribid `@theme static { ... }` (con `static`), como ya viene en el `index.html`. Así todas las variables existen siempre.

---

## Retos extra

1. **Modo oscuro** con el prefijo `dark:` usando vuestros tokens. [Dark mode](https://tailwindcss.com/docs/dark-mode)
2. **Movimiento reducido**: desactivad vuestras animaciones en `@media (prefers-reduced-motion: reduce)` (el hueco ya está preparado al final de `styles.css`).
3. **Pausa al pasar el ratón** por la cinta de ofertas (`animation-play-state: paused`).

---

## Documentación de referencia

- [Tailwind: Theme variables](https://tailwindcss.com/docs/theme)
- [Tailwind: Añadir estilos propios](https://tailwindcss.com/docs/adding-custom-styles)
- [Tailwind: Play CDN](https://tailwindcss.com/docs/installation/play-cdn)
- [MDN: @layer (capas de cascada)](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer) (en inglés)
- [MDN: Animaciones CSS](https://developer.mozilla.org/es/docs/Web/CSS/Guides/Animations/Using)
