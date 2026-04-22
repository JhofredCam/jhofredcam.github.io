---
author: "Jhofred Jahat Camacho Gómez"
publishDate: 2026-04-21T12:00:00Z
title: "Predicción de incumplimiento de crédito con redes neuronales artificiales"
tags:
  - machine-learning
  - redes-neuronales
  - riesgo-crediticio
  - clasificacion-binaria
description: "Evaluación de una red neuronal artificial frente a una regresión logística para monitoreo de riesgo crediticio post-desembolso."
---

# Predicción de incumplimiento de crédito con redes neuronales artificiales

Este proyecto estudia un problema de clasificación binaria sobre `loan_status` para distinguir entre créditos con y sin incumplimiento. El objetivo fue desarrollar una Red Neuronal Artificial (RNA), compararla con una línea base de Regresión Logística y analizar su utilidad práctica en escenarios de seguimiento de cartera.

Aunque el caso puede parecer un score tradicional de aprobación, el conjunto final incluye variables como `last_pymnt_amnt`, `recoveries`, `out_prncp` y `total_rec_late_fee`, que aparecen después del desembolso. Por eso, la lectura correcta del modelo no es originación crediticia pura, sino monitoreo de deterioro, alertamiento y cobranza temprana.

## 1. Delimitación del problema

La tarea sigue siendo supervisada y binaria:

- Clase `1`: crédito con incumplimiento.
- Clase `0`: crédito sin incumplimiento.

Desde negocio, el valor del modelo está en detectar a tiempo créditos problemáticos dentro de una cartera viva. Eso hace especialmente relevante maximizar la sensibilidad sobre la clase riesgosa, incluso si el modelo acepta un mayor número de falsos positivos.

## 2. Análisis descriptivo de datos

Tras la depuración inicial del dataset `Credit Risk Dataset`, el análisis exploratorio trabajó con `268530` filas y `74` columnas. La variable objetivo quedó distribuida así:

- `209711` observaciones en clase `0`
- `58819` observaciones en clase `1`
- tasa aproximada de incumplimiento de `21.9%`

El conjunto final usado en modelamiento quedó en `190394` filas y `33` columnas:

- `146535` casos en clase `0`
- `43859` casos en clase `1`
- tasa de incumplimiento de `0.23036`

Esto indica un desbalance moderado de clases. Para reducir sesgos hacia la clase mayoritaria, el pipeline incorporó ponderación de clases durante el entrenamiento.

### Limpieza y selección de variables

Se identificaron `22` columnas con más de 10% de valores faltantes y también se eliminaron variables altamente correlacionadas o con varianza cercana a cero. El objetivo fue reducir ruido, redundancia y multicolinealidad antes del modelamiento.

Entre las variables descartadas por correlación alta estuvieron:

- `total_rev_hi_lim`
- `total_acc`
- `collection_recovery_fee`
- `total_rec_int`
- `out_prncp_inv`
- `funded_amnt`
- `total_pymnt_inv`
- `installment`
- `funded_amnt_inv`
- `total_rec_prncp`
- `total_pymnt`

### Figuras del EDA

**Mapa de correlación de variables numéricas (Pearson)**  
![Mapa de correlación de variables numéricas usando Pearson](/images/pearson_correlacion1.png)

**Mapa de correlación de variables numéricas (Spearman)**  
![Mapa de correlación de variables numéricas usando Spearman](/images/spearman.png)

**Top 20 variables más importantes del análisis exploratorio**  
![Top 20 variables más importantes del análisis exploratorio](/images/20var_mas_importantes_eda.png)

## 3. Metodología de modelamiento

La estrategia compara una RNA con una `LogisticRegression` como baseline.

### Variables finales

El modelo final usa `loan_status` como objetivo y 10 variables de entrada:

- `last_pymnt_amnt`
- `recoveries`
- `out_prncp`
- `int_rate`
- `term`
- `total_rec_late_fee`
- `tot_cur_bal`
- `dti`
- `initial_list_status`
- `loan_amnt`

Esta selección confirma que el momento de predicción corresponde a una etapa posterior al desembolso.

### Preprocesamiento

El pipeline incluyó:

- imputación por mediana con `SimpleImputer(strategy='median')`
- escalamiento con `StandardScaler`
- transformación con `ColumnTransformer`
- partición estratificada con `train_test_split`
- ponderación de clases con `compute_class_weight(class_weight='balanced')`

### Arquitectura de la RNA

La RNA se implementó con `tf.keras.Sequential` y usó:

- capas densas
- `BatchNormalization`
- `Dropout`
- capa final sigmoide para clasificación binaria

Durante el entrenamiento se monitorizaron:

- `ROC-AUC`
- `PR-AUC`
- precisión
- recall

La mejor configuración encontrada por búsqueda aleatoria fue:

- `units = 384`
- `regularization_type = l1`
- `regularization_strength = 1e-05`
- `num_layers = 3`
- `learning_rate = 0.002`
- `layer_shrink_factor = 0.6`
- `label_smoothing = 0.03`
- `dropout = 0.5`
- `clipnorm = 2.0`
- `batch_size = 512`
- `batch_norm = True`

El umbral de decisión tampoco se dejó en `0.5`. Se ajustó en validación maximizando F2 y quedó en `0.54933`, priorizando la detección de incumplimiento.

## 4. Evaluación comparativa

La comparación entre modelos se resume en la siguiente tabla:

