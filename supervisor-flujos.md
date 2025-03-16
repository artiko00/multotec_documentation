# Flujos de Usuario: Supervisor

Este documento presenta los diagramas que ilustran los flujos y procesos del Supervisor en el sistema Multotec.

## Diagrama de Flujo: Monitoreo de Maquinarias

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

## Diagrama de Secuencia: Consulta de Informes

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

## Diagrama de Estado: Monitoreo de Estados de Maquinarias

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

## Diagrama de Viaje del Usuario: Supervisor

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