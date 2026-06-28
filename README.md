# Ficha de Personaje · Final Fantasy

Ficha interactiva (un solo archivo `index.html`, sin dependencias online salvo las
fuentes de Google). Los nodos del tablero usan **iconos PNG locales** con dos
estados: activado (`on`) y desactivado (`off`).

## Estructura

```
.
├── index.html                 ← la ficha (rutas RELATIVAS a los iconos)
├── .nojekyll                  ← evita que GitHub Pages procese/ignore carpetas
├── README.md
└── assets/
    └── nodos/
        ├── on/                ← nodo activado
        │   ├── fue.png vit.png agi.png vel.png int.png vol.png
        │   ├── tec.png hab.png mbl.png mne.png
        │   └── bloqueo-1.png bloqueo-2.png bloqueo-3.png bloqueo-4.png
        └── off/               ← nodo desactivado (mismos nombres)
```

Todos los iconos están normalizados a **256×256 px**, fondo transparente.

## Publicar en GitHub Pages

1. Crea un repositorio y sube estos archivos (mantén las rutas tal cual).
2. *Settings → Pages →* Source: **Deploy from a branch**, rama `main`, carpeta `/ (root)`.
3. La ficha queda en `https://TU-USUARIO.github.io/TU-REPO/`.

> Las rutas de los iconos son **relativas** (`assets/nodos/on/fue.png`), así que
> funcionan tanto en local (abriendo `index.html`) como en un *project site* bajo
> `/TU-REPO/`. No uses rutas absolutas con `/` inicial.

## Mapeo nombre de archivo → tipo de nodo

| Tipo (código) | Archivo        | Tipo (código) | Archivo          |
|---------------|----------------|---------------|------------------|
| FUE Fuerza    | `fue.png`      | TEC Técnica   | `tec.png`        |
| VIT Vitalidad | `vit.png`      | HAB Habilidad | `hab.png`        |
| AGI Agilidad  | `agi.png`      | MBL M. Blanca | `mbl.png`        |
| VEL Velocidad | `vel.png`      | MNE M. Negra  | `mne.png`        |
| INT Intelecto | `int.png`      | BLO Nivel 1-4 | `bloqueo-1..4.png` |
| VOL Voluntad  | `vol.png`      |               |                  |

## Personalizar

- **Cambiar un icono:** sustituye el PNG correspondiente en `on/` y/o `off/`
  conservando el nombre. Ideal 256×256, fondo transparente.
- **Tamaño del icono en el tablero:** edita `const ICON_SCALE = 2.45;` en
  `index.html` (mayor = icono más grande respecto al nodo).
- **Volver a los círculos de color:** si `nodeIcon()` devuelve `null` para un tipo
  (p. ej. borrando su entrada en `ICON_FILE`), ese nodo vuelve a dibujarse con el
  círculo + etiqueta originales. El respaldo está integrado.

## Cómo se integran (resumen técnico)

El tablero se dibuja como un único `<svg>` en `renderBoard()`. Cada nodo
(`<g data-node>`) ahora pinta un `<image>` con el icono según su **tipo**, su
**nivel** (bloqueos) y si está **activado**. El indicador de posición (anillo
giratorio) y el resaltado de nodos disponibles se conservan por encima del icono,
y el manejador de clic (`closest("[data-node]")`) sigue funcionando sin cambios.
