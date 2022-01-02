# Rust
Una de las características centrales de Rust es el **ownership**. Todos los programas tienen
que manejar la manera en la que ellos usan la memoria de la computadora mientras están corriendo. Algunos lenguajes tienen un garbage collector que constantemente busca memoria que ya no se esté usando mientras el programa está corriendo; en otros lenguajes el programador debe de forma explícita alocar y liberar la memoria. Rusta usa un tercer enfoque: la memoria es manejadad a través de un sistema de ownership con un conjunto de reglas que el compilador verifica en tiempo de compilación. 

**Reglas del Ownership:**
* Cada value en Rust tiene una variable que es llamada **owner**
* Solo puede haber un **owner** a la vez
* Cuando el **owner** se sale del scope el value será soltado.

### Stack, Heap y Rust :
En muchos lenguajes de programación, el programador no tiene que pensar sobre el __stack__ y el __heap__ muy a menudo. Pero en lenguajes de programación como Rust, donde si un valor está en el stack o en el heap  tiene más efecto en como el lenguaje se comporta y por qué tienes que tomar ciertas deciciones, cobra más protagonismo. 

Tanto el __stack__ como el __heap__ son partes de la memoria que hay disponible para que tu código pueda usar en tiempo de ejecución(runtime), pero están estructurados de diferentes formas. El stack guarda valores en el orden em que le llegan y remueve estos valores en el orden opuesto, siguiendo last in, first out(LIFO). Adicionar data al stack es llamado __pushing__ y remover data del stack __popping__. Todos los datos guardados en el stack tienen un tamaño prefijado conocido. Los datos con un tamaño desconocido en tiempo de compilación o con un tamaño que podría cambiar deben ser guardados en el heap. El heap es menos organizado: cuando tu pones datos en el heap, estás requiriendo una cierta cantidad de espacio. El sistema operativo busca un espacio vacío en el heap que es lo suficientemente grande, lo marca como que va a estar en uso, y retorna un __pointer__ , el cual es la dirección en memoria de esa ubicación. Este proceso es llamado __allocating on the heap__ y algunas veces abreviado como solo __allocating__.


