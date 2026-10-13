# kitchen-live — RestaurantFlow

Sistema de gestión para restaurante, pensado para funcionar **sin backend**: todo
corre en el navegador y los datos se guardan en `localStorage`. Sirve para
probar el flujo completo de un restaurante (mesas, pedidos, cocina, caja,
inventario, cierres) con datos de prueba, sin instalar nada.

## Origen del proyecto

El plan original (ver `prompts.txt`) pedía un sistema cliente-servidor
completo para uso real en un restaurante: HTML/CSS/JS en el frontend,
backend en Python con FastAPI, base de datos PostgreSQL, login real con
usuarios y permisos, WebSockets para tiempo real, e instalación en una red
local. Ese documento se conserva íntegro como referencia histórica, con una
sección al principio ("ESTADO REAL DE LA IMPLEMENTACIÓN") que resume, módulo
por módulo, qué se construyó realmente.

Lo que terminó implementándose es una versión más chica, pensada para
demostrar el flujo operativo de punta a punta sin backend ni servidor: no
hay base de datos externa ni login con usuarios reales, y la sincronización
entre pantallas es por lectura de `localStorage`, no por WebSockets. A
cambio, tiene funciones que no estaban en el pedido original, como zonas
del salón, carga rápida de mesa por número, asignación de mozo por mesa, y
un módulo de inventario con recetas y costos en vez del simple control de
stock por producto que se había planeado al inicio.

## Arquitectura técnica

- **Sin build ni dependencias**: cada página (`kitchen-live.html`,
  `dashboard.html`, `pantalla-cocina.html`) es un único archivo HTML con su
  propio `<style>` y `<script>`, sin frameworks ni paso de compilación. Se
  edita y se abre directamente.
- **Persistencia**: todo el estado (mesas, productos, insumos, pedidos,
  caja, cierres, configuración) vive en una sola clave de `localStorage`,
  `restaurantFlowLocalDB`, como un único objeto JSON.
- **"Tiempo real" por polling**: las páginas no usan WebSockets; cada una
  relee `localStorage` periódicamente, así que si se abren varias pestañas
  del mismo navegador (por ejemplo Mapa en una y Cocina en otra) los
  cambios se reflejan con un pequeño retraso, no al instante.
- **Tema oscuro/claro**: se define con variables CSS (`--ink`, `--paper`,
  `--brand`, etc.) sobre `:root`, con un bloque alternativo para
  `:root[data-theme="light"]`. La preferencia se guarda en `localStorage` y
  se aplica con un script mínimo antes de pintar la página, para evitar el
  parpadeo del tema por defecto.

## Cómo usarlo

