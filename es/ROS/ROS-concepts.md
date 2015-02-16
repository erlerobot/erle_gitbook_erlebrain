#ROS: Conceptos


ROS tiene tres niveles de conceptos: El **Nivel de sistema de archivos**, el **Gráfico de nivel de comutación**, y el **nivel de comunidad**. Estos niveles y conceptos son agrupados debajo, y las siguentes secciones se centran a ello con mayor detalle.

Añadiendo a los tres niveles de conceptos, ROS también define dos tipos de nombres: **Nombres de recurso de paquetes** y **Nombres de recurso de gráfico**, también profundizados debajo.

###Nivel de sistema de archivos


El concepto del nivel de sistema de archivos, se centra en cubrir los recursos de ROS que encontrarías en un disco, como por ejemplo:

- **Paquetes**: Los paquetes son la unidad principal de organización de software en ROS. Un paquete puede contener procesos de tiempo de ejecución (nodos), una librería dependiente ROS, conjuntos de datos, archivos de configuración, o cualquier otro archivo útil que esté organizado junto. Los paquetes son el artículo de construcción más pequeño en ROS. Es decir, el artículo más pequeño que puedes crear o distribuir es un paquete.
- **Meta-paquetes**: Los meta-paquetes son paquetes especializados que sirven para representar un grupo dependiente de otros paquetes. La mayoría de los metapaquetes comúnes, se usan como un marcador de posición compatible para un conjunto de Stacks rosbuild.
- **Manifiestos de paquete**: Un manifiesto (paquete .xml) proporciona metadatos sobre un paquete, incluyendo el nombre, la versión, la descripción, información de licencia, dependencias, y otro tipo de información meta, como paquetes exportados. El paquete manifiesto paquete.xml se define en REP-0127.
- **Repositorios**: Una colección de paquetes que comparten un sistema VCS común. Los paquetes que comparten un VCS, comparten la misma versión y pueden ser publicados juntos usando la herramienta de automatización catkin. Normalmente estos repositorios trazarán un mapa de un conjunto de rosbuilds convertidos. Los repositorios también pueden contener un sólo paquete.
**Tipos de Mensaje (msg)**: Descripciones de mensaje, almacenados en my_package/msg/MyMessageType.msg, definen la estructura de datos para los mensajes enviados en ROS.
- **Tipos de Servicios (srv)**: Descripciones de servicio, almacenados en my_package/srv/MyServiceType.srv, definen la solicitud y respuesta de las estructuras de datos para los servicios en ROS.

###Nivel Gráfico de Computación


El Gráfico de Computación es una red peer-to-peer de los procesos de ROS, que procesan datos juntos. Los conceptos básicos del Gráfico de Computación de ROS son *nodos*, *Master*, *Servidor de Parámetro*, *mensajes*, *servicios*, *tópicos*, y *bolsas*, todas ellas proporcionan datos al Gráfico de formas diferentes.

