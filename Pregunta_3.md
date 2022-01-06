# Enumere brevemente las propuestas más comunes en los lenguajes de programación _C#_, _Python_, _Java_, _Go_ y _JavaScript_

## _Go_

Primero que todo el recolector de basura de _Go_ es un recolector de basura **no generacional**, **concurrente**, **tricolor** de **marca y barrido**.

 _Go_ prefiere asignar memoria en el **stack**, por lo que la mayoría de las asignaciones de memoria terminarán allí. Esto significa que _Go_ tiene una **stack** por **goroutine** y, cuando sea posible, _Go_ asignará variables a esta **stack**. El compilador de _Go_ intenta probar que no se necesita una variable fuera de la función realizando un análisis de escape para ver si un objeto "escapa" de la función. Si el compilador puede determinar la duración de una variable, se asignará a una **stack**. Sin embargo, si la vida útil de la variable no está clara, se asignará en el **heap**. Generalmente, si un programa _Go_ tiene un puntero a un objeto, ese objeto se almacena en el **heap**.

 La hipótesis generacional asume que los objetos de corta duración, como las variables temporales, se liberan con mayor frecuencia. Por lo tanto, un **Garbage Collector** generacional se centra en los objetos asignados recientemente. Sin embargo, mencionamos previamente, las optimizaciones del compilador permiten que el compilador de _Go_ asigne objetos con una vida útil conocida al **stack**. Esto significa que habrá menos objetos en el **heap**, por lo que se recolectarán menos objetos como basura. Esto significa que un **Garbage Collector** generacional no es necesario en _Go_. Entonces, _Go_ usa un recolector de basura no generacional. Concurrente significa que el **Garbage Collector** se ejecuta al mismo tiempo que los restantes hilos del programa. Por lo tanto, _Go_ utiliza un recolector de basura **concurrente**, **no generacional**, **marca y barrido** (mark and sweep) es el tipo de **Garbage Collector** y **tricolor** es el algoritmo utilizado para implementar esto.

## _JavaScript_

La noción principal de los algoritmos de recolección se basan en la noción de ``referencia``.  Dentro del contexto de gestión de memoria, se dice que un objeto hace referencia a otro si el primero tiene acceso al segundo (ya sea de forma implícita o explícita). Por ejemplo, un objeto de _JavaScript_ guarda una referencia a su prototipo (``referencia implícita``) y a cualquiera de los valores de sus propiedades (``referencia explícita``).

Hay que mencionar que en este contexto la noción de "objeto" se refiere a algo más amplio que los objetos normales de _JavaScript_ y que también incluye al ámbito de la función (o ámbito de léxico global).

## Usando conteo por referencias

Éste es el algoritmo de recolección más simple. Este algoritmo reduce la definición de "un objejo ya no es necesario" a "un objeto ya no tiene ningún otro objeto que lo referencie". Un objeto es considerado recolectable si existen cero referencias hacia él.

Ejemplo 1:

```Javascript
var o = {
  a: {
    b:2
  }
};
// Se crean dos objetos. Uno es referenciado por el otro como
// una de sus propiedades.
// El otro es referenciado al ser asignado a la variable "o"
// Ninguno puede ser recolectado.


var o2 = o; // la variable "o2" es lo segundo en tener una
            // referencia al objeto.
o = 1;      // ahora el objeto solo tiene una referencia mediante
            // la variable "o2"

var oa = o2.a; // referencia a la propiedad "a" del objeto.
               // ahora el objeto posee dos referencias, una como propiedad
               // la otra como la variable "oa"

o2 = "yo"; // el objeto original "o" ahora ya no tiene
           // referencias a él. Podría ser recolectado.
           // Sin embargo lo que había en la propiedad "a" aún
           // esta refernciado en la variable "oa";
           // no puede ser recolectado aún

oa = null; // lo que estaba en la propiedad "a" del objeto original "o"
           // ahora ya no tiene ninguna referencia.Puede ser recolectado.
```

Limitación : ciclos

Existe una limitación cuando se trata de ciclos.

```Javascript
function f(){
  var o = {};
  var o2 = {};
  o.a = o2; // o referencía o2
  o2.a = o; // o2 referencía o

  return "azerty";
}

f();
// Dos objetos son creados y se referencían uno al otro creando un ciclo
// Estan atrapados en el scope de la funcion después de la llamada
// por lo que son inútiles fuera de la función y podrían ser recolectados.
// Sin embargo, el algoritmo de conteo de referencias considera que como
// ambos objetos estan referenciados (aunque sean a si mismos) ambos
// siguen en uso y por lo tanto no pueden ser recolectados.

```

 En el ejemplo anterior dos objetos son creados y se referencían entre ellos, por lo que se crea un ciclo. Ellos no saldrán del ámbito de la función después del llamado de la función, con lo que serían efectivamente "ya no son necesarios" y por lo cual ser liberados. Sin embargo, el algoritmo de conteo de referencias considera que ya que cada uno de los dos objetos está referenciado por lo menos una vez, ninguno podra ser recolectado. Este simple algoritmo tiene la limitación de que si un grupo de objetos se referencian a sí mismos (y forman un ciclo), nunca pasarán a "ya no ser necesitados" y no podrán ser recolectados nunca.

