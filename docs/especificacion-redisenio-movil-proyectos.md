# Especificación de implementación

## Rediseño móvil de Proyectos — Lilian Trade Backoffice

Este documento es la fuente principal para implementar el rediseño móvil de la vista de proyectos. Debe utilizarse junto con las capturas de referencia y el repositorio de la aplicación.

## 1. Contexto

La vista actual funciona correctamente en tablet y escritorio, pero en móvil está mostrando una tabla de escritorio comprimida. Esto provoca:

- Texto partido letra por letra.
- Columnas que se desbordan horizontalmente.
- Acciones que ocupan demasiado espacio.
- Búsqueda y contenido superpuestos.
- Dificultad para acceder al detalle de un proyecto.

La solución debe ser móvil primero, inspirada en patrones de Asana y WhatsApp, pero manteniendo la identidad visual de Lilian Trade.

## 2. Objetivo

Transformar la vista móvil de proyectos en una experiencia basada en:

- Lista vertical de proyectos.
- Panel deslizante para consultar el proyecto.
- Vista de detalle del proyecto.
- Chat asociado al proyecto.
- Campo visible de “Última actualización”.
- Espacio preparado para un futuro resumen generado por IA.

El resumen de IA no debe implementarse todavía.

## 3. Repositorio y entorno

Repositorio objetivo:

```text
https://github.com/jamolinos/fe-liliantrade-backoffice
```

La implementación debe realizarse sobre una rama de trabajo, no directamente sobre producción ni sobre la rama principal sin revisión.

Antes de modificar código:

1. Identificar el framework y la estructura de rutas.
2. Localizar la vista actual de proyectos.
3. Localizar el cliente de Supabase.
4. Ejecutar la instalación existente del proyecto.
5. Ejecutar los tests y el build actuales para establecer una línea base.
6. Confirmar dónde se definen los estilos globales, iconos y componentes reutilizables.

No incluir tokens, claves secretas ni variables privadas en el repositorio.

## 4. Alcance funcional

### Incluido

- Rediseño responsive de la vista móvil de proyectos.
- Lista vertical sin agrupaciones visuales tipo tabla.
- Búsqueda por nombre de proyecto o cliente.
- Filtros horizontales por estado.
- Menú de acciones por proyecto.
- Panel de proyecto que entra desde la derecha.
- Pestaña “Proyecto”.
- Pestaña “Chat”.
- Envío de mensajes del chat.
- Listado de archivos recientes.
- Campo “Última actualización”.
- Navegación inferior móvil.
- Uso del logo Lilian de forma sutil y legible.

### Fuera de alcance

- Resumen automático de IA.
- Notificaciones push.
- Edición completa de proyectos desde el panel.
- Nuevo sistema de permisos.
- Migraciones de datos innecesarias.
- Rediseño de escritorio salvo que sea necesario para no romperlo.
- Cambio del modelo de estados del pipeline.

## 5. Diseño visual

### Identidad

- Utilizar el logo real de Lilian Trade.
- El logo debe aparecer en la zona superior, con tamaño discreto pero legible.
- Mantener el azul marino de la marca como color principal.
- Utilizar azul claro para selección, enlaces y estados destacados.
- Usar colores suaves para los estados del pipeline.
- Mantener fondos claros y suficiente contraste.

### Tipografía

- Mantener la tipografía actual de la aplicación si ya existe una definida.
- Títulos de navegación inferior: aproximadamente 13 px.
- Título principal de la vista: aproximadamente 28 px.
- Nombre de proyecto en la lista: aproximadamente 15–16 px.
- Texto secundario: mínimo 12 px.
- No reducir contenido esencial por debajo de 11 px.

### Referencias visuales

- Lista y panel: patrones de Asana móvil.
- Navegación inferior: jerarquía y legibilidad similares a WhatsApp.
- Colores y logo: identidad actual de Lilian Trade.

## 6. Vista principal: lista de proyectos

