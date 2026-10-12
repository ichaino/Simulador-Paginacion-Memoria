# Simulador de Paginación de Memoria

### Tarea 3 - Sistemas Operativos

Proyecto académico desarrollado en C que simula el funcionamiento de la **paginación de memoria** en un sistema operativo. Permite observar la administración de memoria física (RAM), memoria virtual y área de intercambio (*swap*), junto con la creación y finalización de procesos, los accesos a direcciones virtuales y los fallos de página (*page faults*).

La simulación se realiza a nivel lógico mediante estructuras de datos que representan procesos, páginas, marcos de memoria y espacios de intercambio, sin almacenar contenido real en memoria.

## Contenido

* `tarea.c`: código fuente del simulador.
* `README.md`: documentación del proyecto.

## Herramientas utilizadas

* Lenguaje C
* GCC
* Estándar C11
* Sistemas UNIX/Linux
* Algoritmo de reemplazo FIFO

## Compilación

En una terminal Linux/UNIX, ejecutar:

```bash
gcc -std=c11 -O2 -o sim_paginacion_fifo tarea.c
```

## Ejecución

Para iniciar el simulador:

```bash
./sim_paginacion_fifo
```

El programa solicitará los siguientes parámetros:

1. Tamaño de la memoria física en MB.
2. Tamaño de página en KB.
3. Tamaño mínimo de los procesos en KB.
4. Tamaño máximo de los procesos en KB.

### Ejemplo de entrada

```text
10
4
500
1500
```

## Funcionamiento de la simulación

### 1. Creación de procesos

Cada 2 segundos se genera un proceso de tamaño aleatorio. Se calcula la cantidad de páginas necesarias y se intenta asignar memoria RAM. Si no hay espacio disponible, las páginas se asignan al área de intercambio (*swap*), siempre que exista capacidad.

### 2. Acceso a direcciones virtuales

A partir del segundo 30, se generan accesos aleatorios a direcciones virtuales cada 5 segundos. Se identifica la página correspondiente y se comprueba si está cargada en RAM.

Si la página no se encuentra en memoria física, se produce un **fallo de página (*page fault*)**. Cuando la RAM está llena, se aplica la política de reemplazo FIFO.

### 3. Finalización de procesos

Cada 5 segundos desde el segundo 30, se selecciona un proceso aleatorio para finalizarlo y liberar las páginas que ocupa en RAM y en el área de intercambio.

## Política de reemplazo FIFO

El simulador utiliza **FIFO (*First In, First Out*)**, una política que reemplaza la página que lleva más tiempo cargada en memoria física.

Cuando ocurre un fallo de página y la RAM está llena:

1. Se identifica la página más antigua en la cola FIFO.
2. La página seleccionada se traslada al área de intercambio (*swap*).
3. La nueva página ocupa el marco de memoria liberado.

## Condiciones de término

La simulación finaliza cuando no existe capacidad disponible para continuar incorporando páginas: no quedan marcos de RAM libres y tampoco hay espacios disponibles en el área de intercambio.

## Estructuras principales

* **`Process`:** representa un proceso, su identificador, tamaño y páginas asociadas.
* **`Page`:** representa una página y su ubicación en RAM o swap.
* **`Frame`:** representa un marco de memoria física.
* **`SwapSlot`:** representa una posición disponible en el área de intercambio.
* **Cola FIFO:** mantiene el orden de carga de las páginas para gestionar el reemplazo.