Internet Explorer 6 y 7 son conocidos por tener recolectores de basura por conteo de referencias para los objetos del ``DOM``. Los ciclos son un error común que pueden generar fugas de memoria (memory leaks).

Ejemplo 2:

```Javascript
var div;
window.onload = function(){
  div = document.getElementById("miDiv");
  div.referenciaCircular = div;
  div.muchosDatos = new Array(10000).join("*");
};
```

En el ejemplo anterior, el elemento del DOM "miDiv" posée una referencia circular a sí mismo en la propiedad "referenciaCircular". Si la propiedad no es explícitamente removida o asignada con el valor null,  un algoritmo de conteo de referencias siempre va a dejar por lo menos una referencia intacta y va a mantener el elemento del DOM activo en memoria incluso cuando es removido del DOM. Si el objeto del DOM contiene una gran cantidad de datos (ejemplificado en la propiedad "muchosDatos"), la memoria consumida por estos datos nunca será liberada.

## _Python_

El proceso de reservar y liberar memoria en _Python_ es automático. El programador no tiene que reservar o Liberar memoria a diferencia de lenguajes con mecanismos de reserva dinámica de memoria como _C_ y _C++_. _Python_ usa dos estrategias para la reserva de memoria:

* Reference Counting
* Garbage Collection

### Ejemplo 1:

```Python
x = 10
```

Cuando ```x = 10```  se ejecuta un objeto de tipo integer 10 es creado en memoria y su referencia es asignada a la variable x, esto es debido a que todo es un objeto en _Python_.

![Example1](./Images/python1.png)

Verifiquemos que esto sea verdadero.

```Python

x = 10
y = x
  
if id(x) == id(y):
    print("x and y refer to the same object")
```

Output:
```
x and y refer to the same object
```

En el ejemplo anterior, ``y = x`` crea otra variable de referencia y la cual hace referencia al mismo objeto debido a que _Python_ optimiza la utilización de memoria haciendo la asignación (allocation) de una mismo referencia de un objeto a una nueva variable si este objeto ya existia previamente con el mismo valor.

![Example2](./Images/python2.png)

Ahora cambiemos el valor de ``x``  y veamos que pasa.

```Python

x = 10
y = x
x += 1
  
if id(x) != id(y):
    print("x and y do not refer to the same object")
```

```Python
x and y do not refer to the same object
```

Entonces, ahora ``x`` referencia a un nuevo objeto ``x`` y la conexión entre ``x`` y ``10`` se pierde pero ``y`` sigue
referenciando a ``10``

![Example3](./Images/python3.png)

### Ejemplo 2:

```Python

# Literal 9 is an object
b = 9
 
# Reference count of object 9
# becomes 0.
b = 4
```

El valor literal 9 es un objeto. El __reference count__ del objeto 9 es incrementado a 1 en la línea 1. En la línea 2 este reference count llega  a 0 y es desreferenciado el objeto. Por tanto el garbage collector libera(deallocates) el objeto.

Formas de hacer un objeto ilegible para el ``garbage collection``:

```Python
x = []
x.append(l)
x.append(2)
 
# delete the list from memory or
# assigning object x to None(Null)
del x
# x = None
```

El reference count para la lista creada es ahora 2. Sin embargo, no puede ser alcanzado desde dentro de _Python_ y no puede ser posible usarlo de nuevo, esto es considerado basura(garbage). Esta lista por tanto nunca es liberada(freed).

### Garbage collection automática de ciclos:

Debido que a los ciclos de referencia(reference cycles) toman un costo computacional para descubrirse, el garbage collection debe ser una actividad programada. _Python_ programa el garbage collection basado en la cantidad límite(threshold) de objetos asignados(object allocations) y objetos liberados(object deallocations). Cuando el número de alloctions menos el número de dealloactions es mayor que el threshold, el garbage collector se ejecutará. Uno puede inspeccionar el threshold para nuevos objetos importando en módulo  ``recolección de basura`` y preguntando por el garbage collection thresholds:

```Python
# loading gc
import gc
 
# get the current collection
# thresholds as a tuple
print("Garbage collection thresholds:",
                    gc.get_threshold())
```

