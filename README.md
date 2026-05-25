# Robustez y Seguridad en Sistemas de Visión Artificial frente a Ataques Adversarios

**David Ortega Lozano**

Trabajo de Fin de Grado (TFG)

Grado en Ingeniería Informática

---

## Descripción general

Este repositorio contiene el experimento práctico desarrollado para el Trabajo de Fin de Grado (TFG) centrado en el estudio de la robustez de modelos de visión por computador frente a ataques adversarios. El objetivo principal del proyecto es analizar cómo pequeñas perturbaciones imperceptibles para el ser humano pueden degradar significativamente el rendimiento de una red neuronal convolucional (CNN) entrenada para clasificación de imágenes.

El experimento utiliza el conjunto de datos German Traffic Sign Recognition Benchmark (GTSRB) y estudia el impacto de los ataques adversarios FGSM (Fast Gradient Sign Method) y PGD (Projected Gradient Descent), así como la efectividad del entrenamiento adversario como mecanismo de defensa.

---

## Objetivos del experimento

Los objetivos principales de este trabajo son:

- Entrenar un modelo CNN para clasificación de señales de tráfico.
- Evaluar la vulnerabilidad del modelo frente a ataques adversarios FGSM y PGD.
- Implementar entrenamiento adversario utilizando ambos métodos.
- Estudiar si el entrenamiento adversario mejora realmente la robustez del clasificador.
- Comparar la capacidad de generalización de la robustez aprendida.

---

## Conjunto de datos

El experimento utiliza el dataset:

- **German Traffic Sign Recognition Benchmark (GTSRB)**

Características principales:

- 43 clases de señales de tráfico.
- Aproximadamente 39.000 imágenes de entrenamiento.
- Aproximadamente 12.000 imágenes de prueba.
- Imágenes RGB redimensionadas a 64 x 64 píxeles.

El dataset se descarga automáticamente mediante `torchvision.datasets.GTSRB`.

---

## Arquitectura del modelo

El clasificador utilizado es una red neuronal convolucional (CNN) compuesta por:

- 3 capas convolucionales con activación ReLU.
- Capas MaxPooling para reducción espacial.
- 2 capas totalmente conectadas.
- Dropout del 30% para reducir sobreajuste.

La arquitectura fue diseñada buscando un equilibrio entre capacidad de representación y coste computacional, especialmente debido al elevado tiempo requerido por el entrenamiento adversario.

---

## Ataques adversarios implementados

### FGSM (Fast Gradient Sign Method)

Método de una sola iteración que genera perturbaciones siguiendo el signo del gradiente de la función de pérdida.

Características:

- Ataque rápido y computacionalmente eficiente.
- Alta capacidad de degradación para perturbaciones pequeñas.
- Menor capacidad de optimización que PGD.

---

### PGD (Projected Gradient Descent)

Método iterativo basado en múltiples actualizaciones adversarias proyectadas dentro de una bola L_∞.

Configuración empleada:

- 10 iteraciones (`steps = 10`).
- Tamaño del paso (`alpha = epsilon / steps`).

Características:

- Mayor agresividad y efectividad.
- Generación de perturbaciones más optimizadas.
- Mayor coste computacional.

---

## Entrenamiento adversario

Se implementa entrenamiento adversario utilizando tanto FGSM como PGD.

Configuración:

- 10 modelos adversarios entrenados.
- 5 modelos entrenados con FGSM.
- 5 modelos entrenados con PGD.
- Valores de \\(\epsilon\\):

```python
[0.01, 0.03, 0.05, 0.1, 0.2]
```

Durante el entrenamiento:

- Aproximadamente la mitad de los mini-lotes se sustituyen por ejemplos adversarios generados dinámicamente.
- La otra mitad permanece limpia.
- Los ejemplos adversarios se generan síncronamente durante el entrenamiento.
- Se aplica un periodo de calentamiento para perturbaciones elevadas.

---

## Estructura del notebook

El notebook contiene las siguientes secciones principales:

1. Preparación del entorno.
2. Carga y preprocesamiento del dataset.
3. Definición de la arquitectura CNN.
4. Entrenamiento del modelo limpio.
5. Implementación del ataque FGSM.
6. Implementación del ataque PGD.
7. Evaluación del modelo.
8. Generación de ejemplos adversarios.
9. Entrenamiento adversario.
10. Comparación de resultados y visualización.

---

## Requisitos

El experimento fue desarrollado utilizando:

- Python 3
- PyTorch
- torchvision
- NumPy
- Matplotlib
- PIL
- tqdm
- Google Colab
- GPU NVIDIA T4

### Instalación recomendada

```bash
pip install torch torchvision matplotlib numpy tqdm pillow
```

---

## Ejecución del notebook

El notebook está diseñado para ejecutarse secuencialmente.

### Pasos recomendados

1. Abrir el notebook en Google Colab.
2. Activar aceleración por GPU:
   - `Entorno de ejecución -> Cambiar tipo de entorno de ejecución -> GPU`
3. Ejecutar todas las celdas en orden.

El notebook:

- Descarga automáticamente el dataset GTSRB.
- Entrena el modelo base.
- Genera ataques adversarios.
- Evalúa la robustez.
- Entrena modelos adversariales.
- Genera gráficas comparativas.

---

## Resultados principales

Los resultados obtenidos muestran que:

- El modelo limpio alcanza aproximadamente un 95% de precisión sobre imágenes normales.
- Tanto FGSM como PGD degradan rápidamente la precisión del modelo.
- PGD resulta considerablemente más agresivo que FGSM.
- El entrenamiento adversario mejora la robustez general del clasificador.
- El entrenamiento adversario con PGD generaliza mejor frente a otros ataques.
- FGSM mejora principalmente la robustez frente a FGSM, pero mantiene vulnerabilidades frente a PGD.

---

## Visualizaciones generadas

El notebook incluye:

- Ejemplos visuales de ataques adversarios.
- Evolución de precisión frente a distintos valores de epsilon.
- Comparativas de robustez.
- Gráficas de entrenamiento.
- Comparaciones entre modelos entrenados adversarialmente.

---

## Limitaciones

Debido a restricciones de memoria y tiempo de ejecución:

- Se utilizó una CNN relativamente sencilla.
- Las imágenes fueron redimensionadas a 64 x 64.
- PGD se limitó a 10 iteraciones.
- Los ejemplos adversarios se generan dinámicamente en lugar de almacenarse.

---

## Licencia

Este proyecto tiene fines exclusivamente académicos y educativos.
