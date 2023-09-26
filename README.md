# springboot_docu
# Spring Boot

Spring Boot es un proyecto dentro de la tecnología Spring diseñado para simplificar el desarrollo de aplicaciones Java Spring. Su objetivo principal es permitir a los desarrolladores crear aplicaciones ejecutables con una configuración mínima, manteniendo la flexibilidad y la preparación para producción. En resumen, Spring Boot ofrece:

- Configuración flexible de Java Beans, XML y transacciones de bases de datos.
- Soporte para procesamiento por lotes y gestión de puntos finales REST.
- Configuración automática sin necesidad de ajustes manuales.
- Gestión sencilla de dependencias.
- Contenedor de servlet integrado.

En Spring Boot, la configuración se realiza automáticamente según las dependencias presentes en el proyecto, gracias a la anotación `@EnableAutoConfiguration`. Por ejemplo, si tienes MySQL en el classpath pero no has configurado una conexión de base de datos, Spring Boot configurará automáticamente una base de datos en memoria. El punto de entrada de una aplicación Spring Boot es una clase anotada con `@SpringBootApplication` que contiene el método principal. Además, Spring Boot escanea automáticamente todos los componentes del proyecto mediante `@ComponentScan`.

# Estructura típica del código de Spring Boot

Spring Boot no impone una estructura de código estrictamente necesaria para desarrollar aplicaciones, pero se recomiendan ciertas prácticas para mantener una organización lógica del proyecto. A continuación, se presenta un ejemplo de las carpetas típicas en un proyecto de Spring Boot:

- **config**: Contiene clases Java que leen valores de los archivos de propiedades (property files) o propiedades de la aplicación.

- **constants**: Aquí se encuentran clases que almacenan las constantes utilizadas en la aplicación.

- **controller**: Contiene las clases de controlador, que gestionan las solicitudes HTTP y las respuestas correspondientes.

- **model**: En esta carpeta se ubican las clases de beans que modelan la estructura de la aplicación.

- **dao**: Contiene clases con la lógica de acceso a la base de datos, como implementaciones de repositorios o clases de acceso a datos.

- **security**: En esta carpeta se incluyen clases con código relacionado con la seguridad de la aplicación, como configuraciones de autenticación y autorización.

- **service**: Contiene las clases de servicio que son invocadas por los controladores para llevar a cabo las funcionalidades de negocio.

- **util**: Aquí se encuentran clases con código de soporte, como utilidades y funciones de ayuda.

A continuación, se muestra un ejemplo de una estructura típica en una aplicación de Spring Boot:

```markdown
proyecto/
|-- src/
|   |-- main/
|   |   |-- java/
|   |   |   |-- com/
|   |   |   |   |-- ejemplo/
|   |   |   |   |   |-- config/
|   |   |   |   |   |-- constants/
|   |   |   |   |   |-- controller/
|   |   |   |   |   |-- model/
|   |   |   |   |   |-- dao/
|   |   |   |   |   |-- security/
|   |   |   |   |   |-- service/
|   |   |   |   |   |-- util/
|   |   |-- resources/
|   |   |   |-- application.properties
|   |   |-- ...
|-- ...
```

Esta estructura ayuda a mantener un proyecto de Spring Boot organizado y fácil de mantener.


# Spring Beans e Inyección de Dependencias en Spring Boot

En Spring Boot, podemos aprovechar todas las técnicas aprendidas en Spring Framework para definir Spring Beans y gestionar la inyección de dependencias. Esto se logra utilizando la anotación `@ComponentScan` para buscar y registrar automáticamente los Spring Beans y `@Autowired` para realizar la inyección de dependencias correspondiente.

En el diseño típico de Spring Boot, no es necesario especificar argumentos para la anotación `@ComponentScan`, ya que todos los archivos de clase de componentes se registran automáticamente como Spring Beans. El siguiente ejemplo ilustra el proceso de cableado automático de objetos y la creación de un Bean:

- `@SpringBootApplication`
- `@EnableAutoConfiguration`

La anotación `@EnableAutoConfiguration`, como su nombre indica, habilita la configuración automática de la aplicación. Esto significa que Spring Boot buscará Beans de configuración automática en su classpath y los aplicará automáticamente. Es importante notar que esta anotación debe usarse en conjunto con `@Configuration`.

