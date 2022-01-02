# Rust
Una de las características centrales de Rust es el **ownership**. Todos los programas tienen
que manejar la manera en la que ellos usan la memoria de la computadora mientras están corriendo. Algunos lenguajes tienen un garbage collector que constantemente busca memoria que ya no se esté usando mientras el programa está corriendo; en otros lenguajes el programador debe de forma explícita alocar y liberar la memoria. Rusta usa un tercer enfoque: la memoria es manejadad a través de un sistema de ownership con un conjunto de reglas que el compilador verifica en tiempo de compilación. 

**Reglas del Ownership:**
* Cada value en Rust tiene una variable que es llamada **owner**
* Solo puede haber un **owner** a la vez
* Cuando el **owner** se sale del scope el value será soltado.

