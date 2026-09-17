# kitchen-live
Hola, Este documento contiene prompts ordenados por fases para construir RestaurantFlow de manera progresiva. Se recomienda usar un prompt por vez, revisar el resultado y recién después continuar con el siguiente.
Reglas generales para usar todos los prompts
•	No generar todo el sistema de una sola vez: trabajar por módulos y fases.
•	No cambiar nombres de archivos, variables, rutas o tablas existentes sin explicar primero el motivo.
•	Entregar el código separado por archivo y aclarar exactamente dónde colocarlo.
•	Comentar las partes importantes del HTML, CSS, JavaScript, Python y SQL.
•	Verificar siempre la conexión entre frontend, backend, API, WebSocket y base de datos.
•	No inventar funcionalidades que contradigan las decisiones del proyecto.
•	Mantener el sistema preparado para funcionar en una PC o servidor dentro de la red local.
•	Aplicar validaciones en frontend y backend.
•	No eliminar información histórica de pedidos, pagos, cierres o cambios de precio.
•	Antes de modificar código existente, pedir los archivos actuales o indicar qué información falta.
Contexto fijo del proyecto
Nombre: RestaurantFlow
Tipo: sistema de gestión para restaurante
Tecnologías: HTML, CSS, JavaScript, Python, FastAPI y PostgreSQL
Despliegue: una PC o servidor dentro del restaurante, accesible por red local
Pedidos: el mozo toma pedidos desde una mesa
Mesas: cantidad configurable desde administración
Mapa: se puede cargar una imagen del plano del restaurante y colocar mesas encima
Cocina: cantidad de cocineros y estaciones configurable
Comandas: digitales, sin impresión
Pagos: carga manual dentro del sistema
Productos: visibles para todos, pero crear/modificar/precios solo para administrador
Stock: productos terminados, controlado por día
Cierre: productos vendidos individualmente, cantidades, personas y tenedores
Opciones de productos: por ejemplo, sorrentinos con salsa, extras y observaciones
Tiempo real: WebSockets para cocina, mesas, pedidos y estados
Índice de fases
•	Fase 0 — Definición y reglas del proyecto
•	Fase 1 — Estructura inicial y entorno
•	Fase 2 — Diseño definitivo de la base de datos
•	Fase 3 — Conexión FastAPI + PostgreSQL
•	Fase 4 — Usuarios, login y permisos
•	Fase 5 — Productos, categorías, precios y opciones
•	Fase 6 — Mesas y mapa visual del restaurante
•	Fase 7 — Pedidos y comandas
•	Fase 8 — Cocina en tiempo real
•	Fase 9 — Caja y pagos manuales
•	Fase 10 — Stock diario
•	Fase 11 — Gastos y compras
•	Fase 12 — Cierres y reportes
•	Fase 13 — Dashboard
•	Fase 14 — Seguridad, validaciones y errores
•	Fase 15 — Pruebas completas
•	Fase 16 — Instalación en red local
•	Fase 17 — Documentación y mantenimiento
Fase 0 — Analizar y fijar el alcance
Actuá como analista funcional y arquitecto de software. Necesito desarrollar un sistema llamado RestaurantFlow para un restaurante profesional.

Características confirmadas:
- El mozo toma pedidos en mesa.
- Las mesas se crean y configuran desde administración.
- Se puede cargar una imagen como mapa del restaurante y colocar las mesas sobre ella.
- La cantidad de cocineros y estaciones no es fija.
- Las comandas serán digitales y no se imprimirán.
- Los pagos se cargarán manualmente.
- Los productos se pueden consultar desde el sistema.
- Solo el administrador puede crear productos, modificar nombres, precios, categorías, imágenes y opciones.
- El stock será de productos terminados y se controlará por día.
- El cierre debe mostrar cada producto vendido individualmente, su cantidad, los detalles de los productos y la cantidad de personas y tenedores.
- Se utilizarán HTML, CSS, JavaScript, Python, FastAPI y PostgreSQL.
- El sistema funcionará en una PC o servidor dentro de la red local.

Entregá:
1. Alcance funcional.
2. Funciones incluidas en la primera versión.
3. Funciones que deben quedar para una segunda versión.
4. Roles y permisos.
5. Reglas de negocio.
6. Riesgos y decisiones importantes.
7. Una lista de requisitos funcionales y no funcionales.
No escribas código todavía.
Fase 1 — Crear la estructura del proyecto
Actuá como desarrollador senior de Python y frontend. Creá la estructura inicial completa de un proyecto llamado RestaurantFlow con FastAPI, PostgreSQL, HTML, CSS y JavaScript.

