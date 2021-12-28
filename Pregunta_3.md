# Enumere brevemente las propuestas más comunes en los lenguajes de programación C#, Python, Java, Go y JavaScript.

## Go

Primero que todo el recolector de basura de _Go_ es un recolector de basura **no generacional**, **concurrente**, **tricolor** de **marca y barrido**.

 _Go_ prefiere asignar memoria en el **stack**, por lo que la mayoría de las asignaciones de memoria terminarán allí. Esto significa que _Go_ tiene una _stack_ por **goroutine** y, cuando sea posible, _Go_ asignará variables a esta _stack_. El compilador de _Go_ intenta probar que no se necesita una variable fuera de la función realizando un análisis de escape para ver si un objeto "escapa" de la función. Si el compilador puede determinar la duración de una variable, se asignará a una _stack_. Sin embargo, si la vida útil de la variable no está clara, se asignará en el _heap_. Generalmente, si un programa _Go_ tiene un puntero a un objeto, ese objeto se almacena en el _heap_.

 La hipótesis generacional asume que los objetos de corta duración, como las variables temporales, se liberan con mayor frecuencia. Por lo tanto, un **Garbage Collector** generacional se centra en los objetos asignados recientemente. Sin embargo, mencionamos previamente, las optimizaciones del compilador permiten que el compilador de _Go_ asigne objetos con una vida útil conocida a _stack_. Esto significa que habrá menos objetos en el _heap_, por lo que se recolectarán menos objetos como basura. Esto significa que un **Garbage Collector** generacional no es necesario en _Go_. Entonces, _Go_ usa un recolector de basura no generacional. Concurrente significa que el **Garbage Collector** se ejecuta al mismo tiempo que los restantes hilos del programa. Por lo tanto, _Go_ utiliza un recolector de basura **concurrente**, **no generacional**, **marca y barrido** (mark and sweep) es el tipo de **Garbage Collector** y **tricolor** es el algoritmo utilizado para implementar esto.

## JavaScript

JavaScript asigna memoria automáticamente cuando se crean objetos y la libera cuando ya no se usan, e implementa el algoritmo de **marca y barrido** con el fin de "recolectar la basura".

