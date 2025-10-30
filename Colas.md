# Clase: Colas en Java (Queues)
 Objetivo de la clase

- Al finalizar la lección, el estudiante será capaz de:

- Comprender el concepto de cola (queue) como estructura de datos.

- Implementar colas en Java usando diferentes enfoques.

- Aplicar colas en ejemplos reales como gestión de procesos o sistemas de atención.

## 1. Concepto Teórico

Una cola (Queue) es una estructura de datos lineal que sigue el principio FIFO (First In, First Out), es decir, el primero en entrar es el primero en salir.

### Analogía:
Imagina una fila en un banco: el primer cliente que llega es el primero que será atendido.

### Operaciones básicas


| Operación               | Descripción                                | Ejemplo                              |
| ----------------------- | ------------------------------------------ | ------------------------------------ |
| `enqueue()` o `offer()` | Inserta un elemento al final de la cola.   | Agregar cliente al final de la fila. |
| `dequeue()` o `poll()`  | Elimina el primer elemento de la cola.     | Atender al primer cliente.           |
| `peek()`                | Muestra el primer elemento sin eliminarlo. | Ver quién está primero.              |
| `isEmpty()`             | Verifica si la cola está vacía.            | No hay nadie en la fila.             |


## 2. Implementación en Java

En Java, las colas se pueden implementar de varias maneras.
El paquete más usado es java.util, que provee interfaces y clases para trabajar con colas.

### 2.1 Usando la interfaz Queue y la clase LinkedList
``` Java
import java.util.LinkedList;
import java.util.Queue;

public class EjemploCola {
    public static void main(String[] args) {
        Queue<String> cola = new LinkedList<>();

        // Encolar elementos
        cola.offer("Cliente 1");
        cola.offer("Cliente 2");
        cola.offer("Cliente 3");

        System.out.println("Cola actual: " + cola);

        // Consultar el primero sin eliminarlo
        System.out.println("Siguiente en la cola: " + cola.peek());

        // Desencolar (atender)
        System.out.println("Atendiendo: " + cola.poll());

        System.out.println("Cola después de atender: " + cola);
    }
}
```

 Salida esperada

 ``` Bash
Cola actual: [Cliente 1, Cliente 2, Cliente 3]
Siguiente en la cola: Cliente 1
Atendiendo: Cliente 1
Cola después de atender: [Cliente 2, Cliente 3]
```

### 2.2 Implementación manual de una Cola

A veces, en estructuras de datos, se pide crear tu propia clase Cola.
``` Java
public class Cola<T> {
    private java.util.LinkedList<T> elementos = new java.util.LinkedList<>();

    // Encolar
    public void encolar(T item) {
        elementos.addLast(item);
    }

    // Desencolar
    public T desencolar() {
        if (estaVacia()) {
            throw new IllegalStateException("La cola está vacía");
        }
        return elementos.removeFirst();
    }

    // Ver el primero
    public T frente() {
        if (estaVacia()) {
            throw new IllegalStateException("La cola está vacía");
        }
        return elementos.getFirst();
    }

    // Verificar si está vacía
    public boolean estaVacia() {
        return elementos.isEmpty();
    }

    @Override
    public String toString() {
        return elementos.toString();
    }
}
```

Y su uso:

``` Java
public class TestCola {
    public static void main(String[] args) {
        Cola<String> cola = new Cola<>();

        cola.encolar("A");
        cola.encolar("B");
        cola.encolar("C");

        System.out.println("Cola: " + cola);
        System.out.println("Frente: " + cola.frente());
        System.out.println("Desencolado: " + cola.desencolar());
        System.out.println("Cola actual: " + cola);
    }
}
```

## 3. Tipos de colas


| Tipo de Cola                          | Descripción                                     | Ejemplo en Java |
| ------------------------------------- | ----------------------------------------------- | --------------- |
| **Cola simple (FIFO)**                | El primero que entra es el primero que sale.    | `LinkedList`    |
| **Cola doble (Deque)**                | Permite insertar y eliminar por ambos extremos. | `ArrayDeque`    |
| **Cola de prioridad (PriorityQueue)** | Los elementos se ordenan según su prioridad.    | `PriorityQueue` |



### Ejemplo de PriorityQueue

```Java
import java.util.PriorityQueue;

public class EjemploColaPrioridad {
    public static void main(String[] args) {
        PriorityQueue<Integer> cola = new PriorityQueue<>();

        cola.offer(30);
        cola.offer(10);
        cola.offer(20);

        System.out.println("Cola ordenada por prioridad: " + cola);

        while (!cola.isEmpty()) {
            System.out.println("Atendiendo: " + cola.poll());
        }
    }
}
```

Salida esperada:

```
Cola ordenada por prioridad: [10, 30, 20]
Atendiendo: 10
Atendiendo: 20
Atendiendo: 30
```


## 4. Aplicaciones reales de las colas

| Aplicación                            | Descripción                                           |
| ------------------------------------- | ----------------------------------------------------- |
|**Sistema de impresión**          | Los documentos se imprimen en el orden en que llegan. |
|**Sistema de atención al cliente** | Las personas son atendidas según su llegada.          |
|**Procesos en CPU**                | El sistema operativo organiza procesos en colas.      |
|**Gestión de tareas o mensajes**   | Los mensajes se procesan en orden de llegada.         |



## 5. Ejercicio prácticos
### Enunciado

Simula el sistema de atención en una clínica.
Cada paciente tiene un nombre.

Se debe permitir:

* Registrar pacientes en la cola.
* Atender al primero en la cola.
* Mostrar los pacientes pendientes.