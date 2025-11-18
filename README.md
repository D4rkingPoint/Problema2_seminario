# Dinámicas de Opinión y Polarización en Redes Sociales durante una Crisis Nacional

## Autores: 

- Leonardo Ponce 202030531-5 (leonardo.ponde@usm.cl)
- Álvaro Pozo 202030535-8 (alvaro.pozo@usm.cl)

---

## Descripción del problema abordado

Este proyecto analiza la dinámica de opinión y la polarización en redes sociales (Twitter) en el contexto de una crisis nacional (basado en un dataset de incendios). El objetivo es comprender cómo se forman, agrupan y evolucionan las posturas ideológicas (positivas, negativas o neutras) dentro de la red de usuarios.

Para ello, el análisis combina tres pilares fundamentales:

1.  **Procesamiento de Lenguaje Natural (PLN):** Se preprocesa el contenido de los tuits y se utiliza un modelo de análisis de sentimientos (`pysentimiento`) para asignar una postura a cada publicación.
2.  **Análisis de Redes Sociales (SNA):** Se construye un grafo dirigido de interacciones (menciones y respuestas) usando `NetworkX`. Sobre este grafo, se detectan comunidades (algoritmo de Louvain) y se identifican nodos influyentes (PageRank) y nodos "puente" (Betweenness Centrality).
3.  **Modelado y Simulación de Opinión:** Se implementa un modelo de **Confianza Limitada (Bounded Confidence)** para simular cómo las opiniones de los nodos evolucionan en el tiempo. Se evalúan diferentes escenarios (variando la tolerancia) y se simulan intervenciones (censura y refuerzo de puentes) para medir la resiliencia de la red a la polarización.

El proyecto busca responder a preguntas como: ¿Qué tan polarizada está la conversación? ¿Qué comunidades actúan como puentes? ¿Tiende la red al consenso o a la fragmentación? y ¿Cómo cambia la dinámica si se interviene la red?

---

## Dependencias técnicas y librerías necesarias

El proyecto se desarrolla en **Python 3.9+** y requiere las siguientes librerías principales:

### Dependencias principales

* **pandas** y **openpyxl**: Para la carga y manipulación de datos desde archivos Excel (`.xlsx`).
* **numpy**: Para operaciones numéricas (utilizado por pandas).
* **nltk**: Para el preprocesamiento de texto (limpieza inicial).
* **pysentimiento**: Para el análisis de sentimientos de los tuits, basado en modelos Transformers.
* **torch**: Como motor (backend) de `pysentimiento`.
* **networkx**: Para la creación, manipulación y análisis del grafo de red.
* **community (python-louvain)**: Para la detección de comunidades (algoritmo de Louvain).
* **matplotlib** y **seaborn**: Para la visualización de datos, distribuciones y redes.

### Instalación


1.  **Instalar las librerías:**
    Puedes instalar todas las dependencias usando el siguiente comando:
    ```bash
    pip install pandas openpyxl numpy nltk pysentimiento torch networkx python-louvain matplotlib seaborn
    ```

---

## Instrucciones de ejecución

Este proyecto está diseñado para ser ejecutado de forma secuencial, idealmente en un entorno como **Jupyter Notebook** o un script de Python (`.py`) que se ejecute paso a paso.

1.  **Estructura del Proyecto:**
    Asegúrese de mantener la siguiente estructura de carpetas para que las rutas funcionen correctamente:
    ```
    nombre_proyecto/
    ├── data/
    │   └── datos_incendio.xlsx   <-- COLOQUE SU ARCHIVO DE DATOS AQUÍ
    ├── src/
    │   └── Opinion_y_polarizacion_rrss.ipynb         <-- SCRIPT O NOTEBOOK DE ANÁLISIS
    ├── results/                  <-- Carpeta de salida (se crea automáticamente)
    └── README.md
    ```

2.  **Ejecución del Script:**
    * Abra su script o Jupyter Notebook (ej. `Opinion_y_polarizacion_rrss.ipynb`).
    * Ejecute el código en orden. El análisis sigue un flujo lógico:

    * **Fase 1: Carga y Preprocesamiento:** Se leen los datos de `data/` y se limpian los tuits.
    * **Fase 2: Análisis de Sentimientos:** Se aplica `pysentimiento` para crear la columna `Sentiment`.
    * **Fase 3: Construcción y Análisis de Red Estática:** Se crea el grafo `G` con `NetworkX`, se calculan métricas (`pagerank`, `betweenness`), se detectan comunidades (`community_id`) y se guardan los resultados en `node_data`.
    * **Fase 4: Visualización Estática:** Se generan y guardan los primeros gráficos (como el boxplot de sentimientos y el mapa de comunidades) en la carpeta `results/`.
    * **Fase 5: Dinámica de Opinión (Simulación):** Se define y ejecuta el modelo *Bounded Confidence* (`simulate_bounded_confidence`). Se simulan diferentes escenarios de tolerancia e intervención.
    * **Fase 6: Visualización de Simulación:** Se generan y guardan los gráficos de evolución ("Antes y Después", mapas de polarización) en la carpeta `results/`.

3.  **Revisión de Resultados:**
    * Todos los gráficos (`.png`) y archivos de datos (`.xlsx`) generados durante la ejecución se guardarán automáticamente en la carpeta `results/` (ubicada en la raíz del proyecto, fuera de `src/`).
    * Preste atención a la salida en la consola, ya que le informará sobre el progreso, el número de comunidades encontradas y las rutas de guardado.

**Nota Importante:** La ejecución de la **Fase 3** (cálculo de *Betweenness Centrality*) y la **Fase 5** (simulaciones de N pasos) puede ser computacionalmente intensiva y tardar varios minutos, dependiendo del tamaño de su dataset y la potencia de su máquina.