Necesito:
- Estructura de carpetas.
- Entorno virtual.
- requirements.txt.
- Archivo .env de ejemplo.
- main.py inicial.
- Carpeta app organizada en config, database, models, schemas, routers, services, dependencies y websocket.
- Carpeta frontend con pages, css, js, assets y uploads.
- Carpeta database con migrations y seed.sql.
- Carpeta tests.
- README inicial.

Entregá los comandos para Windows PowerShell y Git Bash.
Cada archivo debe estar separado, comentado y explicado.
No mezcles todo en un único archivo.
No avances todavía con todos los módulos.
Fase 2 — Diseñar la base de datos definitiva
Diseñá la base de datos completa de RestaurantFlow para PostgreSQL.

Debe incluir como mínimo:
- roles
- usuarios
- estaciones
- usuario_estaciones
- mapas_restaurante
- mesas
- categorías de productos
- productos
- historial_precios
- grupos_opciones
- opciones_producto
- producto_grupos_opciones
- pedidos
- detalle_pedidos
- detalle_pedido_opciones
- tareas_cocina
- historial_estados
- cajas
- aperturas y cierres de caja
- pagos
- movimientos_caja
- stock_diario
- cierres_stock_diarios
- movimientos_stock
- proveedores
- compras
- detalle_compras
- gastos

Considerá:
- claves primarias y foráneas
- índices
- restricciones
- estados válidos
- campos created_at y updated_at
- borrado lógico cuando corresponda
- historial de precios
- conservar el precio histórico de cada pedido
- cantidad de personas y tenedores
- fecha operativa del restaurante
- evitar doble cobro y doble descuento de stock

Entregá:
1. Diagrama entidad-relación en Mermaid.
2. SQL completo de creación.
3. Explicación de cada tabla.
4. Datos iniciales de roles y estados.
5. Recomendaciones para migraciones con Alembic.
Fase 3 — Conectar FastAPI con PostgreSQL
Implementá la conexión de RestaurantFlow entre FastAPI y PostgreSQL usando SQLAlchemy y Alembic.

Necesito:
- config.py
- database.py
- variables de entorno
- conexión segura
- creación de sesión
- dependencia get_db
- configuración de SQLAlchemy
- estructura base de modelos
- configuración de Alembic
- primera migración
- endpoint GET /health
- endpoint de prueba de base de datos

Explicá:
- dónde colocar cada archivo
- cómo instalar dependencias
- cómo ejecutar FastAPI
- cómo ejecutar Alembic
- cómo verificar que la conexión funciona
- errores frecuentes y cómo resolverlos

No generes todavía todos los modelos si eso hace que la respuesta sea demasiado extensa. Comenzá con la base técnica.
Fase 4 — Login, usuarios y permisos
Desarrollá el módulo de autenticación y autorización de RestaurantFlow con FastAPI.

Roles:
- Administrador
- Encargado de cocina
- Cocinero
- Mozo
- Cajero
- Encargado de stock

Necesito:
- tabla y modelo de usuarios
- contraseñas hasheadas
- login
- token JWT o mecanismo apropiado
- usuario actual
- permisos por rol
- alta, modificación, activación y desactivación de usuarios
- protección de rutas
- frontend de login
- manejo de sesión
- cierre de sesión

Reglas:
- Solo administrador modifica productos y precios.
- Solo usuarios autorizados acceden a caja.
- Solo cocina accede a tareas de cocina.
- Solo stock modifica stock.
- No permitir que un usuario desactivado ingrese.

Entregá código separado por archivo, comentado y con ejemplos de prueba.
Fase 5 — Productos, categorías, precios y opciones
Desarrollá el módulo de productos de RestaurantFlow.

Funcionalidades:
- listar productos
- crear productos
- editar productos
- activar/desactivar productos
- categorías
- imágenes
- precio actual
- historial de precios
- descripción
- observaciones
- grupos de opciones
- opciones como salsas, extras y cambios
- precio adicional de cada opción
- opciones obligatorias y opcionales

Ejemplo:
Producto: Sorrentinos
Opciones:
- Salsa fileto
- Salsa crema
- Salsa mixta
- Queso extra
- Sin cebolla
- Observación libre