## Starters en Spring Boot

En Spring Boot, los "starters" son conjuntos de dependencias preconfiguradas que simplifican la configuración de proyectos específicos. Estos "starters" proporcionan las bibliotecas y configuraciones necesarias para diferentes tipos de aplicaciones, lo que agiliza el desarrollo y permite enfocarse en la lógica de negocio.

En resumen, en Spring Boot podemos aprovechar las técnicas de Spring Framework para gestionar Spring Beans y la inyección de dependencias, además de utilizar "starters" que simplifican la configuración de proyectos.


Spring es un **framework** para desarrollo de aplicaciones Java. Spring Boot es una **extensión**, un suite, pre-configurado para ejecutar Spring fuera-de-la-caja utilizando las mejores prácticas y sin perder flexibilidad. Con Spring Boot solo hay que determinar qué tipo de proyecto estaremos utilizando y él se encarga de resolver todas las **dependencias** para que la aplicación funcione. Spring Boot se puede ejecutar como una aplicación **Stand**-**alone**, pero también es posible ejecutar aplicaciones web, ya que es posible desplegar las aplicaciones mediante un servidor web integrado, como es el caso de **Tomcat**, Jetty o Undertow. Al instalar estos **Starters** Spring Boot se encargará de hacer encajar las dependencias de tal forma que ambos proyectos puedan encajar de forma natural en nuestra solución con sus **versiones** correspondientes.

## Seguridad en Java Spring

Hasta este punto del curso, apenas se han abordado los elementos de seguridad en Java Spring. A continuación, se presenta la configuración más simple de seguridad para nuestra aplicación.

Para comenzar, se incluye el siguiente starter en el archivo `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

En este punto, es de esperar que el alumno haya notado un patrón: la mayoría de las bibliotecas de Spring se pueden importar fácilmente a nuestro proyecto utilizando iniciadores de arranque simples, también conocidos como "starters". Una vez que `spring-boot-starter-security` esté presente, todos los puntos finales (end points) estarán protegidos por defecto, utilizando `httpBasic` o `formLogin`, según la estrategia de negociación de contenido de Spring Security.

Por lo tanto, si tenemos este iniciador en la ruta de clases, generalmente deberíamos definir nuestra propia configuración de seguridad personalizada. Esto se logra extendiendo la clase básica de seguridad `WebSecurityConfigurerAdapter`, como se muestra a continuación:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .anyRequest()
            .permitAll()
            .and().csrf().disable();
    }
}
```

En este ejemplo, permitimos el acceso sin restricciones a todos los puntos finales. Sin embargo, es importante tener en cuenta que Spring Security es un tema extenso y no se puede abordar fácilmente en unas pocas líneas de configuración.


# Logging en Spring Boot

Spring Boot utiliza el registro de Apache Commons para sus registros internos. Ofrece soporte predeterminado para Java Util Logging, Log4j2 y Logback. Esto permite configurar el registro en la consola y en archivos. Cuando se utilizan Spring Boot Starters, Logback brinda un sólido soporte para el registro. Además, Logback también es compatible con Common Logging, Util Logging, Log4J y SLF4J.

## Formato de Registro

El registro en Spring Boot se puede personalizar según tus necesidades. Puedes configurar el formato y el contenido de los registros para que se ajusten a tus requerimientos.

## Salida de Registro en Consola

Spring Boot facilita la configuración del registro en la consola, lo que te permite ver los registros directamente en la terminal.

## Salida de Registro en Archivos

Además de la consola, puedes configurar Spring Boot para que registre la información en archivos, lo que resulta útil para el almacenamiento a largo plazo de registros.

## Niveles de Registro

Spring Boot ofrece varios niveles de registro, como INFO, DEBUG, WARN y ERROR, que te permiten controlar la cantidad de información que se registra según tus necesidades.


# Spring Boot Runners

Las interfaces `Application Runner` y `Command Line Runner` permiten ejecutar código después de que se inicia la aplicación Spring Boot. Estas interfaces se utilizan para llevar a cabo acciones inmediatamente después de iniciar la aplicación. El siguiente ejemplo muestra cómo implementar la interfaz `Application Runner` en el archivo de la clase principal.

