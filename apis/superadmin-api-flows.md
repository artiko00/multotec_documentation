# Flujos de API para el Superadministrador

Este documento describe los principales flujos de API para las operaciones que puede realizar un superadministrador en el sistema Multotec. Se incluyen diagramas que muestran el flujo de peticiones y respuestas entre el cliente y el servidor.

## Índice

- [Flujos de API para el Superadministrador](#flujos-de-api-para-el-superadministrador)
  - [Índice](#índice)
  - [Autenticación](#autenticación)
  - [Gestión de Empresas](#gestión-de-empresas)
    - [Crear Empresa](#crear-empresa)
    - [Listar Empresas](#listar-empresas)
    - [Obtener Detalles de Empresa](#obtener-detalles-de-empresa)
    - [Actualizar Empresa](#actualizar-empresa)
    - [Desactivar Empresa](#desactivar-empresa)
  - [Gestión de Administradores](#gestión-de-administradores)
    - [Crear Administrador](#crear-administrador)
    - [Asignar Administrador a Empresa](#asignar-administrador-a-empresa)
    - [Revocar Acceso de Administrador](#revocar-acceso-de-administrador)
    - [Listar Administradores de una Empresa](#listar-administradores-de-una-empresa)
  - [Notas Importantes](#notas-importantes)

## Autenticación

El superadministrador debe autenticarse antes de poder realizar cualquier operación en el sistema.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant DB as Base de Datos

    Cliente->>API: POST /api/auth/login
    Note right of Cliente: {email, password}
    API->>Auth: Validar credenciales
    Auth->>DB: Verificar usuario y rol
    DB-->>Auth: Devolver información usuario
    Auth-->>API: Usuario verificado con rol SUPERADMIN
    API-->>Cliente: 200 OK + Token JWT
    Note left of API: {access_token, token_type}
```

## Gestión de Empresas

### Crear Empresa

Proceso para crear una nueva empresa en el sistema.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: POST /api/empresas
    Note right of Cliente: {nombre, nit, direccion, telefono, email}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Crear empresa
    Service->>Service: Validar datos
    Service->>DB: Verificar que no exista NIT
    DB-->>Service: No existe
    Service->>DB: Guardar empresa
    DB-->>Service: Empresa creada
    Service-->>API: Devolver empresa
    API-->>Cliente: 201 Created + Datos empresa
    Note left of API: {id, nombre, nit, ...}
```

### Listar Empresas

Obtener un listado de empresas con paginación y filtros opcionales.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: GET /api/empresas?skip=0&limit=10&nombre=Acme&estado=ACTIVA
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Obtener empresas
    Service->>DB: Consultar con filtros
    DB-->>Service: Devolver resultados
    Service-->>API: Lista de empresas + total
    API-->>Cliente: 200 OK
    Note left of API: {empresas: [...], total: 42}
```

### Obtener Detalles de Empresa

Obtener información detallada de una empresa específica.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: GET /api/empresas/{empresa_id}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Obtener empresa por ID
    Service->>DB: Consultar empresa
    DB-->>Service: Devolver empresa
    Service-->>API: Datos de empresa
    API-->>Cliente: 200 OK
    Note left of API: {id, nombre, nit, ...}
```

### Actualizar Empresa

Modificar la información de una empresa existente.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: PUT /api/empresas/{empresa_id}
    Note right of Cliente: {nombre, direccion, ...}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Actualizar empresa
    Service->>DB: Verificar existencia
    DB-->>Service: Empresa existe
    Service->>Service: Validar datos
    Service->>DB: Actualizar empresa
    DB-->>Service: Empresa actualizada
    Service-->>API: Devolver datos actualizados
    API-->>Cliente: 200 OK
    Note left of API: {id, nombre, ...}
```

### Desactivar Empresa

Marcar una empresa como inactiva en el sistema.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: DELETE /api/empresas/{empresa_id}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Desactivar empresa
    Service->>DB: Verificar existencia
    DB-->>Service: Empresa existe
    Service->>DB: Actualizar estado a INACTIVA
    DB-->>Service: Empresa desactivada
    Service-->>API: Confirmación
    API-->>Cliente: 200 OK
    Note left of API: {message: "Empresa eliminada correctamente", success: true}
```

## Gestión de Administradores

### Crear Administrador

Registrar un nuevo administrador en el sistema.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as UsuarioService
    participant DB as Base de Datos

    Cliente->>API: POST /api/usuarios
    Note right of Cliente: {nombre, apellido, email, password, rol: "ADMIN"}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Crear administrador
    Service->>Service: Validar datos
    Service->>DB: Verificar que email no exista
    DB-->>Service: Email disponible
    Service->>Service: Hashear contraseña
    Service->>DB: Guardar usuario
    DB-->>Service: Usuario creado
    Service-->>API: Devolver administrador
    API-->>Cliente: 201 Created
    Note left of API: {id, nombre, email, ...}
```

### Asignar Administrador a Empresa

Asignar un administrador existente a una empresa específica.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: POST /api/empresas/{empresa_id}/administradores
    Note right of Cliente: {administrador_id: 123}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Asignar administrador
    Service->>DB: Verificar existencia empresa y admin
    DB-->>Service: Ambos existen
    Service->>DB: Verificar que no exista relación activa
    DB-->>Service: No existe relación
    Service->>DB: Crear relación empresa-admin
    DB-->>Service: Relación creada
    Service-->>API: Devolver relación
    API-->>Cliente: 201 Created
    Note left of API: {id, empresa_id, administrador_id, ...}
```

### Revocar Acceso de Administrador

Eliminar el acceso de un administrador a una empresa.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: DELETE /api/empresas/{empresa_id}/administradores/{admin_id}
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Revocar acceso
    Service->>DB: Verificar existencia de relación
    DB-->>Service: Relación existe
    Service->>DB: Marcar relación como inactiva
    DB-->>Service: Relación actualizada
    Service-->>API: Confirmación
    API-->>Cliente: 200 OK
    Note left of API: {message: "Administrador desasignado", success: true}
```

### Listar Administradores de una Empresa

Obtener el listado de administradores asignados a una empresa específica.

```mermaid
sequenceDiagram
    participant Cliente
    participant API as API Multotec
    participant Auth as Servicio Auth
    participant Service as EmpresaService
    participant DB as Base de Datos

    Cliente->>API: GET /api/empresas/{empresa_id}/administradores
    API->>Auth: Validar token + permisos SUPERADMIN
    Auth-->>API: Token válido
    API->>Service: Obtener administradores
    Service->>DB: Consultar relaciones activas
    DB-->>Service: Devolver administradores
    Service-->>API: Lista de administradores
    API-->>Cliente: 200 OK
    Note left of API: {administradores: [...], total: 3}
```

## Notas Importantes

- Todos los endpoints requieren autenticación mediante token JWT.
- Sólo usuarios con rol SUPERADMIN pueden acceder a estos endpoints.
- Las respuestas de error incluyen códigos HTTP apropiados (400, 401, 403, 404, 500) y mensajes descriptivos.
- La paginación está disponible en endpoints de listado mediante parámetros `skip` y `limit`.
- Los filtros pueden aplicarse como parámetros de consulta en los endpoints de listado. 