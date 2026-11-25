# Algoritmos de Clustering: Investigación y Desarrollo

**Autora:** Anna Costa

---

## 📚 Contenido del Repositorio
- [Introducción](#introducción)
- [¿Qué es el Aprendizaje No Supervisado?](#qué-es-el-aprendizaje-no-supervisado)
- [Algoritmo de Clustering: Definición](#algoritmo-de-clustering-definición)
- [Clustering Basado en Centroides vs. Jerárquico](#clustering-basado-en-centroides-vs-jerárquico)
- [Clustering Basado en Densidad (DBSCAN)](#clustering-basado-en-densidad-dbscan)
- [Importancia de los Hiperparámetros y Evaluación](#importancia-de-los-hiperparámetros-y-evaluación)
- [Algoritmos Descriptivos](#algoritmos-descriptivos)
  - [K-Means](#k-means)
  - [Clustering Jerárquico (Aglomerativo)](#clustering-jerárquico-aglomerativo)
  - [DBSCAN](#dbscan)
  - [Gaussian Mixture Models (GMM)](#gaussian-mixture-models-gmm)
- [Tabla Comparativa Resumida](#tabla-comparativa-resumida)
- [Referencias y Lectura Adicional](#referencias-y-lectura-adicional)
- [Conclusión](#conclusión)

---

## Introducción

El **clustering** o agrupamiento es una técnica fundamental en el **aprendizaje no supervisado**, donde los datos se organizan automáticamente en grupos naturales sin usar etiquetas predefinidas. Es especialmente útil cuando necesitamos descubrir estructuras ocultas en datos no etiquetados.

---

## ¿Qué es el Aprendizaje No Supervisado?

### Definición
El **aprendizaje no supervisado** es un paradigma de machine learning donde el modelo se entrena con datos que **no tienen etiquetas**. A diferencia del aprendizaje supervisado, no hay variables objetivo predefinidas; el algoritmo debe descubrir patrones por sí mismo.

### Diferencias con el Aprendizaje Supervisado

| Característica | Aprendizaje No Supervisado | Aprendizaje Supervisado |
|----------------|----------------------------|------------------------|
| **Datos de entrada** | Sin etiquetas (no rotulados) | Con etiquetas (rotulados) |
| **Objetivo** | Descubrir estructura oculta | Predecir o clasificar |
| **Ejemplos** | Clustering, reducción de dimensionalidad | Clasificación, regresión |
| **Evaluación** | Difícil: no hay una "respuesta correcta" con la que comparar | Directa: se compara la predicción con la etiqueta real |

### Propósito del Aprendizaje No Supervisado
- Descubrir grupos naturales en los datos
- Detectar anomalías o outliers
- Reducir la dimensionalidad
- Pre-procesamiento para tareas supervisadas

---

## Algoritmo de Clustering: Definición

Un **algoritmo de clustering** es una técnica que agrupa objetos similares dentro del mismo cluster y separa objetos distintos en clusters diferentes. El principio fundamental es:
- **Intra-cluster**: Los elementos dentro de un grupo deben ser similares
- **Inter-cluster**: Los grupos deben ser muy diferentes entre sí

---

## Clustering Basado en Centroides vs. Jerárquico

### 📌 Clustering Basado en Centroides (Particional)

**Concepto:** Divide los datos en K clusters predefinidos, donde cada cluster se representa con un **centroide** (punto central calculado a partir de los puntos del cluster).

**Algoritmo representativo:** **K-Means**

#### Características:
- Requiere especificar el número de clusters (`k`) antes de entrenar
- Algoritmo iterativo que minimiza la varianza intra-cluster
- Cada punto se asigna al centroide más cercano
- Tiende a encontrar óptimos locales

**Proceso:**
1. Inicializar `k` centroides (aleatoriamente o con K-Means++)
2. Asignar cada punto al centroide más cercano
3. Recalcular los centroides como el promedio de puntos del cluster
4. Repetir pasos 2-3 hasta convergencia

**Ventajas:**
- ✅ Rápido y eficiente para grandes conjuntos de datos
- ✅ Fácil de implementar e interpretar
- ✅ Escala bien con el número de muestras (con Mini-Batch K-Means, incluso a millones de puntos)

**Desventajas:**
- ❌ Debe especificarse el número de clusters
- ❌ Sensible a valores atípicos (outliers), porque desplazan la media
- ❌ Asume que los clusters son convexos y de tamaño similar
- ❌ Puede quedar atrapado en óptimos locales
- ❌ En alta dimensionalidad las distancias euclídeas pierden capacidad de discriminar

#### Casos de uso comunes:
- Segmentación de clientes en marketing
- Análisis de comportamiento de usuarios
- Agrupamiento de documentos similares

---

### 📌 Clustering Jerárquico

**Concepto:** Construye una jerarquía de clusters, representada como un **dendrograma**. Puede ser aglomerativo (abajo-arriba) o divisivo (arriba-abajo).

#### Características:
- No requiere especificar el número de clusters de antemano
- Permite visualizar la estructura jerárquica mediante dendrogramas
- Útil cuando no se conoce el número óptimo de clusters

**Proceso (Aglomerativo):**
1. Cada punto es inicialmente su propio cluster
2. Fusionar iterativamente los clusters más cercanos
3. Continuar hasta formar un solo cluster o alcanzar criterio deseado

**Ventajas:**
- ✅ No necesita especificar el número de clusters de antemano
- ✅ Proporciona estructura jerárquica (dendrogramas)
- ✅ Admite cualquier medida de distancia o similitud

**Desventajas:**
- ❌ Computacionalmente costoso: O(n³) en tiempo y O(n²) en memoria en la implementación estándar
- ❌ Difícil de aplicar a grandes conjuntos de datos
- ❌ Las fusiones son irreversibles: un error en un paso temprano se arrastra hasta el final
- ❌ Puede producir clusters de tamaños muy desiguales

#### Casos de uso comunes:
- Análisis exploratorio de datos
- Biología (clustering genético)
- Estudios taxonómicos y genealógicos

---

## Clustering Basado en Densidad (DBSCAN)

### 🔍 ¿Qué es DBSCAN?

**DBSCAN** (Density-Based Spatial Clustering of Applications with Noise) agrupa los datos basándose en regiones de alta densidad conectadas, separando regiones de baja densidad como ruido.

### 🆚 Diferencias con otros enfoques:

| Aspecto | Centroides | Jerárquico | DBSCAN |
|---------|------------|------------|---------|
| **Representación** | Centroide puntual | Dendrograma | Regiones de densidad |
| **Forma de clusters** | Convexos | Depende del linkage (single: alargados; Ward: compactos) | Arbitrarias (forma libre) |
| **Manejo de ruido** | Difícil | Limitado | Excelente (detecta outliers) |
| **Tamaño de clusters** | Similar | Variable | Variable |

### 📐 Parámetros de DBSCAN:
- **`eps` (ε):** Radio de vecindad
- **`min_samples`:** Número mínimo de puntos dentro del radio `eps` para que un punto sea **núcleo**

### ✅ Ventajas:
- ✅ No requiere especificar el número de clusters
- ✅ Detecta y maneja ruido/outliers naturalmente
- ✅ Encuentra clusters de formas complejas (no convexos)
- ✅ Eficiente en baja dimensionalidad gracias a índices espaciales (KD-tree, Ball-tree)

### ❌ Desventajas:
- ❌ Muy sensible a la elección de `eps` y `min_samples`
- ❌ Dificultad con clusters de densidades muy diferentes (un único `eps` no sirve para todos)
- ❌ Pierde eficacia en alta dimensionalidad (maldición de la dimensión)
- ❌ Los puntos frontera entre dos clusters pueden asignarse a uno u otro según el orden de procesamiento

### 💡 Casos de uso comunes:
- Detección de anomalías en transacciones
- Análisis de redes sociales (grupos conectados)
- Procesamiento de imágenes (segmentación)

---

## Importancia de los Hiperparámetros y Evaluación

### ⚠️ ¿Por qué es crucial?

Una elección incorrecta del número de clusters o de los hiperparámetros puede llevar a:
1. **Sobresegmentación:** Crear muchos clusters pequeños y sin sentido
2. **Infrasegmentación:** Fusionar grupos distintos en uno solo, perdiendo información valiosa
3. **Resultados erróneos:** Conclusiones inválidas sobre la estructura de los datos

### 📊 Métodos para Evaluar la Calidad del Agrupamiento

#### 1️⃣ **Método del Codo (Elbow Method)**

**Descripción:** Gráfica del error total dentro de los clusters (inercia o suma de distancias al cuadrado a cada centroide) frente al número de clusters (k). Se busca el "codo", el punto a partir del cual añadir más clusters apenas reduce la inercia.

```
Inercia
  ↑
  │\
  │ \
  │  \       ← Codo óptimo
  │   \
  │    \
  │     \_______→ k (número de clusters)
```

**Ventajas:**
- ✅ Visual e intuitivo
- ✅ Simple de implementar
- ✅ No requiere conocimiento externo del problema

**Limitaciones:**
- ❌ El punto del "codo" es subjetivo
- ❌ A veces la curva es poco pronunciada y no hay un codo claro

---

#### 2️⃣ **Coeficiente de Silueta (Silhouette Score)**

**Fórmula:**
```
s(i) = (b(i) - a(i)) / max(a(i), b(i))
```

Donde:
- `a(i)` = distancia promedio del punto `i` a los demás puntos de su mismo cluster
- `b(i)` = distancia promedio del punto `i` a los puntos del cluster vecino más cercano

El score global es la media de `s(i)` sobre todos los puntos y va de -1 a 1.

**Interpretación:**
- **Valores negativos:** El punto probablemente está asignado al cluster equivocado
- **Cerca de 0:** El punto está en la frontera entre dos clusters
- **Cerca de 1:** El punto está bien agrupado y lejos de otros clusters

**Ventajas:**
- ✅ No requiere etiquetas reales
- ✅ Cuantifica cuán "bien" asignado está cada punto
- ✅ Valida a la vez la cohesión interna y la separación entre clusters

**Limitaciones:**
- ❌ Favorece clusters convexos y compactos, por lo que penaliza a DBSCAN cuando encuentra formas arbitrarias
- ❌ En DBSCAN hay que excluir los puntos de ruido antes de calcularlo
- ❌ Costoso en grandes datasets: O(n²) distancias

---

#### 3️⃣ **Elección de Hiperparámetros en Otros Algoritmos**

| Algoritmo | Hiperparámetro | Cómo elegirlo |
|-----------|----------------|---------------|
| **Jerárquico** | Altura de corte del dendrograma | Cortar donde haya el mayor salto vertical entre fusiones |
| **DBSCAN** | `min_samples` | Regla práctica: al menos `d + 1` (d = nº de dimensiones); habitual `2·d` |
| **DBSCAN** | `eps` | Gráfico de k-distancias: ordenar la distancia de cada punto a su k-ésimo vecino (k = `min_samples`) y elegir el valor del codo |
| **GMM** | Nº de componentes | Criterios de información **BIC** o **AIC**: elegir el número de componentes con el valor más bajo |

---

#### 📋 Otros Métodos Adicionales:

| Método | Qué mide | Interpretación |
|--------|----------|----------------|
| **Calinski-Harabasz** | Relación entre la dispersión inter-cluster y la intra-cluster | Cuanto más alto, mejor |
| **Davies-Bouldin** | Media, para cada cluster, de su similitud con el cluster más parecido (dispersión interna frente a distancia entre centroides) | Cuanto más bajo, mejor (mínimo 0) |
| **Gap Statistic** | Compara la inercia obtenida con la de datos aleatorios de referencia | Elegir el k con mayor "gap" |

---

## Algoritmos Descriptivos

### K-Means

#### Mecanismo:
1. Inicializar `k` centroides (aleatoriamente o con K-Means++ para mejor convergencia)
2. Asignar cada punto al centroide más cercano
3. Recalcular centroides como media de puntos del cluster
4. Repetir hasta convergencia (las asignaciones dejan de cambiar)

#### Ventajas:
- ✅ Muy rápido: O(n·k·d) por iteración
- ✅ Fácil de implementar e interpretar
- ✅ Convergencia garantizada en un número finito de iteraciones (aunque sea a un óptimo local)

#### Desventajas:
- ❌ Debe especificarse `k` antes
- ❌ Sensible a outliers e inicialización (se mitiga con K-Means++ y varias ejecuciones, `n_init`)
- ❌ Asume clusters esféricos y de tamaño similar
- ❌ Requiere escalar las variables, ya que se basa en distancias euclídeas

#### Casos de uso:
- Segmentación de mercado
- Análisis de comportamiento de usuarios
- Compresión de imágenes (cuantización de color)

---

### Clustering Jerárquico (Aglomerativo)

#### Mecanismo:
1. Cada punto es su propio cluster inicial
2. Calcular distancia entre todos los pares de clusters
3. Fusionar los clusters más cercanos
4. Actualizar matriz de distancias y repetir
5. Construir dendrograma con todas las fusiones

**Criterios de fusión (linkage):**
- **Single:** Distancia mínima entre puntos de ambos clusters (tiende a formar cadenas)
- **Complete:** Distancia máxima entre puntos de ambos clusters (clusters compactos)
- **Average:** Promedio de todas las distancias entre pares de puntos
- **Ward:** Fusiona los clusters que menos aumentan la varianza intra-cluster (clusters más balanceados)

#### Ventajas:
- ✅ No necesita `k` de antemano (se elige del dendrograma)
- ✅ Dendrograma muestra estructura completa
- ✅ Determinista: no depende de una inicialización aleatoria, a diferencia de K-Means

#### Desventajas:
- ❌ Costoso: O(n³) en tiempo y O(n²) en memoria
- ❌ Difícil escalar a millones de puntos
- ❌ Irreversible: una mala fusión no se puede deshacer
- ❌ Sensible al criterio de linkage elegido

#### Casos de uso:
- Análisis exploratorio inicial
- Estudios biológicos y genéticos
- Genealogías y taxonomías

---

### DBSCAN

**DBSCAN** (Density-Based Spatial Clustering of Applications with Noise) clasifica cada punto en uno de tres tipos:
- **Núcleo:** tiene al menos `min_samples` puntos en su radio `eps`
- **Frontera:** no es núcleo, pero está dentro del radio `eps` de un punto núcleo
- **Ruido:** no es ni núcleo ni frontera

#### Mecanismo:
1. Elegir un punto no visitado
2. Si es núcleo, crear un cluster nuevo con él y sus vecinos en el radio `eps`
3. Para cada vecino que también sea núcleo, añadir sus vecinos al cluster (propagación de densidad)
4. Si no es núcleo y no está al alcance de ningún núcleo, marcarlo como ruido
5. Repetir hasta procesar todos los puntos

#### Ventajas:
- ✅ Detecta clusters de formas arbitrarias
- ✅ Maneja outliers naturalmente (los etiqueta como ruido, `-1` en scikit-learn)
- ✅ No necesita `k` predefinido
- ✅ O(n·log n) con índice espacial en baja dimensionalidad

#### Desventajas:
- ❌ Sensible a parámetros `eps` y `min_samples`
- ❌ Problema en alta dimensionalidad (maldición de la dimensión)
- ❌ Difícil con clusters de densidades muy diferentes
- ❌ En el peor caso (alta dimensionalidad o sin índice espacial) el coste sube a O(n²)

#### Casos de uso:
- Detección de anomalías y fraude
- Análisis de redes sociales (grupos conectados)
- Procesamiento de imágenes y segmentación
- Geolocalización (detección de hotspots)

---

### Gaussian Mixture Models (GMM)

#### Mecanismo:
**Modelo probabilístico que asume que los datos se generan a partir de una mezcla de varias distribuciones gaussianas.**

**Proceso (usando EM - Expectation Maximization):**
1. Inicializar parámetros (medias, covarianzas, pesos) de cada componente
2. **E-step:** Calcular probabilidad de que cada punto pertenezca a cada cluster
3. **M-step:** Actualizar medias, covarianzas y pesos usando las probabilidades ponderadas
4. Repetir hasta convergencia

**Densidad para un punto x con k componentes:**
```
p(x) = Σᵢ αᵢ * N(x | μᵢ, Σᵢ)
```
Donde:
- `αᵢ` = peso del componente i (suma a 1)
- `μᵢ` = media del componente i
- `Σᵢ` = matriz de covarianza del componente i

#### Ventajas:
- ✅ Modelo probabilístico completo con asignaciones suaves (soft assignments)
- ✅ Maneja clusters elípticos (con covarianza completa)
- ✅ Más flexible que K-Means (no solo esféricos)
- ✅ Proporciona incertidumbre (probabilidades de pertenencia)

#### Desventajas:
- ❌ Debe especificarse número de componentes (se puede elegir con BIC/AIC)
- ❌ Puede quedar atrapado en óptimos locales
- ❌ Costoso computacionalmente: O(n·k·d²) por iteración con covarianza completa
- ❌ Sensible a la inicialización y a la escala de las variables

#### Casos de uso:
- Análisis de datos financieros (modelado de riesgo)
- Procesamiento de voz (reconocimiento de hablante)
- Modelado de distribuciones multimodales
- Imágenes con variación de intensidad

---

## Tabla Comparativa Resumida

| Algoritmo | Tiempo | Necesita k | Maneja Ruido | Forma Cluster | Caso Principal |
|-----------|--------|------------|--------------|---------------|----------------|
| **K-Means** | O(n·k·d) por iteración ✅ | Sí | No | Esférica | Segmentación rápida |
| **Jerárquico** | O(n³) ❌ | No (se elige en el dendrograma) | Parcial | Depende del linkage | Exploratorio |
| **DBSCAN** | O(n·log n) con índice; O(n²) peor caso ⚠️ | No (pero sí `eps` y `min_samples`) | Sí ✅ | Arbitraria | Anomalías, hotspots |
| **GMM** | O(n·k·d²) por iteración ⚠️ | Sí | Parcial | Elíptica | Modelado probabilístico |

---

## Referencias y Lectura Adicional

- **Libro:** Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*. Springer.
- **Libro:** Hastie, T., Tibshirani, R. y Friedman, J. (2009). *The Elements of Statistical Learning* (2.ª ed.). Springer.
- **Documentación:** scikit-learn — [Clustering](https://scikit-learn.org/stable/modules/clustering.html)
- **Papers:**
  - Ester, M., Kriegel, H.-P., Sander, J. y Xu, X. (1996). *A Density-Based Algorithm for Discovering Clusters in Large Spatial Databases with Noise*. KDD-96. (DBSCAN original)
  - Arthur, D. y Vassilvitskii, S. (2007). *k-means++: The Advantages of Careful Seeding*. SODA.
  - Dempster, A. P., Laird, N. M. y Rubin, D. B. (1977). *Maximum Likelihood from Incomplete Data via the EM Algorithm*. Journal of the Royal Statistical Society, Series B.
  - Rousseeuw, P. J. (1987). *Silhouettes: A Graphical Aid to the Interpretation and Validation of Cluster Analysis*. Journal of Computational and Applied Mathematics.

---

## Conclusión

Los algoritmos de clustering son herramientas poderosas para descubrir estructuras ocultas en datos no etiquetados. Cada uno tiene sus ventajas y limitaciones:

- **K-Means:** Rápido y sencillo, ideal para segmentación
- **Jerárquico:** Exploratorio perfecto con dendrogramas
- **DBSCAN:** Excelente para detectar anomalías y clusters de formas complejas
- **GMM:** Modelado probabilístico avanzado con incertidumbre

La elección del algoritmo adecuado depende del problema específico, la estructura esperada de los datos y las restricciones computacionales disponibles.

---

*Documento elaborado como parte del proyecto de investigación sobre Algoritmos de Clustering.*