```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication implements ApplicationRunner {
   public static void main(String[] args) {
      SpringApplication.run(MyApplication.class, args);
   }
   
   @Override
   public void run(ApplicationArguments arg0) throws Exception {
      System.out.println("Hola mundo desde el Application Runner");
   }
}
```

# Propiedades de la Aplicación en Spring Boot

En Spring Boot, los archivos de propiedades se utilizan para centralizar y gestionar todas las configuraciones de la aplicación. Estas propiedades se mantienen típicamente en el archivo `application.properties`, ubicado en el classpath, generalmente en `src/main/resources`. Un ejemplo de archivo `application.properties` se muestra a continuación:

```properties
server.port = 9090
spring.application.name = demoservice
```

Spring Boot también admite configuraciones basadas en YAML. En lugar de `application.properties`, puedes usar `application.yml`, que también debe mantenerse en el classpath. Además, puedes especificar ubicaciones personalizadas para archivos de propiedades al ejecutar el archivo JAR, como se muestra en el siguiente comando:

```shell
java -jar myapp.jar -Dspring.config.location=file:/path/to/custom/application.properties
```

La anotación `@Value` se utiliza para leer valores de propiedades o entornos en código Java. La sintaxis para acceder a una propiedad es `@Value("${nombre_de_la_propiedad}"). A continuación, se muestra un ejemplo de cómo usar `@Value` para obtener el valor de `spring.application.name`:

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class MyApplication {
   @Value("${spring.application.name}")
   private String appName;

   public static void main(String[] args) {
      SpringApplication.run(MyApplication.class, args);
   }

   @RequestMapping("/")
   public String getAppName() {
      return "Application Name: " + appName;
   }
}
```

Es posible definir perfiles activos de propiedades en Spring Boot, lo que te permite cambiar la configuración según el entorno. Por ejemplo, puedes tener un archivo `application-dev.properties` para el entorno de desarrollo y otro `application-prod.properties` para producción, cada uno con configuraciones específicas.

En resumen, las propiedades en Spring Boot se utilizan para configurar y personalizar la aplicación de manera centralizada, y se pueden acceder en el código Java mediante la anotación `@Value`.

# RESTFUL APP

En lugar de tener servicios con métodos diversos, en una RESTful App declaramos Recursos. Un Recurso se refiere a un concepto importante en nuestra aplicación empresarial, como facturas, cursos, compras, etc., que suele ser un objeto de negocio. El diseño RESTful define un nivel de organización que permite acceder a cada recurso de forma independiente, lo que favorece la reutilización, aumenta la flexibilidad y permite la adición de nuevas operaciones como inserción, borrado, búsqueda, etc.

Hasta ahora, para realizar peticiones y enviar datos, se utilizan GET o POST indistintamente. En el nivel 2 del diseño RESTful, las operaciones se categorizan de manera más estricta:

- GET: Se utiliza para solicitar consultas a los recursos.
- POST: Se usa para insertar nuevos recursos.
- PUT: Se emplea para actualizar recursos.
- DELETE: Se utiliza para borrar recursos.

Luego, existe un nivel adicional llamado HATEOAS (Hypertext As The Engine Of Application State).

## HATEOAS

HATEOAS es un acrónimo de "Hypermedia as the Engine of Application State". El objetivo de HATEOAS es que las operaciones que se pueden realizar con una API sean auto-descubribles a través de hipervínculos que el propio API sirve al cliente. De esta forma, el cliente REST no necesita conocer de antemano la forma de interactuar con el servidor, solo debe saber cómo interpretar los hipervínculos.

## Entendiendo REST Spring

En Spring, la configuración de una RESTful App en Java se simplifica mediante Spring Boot.


# Testeando el Contexto de Spring

A partir de Spring 3.1, se proporciona soporte de primera clase para pruebas unitarias de las clases anotadas con `@Configuration`. Se puede utilizar `@RunWith(SpringJUnit4ClassRunner.class)` y `@ContextConfiguration` para configurar las pruebas. Por ejemplo:

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(
    classes = {WebConfig.class, PersistenceConfig.class},
    loader = AnnotationConfigContextLoader.class)
