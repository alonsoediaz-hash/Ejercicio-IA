# Clasificación Automática de Daños Vehiculares mediante Deep Learning

## 1. Definición del Problema
En la industria automotriz y de seguros, la evaluación manual de daños en vehículos tras un siniestro es un proceso costoso, propenso a errores humanos y lento. Este proyecto aborda este problema mediante visión por computadora, respondiendo a la pregunta: *¿Es posible clasificar de forma automática y precisa si un vehículo ha sufrido daños estructurales a partir de una fotografía?* Desarrollar una solución automatizada optimiza los tiempos de respuesta para las aseguradoras y mejora la transparencia del servicio para los usuarios.

## 2. Breve Descripción del Dataset y Fuente
El dataset utilizado contiene imágenes de vehículos divididas de forma equitativa en dos categorías: `00-damage` (autos dañados/chocados) y `01-whole` (autos sanos/completos). El volumen de datos se distribuye de la siguiente manera tras el análisis exploratorio (EDA):

| Conjunto | Autos Dañados (`00-damage`) | Autos Sanos (`01-whole`) | Total por Conjunto |
| :--- | :---: | :---: | :---: |
| **Entrenamiento (Train)** | 920 | 920 | **1.840** |
| **Validación (Val)** | 230 | 230 | **460** |
| **Total General** | **1.150** | **1.150** | **2.300** |

*Nota del EDA:* Se constató que las imágenes originales cuentan con resoluciones promedio de 267x190 píxeles, con dimensiones mínimas de 168x161 y máximas de 313x299. Esto justificó técnicamente el redimensionamiento a 256x256 y un recorte central de 224x224 para estandarizar la entrada de la red sin deformar los patrones estructurales del auto. El dataset se encuentra perfectamente balanceado (50% por clase), eliminando sesgos en el entrenamiento.

## 3. Justificación del Modelo Seleccionado
Se seleccionó la arquitectura **ResNet50** preentrenada con el dataset `IMAGENET1K_V1`. Las razones técnicas de su elección son:
* **Conexiones residuales (Skip Connections):** Permiten entrenar redes profundas evitando la degradación del gradiente.
* **Transfer Learning:** Aprovecha características visuales complejas (bordes, texturas, formas geométricas) ya aprendidas en millones de imágenes, acelerando la convergencia para nuestra tarea específica de detectar deformaciones en carrocerías.
* **Eficiencia:** Logra un equilibrio óptimo entre costo computacional y precisión en comparación con modelos más pesados.

## 4. Metodología Aplicada (Paso a Paso)
1. **Análisis Exploratorio de Datos (EDA):** Verificación del balance de clases, inspección visual de muestras de ambas categorías y análisis estadístico de resoluciones e intensidad de canales de color.
2. **Procesamiento y Aumento de Datos (Regularización):** Se aplicaron transformaciones de redimensionamiento y normalización de ImageNet. Para combatir el sobreajuste (overfitting), se implementó aumento de datos mediante rotaciones aleatorias (`15°`) y giros horizontales (`RandomHorizontalFlip`).
3. **Modificación de la Arquitectura:** Se congelaron los pesos de las primeras capas y se sustituyó la capa final (`model.fc`) por un bloque secuencial compuesto por `nn.Dropout(0.5)` y una capa lineal adaptada a nuestras 2 clases objetivo.
4. **Fine-Tuning Parcial:** Se desbloquearon los parámetros de la última etapa convolucional (`model.layer4`) y de la nueva capa lineal para ajustar los filtros específicos a texturas de choques y abolladuras.
5. **Optimización:** Uso del optimizador `AdamW` con una tasa de aprendizaje baja ($lr = 0.0001$) y penalización de pesos (`weight_decay = 0.01`). La función de pérdida empleada fue `CrossEntropyLoss`.
6. **Entrenamiento y Control:** Bucle de 30 épocas monitorizado con **Early Stopping** (paciencia de 6 épocas) basado en el rendimiento del conjunto de validación.

# Clasificación Automática de Daños Vehiculares mediante Deep Learning

## 1. Definición del Problema
En la industria automotriz y de seguros, la evaluación manual de daños en vehículos tras un siniestro es un proceso costoso, propenso a errores humanos y lento. Este proyecto aborda este problema mediante visión por computadora, respondiendo a la pregunta: *¿Es posible clasificar de forma automática y precisa si un vehículo ha sufrido daños estructurales a partir de una fotografía?* Desarrollar una solución automatizada optimiza los tiempos de respuesta para las aseguradoras y mejora la transparencia del servicio para los usuarios.

## 2. Breve Descripción del Dataset y Fuente
El dataset utilizado contiene imágenes de vehículos divididas de forma equitativa en dos categorías: `00-damage` (autos dañados/chocados) y `01-whole` (autos sanos/completos). El volumen de datos se distribuye de la siguiente manera tras el análisis exploratorio (EDA):

| Conjunto | Autos Dañados (`00-damage`) | Autos Sanos (`01-whole`) | Total por Conjunto |
| :--- | :---: | :---: | :---: |
| **Entrenamiento (Train)** | 920 | 920 | **1.840** |
| **Validación (Val)** | 230 | 230 | **460** |
| **Total General** | **1.150** | **1.150** | **2.300** |