Output:
```
Garbage collection thresholds: (700, 10, 10) 
```

Aquí, el ``threshold`` por defecto en el sistema es 700. Esto significa que el número de allocations vs el número de deallocations tiene que ser mayor que 700 para que el garbage collector automático se ejecute. Por esto qualquier porción de tu código que libere gran cantidad de bloques de memoria es un buen candidato por correr el ``garbage collection`` manual.

### Garbage Collection Manual:

La invocación de forma manual del garbage collector durante la ejecución de un programa puede ser una buena idea en como el manejo de memoria puede ser comsumido por ciclos de referencia.

El garbage collection puede ser invocado de forma manual de la siguiente manera:

```Python

# Importing gc module
import gc
 
# Returns the number of
# objects it has collected
# and deallocated
collected = gc.collect()
 
# Prints Garbage collector
# as 0 object
print("Garbage collector: collected",
          "%d objects." % collected)
```

Output:
```
('Garbage collector: collected', '0 objects.')
```

Si pocos ciclos son creados, como el trabaja el colector de forma manual:

```Python

import gc
i = 0
 
# create a cycle and on each iteration x as a dictionary
# assigned to 1
def create_cycle():
    x = { }
    x[i+1] = x
    print x
 
# lists are cleared whenever a full collection or
# collection of the highest generation (2) is run
collected = gc.collect() # or gc.collect(2)
print "Garbage collector: collected %d objects." % (collected)
 
print "Creating cycles..."
for i in range(10):
    create_cycle()
 
collected = gc.collect()
 
print "Garbage collector: collected %d objects." % (collected)
```

Output:

```
Garbage collector: collected 0 objects.
Creating cycles...
{1: {...}}
{2: {...}}
{3: {...}}
{4: {...}}
{5: {...}}
{6: {...}}
{7: {...}}
{8: {...}}
{9: {...}}
{10: {...}}
Garbage collector: collected 10 objects.
```

Hay 2 formas de realizar el garbage collection de forma manual : basado en el tiempo(``time-based``) y basado en un evento(``event-based``)

1. **Time-based**: Es simple el garbage collector es llamado luego de un intervalo de tiempo prefijado.
2. **Even-based**: Se llama al garbage collector dada la ocurrencia de un evento. Por ejemplo, cuando un usuario cierra la aplicación o cuando la aplicación entra en un estado ``idle``.

## _C#_

La gestión automática de la memoria es posible gracias a Garbage Collection en .NET Framework. Cuando se crea un objeto de clase en tiempo de ejecución, se le asigna cierto espacio de memoria en la memoria del **heap**. Sin embargo, después de que todas las acciones relacionadas con el objeto se hayan completado en el programa, el espacio de memoria asignado es un desperdicio, ya que no se puede utilizar. En este caso, la **recolección de basura** es muy útil ya que libera automáticamente el espacio de memoria cuando ya no es necesario.
La **recolección de basura** siempre funcionará en Managed **Heap** e internamente tiene un motor que se conoce como el motor de optimización.

La **recolección de basura** se produce si se cumple al menos una de varias condiciones. Estas condiciones se dan de la siguiente manera:

* Si el sistema tiene poca memoria fisica, entonces las recoleccion de basura es necesaria.
* Si la memoria asignada a varios objetos en la memoria del **heap** excede un el espacio preestablecido, entonces ocurre la **recolección de basura**.
* Si se llama al método ```gc.Collect```, se produce la **recolección de basura**. Sin embargo, este método solo se llama en situaciones inusuales, ya que normalmente el recolector de basura se ejecuta automáticamente.

Existen 3 fases principales en la recoleccion de basura:
   **Marking Phase(Fase de marcado):** se crea una lista de todos los objetos activos durante la fase de marcado. Esto se hace siguiendo las referencias de todos los objetos raíz. Todos los objetos que no están en la lista de objetos activos se eliminan potencialmente de la memoria del **heap**.
   **Relocating Phase(Fase de reubicación):**: Las referencias de todos los objetos que estaban en la lista de todos los objetos vivos se actualizan en la fase de reubicación para que apunten a la nueva ubicación donde se reubicarán los objetos en la fase de compactación. 
   **Compacting Phase(Fase de compactación)**: El **heap** se compacta en la fase de compactación a medida que se libera el espacio ocupado por los objetos muertos y se mueven los objetos vivos restantes. Todos los objetos activos que quedan después de la recolección de elementos no utilizados se mueven hacia el extremo anterior de la memoria del **heap** en su orden original.

La memoria del **heap** está organizada en 3 generaciones para que varios objetos con diferentes tiempos de vida puedan manejarse apropiadamente durante la **recolección de basura**. Common Language Runtime (CLR) proporcionará la memoria a cada generación en función del tamaño del proyecto. Internamente, Optimization Engine llamará al método de medios de recolección para seleccionar qué objetos entrarán en la generación 1 o la generación 2.

