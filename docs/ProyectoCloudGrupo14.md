# PROYECTO “URBAN PULSE” DESARROLLO SOFTWARE EN LA NUBE

## Grupo 14 \- Andrea Pérez Rodríguez, Alfonso Ramos Rojas, María Muñoz Martín, Daniel Robles Cantos

## **INTRODUCCIÓN**

## **STACK TECNOLÓGICO UTILIZADO**

Utilizaremos como framework Expo con React Native 

1. ## C4

   ## 1.1 Diagrama de Contexto

   ## 1.2 Container diagram

   ## 1.3 Component diagram

   ## 1.4 Container diagram

2. ## ADR

### ADR-001: Usar postgres como base de datos relacional

- *Estado:*  
  Accepted  
- *Contexto:*  
  El sistema requiere una base de datos capaz de gestionar las incidencias y su flujo de vida. Para ello es necesario que soporte los datos de APIS externas, como las coordenadas o los dispositivos inteligentes de la ciudad. Además, debemos tener en cuenta su posterior acoplamiento a un Amazon Web Service  
- *Opciones consideradas:*

	1\. PostgreSQL  
	2\. MySQL  
3\. Supabase

- *Decisión:*  
  Utilizar PostgreSQL mediante Docker Compose en desarrollo local  
- *Consecuencias:*  
+ Cálculo sencillo de distancias y proximidad mediante PostGIS.  
+ Compatibilidad directa con servicios cloud gestionados en AWS.  
- Pérdida de flexibilidad a la hora de almacenar datos.  
- Mayor consumo de recursos ante otras opciones más ligeras.

### ADR-002: Usar Spring Boot como tecnología para backend

- *Estado:*  
  Accepted  
- *Contexto:*  
  El backend debe exponer una API para gestionar usuarios, incidencias, categorías y datos obtenidos de servicios externos. Se necesita una tecnología robusta, mantenible y con buena integración con PostgreSQL y futuros servicios de AWS.  
- *Opciones consideradas:*

	1\. Spring Boot  
	2\. Node.js con Express  
	3\. Django REST Framework  

- *Decisión:*  
  Utilizar Spring Boot para desarrollar una API REST que centralice la lógica de negocio de Urban Pulse.  
- *Consecuencias:*  
+ Ecosistema maduro para seguridad, persistencia de datos y validación de peticiones.  
+ Integración sencilla con PostgreSQL, PostGIS y servicios desplegados en AWS.  
- Mayor consumo de memoria y tiempo de arranque que alternativas ligeras.  
- Requiere conocimientos de Java y del ecosistema Spring.

### ADR-003: Usar como framework React Native como principal tecnología frontend con Expo 

- *Estado:*  
  Accepted  
- *Contexto:*  
  La aplicación debe estar disponible en dispositivos móviles y permitir la consulta y creación de incidencias desde la calle. Se busca reducir el esfuerzo de desarrollo manteniendo una experiencia nativa en Android e iOS.  
- *Opciones consideradas:*

	1\. React Native con Expo  
	2\. Desarrollo nativo separado para Android e iOS  
	3\. Flutter  

- *Decisión:*  
  Utilizar React Native con Expo como tecnología principal para la aplicación móvil.  
- *Consecuencias:*  
+ Una única base de código para Android e iOS.  
+ Expo simplifica la configuración, pruebas en dispositivo y acceso a funcionalidades nativas comunes.  
- Algunas integraciones nativas avanzadas pueden requerir configuración adicional o abandonar el flujo gestionado de Expo.  
- El rendimiento puede ser inferior al de una aplicación desarrollada de forma totalmente nativa.

### ADR-004: Usar OpenStreetMap como API de datos sobre Mapas

- *Estado:*  
  Accepted  
- *Contexto:*  
  Urban Pulse necesita datos cartográficos para situar incidencias, mostrar zonas de la ciudad y facilitar su consulta. La solución debe evitar costes iniciales elevados y permitir combinar la información con datos propios.  
- *Opciones consideradas:*

	1\. OpenStreetMap  
	2\. Google Maps Platform  
	3\. Mapbox  

- *Decisión:*  
  Utilizar OpenStreetMap como fuente de datos cartográficos y de mapas base.  
