
Documentación del Proyecto
API Pokédex en Mulesoft
Índice
1. Introducción (Página 1)
2. Enfoque y Planificación (Páginas 2-3)
 Análisis de Requisitos
 Consideraciones de Diseño
3. Implementación (Página 3)
 Tecnologías Utilizadas
 Endpoints Diseñados
4. Configuración del Entorno de Desarrollo (Página 4-5)
 Instalación de Anypoint Studio
 Configuración de Java 17
 Configuración de Maven
 Configuración de Postman
 Configuración del Proyecto en Anypoint Studio
5. Estructura del Proyecto (Páginas 5-9)
 Flujo en MuleSoft
 Transformación de Datos con DataWeave
 Consideraciones Adicionales
 Componente RAML
6. Futuras Mejoras y Siguientes Pasos (Página 9)
7. Conclusión (Página 10)


Introducción
Este documento detalla el desarrollo de una API en Mulesoft cuyo objetivo es
proporcionar información sobre Pokémon utilizando la API externa de PokéAPI. La
solución incluye el diseño de endpoints, transformación de datos con DataWeave y el
manejo de errores.
Enfoque y Planificación
1. Análisis de Requisitos
Antes de comenzar el desarrollo, fue fundamental analizar los requisitos funcionales y
técnicos para garantizar una implementación eficiente y alineada con los objetivos del
proyecto. A continuación, se detallan los aspectos clave del análisis:
1.1 Requisitos Funcionales
 La API debe permitir la consulta de información de Pokémon a través de dos
métodos:
o Por nombre (Query Parameter name).
o Por ID (Path Parameter {id}).
 La información devuelta debe contener los siguientes datos:
o Nombre del Pokémon.
o Tipo(s) (puede ser uno o más).
o Peso y altura.
o ID en el formato #XXXX.
o Lista de movimientos separados por comas.
o Estadísticas de ataque, defensa y velocidad.
o Fecha y hora de la consulta.
o Lista de áreas donde se puede encontrar el Pokémon.
 Si el usuario ingresa un parámetro vacío, la API debe devolver un mensaje de error
sin llamar a la API externa.
 Si el usuario consulta un Pokémon que no existe, la API debe devolver un mensaje
de error.
 La API debe ser accesible mediante peticiones HTTP GET.


