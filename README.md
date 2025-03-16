# Documentación de Multotec

Esta carpeta contiene la documentación técnica y de usuario para el sistema Multotec, incluyendo diagramas de entidad-relación, flujos de usuario y otros documentos relevantes.

## Estructura de la Documentación

- **entidad-relacion.md**: Diagrama y explicación del modelo de datos del sistema
- **superadministrador-flujos.md**: Diagramas de flujo para el rol de Superadministrador
- **administrador-flujos.md**: Diagramas de flujo para el rol de Administrador
- **analista-flujos.md**: Diagramas de flujo para el rol de Analista
- **supervisor-flujos.md**: Diagramas de flujo para el rol de Supervisor

## Tipos de Diagramas

Para cada rol de usuario, se han creado cuatro tipos de diagramas:

### 1. Diagramas de Flujo (Flowchart)

Muestran el proceso paso a paso que sigue cada usuario para completar una tarea específica, incluyendo puntos de decisión y caminos alternativos.

### 2. Diagramas de Secuencia (Sequence Diagram)

Ilustran la interacción entre el usuario y los diferentes componentes del sistema a lo largo del tiempo, mostrando el intercambio de mensajes y el orden de las operaciones.

### 3. Diagramas de Estado (State Diagram)

Muestran los diferentes estados por los que pasa una entidad (empresa, faena, maquinaria, registro) y las transiciones entre estos estados.

### 4. Diagramas de Viaje del Usuario (Journey)

Proporcionan una visión de alto nivel de la experiencia completa del usuario durante su interacción con el sistema, incluyendo su nivel de satisfacción en cada etapa.

## Cómo Utilizar esta Documentación

### Para Desarrolladores

- Utilice el diagrama entidad-relación como referencia para implementar el modelo de datos
- Consulte los diagramas de flujo y secuencia para entender la lógica de negocio
- Implemente las transiciones de estado según los diagramas de estado

### Para Diseñadores de UI/UX

- Utilice los diagramas de viaje del usuario para entender la experiencia completa
- Consulte los diagramas de flujo para diseñar interfaces que sigan el proceso lógico
- Asegúrese de que la interfaz soporte todos los estados mostrados en los diagramas de estado

### Para Testers

- Utilice los diagramas de flujo como base para crear casos de prueba
- Verifique que todas las transiciones de estado funcionen correctamente
- Pruebe las interacciones entre componentes según los diagramas de secuencia

## Visualización de los Diagramas

Todos los diagramas están creados con MermaidJS, que es compatible con GitHub y muchos editores Markdown. Para visualizarlos:

1. Abra los archivos .md en GitHub o en un editor que soporte MermaidJS
2. Alternativamente, puede copiar el código de los diagramas y pegarlo en el [Editor en línea de Mermaid](https://mermaid.live/)

## Mantenimiento de la Documentación

Al realizar cambios en el sistema:

1. Actualice primero el diagrama entidad-relación si hay cambios en el modelo de datos
2. Modifique los diagramas de flujo y secuencia para reflejar los nuevos procesos
3. Actualice los diagramas de estado si cambian las transiciones posibles
4. Revise y ajuste los diagramas de viaje del usuario si la experiencia cambia 