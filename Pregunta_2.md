# Estrategias 

### Traicing garbage collection 
Traicin garbage collection es una forma de manejo de memoria automatica que consiste en determinar que objetos deben ser desasignado. Rastreando que objetos son alcanzables por una cadena de referencias de ciertas objetos "root", y considerando el resto como "basura" y recolectando estos. Traicing garbage collection es la forma mas comun de recoleccion de basura, tanto es asi que "garbage collection" se refiere al "Traicing garbage collection", en lugar de a otros metodos como el recuento de referencias y hay una gran cantidad de algoritmos utilizados en la implementacion.

#### Accecibilidad de un objeto. 
Informalmente un objeto es accecible si este es referenciado por por al menos una variable en el programa, ya sea directamente o mediante referencias de otros objetos accecibles. Mas especificamente, se puede acceder a los objetos solo de dos formas: 
- Un distiguido conjunto de roots: objetos que son asumidos son alcanzables. Typicamente, eso include todos los objetos referenciados de cualquier parte en la "Pila de llamadas"(que son todas las variables y parametros locales en la funcion que se estan invocando actualmente) y caulquiera varaible global.
- cualquiera referencia de un objeto accecible es accecible, mas formalmente, accecibilidad es transitiva.

La definicion de accesibilidad de "basura" no es optima, en la medida en que la ultima vez que un programa usa un objeto podria ser mucho antes de que ese objeto caiga fuera del alcance del entorno. A veces se hace una distincion entre basura sintactica (aquellos objetos que el programa posiblemente no pueda alcanzar) y basura semantica (esos obejtos que el programa nunca volvera a usar)

El problema de identificar con presicion la basura semantica es parciamente decidible: un programa que asigna un objeto X, ejecuta un programa de entrada arbitrario P termina, requeriria un recolecor de basura semantica para resolver resolver el problema de la deteccion. Aunque los metodos heuristicos conservadores para las deteccion de basura semantica siguen siendo un area de investigacion activa, escencialmente todos los recolectores de basura practicos se enfocan en la basura sintactica.

Otra complicacion con este enfoque es que, en lenguajes con tipos de referencia y tipos por valor sin caja, el recolector de basura necesita de alguna manera porder distinguir que varaibles en la pila o campos en un objeto son valores regulares y cuales son referencias: en la memoria, un numero entero y una referencia pueden parecerse. El recolector de basura necesita saber si tratar el elemento como una referencia y seguirlo, o si es un valor primitivo. Una solucion comun es el uso de punteros etiquetados. 

#### Weak collections
Se peuden disenar estructuras de datos que tengan caractersiticas de seguimiento debiles. Por ejemplo las tables hash debil son utiles. Al igual que la tablas Hash normal, una tabla hash debil mantiene una asocioacion entre un pares de objetos, donde cada clave se entiende como una clave y un valor. Sin embargo la tabla hash no mantiene una referencia solida sobre estos objetos. Se produce en compertamiento especial cuando la clave, el valor o ambos se convierten en basura: la entrada de la tablas hash se elimina espontaneamente. Existen mas refinamientos, como tablas hash que solo tienen claves debiles(las referencias de valor son referencias ordinarias, fuertes) o solo valores debiles(las referencias de claves son fuertes)

Las tablas hash débiles son importantes para mantener asociaciones entre objetos, de modo que los objetos involucrados en la asociación aún pueden convertirse en basura si ya nada en el programa se refiere a ellos (aparte de la tabla hash asociada).

El uso de una tabla hash regular para tal propósito podría conducir a una "pérdida de memoria lógica": la acumulación de datos alcanzables que el programa no necesita y no usará.