- *Consecuencias:*  
+ Datos abiertos, sin dependencia de licencias comerciales para el mapa base.  
+ Posibilidad de utilizar distintos proveedores de teselas y servicios compatibles.  
- La disponibilidad y los límites de uso dependen del proveedor de teselas elegido.  
- Algunas funcionalidades comerciales, como rutas avanzadas, requieren servicios adicionales.

### ADR-005: Usar React-Native-Maps como renderizador de mapas en la aplicación

- *Estado:*  
  Accepted  
- *Contexto:*  
  La aplicación móvil debe representar mapas, marcadores de incidencias y capas relacionadas con cada categoría. Es necesario un componente compatible con React Native y Expo que permita interacción táctil.  
- *Opciones consideradas:*

	1\. React Native Maps  
	2\. MapLibre React Native  
	3\. WebView con una biblioteca web de mapas  

- *Decisión:*  
  Utilizar React Native Maps para renderizar los mapas y los marcadores de incidencias en la aplicación.  
- *Consecuencias:*  
+ Integración directa con los componentes y eventos de React Native.  
+ Soporte para marcadores, regiones y gestos habituales en una aplicación móvil.  
- La configuración de los proveedores de mapas puede variar entre Android e iOS.  
- Las capas geoespaciales complejas pueden requerir procesamiento adicional antes de mostrarse.

### ADR-006: Arquitectura monolítica para el desarrollo inicial

- *Estado:*  
  Accepted  
- *Contexto:*  
  El alcance inicial de Urban Pulse es reducido y el equipo necesita entregar una primera versión funcional con rapidez. Separar el sistema en microservicios añadiría despliegues, comunicación entre servicios y costes operativos innecesarios.  
- *Opciones consideradas:*

	1\. Monolito modular  
	2\. Microservicios  
	3\. Backend serverless  

- *Decisión:*  
  Implementar un monolito modular en Spring Boot durante el desarrollo inicial.  
- *Consecuencias:*  
+ Desarrollo, pruebas y despliegue más sencillos para un equipo pequeño.  
+ Las transacciones y la lógica de negocio se mantienen en una única aplicación.  
- El escalado independiente de módulos concretos no será posible sin una futura separación.  
- Un crecimiento descontrolado del código puede aumentar el acoplamiento si no se mantienen límites entre módulos.

### ADR-007: Almacenar datos geoespaciales usando PostGis para PostgreSQL

- *Estado:*  
  Accepted  
- *Contexto:*  
  Las incidencias y los datos urbanos incluyen coordenadas y requieren consultas por distancia, proximidad y zona. Guardar únicamente latitud y longitud obligaría a realizar estos cálculos en la aplicación y dificultaría las consultas eficientes.  
- *Opciones consideradas:*

	1\. PostGIS sobre PostgreSQL  
	2\. Coordenadas numéricas sin extensión geoespacial  
	3\. Base de datos geoespacial independiente  

- *Decisión:*  
  Utilizar la extensión PostGIS de PostgreSQL para almacenar y consultar datos geoespaciales.  
- *Consecuencias:*  
+ Permite consultas espaciales e índices geográficos directamente en la base de datos.  
+ Mantiene los datos de incidencias y su localización en el mismo sistema de persistencia.  
- Añade complejidad a las migraciones y a la configuración de la base de datos.  
- Requiere conocer los tipos y funciones geoespaciales de PostGIS.

### ADR-008: Utilizaremos la API proporcionada por el ayuntamiento de Málaga ([https://datosabiertos.malaga.eu/](https://datosabiertos.malaga.eu/))

- *Estado:*  
  Accepted  
- *Contexto:*  
  La aplicación debe complementar las incidencias creadas por los usuarios con información pública y actualizada sobre la ciudad de Málaga. Los datos abiertos municipales constituyen una fuente oficial y alineada con el ámbito del proyecto.  
- *Opciones consideradas:*

	1\. Portal de datos abiertos del Ayuntamiento de Málaga  
	2\. Recopilación manual de datos estáticos  
	3\. Proveedores privados de datos urbanos  

- *Decisión:*  
  Consumir los conjuntos de datos públicos disponibles en el portal de datos abiertos del Ayuntamiento de Málaga.  
- *Consecuencias:*  
+ Se utilizan datos oficiales y relevantes para el área de estudio.  
+ Reduce la necesidad de generar o mantener datos urbanos propios.  
- La estructura, disponibilidad y frecuencia de actualización dependen del portal municipal.  
- Será necesario transformar los formatos publicados para adaptarlos al modelo interno.

