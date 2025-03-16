# Flujos de Usuario: Administrador

Este documento presenta los diagramas que ilustran los flujos y procesos del Administrador en el sistema Multotec.

## Diagrama de Flujo: Gestión de Usuarios de Empresa

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

## Diagrama de Secuencia: Creación de Faena

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

## Diagrama de Estado: Ciclo de Vida de una Faena

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

## Diagrama de Viaje del Usuario: Administrador

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