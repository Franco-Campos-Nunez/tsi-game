# Brecha Cero · traspaso para nuevos capítulos

Documento para retomar el trabajo en un chat nuevo. Léelo completo antes de tocar nada.

## Dónde está todo

| Qué | Dónde |
|---|---|
| Juego publicado (lo usan alumnos y colegas) | repo `Franco-Campos-Nunez/tsi-game`, rama `main`, archivo `index.html` → https://franco-campos-nunez.github.io/tsi-game/ |
| Versión congelada del capítulo 8 | rama `cap8-publicado` en `tsi-game` (copia exacta de lo compartido) |
| Desarrollo de capítulos nuevos | rama `desarrollo` en `tsi-game` (GitHub Pages no la publica) |
| Respaldo y herramientas | repo `Franco-Campos-Nunez/Proyecto-TSI`, rama `brecha-cero-backup`, carpeta `brecha-cero/` |
| Vista previa en Claude (borrador entre el profesor y Claude) | artefacto https://claude.ai/artifact/6sk4u2QxFffACm4vD8rtRJ — para publicar ahí desde otro chat: leerlo primero con Artifact `read` y luego publicar con su `url` |

- `brecha-cero.html`: el juego (un solo archivo, sin dependencias, salvo Google Fonts).
- `brecha-cero-jugar.html`: lo mismo envuelto en `<!doctype html>…` con viewport sin zoom. Es lo que se copia a `tsi-game/index.html`.
- `definiciones.py`: definiciones y ejemplos tomados del libro (Laudon) para los conceptos.
- `VOCABULARIO.md`: vocabulario común. Respétalo.
- `check.js` (validador: `node check.js brecha-cero.html`), `bot2.js` (juega misiones solo: `node bot2.js all` o `node bot2.js 2.3`), `mkprev.sh` (genera `preview.html` con `window.__bc` para Playwright).

## Reglas de trabajo del profesor

1. **`main` no se toca** mientras no lo pida explícitamente: el juego ya está en manos de alumnos y colegas. Todo el trabajo nuevo va a la rama `desarrollo` y al artefacto.
2. No publicar en GitHub en cada cambio (gasta créditos). Publicar en el artefacto sí; en GitHub solo cuando lo pida.
3. Comentarios en el artefacto: responder en el mismo hilo con texto plano, sin emojis. En el chat, una línea corta. No resolver hilos.
4. Responder en español. Poco texto en pantalla, estilo arcade/neón, vocabulario consistente.
5. Commits terminan con las líneas de coautoría de la sesión. Sin nombres de modelos en commits. Sin PR salvo que lo pida.
6. Las leyes se explican como categorías globales (RGPD como referencia), sin perder las del libro: otros profesores escriben la prueba.

## Estructura del juego

Jerarquía: **Capítulo › Sección › Misión › Minijuego › Ítem › Concepto**.

- Orden de entrada: primero se elige la **partida** (P1–P3, contiene todos los capítulos) y después el **capítulo**.
- `CHAPTERS` (cerca de `renderTitle`): lista de capítulos del selector. El capítulo 8 es `c8` con `ready:true`; el siguiente está como `next` con `ready:false` («En preparación»).
- `C`: conceptos `{s, t, h, d, x}`: sección, título, gancho corto, definición del libro, ejemplo.
- `SECTORS`: las secciones del capítulo 8 (Vulnerabilidad, Valor, Marco, Defensa y Simulacro). Cada misión tiene `name`, `m`, `brief` (conceptos que se muestran antes) y `phases` (minijuegos).
- `MECH`: los minijuegos disponibles. Los que se usan hoy: Interceptar (`mcq`), Clasificar (`sort`), Circuito (`seq`), Diagrama (`drag`), Cableado (`wire`), Carrera (`run`), Descifrar (`word`), Escáner (`snake`), Auditor (`audit`), Contramedida (`tower`). Portero (`gate`) y Pares (`memo`) se convierten solos en Clasificar y Cableado.
- Guardado: `localStorage` con claves `brecha0:save`, `brecha0:save2`, `brecha0:save3` y `brecha0:slot`, más una copia en la nube solo dentro de Claude.

## Pendiente para el capítulo nuevo (primeros pasos en el chat nuevo)

El selector ya existe, pero hoy solo abre el capítulo 8. Para que el capítulo nuevo funcione hay que:

1. Mover `C` y `SECTORS` a un contenido por capítulo, por ejemplo `CHAPTERS[i].concepts` y `CHAPTERS[i].sectors`, y que el juego use los del capítulo elegido.
2. Guardar el avance de cada capítulo **dentro de la misma partida** (la partida contiene todo), sin romper las partidas actuales del capítulo 8: su avance debe seguir apareciendo.
3. Separar ranking, repaso del día, colección de cartas y FAQ por capítulo donde corresponda.
4. Elegir qué minijuegos usa el capítulo nuevo. Mantener la mayoría para que los alumnos no aprendan juegos nuevos. Interceptar es muy propio de seguridad y podría quedar fuera o recibir otra temática.
5. Escribir el contenido desde el libro: conceptos con definición y ejemplo, 4 secciones y un simulacro final, igual que el capítulo 8.
6. Probar con `check.js` y `bot2.js all` antes de publicar.

## Cómo publicar cuando lo pida

```sh
# desde la carpeta con brecha-cero.html
PRE='<!doctype html><html lang="es"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover"><style>[hidden]{display:none!important}</style></head><body>'
{ printf '%s' "$PRE"; cat brecha-cero.html; printf '\n</body></html>'; } > brecha-cero-jugar.html
cp brecha-cero-jugar.html <tsi-game>/index.html   # en la rama que corresponda
```
