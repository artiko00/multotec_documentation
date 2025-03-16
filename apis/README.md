# Documentación de APIs de Multotec

Esta carpeta contiene documentación detallada sobre los flujos de API y los endpoints disponibles en el sistema Multotec.

## Contenido

- [Flujos de API para el Superadministrador](./superadmin-api-flows.md) - Diagramas y explicaciones de los flujos para operaciones del superadministrador, como gestión de empresas y administradores.

## Diagramas con MermaidJS

Los diagramas en esta documentación están creados utilizando [MermaidJS](https://mermaid-js.github.io/), que proporciona una sintaxis sencilla para generar diagramas a partir de texto. Los diagramas se renderizan automáticamente en entornos que soportan Markdown avanzado, como GitHub.

### Tipos de Diagramas

En esta documentación se utilizan principalmente:

- **Diagramas de Secuencia**: Para mostrar el flujo de interacción entre cliente, API y otros componentes del sistema.
- **Diagramas de Flujo**: Para representar procesos de decisión y flujos de trabajo.

### Cómo Leer los Diagramas

- Las flechas indican la dirección del flujo de información
- Los participantes se muestran en la parte superior
- Las notas proporcionan detalles adicionales sobre el contenido de las peticiones o respuestas
- Las líneas discontinuas generalmente representan respuestas

## Endpoints API

La API de Multotec sigue los principios REST y está estructurada en los siguientes grupos principales:

- `/api/auth` - Endpoints de autenticación y autorización
- `/api/empresas` - Gestión de empresas
- `/api/usuarios` - Gestión de usuarios/administradores

Para acceder a los endpoints protegidos, es necesario incluir el token JWT en el encabezado de autorización:

```
Authorization: Bearer {token}
```

## Convenciones

- **Métodos HTTP**: Se utilizan de acuerdo a su propósito estándar:
  - `GET` para obtener recursos
  - `POST` para crear recursos
  - `PUT` para actualizar recursos completos
  - `PATCH` para actualizaciones parciales
  - `DELETE` para eliminar/desactivar recursos

- **Códigos de Estado**: La API utiliza códigos HTTP estándar para indicar el resultado de las operaciones:
  - `200` - Éxito
  - `201` - Recurso creado
  - `400` - Error en la petición
  - `401` - No autorizado
  - `403` - Prohibido (sin permisos)
  - `404` - Recurso no encontrado
  - `500` - Error del servidor

## Actualización de la Documentación

Al añadir nuevos endpoints o modificar los existentes, asegúrese de actualizar esta documentación para mantenerla sincronizada con la implementación actual. 