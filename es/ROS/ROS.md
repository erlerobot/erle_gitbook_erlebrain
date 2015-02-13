El Robot Operative System o Sistema Operativo de Robot (ROS), dando soporte a los robots del mundo.
=========

* [Concepts](ROS-concepts.md)
* [Technical Overview](rostechnicaloverview.md)
* [Tutorials](rostutorials.md)
	* [Installing and Configuring Your ROS Environment](tutorials/rosinstall.md)
	* [Navigating the ROS Filesystem](tutorials/rosnavigating.md)
	* [Creating a ROS Package](tutorials/creating_a_ros_package.md)
	* [Building a ROS Package](tutorials/building_a_ros_package.md)
	* [Understanding ROS Nodes](tutorials/understanding_ros_nodes.md)
	* [Understanding ROS Topics](tutorials/understanding_ros_topics.md)

![](../img/ros/rosorg-nb.png)

Qué es ROS?
-----
El [Sistema Operativo de Robots](http://www.ros.org/) (ROS) es un **software libre**, meta-sistema operativo** para tu robot, mantenido por [Open Source Robotics Foundation](http://www.osrfoundation.org/) (OSRF). Proporciona los servicios que esperarías de un sistema operativo, incluyendo la abstracción de hardware, el control de bajo nivel de dispositivos, implementación de la funcionalidad usada comúnmente, paso de mensajes entre procesos, y control de paquetes. También proporciona herramientas y librerías para obtener, construir, escribir y correr código sobre múltiples ordenadores. En algunos aspectos, ROS es similar en los 'framework de desarrollo del robot', como en Player, YARP, Orocos, CARMEN Orca, MOOS y Microsoft Robotics Studio.

El gráfico del tiempo de ejecución de ROS es un  *red de procesos peer-to peer* que está débilmente acoplado usando la infraestructura de comunicación ROS. ROS implementa diferentes estilos de comunicación, incluyendo el estilo síncrono del estilo RPC de comunicación sobre los servicios, el streaming asíncrono de datos sobre diferentes tópicos, y el almacenamiento de datos en un *Servidor de parámetro*.

**ROS no es un framework de desarrollo a tiempo real**, pero es posible integrar ROS con código en tiempo real. ROS también tiene una integración con Orocos Real-time Toolkit.

Objetivos
-----
El objetivo principal de ROS es **dar soporte al reuso del código en la investigación y desarrollo de la robótica**. ROS es un framework de desarrollo de procesos repartido, (ó nodos) que permiten ejecutables para ser individualmente diseñados y débilmente acoplados al tiempo de ejecución. Estos procesos pueden ser agrupados en paquetes o montones, que pueden ser compartidos y distribuidos fácilmente. ROS también soporta un sistema federado de repositorios de código que permiten la colaboración para también ser distribuidos. Este diseño, desde el nivel del sistema de archivos al nivel de comunidad, permiten unas decisiones importantes sobre el desarrollo y la implementación, pero todo puede ser traído con las herramientas de la infraestructura ROS.

En soporte a este primer objetivo de compartir la calaboración, hay también otros objetivos del framework de desarrollo de ROS:

- **Ligero**: ROS está diseñado para ser lo más ligero posible -- no te quebraremos la mente() -- ése código que se ha escrito para ROS, puede usarse para otros framework de desarrollo del software en otros robots. A corde con ésto, es que ROS es fácil a la hora de integrarlo con OpenRAVE, Orocos y Player.
- Librerías **ROS-agnostic**: El modelo de estilo de desarrollo preferido es el escribir las librerías del agnóstico de ROS en interfaces funcionales bien claras.
- **Independencia de lenguaje**: El framework de desarrollo de ROS es fácil de implementar en cualquier lenguaje de programación moderno. Ya hemos implementado nuevos lenguajes de programación en *Python*, *C++*, y *Lisp*, y tenemos *librerías experimentales en Java y Lua*.
- **Testeo fácil**: ROS contiene una unidad de integración de testeo incorporada llamada rostest que hace más fácil el abrir y derribar los testeos de accesorios.
- **Escalado**: ROS es apropiado para los grandes sistemas de tiempo de ejecución y para largos procesos de desarrollo.

Sistemas Operativos
-------
Normalmente ROS sólo trabaja en **plataformas basadas en Unix**. En el software de ROS, los sistemas operativos más utilizados para pruebas son *Ubuntu* y *Mac OS X*.

Distribuciones
----------
Las siguientes distribuciones han sido testeadas en el robot [Erle-Brain](http://erlerobot.com).

----

**Predeterminadamente, las imágenes proporcionadas por Erle-Brain vienen con ROS, preinstaladas y funcionando completamente**.

----

| Distro | Fecha de lanzamiento | Poster | Instituciones |
|--------|--------------|--------|-------------|
| [ROS Hydro Medusa](http://wiki.ros.org/hydro) | September 4th, 2013 | ![medusa](../img/ros/hydro.png) | [Instalación](http://wiki.ros.org/hydro/Installation/UbuntuARM) |
| [ROS Groovy Galapagos](http://wiki.ros.org/groovy) | December 31, 2012 | ![medusa](../img/ros/galapagos.jpg) | [Instalación](http://wiki.ros.org/groovy/Installation/UbuntuARM) |

Licencia
--------
Algunos de estos materiales han sido obtenidos de [ROS wiki](http://wiki.ros.org/). Excepto si indica lo contrario, la wiki de ROS está licenciada bajo Creative Commons Attribution 3.0.