### Encabezado

Debe incluir:

- Logo Lilian Trade.
- Identificación breve de la aplicación o backoffice.
- Avatar o acceso al perfil.
- Título “Proyectos”.
- Contador de resultados.
- Texto secundario como “oportunidades en pipeline”.
- Campo de búsqueda.

### Filtros

Mostrar filtros horizontales desplazables:

- Todos.
- Lead.
- Qualified Lead / Calificado.
- In Production / Producción.
- Completed / Completado.

El filtro activo debe tener un tratamiento visual claramente distinguible y accesible.

### Lista

Cada proyecto debe aparecer como una fila vertical continua, no como una tabla comprimida ni como una cuadrícula.

Cada fila debe mostrar:

- Nombre del proyecto.
- Cliente.
- Estado.
- Descripción resumida.
- Última actualización.
- Botón de menú `···`.

Las filas deben separarse con divisores sutiles. No utilizar columnas fijas que generen scroll horizontal.

### Acciones

El menú `···` debe incluir como mínimo:

- Ver proyecto.
- Editar.
- Eliminar.

La acción “Ver proyecto” debe abrir el panel deslizante.

## 7. Campo “Última actualización”

Debe aparecer debajo de “Seguimiento” dentro del detalle del proyecto.

Etiqueta visible:

```text
Última actualización
```

Debe utilizar directamente la fecha de:

```text
projects.updated_at
```

No crear una nueva columna para este dato.

Formato recomendado para móvil:

```text
28 ago 2026, 14:32
```

La zona debe quedar preparada para agregar posteriormente un resumen de IA debajo de la fecha. Por ahora debe mostrar solamente la fecha, sin texto inventado ni placeholder engañoso.

## 8. Panel deslizante del proyecto

### Apertura

- Se abre al tocar una fila o seleccionar “Ver proyecto”.
- Debe entrar desde la derecha con una animación breve.
- Debe ocupar toda la superficie móvil disponible.
- No debe perder el filtro ni la búsqueda que estaban activos.
- Debe abrir inicialmente en la pestaña “Proyecto”.

### Cierre

Debe poder cerrarse mediante:

- Flecha de regreso.
- Deslizamiento hacia la derecha.
- Tecla Escape cuando corresponda en escritorio/responsive.

Al cerrar, la lista debe conservar su posición, búsqueda y filtro.

### Encabezado del panel

Debe mostrar:

- Flecha atrás.
- Menú de acciones.
- Etiqueta “Proyecto”.
- Nombre completo del proyecto.
- Estado visible.

### Pestañas

Usar dos pestañas:

```text
Proyecto | Chat
```

La pestaña activa debe tener:

- Color principal de marca.
- Indicador inferior.
- Estado accesible mediante `aria-selected` o equivalente.

## 9. Pestaña Proyecto

Mostrar, en este orden:

1. Resumen general.
2. Cliente.
3. Responsable.
4. Seguimiento.
5. Última actualización.
6. Descripción.
7. Archivos recientes.

### Resumen general

Debe mostrar el estado actual como badge y los datos principales del proyecto.

### Archivos recientes

Mostrar archivos asociados al proyecto con:

- Icono de tipo de archivo.
- Nombre.
- Tipo o extensión.
- Fecha de actualización.
- Indicador de navegación o acción futura.

## 10. Pestaña Chat

El chat debe estar vinculado al proyecto seleccionado, no a una conversación global.

Debe incluir:

- Separador de fecha.
- Mensajes entrantes.
- Mensajes propios.
- Nombre del remitente.
- Hora del mensaje.
- Indicador de lectura cuando el dato exista.
- Campo para escribir.
- Botón para adjuntar archivo.
- Botón para enviar.

### Envío de mensaje

Al enviar un mensaje:

1. Validar que el texto no esté vacío.
2. Insertar el mensaje en la conversación activa.
3. Asociarlo al chat del proyecto seleccionado.
4. Mantener el texto seguro y escapado al renderizarlo.
5. Limpiar el campo de entrada.
6. Actualizar el scroll del chat hacia el último mensaje.

No utilizar datos simulados cuando el backend real esté disponible.

## 11. Mapeo con Supabase

El proyecto Supabase ya contiene las estructuras necesarias.

### Proyectos

Tabla:

```text
public.projects
```

Campos principales:

| Interfaz | Campo |
|---|---|
| ID | `projects.id` |
| Nombre | `projects.name` |
| Descripción | `projects.description` |
| Creado por | `projects.created_by` |
| Cliente | `projects.customer_company_id` |
| Estado | `projects.status_id` |
| Creado | `projects.created_at` |
| Última actualización | `projects.updated_at` |

### Estados

Tabla:

```text
public.project_statuses
```

Relación:

```text
projects.status_id = project_statuses.id
```

Usar `project_statuses.name` para mostrar el estado.

### Clientes

Tabla:

```text
public.customer_companies
```

Relación:

```text
projects.customer_company_id = customer_companies.id
```

Usar preferentemente `legal_name` y, si corresponde, `commercial_name` como respaldo.

### Chats

Tabla de conversaciones:

```text
public.project_chats
```

Relación:

```text
project_chats.project_id = projects.id
```

Tabla de mensajes:

```text
public.project_chat_messages
```

Campos importantes:

| Interfaz | Campo |
|---|---|
| Texto original | `message_text_raw` |
| Texto chino | `message_text_cn` |
| Texto inglés | `message_text_en` |
| Idioma original | `original_language` |
| Remitente | `sender_id` |
| Fecha/hora | `sent_at` |
| Público | `is_public` |
| Archivo | `file_path`, `file_name`, `file_type`, `file_size` |
| Bot/agente | `sender_bot_name` |

Usar el texto apropiado según la configuración de idioma de la aplicación. Como fallback, usar `message_text_raw`.

### Perfiles

Tabla:

```text
public.profiles
```

Relación:

```text
project_chat_messages.sender_id = profiles.id
```

Usar el perfil para mostrar nombre y avatar cuando esté disponible.

### Archivos

Tabla:

```text
public.project_files
```

Relación:

```text
project_files.project_id = projects.id
```

No mostrar enlaces de archivos sin validar sus permisos y URL de acceso.

## 12. Realtime y estado de interfaz

Si la aplicación ya utiliza Supabase Realtime:

- Suscribirse a nuevos mensajes del chat activo.
- Filtrar por el `chat_id` del proyecto seleccionado.
- Limpiar la suscripción al cerrar el panel o cambiar de proyecto.
- Evitar suscripciones duplicadas.

Si Realtime todavía no está conectado, implementar primero la carga y el envío normales sin inventar una arquitectura paralela.

## 13. Seguridad

- Nunca incluir `service_role` en el frontend.
- Utilizar únicamente la clave pública/publishable configurada para el cliente.
- Respetar las políticas RLS existentes.
- No confiar en `user_metadata` para autorización.
- No mostrar mensajes o archivos de proyectos que el usuario no pueda consultar.
- Validar permisos antes de insertar mensajes.
- No registrar tokens ni claves en consola.

## 14. Responsive

La implementación debe verificarse como mínimo en:

- 320 px.
- 360 px.
- 390 px.
- Ancho tablet.
- Ancho escritorio.

Requisitos:

- No debe existir scroll horizontal accidental.
- El nombre del proyecto debe envolver correctamente.
- El menú de acciones no debe quedar cortado.
- El chat debe mantener visible el compositor de mensajes.
- El panel debe conservar una jerarquía clara en pantallas pequeñas.
- El diseño de escritorio existente no debe romperse.

## 15. Criterios de aceptación

### Lista