*Nota del EDA:* Se constató que las imágenes originales cuentan con resoluciones promedio de 267x190 píxeles, con dimensiones mínimas de 168x161 y máximas de 313x299. Esto justificó técnicamente el redimensionamiento a 256x256 y un recorte central de 224x224 para estandarizar la entrada de la red sin deformar los patrones estructurales del auto. El dataset se encuentra perfectamente balanceado (50% por clase), eliminando sesgos en el entrenamiento.

## 3. Justificación del Modelo Seleccionado
Se seleccionó la arquitectura **ResNet50** preentrenada con el dataset `IMAGENET1K_V1`. Las razones técnicas de su elección son:
* **Conexiones residuales (Skip Connections):** Permiten entrenar redes profundas evitando la degradación del gradiente.
* **Transfer Learning:** Aprovecha características visuales complejas (bordes, texturas, formas geométricas) ya aprendidas en millones de imágenes, acelerando la convergencia para nuestra tarea específica de detectar deformaciones en carrocerías.
* **Eficiencia:** Logra un equilibrio óptimo entre costo computacional y precisión en comparación con modelos más pesados.

## 4. Metodología Aplicada (Paso a Paso)
1. **Análisis Exploratorio de Datos (EDA):** Verificación del balance de clases, inspección visual de muestras de ambas categorías y análisis estadístico de resoluciones e intensidad de canales de color.
2. **Procesamiento y Aumento de Datos (Regularización):** Se aplicaron transformaciones de redimensionamiento y normalización de ImageNet. Para combatir el sobreajuste (overfitting), se implementó aumento de datos mediante rotaciones aleatorias (`15°`) y giros horizontales (`RandomHorizontalFlip`).
3. **Modificación de la Arquitectura:** Se congelaron los pesos de las primeras capas y se sustituyó la capa final (`model.fc`) por un bloque secuencial compuesto por `nn.Dropout(0.5)` y una capa lineal adaptada a nuestras 2 clases objetivo.
4. **Fine-Tuning Parcial:** Se desbloquearon los parámetros de la última etapa convolucional (`model.layer4`) y de la nueva capa lineal para ajustar los filtros específicos a texturas de choques y abolladuras.
5. **Optimización:** Uso del optimizador `AdamW` con una tasa de aprendizaje baja ($lr = 0.0001$) y penalización de pesos (`weight_decay = 0.01`). La función de pérdida empleada fue `CrossEntropyLoss`.
6. **Entrenamiento y Control:** Bucle de 30 épocas monitorizado con **Early Stopping** (paciencia de 6 épocas) basado en el rendimiento del conjunto de validación.

## 5. Resultados Obtenidos
El entrenamiento se detuvo de forma anticipada en la **época 15** al detectar un estancamiento en la pérdida de validación, previniendo el overfitting. El **mejor modelo guardado** se obtuvo en la **época 9** con las siguientes métricas en el set de validación:

* **Accuracy General:** 95%
* **Reporte de Clasificación:**
  * `00-damage`: Precisión: 0.94 | Recall: 0.96 | F1-score: 0.95
  * `01-whole`: Precisión: 0.96 | Recall: 0.94 | F1-score: 0.95

La matriz de confusión demostró un comportamiento sumamente equilibrado, cometiendo muy pocos falsos positivos y falsos negativos (apenas 9 errores en autos dañados y 14 en autos sanos de un total de 460 imágenes de prueba). Adicionalmente, al probar el modelo con una imagen externa del almacenamiento local (`autodañado.jpg`), este predijo correctamente la clase `00-damage` con una **confianza del 99.92%**.

## 6. Conclusiones
* El uso de Transfer Learning junto con un Fine-Tuning enfocado en `layer4` demostró ser una estrategia sumamente efectiva, logrando un 95% de exactitud con un tiempo de entrenamiento reducido.
* Las técnicas de regularización aplicadas (`Dropout` al 50%, `Weight Decay` en AdamW y el aumento de datos en el cargador de entrenamiento) funcionaron de manera óptima, ya que las curvas de aprendizaje mantuvieron una brecha controlada entre el entrenamiento y la validación, evitando que el modelo memorizara los datos.
* El modelo es altamente confiable para tareas de pre-evaluación en entornos reales, logrando diferenciar con alta precisión cambios sutiles en las estructuras y líneas originales de los vehículos chocado versus sanos.

## 6. Conclusiones
* El uso de Transfer Learning junto con un Fine-Tuning enfocado en `layer4` demostró ser una estrategia sumamente efectiva, logrando un 95% de exactitud con un tiempo de entrenamiento reducido.
* Las técnicas de regularización aplicadas (`Dropout` al 50%, `Weight Decay` en AdamW y el aumento de datos en el cargador de entrenamiento) funcionaron de manera óptima, ya que las curvas de aprendizaje mantuvieron una brecha controlada entre el entrenamiento y la validación, evitando que el modelo memorizara los datos.
* El modelo es altamente confiable para tareas de pre-evaluación en entornos reales, logrando diferenciar con alta precisión cambios sutiles en las estructuras y líneas originales de los vehículos chocado versus sanos.
