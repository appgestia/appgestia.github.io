# Gestia · Sistema de diseño

Fuente de verdad de la identidad visual de Gestia. Todo lo que figura acá existe en el código (`styles.css`, `ui.js`, `views.js`). Si una herramienta externa (UI UX Pro Max u otra) recomienda algo que contradice este archivo, gana este archivo.

La base es el trabajo de **Liev** (PR #3 y los commits alrededor): navegación agrupada, menús de acciones, franjas de totales, selector y calendario propios, tarjetas de producto con estados y tablas que pasan a tarjetas en el celular. La identidad verde de noviembre de 2026 cambia los **colores** y algunos **detalles** (sombras, peso tipográfico) sin tocar estructura, componentes ni flujos.

---

## 1. Identidad y filosofía

- **Herramienta de trabajo, no vidriera.** Lo primero es que el comerciante entienda el estado del negocio y cobre rápido. La estética acompaña.
- **Calma y precisión** (referencias: Ramp para datos y jerarquía, Superhuman para tipografía y detalle). Fondos claros, color sólo donde comunica.
- **El verde oscuro es la marca**: barra lateral, logo, avisos de marca. No se pinta la interfaz entera con él.
- **Compacto**: densidad de panel administrativo, sin sacrificar áreas táctiles en el celular.
- **Una sola forma de hacer cada cosa**: antes de crear un estilo, buscar la clase que ya existe.

## 2. Colores

Todos los colores se usan **por variable** (`var(--x)`), nunca en hexadecimal suelto. Definidos en `:root` al principio de `styles.css`, con su versión oscura en `prefers-color-scheme: dark` y `[data-theme="dark"]`.

### Paleta de marca

| Nombre | HEX | Variable | Uso |
|---|---|---|---|
| Verde oscuro | `#183C36` | `--side` | Identidad: barra lateral, barra inferior del celular, banner de datos de ejemplo, avisos (toast), logo, `theme-color` |
| Verde secundario | `#477D6A` | `--accent` | Acciones principales (`.btn.pri`), selección (`.on`), pestañas activas, foco, enlaces, barras de gráficos |
| Marfil | `#F7F7F4` | `--bg` | Fondo de la página |
| Blanco | `#FFFFFF` | `--surface` | Tarjetas, tablas, modales, inputs |
| Grafito | `#252A28` | `--ink` | Texto principal y contraste |

### Neutros y derivados (tema claro)

| Variable | HEX | Uso |
|---|---|---|
| `--muted` | `#626A66` | Texto secundario, etiquetas, encabezados de tabla (5:1 sobre marfil) |
| `--line` | `#E3E4DF` | Bordes y divisores |
| `--soft` | `#F1F1EC` | Fondos suaves: hover, encabezado de tabla, pills neutras, cajas de resumen |
| `--accent-text` | `#3D6E5D` | Verde secundario **cuando es texto**: enlaces, `.lnk`, `.tabact`, precio de `.tile`, opción elegida del desplegable, "Hoy" del calendario (5,4:1 sobre marfil; `#477D6A` como texto da 4,45:1) |
| `--accent-soft` | `#E2EDE8` | Halo de foco en inputs |
| `--accent-ink` | `#FFFFFF` | Texto sobre `--accent` |
| `--mint` | `#A8D8BF` | Acento claro **sólo sobre fondo verde oscuro** (ícono activo del menú, botón del banner) y la barra de "hoy" |
| `--side-ink` / `--side-muted` | `#F7F7F4` / `#A3BBB2` | Texto sobre la barra lateral |

### Estados

| Estado | Texto | Fondo | Uso |
|---|---|---|---|
| Bien | `--good` `#1F7A50` | `--good-bg` `#E2F1E8` | Ganancias, cobrado, vuelto |
| Atención | `--warn` `#9A5A05` | `--warn-bg` `#FBF0DA` | Stock bajo, por vencer |
| Error | `--bad` `#B42318` | `--bad-bg` `#FCE8E5` | Deudas, sin stock, errores, acciones destructivas |
| Info | `--info` `#2D6A86` | `--info-bg` `#E3EEF3` | Avisos neutros, fila seleccionada |

El verde de "bien" (`--good`) es distinto del verde de acción (`--accent`) a propósito: uno informa, el otro invita a hacer clic.

### Tema oscuro

Mismo esquema con grafito: `--bg #131715`, `--surface #1B201E`, `--ink #ECEDE8`, `--side #0F2723`, `--accent #7FB39E` (con `--accent-ink #0F211B`). Las sombras suaves se apagan (`--sh-1: none`).

**Elección del tema**: Ajustes → Apariencia (Claro / Oscuro / Automático, con `.seg`). **Claro es el predeterminado**; Automático sigue al sistema. Se guarda por dispositivo en `localStorage` (`gestia-tema`), fuera de los datos del negocio, y un script en el `<head>` de `index.html` lo aplica antes de pintar para que no parpadee.

### Excepciones permitidas

- Colores de proveedor (`views.js`, `dot()`): son dato del usuario.
- Etiquetas de góndola (`etiquetas.js`): blanco y negro para imprimir.
- Contador rojo del menú (`#e5484d`): alerta sobre fondo oscuro.

## 3. Tipografía

**Inter** variable (Google Fonts, pesos 400–800 y eje óptico `opsz` 14–32), con `font-variant-numeric: tabular-nums` en todo el body para que los importes se alineen.

Inter **trabajada**, no por defecto: `font-feature-settings: 'cv05', 'cv08', 'ss01'` (l con cola, I con serifa —no se confunden con el 1 en códigos—, dígitos abiertos). Las cifras grandes y los títulos usan el corte display: `font-variation-settings: 'opsz' 32`.

| Rol | Tamaño | Peso | Dónde |
|---|---|---|---|
| Título de pantalla | 26px (21px celu) | 650, `-0.025em`, opsz 32 | `.head h1` |
| Cifra de portada | 54px (44 / 38px) | 700, `-0.045em` | `.hoy .big` |
| Título de modal | 19px (17px celu) | 700 | `.md h2` |
| Cifra KPI | 22px (19px celu) | 700, `-0.02em` | `.kpi .v` |
| Total de venta | 24px | 800 | `.total` |
| Título de tarjeta | 14.5px | 650 | `.card h2`, `.archh h3` |
| Texto base | 14.5px | 400 | body, inputs |
| Tabla / botón / nav | 13.5–14px | 400 / 600 / 500 | `td`, `.btn`, `.nav a` |
| Etiqueta de campo | 12px | 600, `--muted` | `.field`, `.kpi .k` |
| Encabezado de tabla / grupo del menú | 12.5px / 12px | 500, en minúscula (sin MAYÚSCULAS espaciadas) | `th`, `.nav .grp` |
| Pill / contador | 11.5px | 600–700 | `.pill`, `.cnt` |

En el celular todos los campos van a **16px** para que el teléfono no haga zoom.

## 4. Espaciado y dimensiones

Escala de hecho: **4 · 6 · 8 · 10 · 12 · 14 · 16 · 24** px.

- Página: `24px 28px` (celu `16px 14px`), ancho máximo 1600px centrado.
- Separación entre bloques de la pantalla: 16px (`.main` gap). Entre tarjetas de una grilla: 14px (`.grid`), KPIs 12px.
- Tarjeta: padding 16px (14px celu). Celda: `9px 12px`.
- Barra lateral: 228px. Grillas: `.g2`, `.g3`, `.g21`, `.kpis`, `.kpis.k3`, `.frm` (dos columnas, una en celu).
- Área táctil en celular (≤860px): botones 40px de alto (`.btn.sm` 36px), botones de ícono 44×44 (stepper, quitar, flechas del calendario), chips, segmentos, pestañas y opciones de menú con padding agrandado.

**Utilidades** (en vez de `style=""`): `.mt0`, `.mt8`, `.mt12`, `.mb8`, `.mb12`, `.grow` (ocupa el espacio libre), `.nw` (sin cortar línea), `.hint` (texto de ayuda bajo un campo). Los `style=""` que quedan son anchos puntuales o valores calculados (barras, colores de proveedor).

## 5. Bordes, radios y sombras

| Token / valor | Uso |
|---|---|
| `--r` 12px | Tarjetas, tablas, KPIs, tiles, calendario |
| 10px | Menús y desplegables, alertas, cajas de resumen (`.ccsum`, `.dcol`), `.biz` |
| `--r-sm` 8px | Botones, inputs, ítems de nav, zonas punteadas |
| 7px | `.btn.sm`, opciones de menú |
| 16px / 18px | Modal / hoja inferior en celu |
| 99px | Pills, chips, contadores, stepper, botón punteado `.tabact` |

- Borde: siempre `1px solid var(--line)`. Punteado (`dashed`) = "acá podés soltar / agregar / está vacío".
- `--sh-1` (`0 1px 2px` grafito al 5%): tarjetas, KPIs, tablas. Apenas se nota; separa del marfil.
- `--sh-pop`: menús, desplegables, calendario.
- Modal y toast: sombra grande propia (elevación máxima).
- `--scrim`: velo detrás de modales y del menú del celular.

## 6. Botones

Clase base `.btn` (borde, fondo blanco, 600, 13.5px, radio 8px). Variantes:

| Clase | Uso |
|---|---|
| `.btn.pri` | Acción principal de la pantalla o del modal. **Una por zona.** |
| `.btn` | Secundaria |
| `.btn.bad` | Destructiva (eliminar, vaciar datos) |
| `.btn.ghost` | Terciaria sin borde (Vaciar venta) |
| `.btn.sm` | En filas, alertas y barras compactas |
| `.tabact` | Acción ligada a pestañas, borde punteado verde (cargar con comprobante) |
| `.lnk` | Enlace de texto ("Ver todo") |
| `.mnu` (`<details>`) | Menú "⋯" / "Más acciones" con lista desplegable |
| `.archvac` | Zona punteada para subir archivos |

Deshabilitado: `[disabled]` (opacidad .5; el principal pasa a gris).

## 7. Inputs y formularios

- `input`, `select`, `textarea`: borde `--line`, radio 8px, padding `8px 10px`; foco = borde `--accent` + halo `--accent-soft` de 3px.
- Siempre dentro de `.field` (etiqueta arriba, 12px gris). `.field.inline` para casilla + texto.
- Formularios con `formModal()` y la grilla `.frm` (`.full` ocupa las dos columnas).
- **Fechas**: `input[type=date]` con el **calendario propio** (`calAbrir`, clase `.cal`). No usar el del sistema.
- **Listas**: `<select>` normal; cerrado muestra una **flecha propia** (mismo trazo y gris `#8C948F` que el ícono del calendario); con mouse se abre el **desplegable propio** (`selAbrir`, clase `.selm`); en celular se abre la lista del sistema.
- Búsqueda: `.srch` (ícono de lupa a la izquierda).
- Importes: `moneyA` para mostrar; campos de efectivo con puntos de miles.
- Validación: en `formModal` el error va **junto al campo** (`aria-invalid`, mensaje `.ferr` enlazado con `aria-describedby`, foco al campo) y se borra al corregir. Para validar desde `leerForm` o `onSave`, lanzar un `Error` con `campo` = nombre del campo; sin `campo`, el mensaje va abajo del formulario. Errores fuera de formularios: `toast(msg, true)` o `.chk .bad`.

## 8. Tarjetas y contenedores

| Clase | Uso |
|---|---|
| `.card` | Contenedor estándar; `h2` como título, `.chd` si lleva acción a la derecha |
| `.kpi` (`.good/.warn/.bad`) | Indicador: etiqueta `.k`, cifra `.v`, detalle `.d`. Si hay período anterior, el detalle lleva `delta(actual, anterior, "vs. …", menos, desde)` → `.dlt` verde/rojo con ▲▼ (y texto oculto para lectores de pantalla). No se muestra si no hay ventas que cubran todo el período anterior (evita porcentajes absurdos con poca historia). `.kpis.k6` = 6 en una fila en pantallas anchas |
| `.ccstrip` | Franja de totales de una sola línea |
| `.ccsum` | Resumen en cifras (4 cajas; `.tot` resaltada) |
| `.demo` | Banner de marca (verde oscuro) |
| `.banner` | Aviso informativo (`.warn`) |
| `.md` (`.wide`) | Modal; pie `.foot` con botones a la derecha |
| `.empty`, `.ccvac`, `.cvac` | Estados vacíos: borde punteado, texto gris, acción sugerida |
| `.tile` | Producto en el punto de venta (`.inc` en la venta, `.out` agotado, `.st.lo` stock bajo) |

## 8 bis. Patrones de pantalla

Piezas compartidas en `ui.js` y `styles.css`. Antes de armar una pantalla nueva, elegir una de estas:

| Patrón | Dónde | Cómo |
|---|---|---|
| **Encabezado fijo** | Todas | `.head` queda pegado arriba con fondo esmerilado (en celular no es fijo). Título + acciones, sin subtítulo de relleno. |
| **Portada** `.hoy` | Panel (lo vendido hoy), Caja (`.cajon`, efectivo esperado) | Bloque `--side` con la cifra que importa en grande (`.big`) y a la derecha su contexto (barras de 14 días, ventas por medio de pago). **Una por pantalla, sólo donde hay una cifra protagonista.** |
| **Franja de cifras** `franja(act, on, items)` | Productos, Clientes, Compras, Inventario | Banda con divisores. Con `act`, cada cifra es un botón que filtra (la elegida queda subrayada en verde); con `cls:'nofil'` o sin `act` es sólo informativa. `.kpis` usa el mismo aspecto. |
| **Renglones** `.filas > .fila` | Compras, Clientes, Inventario, Gastos | Grilla: marca a la izquierda (`.pt` punto de estado, `.pv` línea del color del proveedor, `avatar(nombre)`, `.fd` día) · `.fm` título + detalle · `.fe` estado · `.fn` importe. `.fila.click` con `data-act` abre la ficha. Agrupar con `.grupo > .gh` (mes, vencimiento). No se ordenan: van ordenados por lo más útil. |
| **Fichas** `.provs > .prv` | Proveedores | Tarjeta con `--pc` (color del proveedor) arriba, avatar, `dl` de 3 cifras y pie. |
| **Barra de stock** `.stk` | Productos | Cantidad, barra contra 3 veces el mínimo con marca del mínimo, color de estado. |
| **Banda de cobro** `.cobro` | Venta | Total en grande, vuelto y Cobrar (menta) sobre `--side`, como el visor de una registradora. |
| **Índice lateral** `.ajustes > .ajidx + .ajcol` | Ajustes | Botones `data-act="aj-ir"` que llevan a cada sección; en celular el índice pasa arriba. |
| **Secciones sin caja** | Panel, Reportes, Caja | `#view[data-v="…"]` (la pantalla actual) saca la tarjeta a las secciones: título con línea y contenido sobre el fondo. La tarjeta queda para objetos aparte (venta, arqueo, modales). |
| **Ticket** `pre.ticket` | Venta | Papel `#F5F1E4` con borde troquelado (máscara), monoespaciada. |
| **Más opciones** | Formularios | Campo con `mas:1` en la definición de `formModal` va plegado en `details.mas`; se abre solo si tiene datos o un error. |

## 9. Tablas y listados

- Siempre dentro de `.tw` (scroll horizontal). **Sin caja**: la tabla va sobre el fondo con líneas finas; la fila se ilumina en blanco al pasar.
- Encabezado gris en minúscula con línea inferior más marcada. Importes con `.n` (a la derecha). Totales en `tr.tot` o `tfoot`.
- Filas clicables `tr.click` (hover `--soft`), seleccionada `tr.sel`, inactiva `tr.off`. Todo elemento con `data-act` que no sea botón o enlace se vuelve enfocable con Tab y se abre con Enter/Espacio (automático, `ui.js`).
- **Orden**: toda tabla dentro de `.tw` con 3 filas o más se ordena tocando el encabezado (`aria-sort`, flechita). Reconoce importes, cantidades, porcentajes y fechas; `tr.tot` queda al final. El orden se recuerda mientras la página esté abierta.
- **Celular (≤640px)**: `table.tcards` convierte cada fila en tarjeta; cada `td` lleva `data-l="Columna"`. `data-l=""` = fila de acciones.
- Listas laterales (cuenta corriente) `.ccl`: filas separadas, seleccionada con barra verde a la izquierda.
- Rankings `.tv` y barras horizontales `.hb`.

## 10. Navegación

- **Computadora**: barra lateral `--side` con logo, negocio (`.biz`), grupos (Inicio, Vender, Stock, Gestión, Sistema) y pie con estado del respaldo. Activo: fondo blanco translúcido + ícono `--mint`. Contadores rojos para alertas.
- **Celular (≤860px)**: barra superior con logo y botón de menú (abre la misma barra lateral como cajón) + **barra inferior** con 5 accesos (`.bnav`). En Ventas, `.cartbar` flotante con el total.
- **Pestañas** `.tabs` / `.tabbar`: subrayado `--accent` en la activa.
- **Segmentos** `.seg`: filtro de 3 opciones (activo blanco con anillo verde).
- **Chips** `.chips`: filtros por categoría.
- **Buscador rápido** (`Ctrl K` / `⌘ K`, botón `.qkbtn` en la barra lateral): secciones, acciones (nueva venta, producto, cliente, proveedor, gasto), productos, clientes y proveedores. Flechas para moverse, Enter para abrir.
- **Atajos** (fuera de campos de texto): `N` nueva venta, `/` foco en el buscador de la pantalla, `Esc` cierra. Se muestran con `<kbd>`.
- **Saltar al contenido**: botón `.skip`, primer elemento de la página, visible sólo con Tab.
- **Panel**: portada del día (lo vendido hoy y 14 días), franja de indicadores, alertas y más vendidos, y al final "Para reponer". El título es la fecha, no un saludo.

## 11. Iconografía

- SVG en línea, **trazo** (no relleno), `stroke-width 1.8`, puntas redondeadas, viewBox 24. Tamaño 15–17px en la interfaz, 22px en la barra del celular.
- Color: `currentColor` (heredan del texto); el ícono activo toma `--mint` o `--accent`.
- Logo: red de nodos sobre cuadrado `#183C36` (`icon.svg`, `GESTIA_MARK` en `ui.js`, PNG 192/512).
- No sumar librerías de íconos: seguir dibujando con el mismo estilo de trazo.

## 12. Estados visuales

| Estado | Cómo se ve |
|---|---|
| Hover | Fondo `--soft` (botones, filas, opciones) o borde `--accent` (tiles) |
| Foco teclado | Contorno 2px `--accent` (`:focus-visible`); inputs con halo |
| Seleccionado | Mezcla 14–20% de `--accent` + anillo/borde `--accent` (chips, medios de pago, filas) |
| Éxito | `toast()` verde oscuro; `.pill.good` |
| Error | `toast(msg,true)` rojo; `.guarderr` arriba si falla el guardado; `.al.bad` / `.chk .bad` |
| Advertencia | `.al.warn`, `.pill.warn`, `.vlt-av` |
| Cargando | `.spin` |
| Vacío | `.empty` punteado con acción |
| Deshabilitado | Opacidad .5, cursor no permitido |

Alertas `.al`: renglones tipo bandeja con un **punto de color** y línea fina abajo. Sin cajas ni raya lateral (es un cliché de plantilla). Estados en tablas: `.est` (texto con punto); la `.pill` queda para lo que pide acción (Sin stock, Anulada, margen bajo).

## 13. Responsive

| Corte | Cambio |
|---|---|
| ≤1200px | Acciones de cuenta corriente en 2 columnas |
| ≤980px | Grillas a 1 columna, venta debajo de productos, KPIs de 3 a 2 |
| ≤860px | Navegación de celular (barra arriba + barra inferior + cajón) |
| ≤760px | Pestañas con acción debajo |
| ≤640px | Tablas como tarjetas, modales como hoja inferior con pie fijo |
| ≤560px / ≤440px | Formularios a 1 columna / medios de pago sin ícono |

Respetar `env(safe-area-inset-bottom)` en todo lo fijo abajo, y `prefers-reduced-motion`.

## 14. Reglas de consistencia

1. **Colores sólo por variable.** Un hexadecimal nuevo en `styles.css` fuera de `:root` es un error (salvo las excepciones de la sección 2).
2. **Reusar antes de crear**: `.card`, `.btn`, `.field`, `.tw`, `.pill`, `.empty`, `.mnu`, `selAbrir`, `calAbrir`, `formModal`, `confirmar`, `toast`, `moneyA`.
3. **Un botón principal por zona.** El verde de acción no se usa para decorar.
4. **Verde oscuro = marca**, verde secundario = acción, grafito = texto, verde de "bien" = resultado positivo. Si el verde secundario va como **texto**, usar `--accent-text`; `--accent` queda para rellenos, bordes e íconos.
5. **Toda tabla nueva** lleva `class="tcards"` y `data-l` en cada celda.
6. **Toda pantalla nueva** arranca con `.head` (h1 + `.sub` + `.bar` de acciones) y usa las grillas existentes.
7. **Sin estilos en línea** para espaciado: usar las utilidades de la sección 4.
8. **Detalles del mostrador**: precios como etiqueta de góndola (`.tile .pr` con `/ kg` chico), ticket de papel, planilla de efectivo. Preferir el vocabulario del comercio a los patrones genéricos de panel.
9. **Probar en claro, oscuro y 390px** antes de dar por terminado; correr `cd pruebas && npm test`.
10. Si una herramienta de diseño sugiere otra paleta, tipografía o un `MASTER.md`, no se aplica sin acordarlo: este archivo manda.