public class SpringContextIntegrationTest {
    @Test
    public void contextLoads() {
        // Realizar pruebas aquí
    }
}

En este caso, `WebConfig` y `PersistenceConfig` son clases de configuración Java anotadas con `@Configuration`.

## Usando Spring Boot de Nuevo

Spring Boot proporciona varias anotaciones para configurar el `Spring ApplicationContext` en pruebas unitarias de manera más intuitiva. Puedes cargar solo una parte específica de la configuración de la aplicación o simular todo el proceso de inicio del contexto.

Por ejemplo, puedes usar `@SpringBootTest` para crear todo el contexto de la aplicación sin iniciar el servidor, y luego agregar `@AutoConfigureMockMvc` para inyectar una instancia de `MockMvc` y enviar solicitudes HTTP en pruebas de controladores:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class MyControllerAppIntegrationTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void whenTestApp_thenEmptyResponse() throws Exception {
        this.mockMvc.perform(get("/cars")
            .andExpect(status().isOk())
            .andExpect(...);
    }
}
```

Si deseas evitar crear todo el contexto y probar solo los controladores MVC, puedes utilizar `@WebMvcTest`:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerWebLayerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private IFooService service;

    @Test()
    public void whenTestMvcController_thenRetrieveExpectedResult() throws Exception {
        // ...
        this.mockMvc.perform(get("/cars")
            .andExpect(...);
    }
}
```

Esto te permite centrarte en las pruebas de tus controladores MVC sin cargar todo el contexto de la aplicación.

# Resumen

**Java Spring** ofrece una variedad de proyectos modulares enfocados a resolver problemas en el diseño de aplicaciones empresariales.

Los proyectos más utilizados son:
- **Spring Data**: que es el que hemos utilizado a lo largo de este curso.
- **Spring Security**
- **Spring Cloud**
- **Spring Integration**