| Modelo | Validation PR-AUC | Test PR-AUC | Accuracy | F1 clase 1 |
|---|---:|---:|---:|---:|
| RNA | 0.98030 | 0.98266 | 0.9519 | 0.9040 |
| Regresión Logística | 0.95537 | 0.96029 | 0.9323 | 0.8677 |

### Resultados de la RNA

- Clase `0`: precisión `0.9945`, recall `0.9427`, F1 `0.9679`
- Clase `1`: precisión `0.8370`, recall `0.9827`, F1 `0.9040`
- Accuracy global: `0.9519`
- Macro F1: `0.9360`
- Weighted F1: `0.9532`

### Resultados de la Regresión Logística

- Clase `0`: precisión `0.9883`, recall `0.9230`, F1 `0.9545`
- Clase `1`: precisión `0.7893`, recall `0.9634`, F1 `0.8677`
- Accuracy global: `0.9323`
- Macro F1: `0.9111`
- Weighted F1: `0.9345`

### Lectura técnica

La RNA supera al baseline en las métricas visibles del experimento:

- mejor `PR-AUC` en validación y prueba
- mayor `accuracy`
- mejor `precision` positiva
- mayor `recall` sobre la clase de incumplimiento

En monitoreo de cartera, esta combinación es valiosa porque prioriza detectar casos riesgosos sin perder demasiado control sobre falsos positivos.

Hay una limitación documental importante: en las salidas visibles no quedó consolidado el valor final de `ROC-AUC` para validación y prueba. Conceptualmente la métrica sí formó parte del entrenamiento, pero el entregable final no la recupera numéricamente.

## 5. Interpretabilidad y riesgo

La interpretabilidad se construyó en dos niveles:

- una capa exploratoria basada en EDA
- una capa específica del modelo basada en SHAP

No deben mezclarse como si fueran la misma evidencia.

### Importancia exploratoria

El EDA mostró como variables más influyentes:

- `last_pymnt_amnt`: `0.356140`
- `recoveries`: `0.292410`
- `out_prncp`: `0.193586`
- `int_rate`: `0.029527`
- `term_60 months`: `0.027584`
- `total_rec_late_fee`: `0.017887`
- `tot_cur_bal`: `0.015299`
- `dti`: `0.009023`
- `initial_list_status_w`: `0.008162`
- `loan_amnt`: `0.005557`

### Interpretabilidad con SHAP

La mejor RNA se analizó con SHAP mediante:

- `summary_plot`
- gráfico de barras de importancia

Las figuras confirman interpretabilidad global del modelo, aunque no se consolidó un ranking textual final de valores SHAP en el notebook revisado.

### Lectura de negocio

Los hallazgos apuntan a tres bloques de riesgo:

- comportamiento de pago reciente
- exposición financiera remanente
- severidad estructural del crédito

Esto refuerza que el modelo funciona mejor como herramienta de seguimiento y cobranza temprana que como score de admisión ex ante.

### Figuras de interpretabilidad

**SHAP summary plot de la mejor RNA**  
![SHAP summary plot de la mejor red neuronal artificial](/images/resumen_global_shap.png)

**SHAP bar plot de la mejor RNA**  
![SHAP bar plot de la mejor red neuronal artificial](/images/barplot_shap.png)

## 6. Scorecard y aplicación web

La aplicación convierte la probabilidad de incumplimiento en un score de `300` a `850` de forma inversa al riesgo estimado.

La regla usada es:

```text
score = round(850 - defaultProbability * 550)
```

Una mayor probabilidad de incumplimiento produce un score menor. Es una transformación lineal simple, pensada para visualización y consumo operativo.

### Repositorio y despliegue

- Repositorio: [credit-risk](https://github.com/d0ubt0/credit-risk)
- Aplicación web: [credit-risk-neuronal-network](https://credit-risk-neuronal-network.netlify.app/#)

### Captura de la aplicación

![Captura de la aplicación web de riesgo crediticio](/images/webapp.png)

## 7. Conclusiones y limitaciones

La principal conclusión es que la RNA supera a la Regresión Logística como modelo de monitoreo de riesgo crediticio post-desembolso. Su mejor desempeño en `PR-AUC`, `accuracy`, precisión positiva y recall sobre la clase riesgosa la hace más adecuada cuando el objetivo operativo es detectar incumplimientos con anticipación.

Los aprendizajes más claros del proyecto fueron:

- la limpieza del dataset fue decisiva para mejorar señal y estabilidad.
- ajustar el umbral con F2 desplazó la decisión hacia mayor sensibilidad.
- las variables más influyentes se concentran en pagos recientes, recuperaciones, saldo pendiente y carga financiera.

## Referencias

- Kaggle. *Credit Risk Dataset*. https://www.kaggle.com/datasets/laotse/credit-risk-dataset
- Lundberg, S. M., & Lee, S.-I. (2017). *A unified approach to interpreting model predictions*. https://proceedings.neurips.cc/paper_files/paper/2017/file/8a20a8621978632d76c43dfd28b67767-Paper.pdf
- scikit-learn developers. *scikit-learn: Machine learning in Python*. https://scikit-learn.org/
- TensorFlow Developers. *TensorFlow Keras*. https://www.tensorflow.org/api_docs/python/tf/keras
- d0ubt0. *credit-risk*. https://github.com/d0ubt0/credit-risk
- d0ubt0. *credit-risk-neuronal-network*. https://credit-risk-neuronal-network.netlify.app/