Reglas:
- Todos pueden consultar productos.
- Solo administrador puede modificar productos y precios.
- Los pedidos anteriores deben conservar nombre, precio y opciones históricas.
- Los productos inactivos no aparecen para nuevos pedidos.

Entregá:
- modelos
- schemas
- routers
- servicios
- endpoints
- HTML
- CSS
- JavaScript
- validaciones
- pruebas.
Fase 6 — Mesas y mapa del restaurante
Desarrollá el módulo de mesas y mapa visual de RestaurantFlow.

Necesito:
- crear, editar y desactivar mesas
- número o nombre de mesa
- capacidad
- estado: libre, ocupada, reservada, limpieza, deshabilitada
- cargar una imagen del plano del restaurante
- mostrar la imagen como fondo
- colocar mesas sobre el mapa
- mover mesas
- cambiar tamaño
- guardar posición X, Y, ancho y alto
- seleccionar una mesa para abrir un pedido
- actualizar colores según el estado
- funcionar también sin imagen cargada

La cantidad de mesas debe ser ilimitada y configurable.
La cantidad de mapas también debe poder administrarse, pero solo uno debe estar activo por defecto.

Entregá:
- diseño de base de datos
- endpoints para imágenes y mesas
- pantalla de administración
- pantalla de uso para mozos
- HTML, CSS y JavaScript
- validaciones de archivos
- explicación de cómo guardar la ruta de la imagen.
Fase 7 — Pedidos y comandas digitales
Desarrollá el módulo de pedidos de RestaurantFlow.

El mozo debe poder:
- seleccionar una mesa
- registrar cantidad de personas
- registrar cantidad de tenedores
- agregar productos
- elegir cantidades
- elegir opciones
- agregar observaciones
- modificar el pedido antes de enviarlo
- enviar el pedido a cocina
- consultar el estado
- agregar productos adicionales
- cancelar con permiso y motivo

El pedido debe guardar:
- mesa
- mozo
- fecha operativa
- productos
- cantidades
- precio histórico
- opciones históricas
- observaciones
- personas
- tenedores
- estado
- hora de creación y actualización

Estados sugeridos:
recibido, confirmado, en_preparacion, en_control, listo, entregado, cerrado, pausado y cancelado.

No imprimir comandas.
Entregá backend, frontend, validaciones, transacciones y ejemplos de JSON.
Fase 8 — Cocina en tiempo real con WebSockets
Desarrollá la pantalla de cocina de RestaurantFlow usando WebSockets con FastAPI.

Características:
- mostrar nuevos pedidos automáticamente
- actualizar estados sin recargar
- ver mesa, hora, mozo, productos, cantidades y especificaciones
- filtrar por estación
- permitir que todos los cocineros preparen todos los productos
- permitir asignar una tarea a un cocinero
- mostrar pedidos pendientes, en preparación, en control y listos
- registrar quién inició y terminó una tarea
- reconectar si se corta la conexión
- evitar que dos cocineros tomen la misma tarea por error

Eventos:
- pedido.creado
- pedido.actualizado
- tarea.iniciada
- tarea.finalizada
- pedido.listo
- pedido.cancelado
- mesa.actualizada

Entregá:
- WebSocket backend
- servicio de emisión de eventos
- pantalla de cocina
- HTML, CSS y JavaScript
- manejo de reconexión
- control de errores
- explicación paso a paso.
Fase 9 — Caja y pagos manuales
Desarrollá el módulo de caja de RestaurantFlow.

Necesito:
- crear caja
- abrir caja
- monto inicial
- registrar pagos manuales
- métodos: efectivo, transferencia, tarjeta, billetera digital y otros
- registrar pagos parciales si se decide permitirlos
- impedir doble cobro
- asociar pago con pedido
- registrar movimientos de caja
- registrar gastos
- mostrar total esperado
- cargar monto real al cierre
- calcular diferencia
- permitir observaciones
- consultar historial de cierres

Solo los usuarios autorizados pueden abrir, cobrar y cerrar caja.

Entregá:
- tablas
- modelos
- schemas
- endpoints
- pantalla de caja
- formulario de pago
- cierre de caja
- validaciones
- manejo de errores
- ejemplos de uso.
Fase 10 — Stock diario de productos terminados
Desarrollá el módulo de stock diario de RestaurantFlow.

El stock será de productos terminados, no de ingredientes.

Necesito:
- abrir jornada de stock
- cargar stock inicial
- registrar entradas
- registrar ajustes
- descontar productos vendidos
- registrar anulaciones
- calcular stock final
- mostrar productos agotados
- cerrar stock del día
- historial por fecha
- registrar usuario y motivo de cada ajuste

