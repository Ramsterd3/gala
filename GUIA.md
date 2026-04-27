# Galaxia del Amor — Guía del Proyecto

## ¿Qué es?

Una experiencia visual interactiva en 3D hecha con HTML, CSS y Three.js. Muestra una galaxia animada con anillos, estrellas, texto flotante y un reproductor de música, todo con estética de amor.

---

## Estructura del proyecto

```
gala/
└── Galaxia.html   ← archivo único, todo en uno
```

No requiere instalación ni servidor. Se abre directamente en el navegador.

---

## Tecnologías usadas

| Tecnología | Versión | Uso |
|---|---|---|
| HTML5 / CSS3 | — | Estructura y estilos |
| Three.js | 0.148.0 | Escena 3D |
| Canvas API | — | Texturas generadas en tiempo real |
| Web Audio API | — | Reproductor de música |

---

## Componentes principales

### 1. Reproductor de audio
- Botón ▶️ / ⏸️ para reproducir/pausar
- Barra de progreso clickeable con animación de gradiente
- Muestra tiempo actual / duración
- La canción se carga desde Dropbox y se repite en loop

### 2. Escena 3D (Three.js)
- **Fondo**: textura de nebulosa espacial
- **Campo de estrellas**: 2000 puntos distribuidos aleatoriamente en una esfera de radio 3000
- **Núcleo central**: esfera semitransparente que pulsa con `Math.sin`
- **Texto central**: sprite con "TE AMO ❤️" renderizado en Canvas 2D
- **Glow**: sprite con gradiente radial naranja/rojo que respira
- **Anillos**: dos `RingGeometry` con textura de gradiente, rotan en sentidos opuestos
- **Palabras flotantes**: ~288 sprites con frases románticas que orbitan el centro

### 3. Controles de cámara
| Acción | Efecto |
|---|---|
| Click + arrastrar | Rotar la galaxia |
| Scroll del mouse | Zoom in/out |
| Pellizco táctil (2 dedos) | Zoom en móvil |
| Arrastrar en móvil | Rotar en móvil |

---

## Cómo personalizar

### Cambiar el texto central
```js
const centerTex = makeCenterTextTexture("TU TEXTO AQUÍ");
```

### Cambiar las frases flotantes
Edita el array `baseWords` en el script. Cada elemento es una frase con emoji.

### Cambiar la música
Reemplaza el `src` del elemento `<audio>`:
```html
<source src="TU_ARCHIVO.mp3" type="audio/mpeg">
```

### Cambiar colores del glow y anillos
- Glow: parámetros `color1` y `color2` en `makeGlow()`
- Anillos: gradiente en `ringTexture()`
- Barra de progreso: propiedad `background` en `.progress-bar`

### Ajustar zoom inicial y límites
```js
let targetDist = 300;   // distancia inicial
// límites en el evento wheel:
targetDist = Math.max(160, Math.min(600, targetDist));
```

---

## Cómo ejecutar

1. Abre `Galaxia.html` en cualquier navegador moderno (Chrome, Firefox, Edge)
2. Haz clic en ▶️ para iniciar la música
3. Arrastra para rotar la galaxia
4. Usa el scroll para hacer zoom

> El navegador puede bloquear el audio hasta que el usuario interactúe con la página (política de autoplay). Por eso el botón de play es necesario.

---

## Notas técnicas

- `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))` — limita la resolución para mejor rendimiento en pantallas de alta densidad
- Las texturas de texto se generan con `<canvas>` y se convierten a `THREE.CanvasTexture` en tiempo real
- El loop de animación usa `requestAnimationFrame` con una variable `time` acumulativa para todas las animaciones
- La cámara usa coordenadas esféricas (`rotX`, `rotY`, `currentDist`) convertidas a cartesianas en cada frame
