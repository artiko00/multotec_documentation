# Flujos de Usuario: Superadministrador

Este documento presenta los diagramas que ilustran los flujos y procesos del Superadministrador en el sistema Multotec, alineados con la implementación de arquitectura hexagonal.

## Diagrama de Flujo: Creación de Empresa

```mermaid
flowchart TD
    A[Inicio] --> B[Login como Superadministrador]
    B --> C[Acceder a Módulo de Empresas]
    C --> D[Seleccionar 'Nueva Empresa']
    D --> E[Completar Formulario de Empresa]
    E --> F{Datos Válidos?}
    F -->|No| G[Mostrar Errores]
    G --> E
    F -->|Sí| H[Guardar Empresa]
    H --> I[Asignar Administrador]
    I --> J{Administrador Existente?}
    J -->|No| K[Crear Nuevo Administrador]
    K --> L[Enviar Invitación]
    J -->|Sí| L
    L --> M[Notificar Creación Exitosa]
    M --> N[Fin]
```

## Diagrama de Secuencia: Asignación de Administrador a Empresa

```mermaid
sequenceDiagram
    actor SA as Superadministrador
    participant UI as UI Layer
    participant AP as Application Layer
    participant DO as Domain Layer
    participant IN as Infrastructure Layer
    
    SA->>UI: Iniciar sesión
    UI->>AP: Invocar LoginUseCase
    AP->>IN: AuthRepository.authenticate()
    IN->>AP: Token de sesión
    AP->>UI: Sesión autenticada
    UI->>SA: Confirmar acceso
    
    SA->>UI: Acceder a gestión de empresas
    UI->>AP: Invocar ListarEmpresasUseCase
    AP->>IN: EmpresaRepository.findAll()
    IN->>AP: Lista de empresas
    AP->>UI: Empresas formateadas
    UI->>SA: Mostrar lista de empresas
    
    SA->>UI: Seleccionar empresa
    UI->>AP: Invocar ObtenerEmpresaUseCase
    AP->>IN: EmpresaRepository.findById()
    IN->>AP: Entidad Empresa
    AP->>UI: Detalles de empresa
    UI->>SA: Mostrar detalles de empresa
    
    SA->>UI: Seleccionar "Asignar Administrador"
    UI->>AP: Invocar ListarAdministradoresUseCase
    AP->>IN: UsuarioRepository.findByRole()
    IN->>AP: Lista de administradores
    AP->>UI: Administradores disponibles
    UI->>SA: Mostrar formulario de asignación
    
    SA->>UI: Seleccionar usuario
    SA->>UI: Confirmar asignación
    UI->>AP: Invocar AsignarAdministradorUseCase
    AP->>DO: Validación de reglas de negocio
    DO->>AP: Validación exitosa
    AP->>IN: EmpresaAdministradorRepository.save()
    IN->>AP: Confirmación de asignación
    AP->>IN: NotificationAdapter.send()
    IN->>AP: Notificación enviada
    AP->>UI: Resultado de operación
    UI->>SA: Confirmar asignación exitosa
```

## Diagrama de Estado: Ciclo de Vida de una Empresa

```mermaid
stateDiagram-v2
    [*] --> Creada: Superadmin crea empresa
    Creada --> Configurando: Asignación inicial de administrador
    Configurando --> Activa: Configuración completada
    Activa --> EnPausa: Suspender temporalmente
    EnPausa --> Activa: Reactivar
    Activa --> Inactiva: Desactivar
    Inactiva --> Activa: Reactivar
    Inactiva --> Eliminada: Eliminar permanentemente
    Eliminada --> [*]
```

## Diagrama de Viaje del Usuario: Superadministrador

```mermaid
journey
    title Viaje del Superadministrador
    section Inicio de jornada
      Login al sistema: 5: Superadmin
      Revisar notificaciones: 3: Superadmin
      Verificar empresas activas: 4: Superadmin
    section Gestión de empresas
      Crear nueva empresa: 5: Superadmin
      Asignar administrador: 4: Superadmin
      Configurar parámetros iniciales: 3: Superadmin
    section Gestión de usuarios
      Revisar solicitudes de acceso: 3: Superadmin
      Crear usuarios clave: 4: Superadmin
      Asignar permisos especiales: 5: Superadmin
    section Monitoreo
      Revisar estadísticas globales: 4: Superadmin
      Auditar actividades: 3: Superadmin
      Generar reportes: 4: Superadmin
```

