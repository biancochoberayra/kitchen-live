# kitchen-live — RestaurantFlow

Sistema de gestión para restaurante, pensado para funcionar **sin backend**: todo
corre en el navegador y los datos se guardan en `localStorage`. Sirve para
probar el flujo completo de un restaurante (mesas, pedidos, cocina, caja,
stock, cierres) con datos de prueba, sin instalar nada.

## Cómo usarlo

No requiere build ni servidor: abrí `kitchen-live.html` directamente en el
navegador (doble clic, o `python3 -m http.server` y entrar a esa URL si tu
navegador bloquea `file://`).

Para ver el sistema en uso rápido: **Config → Datos de prueba → Cargar datos
de prueba** (requiere modo admin, PIN por defecto `1234`). Carga mesas,
productos, una caja abierta y pedidos en distintos estados.

La primera vez que se abre `kitchen-live.html` aparece un **tutorial** paso a
paso que explica cada módulo del sistema (no vuelve a aparecer solo después).
Se puede reabrir en cualquier momento, junto con un **acceso rápido** con la
explicación de todas las funciones en una sola lista, desde el botón "Ayuda /
Tutorial" al pie de la barra lateral.

## Páginas del sistema

| Archivo | Qué es |
|---|---|
| `kitchen-live.html` | La aplicación principal. Todo el flujo operativo. |
| `dashboard.html` | Panel de KPIs (ventas, ocupación, productos top, etc.), de solo lectura. |
| `pantalla-cocina.html` | Pantalla grande para la cocina (estilo tablero de KDS), con 3 columnas: Pendiente / En preparación / Listo. Clic izquierdo para avanzar un ítem un paso; clic derecho para retroceder un paso, por si se tocó por accidente. |
| `a.html`, `cocina-ordu.html` | Prototipos previos, se mantienen como referencia histórica. |

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
  dejarle más espacio al mapa. Una vez que se pide la cuenta de una mesa,
  quitar un ítem de la comanda pasa a requerir modo administrador (antes de
  pedir la cuenta, el mozo puede seguir quitando ítems pendientes
  libremente).
- **Cocina** — comandas en vivo por estación, con el mozo asignado a cada
  mesa visible en cada ticket. El avance de estado (pendiente → preparando →
  listo) es solo para el rol Cocina (o el admin); el rol Mozo no puede
  tocarlo, solo ve "A cargo de cocina". Cuando un ítem queda "listo", recién
  ahí el mozo (o el admin) ve el botón "Retirado ✓" para marcarlo como
  entregado sin tener que ir hasta la mesa — es la única acción que el mozo
  puede hacer en esta sección.
- **Productos** — catálogo con categorías, precios, grupos de opciones,
  stock diario, e historial de cambios de precio.
- **Stock** — contador de stock por producto: botones +/− para ajustar a
  mano, además del descuento/restitución automático por ventas y
  anulaciones. Historial de movimientos por fecha y aviso de agotados.
- **Caja** — apertura con monto inicial, cobro de mesas (con pago dividido
  en varios medios de pago), movimientos manuales de ingreso/egreso, cierre
  con monto real contado y diferencia.
- **Cierre** — resumen del día (ventas, productos vendidos, personas,
  cubiertos, caja, cancelados), exportable a CSV (con secciones separadas
  por mesa, pagos, items vendidos y resumen de productos, cada dato en su
  propia celda). Se puede **confirmar** un cierre: a partir de ahí, esa
  fecha queda bloqueada (no se puede tocar el stock).
- **Config** — zonas del salón (nombre + plano por zona), nombres de
  **mozos** (para asignarlos a las mesas), cantidad de cocineros y
  estaciones, PIN de administrador, carga/borrado de datos de prueba.

Un selector de **rol** en la barra lateral (Mozo / Cocina / Caja / Encargado
de stock / Administrador) filtra qué pestañas ve cada uno, y además
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