1.2 Requisitos Técnicos
 Desarrollar La API en Mulesoft dentro de Anypoint Studio.
 Utilizar DataWeave para la transformación de datos.
 Usar la API pública PokéAPI (https://pokeapi.co/).
 El servidor debe escuchar peticiones en el puerto 8081.
 Implementar un manejo de errores eficiente para garantizar la estabilidad del
servicio.
 Cumplir con el estándar RESTful.
 La documentación de la API debe realizarse en RAML.
1.3 Consideraciones de Diseño
 La arquitectura debe seguir el modelo API-Led Connectivity.
 Estructurar correctamente los flujos y subflujos para mantener el modularidad del
código.
 Asegurar que la API sea escalable y fácil de mantener.
Implementación
1. Tecnologías Utilizadas:
o Anypoint Studio
o Mulesoft
o Java 17 (a partir de Anypoint Studio 7.19.0, los nuevos proyectos de Mule
usan JDK 17 por defecto)
o Maven (Se recomienda utilizar version 3.8.X para una mayor estabilidad)
o Postman
o DataWeave
o RAML
2. Endpoints Diseñados:
o GET /pokemon?name={nombre}: (Consulta un Pokémon por nombre.)
o GET /pokemon/{id}: (Consulta un Pokémon por ID.)


Configuración del Entorno de Desarrollo
2.1. Instalación de Anypoint Studio
 Descarga: Accede al enlace proporcionado y descarga la versión más reciente de
Anypoint Studio.
 Instalación: Sigue los pasos del instalador para completar la instalación en tu
sistema operativo.
 Configuración inicial: Al abrir Anypoint Studio por primera vez, se te pedirá que
configures el workspace (espacio de trabajo). Elige una carpeta donde se
almacenarán tus proyectos.
2.2. Configuración de Java 17
(Como explique anteriormente con la última actualización, MuleSoft recomienda Oracle
JDK 1.7.0_79/80 para Anypoint Studio.)
 Verificación: Abre una terminal o línea de comandos y ejecuta el siguiente
comando para verificar la versión de Java: “java -version”
deberías ver algo similar a:
“openjdk version "17.0.1" 2021-10-19”
 En caso de no ver esta versión de java, descarga la versión recomendada de la
página de java y asegúrese de que las variables de entorno de Java estén
configuradas correctamente.
Esto incluye:
JAVA_HOME: apuntar a la carpeta de instalación de JDK 17.
PATH: incluir la ruta al binario de Java.
2.3. Configuración de Maven
 Se recomienda usar versiones de mave 3.8.X para mayor estabilidad.
 Descarga Maven de su sitio oficial y sigue las instrucciones para su instalación.
Si no está instalado, descarga Maven desde su sitio oficial.
 Variables de entorno: Configura las variables de entorno para Maven:
MAVEN_HOME: Debe apuntar a la carpeta de instalación de Maven.
PATH: Debe incluir la ruta al binario de Maven.


2.4. Configuración de Postman
 Instalación: Descarga e instala Postman desde el enlace proporcionado.
 Uso: Postman se utilizará para realizar pruebas de API. Puedes crear colecciones y
enviar solicitudes HTTP GET para probar tanto la PokéAPI como la API que
desarrollarás.
3. Configuración del Proyecto en Anypoint Studio
 Creación de un nuevo proyecto:
 Abre Anypoint Studio.
 Selecciona File > New > Mule Project.
 Asigna un nombre al proyecto, por ejemplo, PokedexAPI.
 Haz clic en Finish.
Estructura del proyecto
En este punto se explica el diseño y la implementación de un flujo en MuleSoft utilizando
Anypoint Studio. El flujo está diseñado para interactuar con la PokeAPI, obteniendo
información sobre Pokémon basada en parámetros de consulta (nombre o ID) y
devolviendo una respuesta en formato JSON.
Componente pokedexapi.xml
Este archivo XML define un flujo de MuleSoft llamado pokedexapiFlow, el cual expone un
servicio HTTP para consultar información sobre Pokémon utilizando la API externa
PokeAPI. El flujo recibe solicitudes HTTP, obtiene datos del Pokémon consultado y
devuelve una respuesta en formato JSON con información estructurada.
Configuración del Listener HTTP
 Se configura un listener HTTP (HTTP_Listener_config) que escucha solicitudes en el
puerto 8081.
 La configuración permite recibir peticiones en todas las interfaces (0.0.0.0).
Flujo pokedexapiFlow
 Se define un listener HTTP que responde en la ruta /api/pokemon.
 Este listener está vinculado a la configuración HTTP_Listener_config.


Transformación del Mensaje (DataWeave)
 Se obtiene el nombre o ID del Pokémon desde los parámetros de la solicitud.
 Se consulta la PokeAPI para recuperar los datos del Pokémon:
 Nombre, tipo(s), peso y estatura.
 ID formateado.
 Movimientos disponibles.
 Estadísticas principales: ataque, defensa y velocidad.
 Áreas donde se encuentra el Pokémon.
 Se agrega la fecha y hora de la consulta.
 La respuesta se genera en formato JSON.
Ejemplo de Respuesta JSON
{
 "nombre": "pikachu",
 "tipo": "electric",
 "peso": 60,
 "estatura": 4,
 "ID": "#0025",
 "movimientos": "thunderbolt, quick attack, iron tail, electro ball",
 "estadisticas": {
 "ataque": 55,
 "defensa": 40,
 "velocidad": 90
 },
 "fecha": "15:30:45 14-02-2025",
 "areas": ["forest", "park"]
}
7
Consideraciones Adicionales
1. Manejo de Errores:
El flujo actual no permite nombres o IDs inválidos ni vacíos. Para un mejor
funcionamiento sería recomendable agregar un bloque try-catch que maneje
posibles fallos en las solicitudes HTTP o en la transformación de datos.
Ejemplos:
2. Optimización:
Si se espera un alto volumen de solicitudes, se puede considerar el uso de caché
para almacenar respuestas frecuentes y reducir el número de solicitudes a la
PokeAPI.
3. Seguridad:
Podria agregarse a futuro una autenticación para proteger el endpoint
Componente pokedex-api.raml

Descripción General
La API está diseñada para interactuar con una base de datos o servicio que proporciona
información sobre Pokémon. Expone dos endpoints principales:
 Obtener información de un Pokémon por nombre.
 Obtener información de un Pokémon por ID.
Tipos de Datos
La API utiliza un tipo de dato llamado PokemonResponse para estructurar las respuestas.
Este tipo define las propiedades que se incluyen en la respuesta JSON.
EJEMPLO:
Endpoints
Obtener Pokémon por Nombre
 Método: GET
 Ruta: /pokemon/?name
 Nombre del Pokémon (obligatorio, tipo string).
 Respuesta: Código 200 con cuerpo en formato JSON (PokemonResponse).
Obtener Pokémon por ID
 Método: GET
 Ruta: /pokemon/?id
 Numero Identificador único del Pokémon (string).
 Respuesta: Código 200 con cuerpo en formato JSON (PokemonResponse).
Consideraciones;
 Validación: El parámetro name o id es obligatorio en el endpoint /pokemon.
 Seguridad: No se incluyen políticas de seguridad, pero pueden agregarse (por
ejemplo, OAuth 2.0 o API Key).
Futuras Mejoras o Siguientes Pasos
 Implementación de un manejo de errores más robusto para incluir respuestas mas
explicativas con códigos HTTP 404 y 500.
 Implementación de caché para reducir las llamadas a la API externa.
 Soporte para más información como habilidades o evolución de los Pokémon.
 Seguridad con autenticación o limitación de requests.
 Optimización del tiempo de respuesta y escalabilidad.
 Implementación de caché para reducir las llamadas a la API externa.
 Soporte para más información como habilidades o evolución de los Pokémon.
 Seguridad con autenticación o limitación de requests.
 Optimización del tiempo de respuesta y escalabilidad.


Conclusión
El desarrollo de la API Pokédex en MuleSoft ha cumplido con los objetivos solicitados,
garantizando una integración eficiente con la PokéAPI y proporcionando un servicio
funcional y estructurado bajo los principios de API-Led Connectivity. La implementación
incluyó el diseño de endpoints RESTful, transformación de datos con DataWeave y un flujo
optimizado en Anypoint Studio, asegurando una correcta modularidad y escalabilidad del
sistema.
Durante el desarrollo se tuvo en cuenta el manejo de errores para validar parámetros y
evitar consultas inválidas. No obstante, se identifican oportunidades de mejora en este
aspecto, como la implementación de mensajes de error más descriptivos y el uso de
códigos HTTP más específicos (404 para recursos no encontrados y 500 para fallos
internos), mejorando la experiencia del usuario y la depuración de problemas.
Como siguientes pasos, se recomienda optimizar el rendimiento con la implementación de
caché para reducir las llamadas a la API externa, mejorar la seguridad con autenticación y
limitación de requests, e incluir más información relevante sobre los Pokémon, como
habilidades y evoluciones.
Este proyecto refleja un enfoque estructurado y escalable en el desarrollo de APIs, con
una base sólida para futuras mejoras y una visión orientada a la estabilidad y crecimiento
del servicio.
