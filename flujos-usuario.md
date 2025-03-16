# Flujos de Usuario en Multotec

Este documento presenta los diagramas de flujo para los diferentes roles de usuario en el sistema Multotec, utilizando MermaidJS para visualizar los procesos e interacciones.

## Índice

1. [Superadministrador](#superadministrador)
2. [Administrador](#administrador)
3. [Analista](#analista)
4. [Supervisor](#supervisor)

## Superadministrador

### Diagrama de Flujo: Creación de Empresa

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

### Diagrama de Secuencia: Asignación de Administrador a Empresa

```mermaid
sequenceDiagram
    actor SA as Superadministrador
    participant S as Sistema
    participant E as Empresa
    participant A as Administrador
    participant N as Notificaciones
    
    SA->>S: Iniciar sesión
    S->>SA: Confirmar acceso
    SA->>S: Acceder a gestión de empresas
    S->>SA: Mostrar lista de empresas
    SA->>E: Seleccionar empresa
    E->>SA: Mostrar detalles de empresa
    SA->>E: Seleccionar "Asignar Administrador"
    E->>SA: Mostrar formulario de asignación
    SA->>S: Buscar usuario existente
    S->>SA: Mostrar resultados
    SA->>A: Seleccionar usuario
    SA->>E: Confirmar asignación
    E->>S: Actualizar relación
    S->>N: Generar notificación
    N->>A: Enviar email de asignación
    S->>SA: Confirmar asignación exitosa
```

### Diagrama de Estado: Ciclo de Vida de una Empresa

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

### Diagrama de Caso de Uso: Viaje del Superadministrador

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

## Administrador

### Diagrama de Flujo: Gestión de Usuarios de Empresa

```mermaid
flowchart TD
    A[Inicio] --> B[Login como Administrador]
    B --> C[Acceder a Módulo de Usuarios]
    C --> D[Seleccionar 'Nuevo Usuario']
    D --> E[Completar Formulario de Usuario]
    E --> F[Seleccionar Rol: Analista/Supervisor]
    F --> G{Datos Válidos?}
    G -->|No| H[Mostrar Errores]
    H --> E
    G -->|Sí| I[Guardar Usuario]
    I --> J[Asignar a Faena]
    J --> K[Enviar Invitación]
    K --> L[Notificar Creación Exitosa]
    L --> M[Fin]
```

### Diagrama de Secuencia: Creación de Faena

```mermaid
sequenceDiagram
    actor A as Administrador
    participant S as Sistema
    participant E as Empresa
    participant F as Faena
    participant U as Usuarios
    
    A->>S: Iniciar sesión
    S->>A: Confirmar acceso
    A->>S: Acceder a gestión de faenas
    S->>A: Mostrar lista de faenas
    A->>S: Seleccionar "Nueva Faena"
    S->>A: Mostrar formulario
    A->>F: Completar datos de faena
    A->>F: Seleccionar ubicación
    A->>F: Establecer fechas
    A->>F: Guardar faena
    F->>S: Crear registro
    A->>F: Asignar usuarios
    F->>U: Actualizar asignaciones
    S->>A: Confirmar creación exitosa
```

### Diagrama de Estado: Ciclo de Vida de una Faena

```mermaid
stateDiagram-v2
    [*] --> Planificada: Creación inicial
    Planificada --> EnPreparacion: Asignación de recursos
    EnPreparacion --> Activa: Inicio de operaciones
    Activa --> EnPausa: Suspender temporalmente
    EnPausa --> Activa: Reanudar
    Activa --> Completada: Finalizar operaciones
    Completada --> Cerrada: Cierre administrativo
    Cerrada --> [*]
```

### Diagrama de Caso de Uso: Viaje del Administrador

```mermaid
journey
    title Viaje del Administrador
    section Inicio de jornada
      Login al sistema: 5: Admin
      Revisar notificaciones: 4: Admin
      Verificar estado de faenas: 5: Admin
    section Gestión de faenas
      Revisar faenas activas: 4: Admin
      Crear nueva faena: 3: Admin
      Asignar recursos: 4: Admin
    section Gestión de usuarios
      Revisar actividad de analistas: 3: Admin
      Asignar tareas a supervisores: 4: Admin
      Gestionar permisos: 3: Admin
    section Reportes
      Generar informes de faenas: 4: Admin
      Revisar métricas de maquinarias: 3: Admin
      Presentar resultados: 5: Admin
```

## Analista

### Diagrama de Flujo: Registro de Datos de Maquinaria

```mermaid
flowchart TD
    A[Inicio] --> B[Login como Analista]
    B --> C[Acceder a Módulo de Maquinarias]
    C --> D[Seleccionar Faena]
    D --> E[Seleccionar Maquinaria]
    E --> F[Iniciar Registro de Datos]
    F --> G[Completar Formulario Técnico]
    G --> H[Adjuntar Evidencias/Fotos]
    H --> I{Datos Completos?}
    I -->|No| J[Guardar Borrador]
    J --> G
    I -->|Sí| K[Enviar Registro]
    K --> L[Sistema Valida Datos]
    L --> M{Validación Exitosa?}
    M -->|No| N[Mostrar Errores]
    N --> G
    M -->|Sí| O[Confirmar Registro]
    O --> P[Fin]
```

### Diagrama de Secuencia: Actualización de Datos Técnicos

```mermaid
sequenceDiagram
    actor AN as Analista
    participant S as Sistema
    participant F as Faena
    participant M as Maquinaria
    participant R as Registro
    
    AN->>S: Iniciar sesión
    S->>AN: Confirmar acceso
    AN->>S: Acceder a maquinarias
    S->>AN: Mostrar lista de faenas
    AN->>F: Seleccionar faena
    F->>AN: Mostrar maquinarias
    AN->>M: Seleccionar maquinaria
    M->>AN: Mostrar detalles y registros
    AN->>M: Iniciar nuevo registro
    M->>AN: Mostrar formulario
    AN->>R: Ingresar datos técnicos
    AN->>R: Adjuntar evidencias
    AN->>R: Enviar registro
    R->>S: Validar datos
    S->>R: Confirmar registro
    S->>AN: Notificar éxito
```

### Diagrama de Estado: Estados de un Registro de Datos

```mermaid
stateDiagram-v2
    [*] --> Borrador: Inicio de registro
    Borrador --> EnEdicion: Modificar datos
    EnEdicion --> Borrador: Guardar sin enviar
    Borrador --> Enviado: Enviar para validación
    Enviado --> Rechazado: Datos incorrectos
    Rechazado --> EnEdicion: Corregir
    Enviado --> Validado: Aprobación
    Validado --> Publicado: Disponible para consulta
    Publicado --> [*]
```

### Diagrama de Caso de Uso: Viaje del Analista

```mermaid
journey
    title Viaje del Analista
    section Inicio de jornada
      Login al sistema: 5: Analista
      Revisar asignaciones: 4: Analista
      Planificar visitas a maquinarias: 5: Analista
    section Trabajo en campo
      Inspeccionar maquinarias: 4: Analista
      Recolectar datos técnicos: 5: Analista
      Documentar estado: 4: Analista
    section Registro en sistema
      Ingresar datos recolectados: 3: Analista
      Adjuntar evidencias: 4: Analista
      Enviar registros: 5: Analista
    section Seguimiento
      Verificar validación de registros: 3: Analista
      Corregir observaciones: 4: Analista
      Completar informes: 4: Analista
```

## Supervisor

### Diagrama de Flujo: Monitoreo de Maquinarias

```mermaid
flowchart TD
    A[Inicio] --> B[Login como Supervisor]
    B --> C[Acceder a Dashboard]
    C --> D[Seleccionar Faena]
    D --> E[Revisar Estado General]
    E --> F[Filtrar por Estado de Maquinaria]
    F --> G[Seleccionar Maquinaria]
    G --> H[Revisar Detalles y Registros]
    H --> I{Requiere Atención?}
    I -->|No| J[Marcar como Revisado]
    I -->|Sí| K[Generar Alerta]
    K --> L[Asignar Prioridad]
    L --> M[Notificar a Responsables]
    J --> N[Continuar con Siguiente Maquinaria]
    M --> N
    N --> O{Todas Revisadas?}
    O -->|No| G
    O -->|Sí| P[Generar Informe de Supervisión]
    P --> Q[Fin]
```

### Diagrama de Secuencia: Consulta de Informes

```mermaid
sequenceDiagram
    actor SU as Supervisor
    participant S as Sistema
    participant F as Faenas
    participant M as Maquinarias
    participant R as Reportes
    
    SU->>S: Iniciar sesión
    S->>SU: Confirmar acceso
    SU->>S: Acceder a reportes
    S->>SU: Mostrar opciones de reportes
    SU->>R: Seleccionar tipo de reporte
    SU->>F: Filtrar por faena
    F->>SU: Mostrar faenas disponibles
    SU->>M: Filtrar por tipo de maquinaria
    SU->>R: Establecer rango de fechas
    SU->>R: Generar reporte
    R->>S: Procesar solicitud
    S->>R: Compilar datos
    R->>SU: Mostrar reporte
    SU->>R: Exportar a PDF
    R->>SU: Descargar archivo
```

### Diagrama de Estado: Monitoreo de Estados de Maquinarias

```mermaid
stateDiagram-v2
    [*] --> EnMonitoreo: Inicio de supervisión
    EnMonitoreo --> Revisada: Verificación rutinaria
    EnMonitoreo --> RequiereAtencion: Detección de anomalía
    RequiereAtencion --> EnIntervencion: Asignar técnico
    EnIntervencion --> EnPruebas: Reparación completada
    EnPruebas --> Operativa: Pruebas exitosas
    EnPruebas --> RequiereAtencion: Fallas persistentes
    Operativa --> EnMonitoreo: Continuar supervisión
    Revisada --> EnMonitoreo: Continuar supervisión
    Operativa --> [*]
```

### Diagrama de Caso de Uso: Viaje del Supervisor

```mermaid
journey
    title Viaje del Supervisor
    section Inicio de jornada
      Login al sistema: 5: Supervisor
      Revisar alertas: 4: Supervisor
      Analizar dashboard: 5: Supervisor
    section Supervisión
      Revisar estado de faenas: 4: Supervisor
      Monitorear maquinarias críticas: 5: Supervisor
      Verificar registros recientes: 3: Supervisor
    section Gestión
      Priorizar intervenciones: 4: Supervisor
      Coordinar con analistas: 3: Supervisor
      Actualizar estados: 4: Supervisor
    section Reportes
      Generar informes de estado: 5: Supervisor
      Analizar tendencias: 4: Supervisor
      Comunicar resultados: 5: Supervisor
``` 