Todos estos proyectos de Spring son modulares y se pueden utilizar instalando las dependencias necesarias en el `POM.XML`. Se aconseja consultar la documentación de cada proyecto en [https://spring.io/projects/](https://spring.io/projects/).


## Requisitos y Solución Propuesta

A continuación se indican una serie de requisitos, explique cuáles serían los proyectos de Java Spring que utilizaría en la arquitectura de la solución para enfrentarse a estos requisitos. Pueden existir varias soluciones distintas para varios requisitos. Siempre es una buena idea razonar el porqué de la decisión.

Estos son los requisitos:

1. SpaceGames crea juegos multijugador en línea basados en sesiones para plataformas móviles. Construyen todos sus juegos usando alguna integración del lado del servidor. Históricamente, han utilizado proveedores de nube para arrendar servidores físicos.
2. Debido a las inesperadas aplicaciones populares de algunos de sus juegos, han tenido problemas para escalar su audiencia global, servidores de datos, bases de datos MySQL y herramientas de análisis.
3. Su modelo actual es escribir estadísticas del juego en archivos y enviarlos a través de una herramienta ETL que los carga en una base de datos MySQL centralizada para generar informes.

**Para abordar estos requisitos, puedes considerar lo siguiente:**

- Escalar dinámicamente hacia arriba o hacia abajo según el nivel de actividad de los jugadores.
- Conexión a un servicio de base de datos transaccional para administrar los perfiles de usuario y el estado del juego.
- Almacenar la actividad del juego en un servicio de base de datos de series temporales para análisis futuros.
- A medida que el sistema va escalando, asegurarse de que los datos no se pierdan debido a los atrasos de procesamiento.

## Requisitos para la plataforma de análisis de datos de los juegos:

- Escalar dinámicamente hacia arriba o hacia abajo según el nivel de actividad del juego.
- Procesar los datos entrantes sobre la marcha directamente desde los servidores del juego.
- Procesar datos que llegan tarde debido a redes móviles lentas.
- Permitir que las consultas accedan al menos a 10 TB de datos históricos.
- Procesar archivos que se cargan con regularidad en los dispositivos móviles de los usuarios.

## Abordando los Requisitos con Proyectos de Java Spring

Para abordar los requisitos mencionados en la arquitectura de la solución utilizando proyectos de Java Spring, aquí hay una posible selección de proyectos y razonamientos para cada uno:

### Requisitos del Back-End del Juego:

1. **Ejecutar análisis intensivos sobre los datos capturados:**

   - Spring Batch: Spring Batch es una excelente elección para tareas de procesamiento por lotes. Puede ejecutar análisis intensivos en datos capturados de manera eficiente. Spring Batch permite definir trabajos y flujos para procesar datos de forma programática o mediante configuración.

2. **Aprovechar su entorno de servidor para ajustar la escala de computación automáticamente según la demanda:**

   - Spring Cloud: Utilizar Spring Cloud para implementar un sistema de microservicios. Esto permitirá la escalabilidad automática y la administración de la carga según la demanda. Spring Cloud proporciona herramientas como Eureka para el descubrimiento de servicios y Ribbon para el equilibrio de carga.

3. **Integrarse con una base de datos NoSQL gestionada:**

   - Spring Data para MongoDB, Redis o Cassandra: Dependiendo del tipo de base de datos NoSQL que desee utilizar, puede integrar Spring Data con MongoDB, Redis o Cassandra. Spring Data proporciona una capa de abstracción para interactuar con bases de datos NoSQL, facilitando la integración y el acceso a los datos.

4. **Almacenar la actividad del juego en un servicio de base de datos de series temporales para análisis futuros:**

   - InfluxDB o TimescaleDB: Puede utilizar bases de datos de series temporales como InfluxDB o TimescaleDB para almacenar datos de actividad del juego. Spring Data se puede configurar fácilmente para interactuar con estas bases de datos y almacenar los datos de manera eficiente.

5. **A medida que el sistema va escalando, asegurarse de que los datos no se pierdan debido a los atrasos de procesamiento:**

   - Apache Kafka o RabbitMQ: Implementar un sistema de mensajería como Apache Kafka o RabbitMQ para asegurarse de que los datos no se pierdan debido a los atrasos de procesamiento. Spring Integration se puede utilizar para integrar estos sistemas de mensajería con la aplicación y garantizar la fiabilidad en la entrega de datos.

### Requisitos para la plataforma de análisis de datos de los juegos:

1. **Escalar dinámicamente hacia arriba o hacia abajo según el nivel de actividad del juego:**

   - Spring Cloud: Continuar utilizando Spring Cloud para la escalabilidad automática y la administración de la carga según la demanda. Puede ajustar la escala de los servicios de análisis en función de la actividad del juego.

2. **Procesar los datos entrantes sobre la marcha directamente desde los servidores del juego:**

   - Spring Integration: Spring Integration puede manejar la integración en tiempo real con los servidores del juego. Puede configurar flujos de integración para procesar datos entrantes y enrutarlos a los servicios adecuados de análisis.

3. **Procesar datos que llegan tarde debido a redes móviles lentas:**

   - Apache Kafka o RabbitMQ: Utilizar un sistema de mensajería como Apache Kafka o RabbitMQ para manejar datos que llegan tarde debido a redes móviles lentas. Spring Integration también puede ser útil para enrutar estos datos a flujos de procesamiento adecuados.

4. **Permitir que las consultas accedan al menos a 10 TB de datos históricos:**

   - Hadoop HDFS o Amazon S3: Para el almacenamiento de datos históricos, puede utilizar sistemas de archivos distribuidos como Hadoop HDFS o almacenamiento en la nube como Amazon S3. Spring se puede utilizar para interactuar con estos sistemas de almacenamiento.

5. **Procesar archivos que se cargan con regularidad en los dispositivos móviles de los usuarios:**

   - Spring Batch: Utilizar Spring Batch para procesar archivos cargados con regularidad desde dispositivos móviles de usuarios. Puede configurar trabajos de Spring Batch para procesar estos archivos de manera eficiente.

En resumen, Spring ofrece una amplia gama de proyectos y herramientas que pueden adaptarse a los requisitos específicos de Space Games para su nuevo juego. La selección de proyectos dependerá de la infraestructura existente, las preferencias tecnológicas y las necesidades precisas de la aplicación.
