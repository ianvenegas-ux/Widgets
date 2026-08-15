# Lilian Trade — Widget móvil de Proyectos

Este paquete contiene el prototipo visual de dos pantallas:

- Lista móvil de proyectos.
- Panel deslizante con detalle del proyecto y chat.

## Qué incluye

- `index.html`: versión autónoma lista para GitHub Pages.
- `especificacion-redisenio-movil-proyectos.md`: instrucciones completas para que otro agente implemente el diseño en el frontend real.

## Publicar en GitHub Pages

1. Copiar `index.html` a la raíz del repositorio objetivo.
2. Confirmar los cambios en una rama de trabajo.
3. Abrir `Settings → Pages`.
4. En `Build and deployment`, seleccionar `Deploy from a branch`.
5. Seleccionar la rama y la carpeta `/ (root)`.
6. Guardar y esperar la ejecución del despliegue.

La URL tendrá este formato:

```text
https://TU_USUARIO.github.io/NOMBRE_DEL_REPOSITORIO/
```

## Importante

Este es un prototipo independiente. Usa datos de ejemplo para demostrar la interacción y no contiene claves de Supabase.

La integración real debe conectarse con las tablas existentes:

- `public.projects`
- `public.project_statuses`
- `public.customer_companies`
- `public.project_chats`
- `public.project_chat_messages`
- `public.project_files`

El campo “Última actualización” debe usar `projects.updated_at`. No hace falta crear una migración para ese campo.

El resumen de IA queda reservado para una etapa posterior y no debe inventarse todavía.