El descuento de stock debe ocurrir una sola vez, preferentemente cuando el pedido pasa a en_preparacion, o mediante una regla claramente definida.

Ejemplo:
Fritas con cheddar:
stock inicial 50
vendidas 20
anuladas 1
ajustes -2
stock final 27

Entregá:
- diseño SQL
- backend
- frontend
- lógica transaccional
- prevención de doble descuento
- reportes diarios
- pruebas.
Fase 11 — Gastos, proveedores y compras
Desarrollá el módulo de gastos y compras de RestaurantFlow.

Necesito:
- registrar gasto
- categoría de gasto
- descripción
- monto
- método de pago
- proveedor opcional
- usuario que registra
- fecha operativa
- comprobante opcional
- editar o anular con historial
- registrar compras
- asociar compras con proveedores
- distinguir gasto de compra de stock
- mostrar total de gastos diarios

Ejemplos:
- limpieza
- mantenimiento
- servicios
- compras de bebidas
- descartables
- reparaciones
- otros

Integrá los gastos con caja y los reportes diarios.
Entregá backend, frontend, SQL, validaciones y permisos.
Fase 12 — Cierres diarios y reportes
Desarrollá el módulo de cierre diario de RestaurantFlow.

Al cerrar el día o caja se debe mostrar:
- total recaudado
- total por método de pago
- cantidad de pedidos
- productos vendidos individualmente
- cantidad de cada producto
- opciones y especificaciones cuando se consulte 'Ver más'
- cantidad de personas
- cantidad de tenedores
- gastos
- stock inicial y final
- anulaciones
- diferencias de caja
- usuario que realizó el cierre
- fecha y hora

Ejemplo:
20 Fritas con cheddar
2 Sorrentinos
8 Hamburguesas completas

Para cada producto se debe poder abrir un detalle.
Para sorrentinos debe mostrar salsa, extras y observaciones.

El cierre confirmado no debe poder modificarse libremente.
Entregá consultas SQL, endpoints, pantallas, filtros por fecha y exportación a CSV o PDF si corresponde.
Fase 13 — Dashboard general
Diseñá e implementá el dashboard principal de RestaurantFlow.

Debe mostrar:
- total recaudado
- boletos o pedidos vendidos
- producto más vendido
- cantidad de pedidos pendientes
- ocupación de mesas
- cantidad de personas
- cantidad de tenedores
- gastos del día
- stock bajo
- ventas por método de pago
- recaudación por día o semana
- productos más vendidos
- estado de cocina

Usá tarjetas KPI y gráficos claros.
No llenar la pantalla de gráficos circulares.
Preferir barras, líneas y tablas cuando sean más útiles.

El dashboard debe respetar permisos por rol.
Entregá HTML, CSS, JavaScript, endpoints y consultas SQL.
Fase 14 — Seguridad, validaciones y errores
Realizá una auditoría de seguridad y robustez para RestaurantFlow.

Revisá:
- validación de datos en frontend y backend
- contraseñas
- permisos
- SQL injection
- subida de imágenes
- extensiones y tamaño de archivos
- doble pago
- doble descuento de stock
- cambios de precio
- pedidos cancelados
- cierres de caja
- errores de WebSocket
- pérdida de conexión
- datos duplicados
- transacciones
- logs
- mensajes de error
- protección de rutas
- variables de entorno
- backups

Entregá:
1. Problemas encontrados.
2. Soluciones.
3. Código corregido.
4. Checklist de seguridad.
5. Pruebas para comprobar cada solución.
Fase 15 — Pruebas completas
Creá un plan de pruebas completo para RestaurantFlow.

Incluí:
- pruebas unitarias
- pruebas de API
- pruebas de base de datos
- pruebas de permisos
- pruebas de frontend
- pruebas de WebSockets
- pruebas de caja
- pruebas de stock
- pruebas de cierre diario
- pruebas de mapa y mesas
- pruebas de productos y opciones
- pruebas de errores de red
- pruebas de múltiples usuarios

Prepará casos con:
- ID
- objetivo
- pasos
- datos de entrada
- resultado esperado
- resultado obtenido
- estado

