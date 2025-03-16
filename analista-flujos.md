# Flujos de Usuario: Analista

Este documento presenta los diagramas que ilustran los flujos y procesos del Analista en el sistema Multotec.

## Diagrama de Flujo: Registro de Datos de Maquinaria

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

## Diagrama de Secuencia: Actualización de Datos Técnicos

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

## Diagrama de Estado: Estados de un Registro de Datos

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

## Diagrama de Viaje del Usuario: Analista

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