### ADR-009: Utilizaremos la API OpenWeatherMap para saber el tiempo de Málaga

- *Estado:*  
  Accepted  
- *Contexto:*  
  Las condiciones meteorológicas pueden aportar contexto a una incidencia urbana y ayudar a los usuarios a interpretar su entorno. Se necesita una fuente accesible que proporcione información actualizada para Málaga.  
- *Opciones consideradas:*

	1\. OpenWeatherMap  
	2\. AEMET OpenData  
	3\. Datos meteorológicos introducidos manualmente  

- *Decisión:*  
  Utilizar OpenWeatherMap para obtener las condiciones meteorológicas de Málaga.  
- *Consecuencias:*  
+ API ampliamente utilizada y sencilla de integrar mediante peticiones HTTP.  
+ Ofrece información actual y previsiones que pueden ampliarse en el futuro.  
- Depende de una clave de API y de los límites del plan de uso seleccionado.  
- La aplicación queda condicionada a la disponibilidad y precisión del proveedor externo.

### ADR-010: Gemini y ChatGPT como IA principal

- *Estado:*  
  Accepted  
- *Contexto:*  
  Urban Pulse puede usar inteligencia artificial para asistir en la clasificación, resumen o análisis inicial de incidencias. Se requiere una solución con capacidad de lenguaje natural que pueda evolucionar sin acoplar toda la aplicación a un único proveedor.  
- *Opciones consideradas:*

	1\. Gemini y ChatGPT mediante una capa de integración del backend  
	2\. Un único proveedor de IA  
	3\. Modelo propio desplegado por el equipo  

- *Decisión:*  
  Integrar Gemini y ChatGPT como proveedores principales de IA, accedidos desde el backend mediante una capa de integración común.  
- *Consecuencias:*  
+ Permite comparar resultados y elegir el proveedor más adecuado para cada caso de uso.  
+ La capa de integración reduce el acoplamiento de la aplicación móvil con las APIs de IA.  
- Aumenta el coste, la gestión de credenciales y el mantenimiento de dos integraciones.  
- Deben aplicarse medidas para no enviar datos personales o sensibles innecesarios a servicios externos.

### ADR-011: Cuando un usuario selecciona una categoría de incidencia, se le muestra un mapa concreto para esa categoría.

- *Estado:*  
  Accepted  
- *Contexto:*  
  Las categorías de incidencia, como movilidad, limpieza o alumbrado, requieren distinta información geográfica para que el usuario pueda localizar y entender el problema. Mostrar siempre las mismas capas reduciría la relevancia del mapa.  
- *Opciones consideradas:*

	1\. Configurar capas de mapa por categoría  
	2\. Mostrar un mapa genérico para todas las categorías  
	3\. Permitir al usuario elegir manualmente todas las capas  

- *Decisión:*  
  Mostrar una configuración de mapa específica para cada categoría de incidencia, con las capas y marcadores relevantes preseleccionados.  
- *Consecuencias:*  
+ La información cartográfica se adapta al contexto de la incidencia.  
+ Se reduce la carga cognitiva al mostrar solo datos relevantes inicialmente.  
- Es necesario mantener la relación entre categorías y sus capas de mapa.  
- Una configuración excesivamente rígida puede ocultar información útil para algunos usuarios.

### ADR-012: Docker compose para el desarrollo local

- *Estado:*  
  Accepted  
- *Contexto:*  
  El equipo necesita ejecutar de forma reproducible el backend y los servicios necesarios, incluida la base de datos PostgreSQL con PostGIS, sin configuraciones manuales distintas en cada equipo.  
- *Opciones consideradas:*

	1\. Docker Compose  
	2\. Instalación manual de dependencias en cada equipo  
	3\. Entorno de desarrollo remoto compartido  

- *Decisión:*  
  Utilizar Docker Compose para levantar el entorno local de desarrollo y sus dependencias.  
- *Consecuencias:*  
+ Configuración homogénea y reproducible para todos los integrantes del equipo.  
+ Facilita iniciar PostgreSQL con PostGIS y otros servicios auxiliares mediante un único comando.  
- Requiere que cada desarrollador tenga Docker instalado y recursos suficientes.  
- Los cambios en la configuración de contenedores deben mantenerse sincronizados con la documentación del proyecto.