Incluí casos como:
- dos mozos cargando pedidos al mismo tiempo
- dos cocineros intentando tomar la misma tarea
- pago duplicado
- pedido cancelado
- producto sin stock
- cambio de precio después de una venta
- cierre de caja con diferencia
- corte de conexión.
Fase 16 — Instalar en una red local
Explicá cómo instalar y ejecutar RestaurantFlow en una PC o servidor dentro del restaurante.

Necesito:
- instalar Python
- instalar PostgreSQL
- crear base de datos
- configurar .env
- ejecutar migraciones
- crear usuario administrador
- ejecutar FastAPI en la IP local
- acceder desde otras computadoras o tablets
- configurar firewall
- reservar IP local
- iniciar el sistema automáticamente
- realizar backups
- restaurar backups
- verificar que todos los dispositivos vean los cambios en tiempo real

Dame comandos para Windows.
Explicá cómo comprobar la IP del servidor y cómo acceder desde otro dispositivo.
Fase 17 — Documentación final y mantenimiento
Generá la documentación técnica y de usuario de RestaurantFlow.

Debe incluir:
- descripción general
- instalación
- configuración
- roles
- uso del mozo
- uso de cocina
- uso de caja
- uso de stock
- administración de productos
- configuración de mesas y mapa
- cierre diario
- solución de problemas
- backups
- actualización del sistema
- estructura de carpetas
- endpoints
- eventos WebSocket
- modelo de base de datos
- convenciones de código
- futuras mejoras

La documentación debe ser clara para estudiantes y personal del restaurante.
Separá manual técnico y manual de usuario.
Prompt maestro para revisar cualquier módulo
Estoy desarrollando RestaurantFlow, un sistema de gestión para restaurante con HTML, CSS, JavaScript, Python, FastAPI y PostgreSQL.

Antes de responder:
1. Revisá el objetivo del módulo.
2. Respetá la arquitectura existente.
3. No cambies nombres de archivos, rutas, tablas o variables sin avisar.
4. Verificá la conexión entre frontend, backend y base de datos.
5. Aplicá permisos por rol.
6. Validá datos en frontend y backend.
7. Usá transacciones cuando haya dinero, stock o estados.
8. Conservá los datos históricos.
9. Entregá el código separado por archivo.
10. Comentá el código importante.
11. Indicá dónde colocar cada archivo.
12. Explicá cómo probarlo.
13. Indicá errores frecuentes.
14. Si falta un archivo o información, pedímelo antes de inventarlo.

Módulo a trabajar:
[ESCRIBIR AQUÍ EL MÓDULO]

Archivos actuales:
[PEGAR AQUÍ LOS ARCHIVOS]

Objetivo específico:
[EXPLICAR AQUÍ EL CAMBIO]
Prompt para pedir una revisión antes de continuar
Revisá el estado actual de RestaurantFlow antes de pasar a la siguiente fase.

Analizá:
- qué partes ya están implementadas
- qué partes faltan
- errores de conexión
- endpoints faltantes
- tablas faltantes
- problemas de permisos
- problemas de diseño
- problemas de validación
- problemas de tiempo real
- problemas de caja o stock
- archivos que deberían reorganizarse

No generes código todavía.
Entregá un checklist ordenado:
1. Correcto
2. Incompleto
3. Incorrecto
4. Próximo paso recomendado
Orden recomendado de uso
•	Usar primero las fases 0, 1, 2 y 3.
•	No avanzar a pedidos hasta tener usuarios, productos y mesas funcionando.
•	No avanzar a caja hasta que los pedidos tengan estados correctos.
•	No avanzar a cierres hasta que pagos, gastos y stock estén registrados correctamente.
•	Después de cada fase ejecutar pruebas.
•	Guardar una copia de seguridad antes de cambiar la base de datos.
•	Trabajar en una rama Git por módulo o fase.
•	Probar primero en una PC local y luego desde otro dispositivo de la red.
Checklist final del sistema
•	Login funcionando.
•	Roles y permisos funcionando.
•	Productos administrables.
•	Historial de precios funcionando.
•	Opciones y especificaciones funcionando.
•	Mesas configurables.
•	Mapa del restaurante cargable.
•	Pedidos desde mesa.
•	Comandas digitales.
•	Cocina en tiempo real.
•	Cocineros configurables.
•	Pagos manuales.
•	Caja y cierres.
•	Stock diario.
•	Gastos y compras.
•	Cantidad de productos vendidos.
•	Cantidad de personas.
•	Cantidad de tenedores.
•	Reportes.
•	Backups.
•	Pruebas.
•	Instalación en red local.
•	Documentación.
