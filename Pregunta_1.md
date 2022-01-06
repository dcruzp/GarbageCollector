# 1. ¿En qué consiste la recolección automática de basura? Contraste contra el manejo manual en _C++_

La recolección automática de basura (**Garbage Collection**) es una forma de manejo de memoria, o sea, es un proceso que se encargar de liberar memoria que ha sido reservada, pero a la cual ya no hay ninguna referencia. Cabe destacar que el problema de decidir si un espacio de memoria es "basura", o sea, que no se va a utilizar más, es indecidible, por lo que los **Garbage Collectors** implementan una solución con ciertas restricciones a este problema. Generalmente los lenguajes de programación de alto nivel tienen **Garbage Collector** por defecto, aunque existe la excepción de _C++_ por ejemplo, el cual no lo tiene por defecto, pero se puede incorporar usando una librería. El principio de los **Garbage Collector** es:

1. Determinar que objetos de un programa no van a ser accedidos en el futuro.

2. Liberar la memoria usada por estos objetos.

La memoria que se asigna en tiempo de compilación es almacenada en el
**stack**, mientras que la memoria que se asigna en tiempo de ejecución se almacena en el **heap**. _C++_ nos permite asignar la memoria de una variable en tiempo de ejecución. Esto se conoce como asignación de memoria dinámica. En otros lenguajes de programación como _Java_ y _Python_, el compilador gestiona automáticamente la memoria asignada a cada variable mediante el **Garbage Collector**, y al programador le es indistinto en donde se almacena esta variable, si en el **stack** o en el **heap**, pero este no es el caso en _C++_. En _C++_, si asignamos una variable dinámicamente, necesitamos liberar manualmente la memoria asignada a esta variable después de que no tengamos uso para ella. Podemos asignar y luego liberar memoria dinámicamente usando los operadores **new** y **delete** respectivamente como se muestra a continuación:

```cpp
#include <iostream>

using namespace std;
 
int main ()
{
    // Inicializar puntero NULL
    int* p = NULL;
 
    // reservar memoria para la variable
    p = new(nothrow) int;
    if (!p)
        cout << "allocation of memory failed\n";
    else {
        // Store value at allocated address
        *p = 29;
        cout << "Value of p: " << *p << endl;
    }
 
    // Reservar bloque de memoria usando new
    float *r = new float(75.25);
 
    cout << "Value of r: " << *r << endl;
 
    // Reservando bloque de memoria de tamaño n
    int n = 5;
    int *q = new(nothrow) int[n];
 
    if (!q)
        cout << "allocation of memory failed\n";
    else {
        for (int i = 0; i < n; i++)
            q[i] = i+1;
 
        cout << "Value store in block of memory: ";
        for (int i = 0; i < n; i++)
            cout << q[i] << " ";
    }
 
    // Liberando la memoria reservada para estas variables
    delete p;
    delete r;
 
    // Liberando el bloque de memoria reservado para este array
    delete[] q;
 
    return 0;
}
```

Cabe destacar que el **Garbage Collector** opera sobre el **heap**, no sobre el **stack**.

El **Garbage Collector** tiene sus ventajas y desventajas:

Ventajas:

1. Exime al programador de la responsabilidad de liberar manualmente la memoria (lo que puede causar lo que se conoce como _memory leak_ en lenguajes con manejo de memoria manual como _C/C++_), y le permite enfocarse exclusivamente en el programa, e ignorar por completo el manejo de memoria.

2. Permite reservar memoria para objetos en el **heap** de forma eficiente.

3. Se encarga de liberar la memoria reservada para ciertos objetos que ya no está siendo referenciada, permitiendo así el uso de esta memoria para futuras asignaciones.

4. Proporciona seguridad de memoria, impidiendo que los objetos puedan usar para sí mismos, espacios de memoria reservados para otros objetos.

Desventajas:

Como es lógico el **Garbage Collector** es un programa por lo que consume poder de cómputo a la hora de decidir cuando liberar memoria reservada. El costo a pagar por tener un programa que se encarga de liberar y reservar memoria de forma óptima y automática es lo que se conoce como **overhead** en _Ciencias de la Computación_, que no es más que cualquier combinación de tiempo de cálculo, memoria, ancho de banda u otros recursos en exceso o indirectos que se requieren para realizar una tarea específica, lo que puede llevar a menor o desigual rendimiento, aunque en realidad es menos de lo que se asume que es. Varias investigaciones aseguran que los **Garbage Collector** necesitan 5 veces la memoria para compensar esta deficiencia y para funcionar igual de rápido que el mismo programa corriendo en un lenguaje con manejo explícito de memoria con una declaración ideal de este procedimiento en el código. Pero hay otras personas aseguran que los programadores rara vez escriben código de manejo de memoria de forma eficiente. Por tanto las siguientes propiedades son deseables en un **Garbage Collector**:

1. Debe identificar la mayor parte de la basura.
2. Todo lo que identifique como basura debe ser basura.
3. Debería imponer un costo (**overhead**) de tiempo adicional bajo.
4. Durante la recolección de basura, el programa puede pausarse; estas pausas deben ser breves.

La mayoría de los **Garbage Colector** en la actualidad poseen todas estas propiedades.