## Implementación en Arquitectura Hexagonal

La implementación de los flujos del Superadministrador sigue estrictamente los principios de la arquitectura hexagonal, separando claramente las diferentes capas de la aplicación.

### 1. Capa de Dominio

**Entidades principales:**
- `Empresa`: Entidad central para la gestión de empresas
- `Usuario`: Entidad que representa a los administradores y otros tipos de usuarios
- `EmpresaAdministrador`: Entidad que representa la relación entre una empresa y sus administradores

**Interfaces de Repositorio (Puertos):**
- `EmpresaRepositoryInterface`: Define operaciones para persistir y recuperar empresas
- `UsuarioRepositoryInterface`: Define operaciones para gestionar usuarios
- `EmpresaAdministradorRepositoryInterface`: Define operaciones para gestionar las relaciones

**Servicios de Dominio:**
- `EmpresaValidacionService`: Implementa reglas de negocio para validación de empresas
- `AdministradorValidacionService`: Implementa reglas de negocio para validación de administradores

### 2. Capa de Aplicación

**Casos de Uso:**
- `CrearEmpresaUseCase`: Orquesta la creación de una nueva empresa
- `ListarEmpresasUseCase`: Recupera el listado de empresas según criterios
- `AsignarAdministradorUseCase`: Gestiona la asignación de un administrador a una empresa
- `CambiarAdministradorPrincipalUseCase`: Cambia el administrador principal de una empresa
- `DesactivarEmpresaUseCase`: Desactiva una empresa en el sistema

**DTOs (Objetos de Transferencia de Datos):**
- `EmpresaDTO`: Representa los datos de una empresa para transferencia entre capas
- `AdministradorDTO`: Representa los datos de un administrador para transferencia
- `EmpresaAdministradorDTO`: Representa la relación para transferencia entre capas

### 3. Capa de Infraestructura

**Controladores API:**
- `EmpresaController`: Maneja las peticiones HTTP relacionadas con empresas
- `AdministradorController`: Maneja las peticiones HTTP relacionadas con administradores
- `EmpresaAdministradorController`: Maneja las peticiones para gestionar relaciones

**Adaptadores de Repositorio:**
- `SQLAlchemyEmpresaRepository`: Implementación concreta del repositorio de empresas
- `SQLAlchemyUsuarioRepository`: Implementación concreta del repositorio de usuarios
- `SQLAlchemyEmpresaAdministradorRepository`: Implementación para las relaciones

**Seguridad y Autorización:**
- `RoleMiddleware`: Middleware para validar roles de usuario en las peticiones
- `RoleRequired`: Decorador para proteger endpoints según el rol requerido

### Flujo de Datos en Arquitectura Hexagonal

1. **Petición de Usuario**: El Superadministrador interactúa con la UI
2. **Controlador API**: Recibe la petición y la transforma en una llamada al caso de uso
3. **Caso de Uso**: Coordina la lógica de aplicación utilizando entidades de dominio
4. **Entidades de Dominio**: Aplican reglas de negocio
5. **Repositorios**: Los casos de uso interactúan con las interfaces de repositorio
6. **Adaptadores de Repositorio**: Implementan la persistencia real de datos
7. **Respuesta**: El resultado fluye de vuelta siguiendo el mismo camino inverso

### Beneficios para el Flujo del Superadministrador

- **Separación de Responsabilidades**: Cada capa tiene un propósito claro
- **Testabilidad Mejorada**: Las capas de dominio y aplicación son fácilmente testeables
- **Independencia de Infraestructura**: El dominio no depende de detalles de implementación
- **Flexibilidad para Cambios**: Es posible cambiar la tecnología de persistencia o UI sin afectar la lógica de negocio
- **Mantenibilidad**: Estructura clara que facilita el mantenimiento futuro 