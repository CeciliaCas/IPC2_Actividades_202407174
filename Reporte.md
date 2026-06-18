#   Arquitectura Multi-Nivel (N-Tier) y Patrón MVC en .NET

## Parte 1: Fundamentación Teórica y Análisis Crítico

### 1. El Tránsito hacia los Sistemas Distribuidos y Multi-Capa

#### La Limitación del Monolito Local

Cuando la interfaz de usuario, la lógica de negocio y la base de datos se encuentran en una sola computadora, el sistema depende completamente de los recursos de esa máquina. Esto dificulta la escalabilidad, limita el acceso simultáneo de múltiples usuarios y complica la sincronización de datos cuando se necesitan varias estaciones de trabajo conectadas.

#### Distinción Crítica (Layers vs. Tiers)

**Capas Lógicas (Layers):**
Son divisiones internas del software que organizan las responsabilidades de la aplicación.

Ejemplos:

- Capa de Presentación
- Capa de Negocio
- Capa de Datos

**Niveles Físicos (Tiers):**
Son los equipos o servidores donde se ejecutan las capas del sistema.

Ejemplos:

- Servidor Web
- Servidor de Aplicaciones
- Servidor de Base de Datos

Las capas representan una separación lógica mientras que los niveles representan una separación física.

#### Responsabilidades en la Arquitectura de 3 Niveles

##### Nivel 1: Capa de Presentación (Presentation Tier)

Su función es interactuar directamente con el usuario, mostrar información y recibir solicitudes.

Tecnologías comunes:

- HTML
- CSS
- JavaScript
- Razor Pages
- ASP.NET MVC

##### Nivel 2: Capa de Aplicación o Negocio (Application Tier)

Se encarga de procesar la lógica del sistema, validar datos y aplicar las reglas del negocio.

Tecnologías comunes:

- ASP.NET Core
- C#
- APIs REST
- Servicios Web

##### Nivel 3: Capa de Datos (Data Tier)

Administra el almacenamiento, recuperación y actualización de la información.

Tecnologías comunes:

- SQL Server
- MySQL
- PostgreSQL
- Oracle

#### Seguridad Perimetral

Exponer directamente el puerto de una base de datos a Internet representa un riesgo importante de seguridad, ya que podría permitir accesos no autorizados, robo de información o ataques al sistema. La buena práctica consiste en mantener la base de datos en una red privada y permitir el acceso únicamente desde el servidor de aplicaciones mediante mecanismos de autenticación y control de acceso.

---

### 2. Desacoplamiento Lógico con el Patrón MVC

#### La Crisis del Código Espagueti

Cuando las consultas SQL, la lógica de negocio y la interfaz gráfica se encuentran mezcladas en un mismo archivo, el mantenimiento se vuelve complejo, aumentan los errores y resulta difícil realizar pruebas unitarias. Además, cualquier modificación puede afectar múltiples funcionalidades del sistema.

#### Separación de Preocupaciones (SoC)

##### Modelo (Model)

Representa los datos y las reglas de negocio del sistema. Su responsabilidad es administrar la información y no debe conocer la forma en que los datos son mostrados al usuario.

##### Vista (View)

Es la encargada de presentar la información al usuario. Puede contener elementos visuales y de presentación, pero no debe incluir consultas SQL ni lógica de negocio.

##### Controlador (Controller)

Actúa como intermediario entre la Vista y el Modelo. Recibe solicitudes, coordina el procesamiento de la información y selecciona la respuesta adecuada para el usuario.

#### Métricas de Ingeniería de Software

El patrón MVC favorece la Alta Cohesión porque cada componente tiene responsabilidades bien definidas. También promueve el Bajo Acoplamiento porque los componentes trabajan de forma independiente, facilitando el mantenimiento, las pruebas y la escalabilidad del software.

---

## Parte 2: Modelado del Ciclo de Vida y Enrutamiento Semántico

### 1. Mapeo Analítico de URLs

| URL Entrante | Controlador | Acción | ID |
|-------------|------------|---------|-----|
| https://ingenieria.usac.edu.gt/ControlAcademico/Login | ControlAcademicoController | Login | Ninguno |
| https://ingenieria.usac.edu.gt/Estudiante/Historial/20260123 | EstudianteController | Historial | 20260123 |
| https://ingenieria.usac.edu.gt/Asignacion/Detalle/10 | AsignacionController | Detalle | 10 |
| https://ingenieria.usac.edu.gt/Home | HomeController | Index | Ninguno |

### 2. Diagramación del Flujo Interactivo

**Paso 1:** El usuario hace clic en un enlace o botón dentro del navegador.

**Paso 2:** El navegador envía una solicitud HTTP al servidor web.

**Paso 3:** El sistema de enrutamiento analiza la URL y determina qué controlador y acción deben ejecutarse.

**Paso 4:** El controlador procesa la solicitud, consulta o modifica datos mediante el modelo y prepara la respuesta.

**Paso 5:** La vista genera el contenido HTML dinámicamente y el servidor envía la respuesta al navegador para ser mostrada al usuario.

---

## Parte 4: Auditoría y Control de Calidad

### Prueba de Cohesión (GET)

La respuesta es limpia porque el controlador únicamente recibe la solicitud y entrega la información a la vista. No contiene consultas SQL ni lógica de negocio compleja dentro de sus métodos.

La prueba GET fue realizada accediendo a la URL
http://localhost:5285/Estudiante/Listar,
obteniéndose correctamente el listado de estudiantes en formato JSON.

### Evaluación de Antipatrones

Los métodos implementados cumplen con la recomendación de controladores delgados (Skinny Controllers), ya que cada acción realiza una única responsabilidad y no supera las 20 líneas de código.



---

## Parte 5: Referencias Bibliográficas

Facultad de Ingeniería, USAC. (2026). *Sesión 11: Modelado Base y Arquitecturas de Despliegue. Evolución de Sistemas Distribuidos, Fundamentos del Modelo Cliente-Servidor y Diseño Físico Multi-Capas (N-Tier).* Laboratorio del curso Introducción a la Programación y Computación 2. Guatemala.

Facultad de Ingeniería, USAC. (2026). *Sesión 12: Arquitectura y Componentes del Patrón MVC. Desacoplamiento Lógico de Software, Ciclo de Vida de las Peticiones y Enrutamiento en Aplicaciones Interactivas Modernas.* Laboratorio del curso Introducción a la Programación y Computación 2. Guatemala.