# GML_NN: Librería de Redes Neuronales en C

[![Language](https://img.shields.io/badge/Language-C-blue.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Environment](https://img.shields.io/badge/Environment-GNU/Linux-green.svg)](https://www.gnu.org/linux/)

Este repositorio contiene **GML_NN**, una librería implementada desde cero en **C estándar** para la creación, entrenamiento y uso de **Redes Neuronales Artificiales** de tipo **Perceptrón Multicapa (MLP)**. El proyecto fue desarrollado como Trabajo Fin de Grado (TFG) en la Escuela Técnica Superior de Ingenieros Informáticos de la Universidad Politécnica de Madrid.

El objetivo principal es ofrecer una herramienta en C que permita describir, entrenar mediante **retropropagación** (`backpropagation`) y utilizar redes neuronales para resolver problemas de clasificación, predicción y reconocimiento de patrones.

### 📄 Documentación Detallada

Este repositorio incluye:
* La **memoria completa del TFG**, donde se documenta en profundidad todo el proceso de diseño, la base teórica (desde el Perceptrón hasta la retropropagación) y las decisiones de implementación. Puedes consultarla en el repositorio oficial de la UPM: [Archivo Digital UPM](https://oa.upm.es/82476/). Para un análisis exhaustivo de los algoritmos, las **funciones de activación**, las **funciones de error**, los **optimizadores** y las **estructuras de datos** utilizadas, se recomienda consultar dicho documento.
* **Manuales de uso** detallados para la librería principal (`gml_nn.h`) y sus módulos auxiliares (`matrix.h`, `data_handler.h`) en la carpeta `manual/`. Estos manuales proporcionan una guía rápida sobre la interfaz de la librería y ejemplos de código.

---

## 1. Características Principales de GML_NN ✨

* **Creación Flexible de Redes:** Permite definir arquitecturas MLP con un número arbitrario de capas y neuronas por capa.
* **Entrenamiento Supervisado:** Implementa el algoritmo de **retropropagación** (`backpropagation`) para ajustar los pesos.
* **Variedad de Funciones de Activación:** Incluye funciones comunes como Sigmoide, Tanh, Sigmoide Optimizado (LeCun), ReLU, Leaky ReLU y Softplus.
* **Funciones de Error Configurables:** Implementa Error Cuadrático Medio (MSE) y su variante dividida por 2.
* **Optimizadores:** Soporta Descenso de Gradiente Estocástico (SGD), por lotes (Batch) y mini-lotes, con opción de añadir **Momento**.
* **Personalización:** Permite definir y usar **funciones de activación y de error personalizadas** por el usuario (calculando derivadas mediante diferencias finitas).
* **Gestión de Modelos:** Funcionalidad para **guardar** el estado de una red entrenada en un fichero `.nn` y **cargarla** posteriormente.
* **Modularidad:** Incluye módulos auxiliares para operaciones con **matrices (`matrix.h`)** y **manejo de datos (`data_handler.h`)** desde ficheros `.csv`.

---

## 2. Arquitectura y Detalles Técnicos 🛠️

La librería está estructurada en torno a dos `struct` principales:

1.  `layer`: Representa una capa de la red, almacenando sus **pesos (`W`)**, matrices auxiliares para el entrenamiento (`dW`, `vw`, etc.) y la salida (`out`). Los pesos y sesgos se gestionan conjuntamente en una única matriz por capa para optimizar operaciones.
2.  `neural_net`: Contiene la configuración global de la red (tasa de aprendizaje, tasa de decadencia para momento, semilla aleatoria, tamaño de lote, función de error) y un array de punteros a las `layer` que la componen.



El **entrenamiento** se basa en el cálculo del gradiente del error respecto a cada peso, propagando este error desde la capa de salida hacia la de entrada. La implementación modular permite manejar diferentes funciones de activación y error sin necesidad de reescribir todo el algoritmo de retropropagación. Se utiliza la **regla delta generalizada**, adaptada para trabajar con punteros a funciones para las derivadas.

---

## 3. Módulos Auxiliares

* **`matrix.h` / `matrix.c`**: Implementa una estructura `matrix` y operaciones fundamentales (creación, liberación, acceso, suma, resta, producto matricial, producto escalar) necesarias para el *feedforward* y *backpropagation*.
* **`data_handler.h` / `data_handler.c`**: Facilita la carga de datos desde ficheros `.csv`, separación en conjuntos de entrada/salida, división en entrenamiento/prueba, barajado (Fisher-Yates), normalización (MinMax) y transformación de clases enteras a formato binario (*one-hot encoding* adaptado).

---

## 4. Cómo Compilar y Ejecutar Ejemplos 🚀

La librería no tiene dependencias externas más allá del compilador C (gcc recomendado) y la librería matemática (`-lm`). Para compilar un ejemplo (ej. `xor.c`), puedes usar un `Makefile` similar al siguiente:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -g -I. # Añade -I. para incluir cabeceras locales
LDFLAGS = -lm
SRC = matrix.c data_handler.c gml_nn.c tu_ejemplo.c # Reemplaza tu_ejemplo.c
TARGET = tu_ejecutable # Reemplaza tu_ejecutable

$(TARGET): $(SRC)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

clean:
	rm -f $(TARGET)
```

1.  Guarda el código anterior como `Makefile`.
2.  Reemplaza `tu_ejemplo.c` y `tu_ejecutable`.
3.  Ejecuta `make` en la terminal para compilar.
4.  Ejecuta `./tu_ejecutable` para correr el programa.

El repositorio incluye ejemplos prácticos:
* Resolución de puertas lógicas (XOR).
* Funciones lógicas más complejas.
* Clasificación de puntos en 2D (incluyendo datos no lineales como espirales).
* Predicción con datos reales (Diabetes, VIH).
* Reconocimiento de dígitos MNIST (con resultados limitados).

---

## 5. Resultados y Limitaciones 📊

La librería demuestra ser capaz de resolver problemas de clasificación lineal y no lineal con éxito en varios conjuntos de datos, logrando tasas de acierto comparables a las de otras herramientas en los datasets de Diabetes y VIH.

Sin embargo, en el problema de reconocimiento de dígitos **MNIST**, el rendimiento fue limitado (en torno al 58% en el mejor caso). La memoria del TFG identifica posibles causas:
* Uso exclusivo de MLP (las CNNs suelen ser más adecuadas para imágenes).
* Conectividad total entre capas.
* Normalización MinMax podría no ser ideal para MNIST.
* Falta de optimizadores más avanzados (como Adam, RMSProp).
* Falta de funciones de error más específicas para clasificación multiclase (como Cross-Entropy).

---

## 6. Futuras Líneas de Trabajo 🔮

La memoria del TFG sugiere varias mejoras posibles:
* Refactorizar el uso de punteros a funciones para optimizar el rendimiento.
* Expandir el catálogo de funciones de activación y error (ej. Cross-Entropy).
* Implementar optimizadores avanzados (Adam, RMSProp).
* Permitir eliminar conexiones entre neuronas (redes no totalmente conectadas).
* Añadir soporte para capas convolucionales (CNNs) y pooling.
* Mejorar las herramientas de visualización.