- **Generación 0:** todos los objetos de corta duración, como las variables temporales, están contenidos en la generación 0 de la memoria del **heap**. Todos los objetos recién asignados también son objetos de generación 0 implícitamente a menos que sean objetos grandes. En general, la frecuencia de **recolección de basura** es la más alta en la generación 0. 
- **Generación 1:** si el espacio ocupado por algunos objetos de generación 0 que no se liberan en una ejecución de **recolección de basura**, estos objetos se mueven a la generación 1. Los objetos de esta generación son una especie de búfer entre los objetos de corta duración en la generación 0 y los objetos longevos de la generación 2. 
- **Generacion 2:** si el espacio ocupado por algunos objetos de la generación 1 que no se liberan en la siguiente ejecución de **recolección de basura**, estos objetos se mueven a la generación 2. Los objetos de la generación 2 tienen una vida larga, como los objetos estáticos, ya que permanecen en la memoria del **heap**. durante todo el proceso.

La **recolección de basura** de una generación implica la **recolección de basura** de todas sus generaciones más jóvenes. Esto significa que se liberan todos los objetos de esa generación en particular y sus generaciones más jóvenes. Por este motivo, la recolección de elementos no utilizados de la generación 2 se denomina recolección de elementos no utilizados completa, ya que se liberan todos los objetos de la memoria dinámica. Además, la memoria asignada a la Generación 2 será mayor que la memoria de la Generación 1 y, de manera similar, la memoria de la Generación 1 será mayor que la memoria de la Generación 0 (Generación 2> Generación 1> Generación 0).

## _Java_

Las aplicaciones _Java_ obtienen objetos en la memoria según sea necesario. La tarea de la **recolección de basura** (**Garbage Collection**) en la máquina virtual de _Java_ (JVM) es determinar automáticamente qué memoria ya no está siendo utilizada por una aplicación _Java_ y reciclar esta memoria para otros usos. Debido a que la memoria se recupera automáticamente en la JVM, los desarrolladores de aplicaciones _Java_ no tienen que tener que liberar explícitamente los objetos de memoria que no se utilizan.

La operación de **recolección de basura** se basa en la premisa de que la mayoría de los objetos utilizados en el código _Java_ son de corta duración y pueden recuperarse poco después de su creación. Debido a que los objetos sin referencia se eliminan automáticamente de la memoria del **heap**, la **recolección de basura** hace que _Java_ sea eficiente en la memoria.

Hay dos tipos de **recolección de basura** que generalmente ocurren en _Java_:

* Se dice que se ha producido una **recolección de basura** menor o incremental cuando se eliminan los objetos inalcanzables en la memoria del **heap** de generación joven.
* Se dice que se ha producido una **recolección de basura** mayor o completa cuando se eliminan los objetos que sobrevivieron a la **recolección de basura** menor y se copiaron en la memoria del **heap** de generación anterior o permanente. En comparación con la generación joven, la **recolección de basura** completa ocurre con menos frecuencia en la generación anterior.

Para liberar memoria, la JVM debe detener la ejecución de la aplicación durante al menos un breve período de tiempo y ejecutar **recolección de basura**. Este proceso se llama "stop-the-world". Esto significa que todos los subprocesos, excepto los subprocesos de , dejarán de ejecutarse hasta que se ejecuten los subprocesos de **recolección de basura** y el recolector de basura libere los objetos.

Las implementaciones modernas de **recolección de basura** intentan minimizar las detenciones en la ejecución del programa como consecuencia del "stop-the-world" haciendo todo el trabajo posible en segundo plano (es decir, utilizando un hilo separado), por ejemplo, marcando instancias de basura inaccesibles mientras el proceso de la aplicación continúa ejecutándose.

Las JVM modernas como **Azul Zing** utilizan el colector de compactación continuamente concurrente (C4), que elimina las pausas de recolección que limitan la escalabilidad en el caso de las JVM convencionales.

La JVM tradicional de Oracle HotSpot tiene cuatro formas de realizar la actividad de **recolección de basura**:

* Serie donde solo un hilo ejecutó el GC
* Paralelo donde se ejecutan varios subprocesos menores simultáneamente, cada uno de los cuales ejecuta una parte de la **recolección de basura**.
* Concurrent Mark Sweep (CMS), que es similar al paralelo, también permite la ejecución de algunos subprocesos de aplicación y reduce la frecuencia del "stop-the-world".
* G1, que también se ejecuta en paralelo y al mismo tiempo, pero funciona de manera diferente a CMS.
