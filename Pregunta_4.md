# _Rust_

Una de las características centrales de _Rust_ es el **ownership**. Todos los programas tienen que manejar la manera en la que ellos usan la memoria de la computadora mientras están corriendo. Algunos lenguajes tienen un garbage collector que constantemente busca memoria que ya no se esté usando mientras el programa está corriendo; en otros lenguajes el programador debe de forma explícita alocar y liberar la memoria. _Rust_a usa un tercer enfoque: la memoria es manejadad a través de un sistema de ownership con un conjunto de reglas que el compilador verifica en tiempo de compilación.

**Reglas del Ownership:**

* Cada value en _Rust_ tiene una variable que es llamada **owner**
* Solo puede haber un **owner** a la vez
* Cuando el **owner** se sale del scope el value será soltado.

## **Stack**, **Heap** y _Rust_

En muchos lenguajes de programación, el programador no tiene que pensar sobre el __stack__ y el __heap__ muy a menudo. Pero en lenguajes de programación como _Rust_, donde si un valor está en el stack o en el heap  tiene más efecto en como el lenguaje se comporta y por qué tienes que tomar ciertas deciciones, cobra más protagonismo.

Tanto el __stack__ como el __heap__ son partes de la memoria que hay disponible para que tu código pueda usar en tiempo de ejecución(runtime), pero están estructurados de diferentes formas. El stack guarda valores en el orden em que le llegan y remueve estos valores en el orden opuesto, siguiendo last in, first out(LIFO). Adicionar data al stack es llamado __pushing__ y remover data del stack __popping__. Todos los datos guardados en el stack tienen un tamaño prefijado conocido. Los datos con un tamaño desconocido en tiempo de compilación o con un tamaño que podría cambiar deben ser guardados en el heap. El heap es menos organizado: cuando tu pones datos en el heap, estás requiriendo una cierta cantidad de espacio. El sistema operativo busca un espacio vacío en el heap que es lo suficientemente grande, lo marca como que va a estar en uso, y retorna un __pointer__ , el cual es la dirección en memoria de esa ubicación. Este proceso es llamado __allocating on the heap__ y algunas veces abreviado como solo __allocating__.

Hacer pushing a la stack es más rápido que allocating en el heap debido que el sistema operativo nunca tiene que buscar un lugar para almacenar la nueva data; esa localización siempre está siempre en el tope de la pila(stack). Comparativamente allocating space en el heap requiere más trabajo, debido a que el sistema operativo debe buscar primero un espacio en memoria lo suficientemente grande para mantener la data y luego llevar a cabo un proceso para prepararse para la nueva allocation. Acceder a los datos en el heap es mas lentoo que acceder a los datos en la pila(stack) debido a que tienes que seguir un puntero(pointer) para llegar a ellos.

Hacer un seguimiento de que que partes del código están usando que datos en el heap, minimizando la cantidad de datos duplicados en el heap, y limpiando datos no usados en el heap para que no te quedes sin espacio son todos los problemas que el __ownership__ direcciona.