Estos conceptos se implementan en el repositorio [ros_comm](http://wiki.ros.org/ros_comm).

- [**Nodos**](http://wiki.ros.org/Nodes): Los nodos son procesos que ejecutan el cálculo. ROS está diseñado para ser modular en una escala de grano fino; un sistema de control de robot normalmente consta de varios nodos. Por ejemplo, un nodo controla un láser telémetro, un nodo controla las hélices del motor, un nodo interpreta la localización, un nodo crea una planificación de una trayectoria, un nodo proporciona una vista gráfica del sistema, etc. Un nodo ROS está escrito con el uso de ROS [client library](http://wiki.ros.org/Client%20Libraries), como [roscpp](http://wiki.ros.org/roscpp) o [rospy](http://wiki.ros.org/rospy).
- [**Master**](http://wiki.ros.org/Master): El ROS Master proporciona el nombre de registro y búsqueda del resto del Gráfico de Computación. Sin Master, los nodos no podrían buscarse entre ellos, intercambiar mensajes, o invocar servicios.
- [**Servidor de Parámetro**](http://wiki.ros.org/Parameter%20Server): El servidor de parámetro permite a los datos que éstos sean almacenados por clave en una localización central. Esto es normalmente parte del Master.
- [**Mensajes**](http://wiki.ros.org/Messages): Los Nodos se comunican ente ellos enviándose [messages](http://wiki.ros.org/Messages). Un mensaje es simplemente una estructura de datos, que comprende campos escritos. Los tipos primitivos estándar (íntegro, punto flotante, booleano, etc.) son soportados, como son colecciones de tipos primitivos. Los mensajes pueden incluir estructuras anidadas arbitrariamente y colecciones (como las estructuras C).
- [**Tópicos**](http://wiki.ros.org/Topics): Los mensajes son enrutados por un sistema de transporte con unas semánticas de publicación / subscripción en un determinado [tópico](http://wiki.ros.org/Topics). Un tópico es un [nombre](http://wiki.ros.org/Names) que se usa para identificar el contenido del mensaje. Un nodo que está interesado en un tipo específico de datos se suscribirá al tópico apropiado. Habrá múltiples publicadores concurrentes y suscriptores para un sólo tópico, y un nodo puede publicar y/o suscribirse a múltiples tópicos. En general, los publicadores y los subscriptores no conocen la existencia entre ellos. La idea es desacoplar la producción de la información desde su consumo. Lógicamente, uno puede entender un tópico como un mensaje escrito en el bus. Cada bus tiene un nombre, y cualquiera puede conectarse al bus para enviar o recibir mensajes, tantos como carácteres tenga el mensaje. ![](http://ros.org/images/wiki/ROS_basic_concepts.png)
- [**Servicios**](http://wiki.ros.org/Services): El modelo publicación / subscripción es un paradigma de comunicación muy flexible, pero es de forma muchos a muchos, una forma de transporte no es apropiada para las interacciones tipo solicitud / respuesta, que normalmente hacen falta en un sistema de distribución. El modo solicitud / respuesta se hace mediante los servicios, que son definidos por un par de estructuras de mensaje: una para la solicitud y otra para la respuesta. Un nodo solicitador ofrece un servicio debajo de un nombre, y un cliente utiliza el servicio enviando el mensaje de solicitud y esperando a la respuesta. Las librerías cliente de ROS normalmente presentan una interacción con el programador como si fuera una llamada remota de procedimiento.
- [**Bags**](http://wiki.ros.org/Bags): Los Bags son un formato para guardar y repetir los mensajes de datos de ROS. Los Bags son un mecanismo importante para almacenar datos, como un sensor de datos, que puede ser dificil el almacenarlo, pero es necesario para su desarrollo y el testeo de algoritmos.

El ROS Master actúa como un nombre de servicio en el Gráfico de Comptación de ROS. Almacena tópicos e información de servicios de registro para los nodos de ROS. Los nodos se comunican con el Master para relatar la información del registro. Como los nodos se comunican con el master, pueden recibir información sobre otros nodos registrados y hacer una conexión apropiada. El Master también hara una llamada de vuelta a esos nodos cuando la información del registro cambie, que permite a los nodos que creen conexiónes dinámicamente como nuevos nodos son ejecutados.

Los nodos se conectan a otros nodos directamente; el Master solo proporciona información de búsqueda, parecido a un servidor DNS. Los nodos que se subscriben a un tópico, harán una solicitud de conexión desde los nodos que publiquen dicho tópico, y establecerán esa conexión sobre un protocolo de conexión acordado. El protocolo más utilizado en ROS se llama [TCPROS](http://wiki.ros.org/ROS/TCPROS), que usa sockets estándar TCP/IP.


Esta arquitectura permite operaciones desacopladas, donde los nombres son el factor clave, donde sistemas más complejos y mayores pueden ser construidos. Los nombres tienen un rol importante en ROS: nodos, tópicos, servicios, y parámetros, todos tienen nombres. Cada librería cliente de ROS soporta [remapeo de nombres en la línea de comandos](http://wiki.ros.org/Remapping%20Arguments), que significa que un programa compilado puede ser reconfigurado en el tiempo de ejecución para operar en un Gráfico de Computación de diferente topología.

----

Por ejemplo, para controlar un telémetro láser Hokuyo, podemos comenzar con el driver hokuyo_node, que se comunica con el láser y publica los mensajes de sensor_msgs/LaserScan el el tópico de escaneo. Para procesar esos datos, debemos escribir un nodo usando laser_filters que subscribe a los mensajes el tópico del escaneo. Después de la subscripción, nuestro filtro empezará automáticamente a recibir mensajes del láser.

Ahora los dos lados están desacoplados. Lo que hace el nodo hokuyo_node es publicar escaneos, sin el conocimiento de quién está subscrito. Lo que hace el filtro es subscribirse a escaneos, sin el conocimiento de quién está publicándoles. Los dos nodos pueden empezar, acabar, y reiniciarse, en cualquier orden, sin inducir cualquier tipo de condición de errores.

Más tarde deberíamos añadir otro láser a nuestro robot, por lo que necesitamos reconfigurar nuestro sistema. Todo lo que necesitamos es remapear los nombres que se van a usar. Cuando comenzemos nuestro primer hokuyo_node, deberíamos decirle que remapee el escaneo a base_scan, y hacer lo mismo con nuestro nodo de filtro. Ahora, los dos nodos se comunicarán usando el tópico base_scan, y no oír mensajes en el tópico de escaneo. Después podremos empezar otro hokuyo_node para el nuevo telémetro láser.



###Nivel de comunidad

Los conceptos del Nivel de la Comundidad ROS, son recursos de ROS que permiten que comunidades separadas intercambien software y conocimiento. Estos recursos incluyen:

- [**Distribuciones**](http://wiki.ros.org/Distributions): Las distribuciones de ROS son colecciones de pilas versionadas que puedes instalar. Las distribuciones juegan un rol muy parecido a las distribuciones de Linux:
- [**Repositorios**](http://wiki.ros.org/Repositories): ROS confía en una red federada de repositorios de código, donde diferentes instituciones pueden desarrollar y publicar el código del software de los componentes del propio robot.
- [**La Wiki ROS**](http://wiki.ros.org/Documentation): La comunidad de Wiki de ROS es el foro principal para documentar información sobre ROS. Cualquiera puede inscribirse en una cuenta y contribuir con su propia documentación, proporcionar correciones o actualizaciones, escribir tutoriales, y más.
- **Bug Ticket System**: Por favor ver [Tickets](http://wiki.ros.org/Tickets) para información sobre los tickets de archivo.
- [**Listas de envío**](http://wiki.ros.org/Mailing%20Lists): Los usuarios de la lista de envío de ROS, es el canal principal de comunicación sobre nuevas actualizaciones de ROS, también como el foro para hacer preguntas sobre el software ROS.
- [**Respuestas ROS**](http://answers.ros.org/): Un sitio de preguntas y respuestas para responder tu preguntas relacionadas con ROS.
- [**Blog**](http://www.willowgarage.com/blog): El [Blog Willow Garage](http://www.willowgarage.com/blog) proporciona actualizaciones regulares, incluyendo fotos y vídeos.

###Nombres


###Nombres del Recurso Gráfico

Los nombres del recurso gráfico proporcionan un nombrado estructurado que se usa para los recursos en el Gráfico de Computación de ROS, como Nodos, Parámetros, Tópicos, y Servicios. Estos nombres son muy potentes en ROS, y centran en cómo son de grandes y están compuestos los sistemas más complicados de ROS, así que es más dificil el entender cómo estos nombres funcionan y cómo puedes controlarlos.

Antes de que describamos los nombres más profundamente, aquí hay algunos ejemplos:

- / (el espacio de nombre global)
- /foo
- /stanford/robot/name
- /wg/node1

Los nombres de recurso gráfico son un mecanismo importante en ROS para proporcionar encapsulación. Cada recurso está dentro de un espacio de nombre, que debería compartirse con muchos otros recursos. En general, los recursos pueden crear recursos dentro de su nombre de espacio y pueden acceder a recursos dentro o encima de su propio espacio de nombre. Las conexiones se pueden hacer en recursos de diferentes espacios de nombres. Esta encapsulación aisla diferentes porciones del sistema, de accidentalmente sobreescribir el recurso mal nombrado o globalmente piratear los nombres.

Los nombres se resuelven relativamente, entonces los recursos no necesitan saber en que espacio de nombre se encuentran. Esto simplifica el programar como nodos que trabajan juntos, pueden ser escritos como si todos estuvieran en el nombre de espacio de alto nivel. Cuando estos nodos se integran en un sistema mayor, pueden caer en el nombre de espacio que defina cada colección de código. Por ejemplo, uno puede conseguir una demo de Stanford y de Willow Garage y unirlas en una nueva demo con subgráficos Standford y WG. Si las dos demos tuvieran un Nodo llamado 'cámara', no habría conflicto entre ellas. Herramientas (ej. visualización gráfica) como también los parámetros (ej. demo_name) que necesitan ser visibles en todo el gráfico, pueden ser creadas en nodos de alto nivel.

#####Nombres válidos

Un nombre válido tiene las siguientes características:

1. El primer carácter es un caracter alfa ([a-z|A-Z]), tilde (~) o barra inclinada (/).
2. Los carácteres subsecuentes pueden ser alfanuméricos ([0-9|a-z|A-Z]), barrabaja (_), o barra inclinada (/).

------

**Excepción**: los nombres de base (descritos debajo) no pueden contener barras inclinadas (/) o tildes (~).

------

#####Resolviendo

Existen cuatro tipos de Nombres de Recurso de Gráfico en ROS: base, relativo, global y privado, que tiene la siguiente sintaxis:

- base
- relative/name
- /global/name
- ~private/name

Predeterminadamente, la resolución se hace relativa al nombre del espacio del nodo. Por ejemplo, el nodo `/wg/node1` tiene el nombre de espacio `/wg`, entonces el nombre node2 resolverá a `/wg/node2`.

Nombres sin ningún calificador de nombre de espacio absolutamente son nombres de base. Los nombres de base actualmente son una subclase de nombres relativos y tienen las mismas normas de resolución. Los nombres de base se usan más a menudo para iniciar el nombre del nodo.

Los nombres que empiezan con "/" son globales -- se consideran completamente resueltas. *Los nombres globales deben evitarse lo máximo posible, ya que limitan la portabilidad del código*. 

Los nombres que empiezan por "~" son privados. Convierten el nodo en un nombre de espacio. Por ejemplo, el nodo1 en un nombre de espacio `/wg/` tiene el nombre de espacio privado `wg/node1`. Los nombres privados son útiles para el paso de parámetros a un nodo específico vía el servidor de parámetro.

Aquí hay algún ejemplo de nombres de resolución:

|Nodo | Relativo (predeterminado) | Global | Privado |
| -- | -- | -- | -- |
| `/node1` | `bar -> /bar` | `/bar -> /bar` | `~bar -> /node1/bar`|
|`/wg/node2` | `bar -> /wg/bar` | `/bar -> /bar` | `~bar -> /wg/node2/bar` |
| `/wg/node3` | `foo/bar -> /wg/foo/bar` | `/foo/bar -> /foo/bar` | `~foo/bar -> /wg/node3/foo/bar` |

#####Remapeando

Cualquier nombre dentro de un nodo ROS puede remapearse cuando el nodo es lanzado en la línea de comandos. Para más información de ésta característica, ver [Remapeando Argumentos](http://wiki.ros.org/Remapping%20Arguments).

####Nombres de Recurso de Paquete

Los nombres de recurso de paquete se usan en ROS con los conceptos del nivel del sistema de archivos para simplificar los procesos que se refieren a tipos de datos y archivos en un disco. Los nombres de recurso de paquetes son muy simples: son el nombre del paquete en el que se encuentra el recurso, más el nombre del recurso. Por ejemplo, si el nombre `std_msgs/String` se refiere al mensaje "String", escrito en el paquete "std_msgs".

Algunos de los archivos relacionados con ROS que pueden referirse a usar los nombres de recurso de paquete incluyen:

- [Tipos de mensaje (msg)](http://wiki.ros.org/msg)
- [Tipos de servicio (srv)](http://wiki.ros.org/srv)
- [Tipos de nodo](http://wiki.ros.org/Nodes)

Los nombres de recurso de paquete son muy similares a las rutas de archivo, excepto que son mucho más cortas. Esto es debido a la habilidad de ROS de localizar los paquetes en el disco y hacer suposiciones adicionales sobre su contenido. Por ejemplo, las descripciones de mensaje siempre se almacenan en el subdirectorio `msg` y tienen la extensión `.msg`, así que `std_msgs/String` es accesible a `path/to/std_msgs/msg/String.msg`. Similarmente, el tipo de nodo `foo/bar` es equivalente a buscar un archivo llamado `bar` en el paquete `foo` con permisos de ejecución.

#####Nombres válidos

Los paquetes de nombre de recurso tiene unas normas estrictas de nombrado de archivos, como normalmente son usados en el código auto generado. Por ésta razón, el paquete ROS no puede tener carácteres especiales distintos de una barrabaja, y deben empezar con un carácter alfabético. Un nombre válido tiene las siguientes características:

1. El primer carácter es un caracter alfa ([a-z|A-Z]), tilde (~) o barra inclinada (/).
2. Los carácteres subsecuentes pueden ser alfanuméricos ([0-9|a-z|A-Z]), barrabaja (_), o barra inclinada (/).
3. Al menos hay una barra inclinada ('/').