Instrucciones más detalladas, con pasos para solucionar problemas si la
página no carga bien, en el documento
[RestaurantFlow — Instalación y uso](https://docs.google.com/document/d/19az3on9c9mE7NfaK6XuhyoNB42ro-rTmir-n5SbFR30/edit?usp=drivesdk).

No requiere build ni servidor: abrí `kitchen-live.html` directamente en el
navegador (doble clic, o `python3 -m http.server` y entrar a esa URL si tu
navegador bloquea `file://`).

**Con Live Server (VS Code):** clic derecho en `kitchen-live.html` → *Open with
Live Server* (el puerto está fijado en 5501 en `.vscode/settings.json`). Las
tres páginas comparten `localStorage` solo si se abren bajo el **mismo
origen**: usá siempre `http://127.0.0.1:5501/...` o siempre
`http://localhost:5501/...`, sin mezclarlos. Ojo también con que abrir por
`file://` usa un `localStorage` aparte del de Live Server, así que los datos
de prueba no se ven de un modo al otro. El modo admin vive en memoria: si
Live Server recarga la página al guardar un archivo, hay que volver a
activarlo (el tema, el rol y los datos sí se conservan).

> **Nota para quien edite el código:** Live Server inyecta su script de
> recarga justo antes del *primer* `</body>` que encuentra en el archivo, aunque
> esté dentro de un string de JavaScript. Por eso, dentro de los `<script>`
> hay que escribir `<\/body>` (con la barra escapada) y nunca `</body>`
> literal; si no, la página se rompe y muestra su propio código como texto.

Para ver el sistema en uso rápido: **Config → Datos de prueba → Cargar datos
de prueba** (requiere modo admin, PIN por defecto `1234`). Carga 16 mesas en 3
zonas, 4 mozos, 25 productos con receta, 34 insumos (con compras, mermas,
conteos y algunos agotados o bajo el mínimo), una caja abierta con
movimientos, comandas abiertas en todos los
estados (pendiente, preparando, listo, entregado, cuenta pedida), **unos 60
días de historial de ventas** (con picos, días flojos, pagos divididos y
cancelaciones), cierres confirmados de días anteriores (ayer y anteayer
quedan sin confirmar para probarlo), historial de precios y de movimientos de
inventario. Si ya cargaste datos de prueba antes, conviene "Vaciar todos los
datos" primero, porque cargarlos de nuevo los duplica.

La primera vez que se abre `kitchen-live.html` aparece un **tutorial** paso a
paso que explica cada módulo del sistema (no vuelve a aparecer solo después).
Se puede reabrir en cualquier momento, junto con un **acceso rápido** con la
explicación de todas las funciones en una sola lista, desde el botón "Ayuda /
Tutorial" al pie de la barra lateral.

## Páginas del sistema

| Archivo | Qué es |
|---|---|
| `kitchen-live.html` | La aplicación principal. Todo el flujo operativo. |
| `dashboard.html` | Panel de solo lectura: KPIs del día (ventas, ocupación, productos top, margen bruto estimado, etc.), insumos para reponer, **gráficos de líneas** (ventas por día, ventas por hora del día, ticket promedio por día, personas atendidas por día), **gráficos de torta** (ventas por medio de pago, por categoría y por mozo, y cancelaciones por motivo) con selector de período de 7/14/30/60 días, tooltips al pasar el mouse y un "Ver datos" con la tabla de cada gráfico, y estadísticas históricas (mejor día de ventas, producto más vendido de la semana/mes/año). |
| `pantalla-cocina.html` | Pantalla grande para la cocina (estilo tablero de KDS), con 3 columnas: Pendiente / En preparación / Listo. Clic izquierdo para avanzar un ítem un paso; clic derecho para retroceder un paso, por si se tocó por accidente. |
| `a.html`, `cocina-ordu.html` | Prototipos previos, se mantienen como referencia histórica. |

El **tema oscuro/claro** (Config → Apariencia) se aplica a las tres páginas
activas, y si lo cambiás con otra pestaña abierta, esta se actualiza sola.

Las tres páginas activas están enlazadas entre sí y leen/escriben la misma
base de datos en `localStorage` (clave `restaurantFlowLocalDB`), así que
podés tenerlas abiertas en pestañas distintas del mismo navegador y ver los
cambios reflejarse (con un pequeño polling, no en tiempo real instantáneo).

`kitchen-live.html` y `dashboard.html` son responsive: en pantallas chicas
el rol y la navegación quedan colapsados detrás de un botón flotante en la
esquina (menú tipo cajón), para dejarle más espacio al contenido, y las
tablas con muchas columnas se pueden desplazar horizontalmente en vez de
achicarse. `pantalla-cocina.html` está pensada para una pantalla grande de
cocina y no tiene este tratamiento mobile.

## Módulos de `kitchen-live.html`

- **Mapa · Mozo** — plano del salón, mesas (libre/ocupada/cuenta pedida),
  toma de pedidos con opciones y observaciones, cancelación de pedidos con
  motivo. Soporta **zonas** (salón, terraza, planta alta, etc.), cada una con
  su propio plano, útil para restaurantes grandes, con flechas ‹ › para
  cambiar de zona. Tiene un buscador de **carga rápida** para abrir una mesa
  por número sin buscarla visualmente. Al abrir una mesa (o en cualquier
  momento mientras está ocupada) se puede elegir qué **mozo** la está
  atendiendo, de la lista cargada en Config. En pantallas chicas, el rol y
  la navegación se guardan en un botón minimizado en la esquina para
  dejarle más espacio al mapa. El salón tiene dos vistas, **Plano** y
  **Lista**: en el celular arranca en Lista (tarjetas grandes por mesa con
  el mozo, las personas, el tiempo abierta y cuántos ítems están listos para
  retirar, con filtros Libres / Ocupadas / Cuenta pedida), y la comanda se
  abre como una hoja desde abajo con botones cómodos para el pulgar. La
  elección se recuerda en cada dispositivo. Una vez que se pide la cuenta de una mesa,
  quitar un ítem de la comanda pasa a requerir modo administrador (antes de
  pedir la cuenta, el mozo puede seguir quitando ítems pendientes
  libremente). **Cargar productos a una mesa** está pensado para el
  celular: el selector tiene buscador y categorías fijas arriba, filas
  grandes que se tocan enteras (agrupadas por categoría) y marca los
  productos sin insumos. Al elegir uno se configuran las opciones (las
  obligatorias que faltan se resaltan), la cantidad con botones − / +, notas
  rápidas ("Sin sal", "A punto"...) y el total se ve en vivo en el botón fijo
  de abajo, con la opción "Agregar y cargar otro" para seguir sumando ítems.
- **Cocina** — comandas en vivo por estación, con el mozo asignado a cada
  mesa visible en cada ticket. El avance de estado (pendiente → preparando →
  listo) es solo para el rol Cocina (o el admin); el rol Mozo no puede
  tocarlo, solo ve "A cargo de cocina". Cuando un ítem queda "listo", recién
  ahí el mozo (o el admin) ve el botón "Retirado ✓" para marcarlo como
  entregado sin tener que ir hasta la mesa — es la única acción que el mozo
  puede hacer en esta sección. Los tickets se muestran en una grilla de 4
  columnas (3 / 2 / 1 en pantallas más chicas). Con el **rol Mozo**, Cocina
  se organiza distinto: un **filtro por nombre de mozo** (se recuerda en cada
  dispositivo) para ver solo las propias mesas, una sección **"Listos para
  retirar"** con el botón "Retirado ✓" arriba de todo, y las comandas que
  siguen en cocina agrupadas por mozo (por estación). Con "Todos" se ven los
  grupos de cada mozo; cada mozo del filtro muestra cuántos ítems tiene
  listos.
- **Productos** — catálogo con categorías (grilla de 4 columnas, 3 / 2 en
  pantallas más chicas), precios, grupos de opciones, **receta** (qué
  insumos y cuánto de cada uno consume una porción, con su costo y margen
  calculados en vivo) e historial de cambios de precio.
- **Inventario** — reemplaza al viejo contador de stock por producto: el
  control se hace sobre los **insumos** (ingredientes), que es lo que
  realmente se agota en un restaurante.
  - *Insumos*: stock, mínimo, costo y proveedor de cada uno, con alertas de
    bajo mínimo / agotado, buscador y filtro. Acciones rápidas: comprar,
    registrar **merma** (con motivo) y **conteo físico** (registra la
    diferencia). Alta, edición y baja de insumos solo en modo admin.
  - *Compras*: registro de compras con varios ítems y proveedor (actualiza
    el **costo promedio** de cada insumo), **lista de compras sugerida**
    agrupada por proveedor (con "Copiar pedido" y "Registrar compra"
    precargado) e historial de compras.
  - *Costos y margen*: costo, margen y **food cost** de cada plato según su
    receta, cuántas porciones alcanzan con el stock actual y qué productos
    todavía no tienen receta.
  - *Movimientos*: historial por fecha y por tipo (compra, consumo por
    venta, anulación, merma, conteo), valorizado, con totales del día.
  Al cargar un producto en una comanda se descuentan solos los insumos de
  su receta (y se devuelven si se quita o se anula); si faltan insumos el
  producto aparece como "Sin insumos" y no se puede agregar (el admin puede
  forzarlo).
- **Caja** — apertura con monto inicial, cobro de mesas (con pago dividido
  en varios medios de pago), movimientos manuales de ingreso/egreso, cierre
  con monto real contado y diferencia.
- **Cierre** — resumen del día (ventas, productos vendidos, personas,
  cubiertos, caja, cancelados), exportable a **Excel** (`.xls`, con
  secciones en colores distintos, filas con ancho/alto propio, medios de
  pago resaltados con su propio color y el producto más vendido destacado —
  no un CSV plano). Incluye el **costo de lo vendido y el margen bruto**
  estimados según las recetas. Las secciones "Productos vendidos", "Detalle
  por mesa" y "Pedidos cancelados" se pueden minimizar. Se puede **confirmar**
  un cierre: a partir de ahí, esa fecha queda bloqueada.
- **Config** — zonas del salón (nombre + plano por zona), nombres de
  **mozos** (para asignarlos a las mesas), cantidad de cocineros y
  estaciones, PIN de administrador, **tema oscuro/claro**, carga/borrado de
  datos de prueba.

Un selector de **rol** en la barra lateral (Mozo / Cocina / Caja /
Administrador) filtra qué pestañas ve cada uno, y además
condiciona qué puede tocar dentro de Cocina (avanzar estados vs. marcar
"Retirado") y dentro de una comanda con cuenta pedida (quitar ítems). Es
solo organizativo — no reemplaza al **modo admin** (PIN), que es el único
control de acceso real del sistema (protege edición de productos, config y
acciones sensibles como cancelar un pedido o confirmar un cierre). Elegir
qué mozo atiende una mesa tampoco es un login: es solo una etiqueta sobre
el pedido, cualquiera con el rol correspondiente puede cambiarla.

## Qué NO tiene (a propósito)

Este proyecto prioriza tener un flujo operativo completo con datos de
prueba, no una implementación de producción. Quedan fuera del alcance:

- Backend real / base de datos externa (todo vive en `localStorage` del
  navegador — los datos no se comparten entre dispositivos ni sobreviven a
  un borrado de datos del sitio).
- Login con usuarios reales, contraseñas o sesiones — el "rol" es solo una
  preferencia de UI, y el "modo admin" es un PIN simple.
- Asignación de tareas de cocina a un cocinero específico (requeriría
  login individual).
- Instalación en red local / despliegue en producción.
- Suite de pruebas automatizadas formal.

## Datos de ejemplo

`Config → Vaciar todos los datos` borra todo lo cargado en el navegador
actual. Es irreversible (no hay backend del que recuperarlo).
