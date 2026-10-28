# kitchen-live — RestaurantFlow

Sistema de gestión para restaurantes pensado para funcionar dentro del local,
en una PC o servidor accesible por la red interna. Cubre todo el circuito de
trabajo del salón: el mozo toma el pedido en la mesa, la comanda llega a cocina
en forma digital, cocina marca los estados de preparación y caja registra los
pagos y el cierre del día.

Este repositorio contiene el **material de diseño y los prototipos de testeo**
del sistema. Todavía no es la aplicación final (FastAPI + PostgreSQL): es la
etapa donde se prueban las pantallas, los flujos y las reglas de negocio antes
de escribir el backend definitivo.

## Qué resuelve el proyecto

- **Mesas y mapa del salón**: cantidad de mesas configurable y posibilidad de
  cargar una imagen del plano del restaurante para ubicar las mesas encima.
- **Pedidos**: el mozo abre la mesa, agrega productos con sus opciones
  (salsas, extras, observaciones) y envía la comanda.
- **Cocina**: tablero con los pedidos separados por estado
  (pendientes, en preparación, listos para servir), con estaciones y cantidad
  de cocineros configurables.
- **Productos**: catálogo con categorías, precios y opciones. Visible para
  todos, pero editable sólo por el administrador.
- **Caja**: carga manual de pagos y cobro de mesas.
- **Cierre del día**: productos vendidos, cantidades, detalle por mesa,
  personas y tenedores.
- **Configuración**: plano, mesas, cocina, seguridad (PIN de administrador)
  y carga de datos de prueba.

## Contenido del repositorio

| Archivo | Descripción |
| --- | --- |
| `a.html` | Prototipo funcional y autocontenido de RestaurantFlow. Incluye las vistas Mapa/Mozo, Cocina, Productos, Caja, Cierre y Configuración. Guarda los datos en `localStorage` del navegador, así que se puede probar el circuito completo sin backend. |
| `cocina-ordu.html` | Maqueta visual alternativa (tema oscuro) del panel de cocina y del dashboard: mapa del salón, pedidos recientes, historial, movimientos de caja, menú y configuración. Sirve como referencia de diseño. |
| `prompts.txt` | Guía de construcción del sistema dividida en fases (Fase 0 a Fase 17), con las reglas generales del proyecto, el alcance fijo y el checklist final. Es el documento que ordena en qué orden se desarrolla cada módulo. |

## Cómo probarlo

No necesita instalación ni servidor. Se puede abrir directamente el archivo en
el navegador:

```bash
xdg-open a.html
```

O, si se prefiere servirlo por HTTP:

```bash
python3 -m http.server 8000
```

Luego entrar a `http://localhost:8000/a.html` (o `cocina-ordu.html`).

En `a.html`, la sección **Configuración → Datos de prueba** permite cargar
mesas, productos y pedidos de ejemplo para recorrer el flujo completo. El PIN
de administrador por defecto es `1234`.

> Los datos del prototipo viven en el `localStorage` del navegador: se borran
> al limpiar los datos del sitio y no se comparten entre dispositivos.

## Stack previsto para la versión final

- **Frontend**: HTML, CSS y JavaScript
- **Backend**: Python con FastAPI
- **Base de datos**: PostgreSQL
- **Tiempo real**: WebSockets para cocina, mesas, pedidos y estados
- **Despliegue**: una PC o servidor dentro del restaurante, accesible por red local

## Estado actual

Etapa de prototipado y testeo de interfaz. Los próximos pasos siguen el orden
de fases descripto en `prompts.txt`: base de datos, conexión FastAPI +
PostgreSQL, usuarios y permisos, y de ahí en adelante cada módulo del sistema.