- [ ] La vista móvil ya no utiliza una tabla comprimida.
- [ ] Los proyectos aparecen en una lista vertical.
- [ ] La búsqueda funciona por proyecto y cliente.
- [ ] Los filtros actualizan la lista.
- [ ] El logo es visible, sutil y no desplaza el contenido.
- [ ] La navegación inferior tiene títulos legibles.
- [ ] Se muestra “Última actualización” en cada proyecto o en su detalle, según la composición elegida.

### Panel

- [ ] Tocar una fila abre el panel desde la derecha.
- [ ] El proyecto seleccionado es el correcto.
- [ ] El panel muestra estado, cliente, responsable, seguimiento y última actualización.
- [ ] El campo utiliza `projects.updated_at`.
- [ ] La descripción se carga desde el proyecto real.
- [ ] Los archivos corresponden al proyecto seleccionado.
- [ ] La flecha atrás cierra el panel.
- [ ] El gesto de deslizar a la derecha cierra el panel cuando la plataforma lo permite.
- [ ] El filtro y la búsqueda se conservan al cerrar.

### Chat

- [ ] El chat corresponde al proyecto seleccionado.
- [ ] Los mensajes se cargan desde `project_chat_messages`.
- [ ] Se distingue visualmente entre mensajes propios y externos.
- [ ] No se pueden enviar mensajes vacíos.
- [ ] Un mensaje enviado aparece sin recargar la página.
- [ ] El mensaje se guarda en Supabase.
- [ ] La conversación se desplaza al mensaje más reciente.
- [ ] Los archivos adjuntos respetan las políticas existentes.

### Calidad

- [ ] Los tests existentes pasan.
- [ ] Se agregan tests para apertura/cierre del panel.
- [ ] Se agrega un test para cambio entre Proyecto y Chat.
- [ ] Se agrega un test para envío de mensajes.
- [ ] Se verifica el responsive en 320, 360 y 390 px.
- [ ] El build de producción pasa.
- [ ] No aparecen errores nuevos en consola.

## 16. Verificación recomendada

El agente debe ejecutar los comandos que ya existan en el proyecto. Como mínimo:

```bash
npm install
npm test
npm run build
```

Si el proyecto utiliza otros gestores o scripts, respetar los comandos definidos en `package.json`.

La verificación debe incluir:

1. Lista inicial de proyectos.
2. Búsqueda.
3. Cambio de filtro.
4. Apertura del primer proyecto.
5. Revisión de “Última actualización”.
6. Cambio a Chat.
7. Envío de mensaje.
8. Cierre del panel.
9. Confirmación de que la lista conserva su estado.
10. Verificación en móvil, tablet y escritorio.

## 17. Preparación para futura IA

Debajo de “Última actualización” debe reservarse una sección semántica y visualmente preparada para incorporar más adelante:

```text
Resumen del proyecto
```

Por ahora:

- No llamar a ningún modelo.
- No generar texto automático.
- No guardar resúmenes.
- No mostrar contenido falso.
- Puede dejarse fuera de la interfaz hasta que se defina el flujo de IA.

Cuando se implemente, el resumen deberá asociarse al proyecto correcto, mostrar fecha de generación y manejar estados de carga, error y ausencia de resumen.

## 18. Decisiones de implementación

- Reutilizar componentes y estilos existentes cuando sea posible.
- No duplicar el cliente de Supabase.
- No crear una segunda fuente de verdad para proyectos o chats.
- No crear migraciones para `updated_at`, porque ya existe.
- Mantener las acciones destructivas protegidas por confirmación.
- Mantener la accesibilidad de botones, pestañas, formularios y navegación.
- Preferir cambios pequeños y revisables.

## 19. Entrega esperada

La entrega debe incluir:

- Cambios de código en una rama de trabajo.
- Tests nuevos o actualizados.
- Evidencia del build.
- Evidencia de la verificación responsive.
- Resumen de archivos modificados.
- Nota explícita indicando que el resumen de IA quedó fuera de alcance.
- Pull request o diff revisable antes de mezclar a la rama principal.

