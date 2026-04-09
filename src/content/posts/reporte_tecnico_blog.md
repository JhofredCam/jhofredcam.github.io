---
author: "Sebastián Pabón Núñez, Jhofred Jahat Camacho Gómez"
publishDate: 2026-03-24T12:00:00Z
title: "Reporte técnico: Optimización numérica y combinatoria"
tags:
  - optimizacion
  - algoritmos-bioinspirados
  - optimizacion-numerica
  - optimizacion-combinatoria
  - tsp
description: "Estudio comparativo de GD, DE, EA y PSO en funciones benchmark y ACO vs GA para TSP de 32 capitales de México."
---
# Reporte técnico: Optimización numérica y combinatoria

**Curso:** Redes Neuronales y Algoritmos Bioinspirados  
**Equipo:** Sebastián Pabón Núñez, Jhofred Jahat Camacho Gómez  
**Fecha:** 24 de marzo de 2026  
**Repositorio de código:** [GitHub - rna-optimizacion-heuristica](https://github.com/d0ubt0/rna-optimizacion-heuristica)

---

## Optimización Numérica

### 1. Introducción
El presente reporte detalla el estudio comparativo de diversos algoritmos de optimización aplicados a funciones de prueba clásicas en el análisis numérico. El objetivo principal es evaluar cómo diferentes estrategias de búsqueda —desde las basadas en el gradiente local hasta las metaheurísticas de población— enfrentan desafíos como la multimodalidad y los valles estrechos en espacios de 2 y 3 dimensiones.

---

### 2. Fundamento Teórico

Para entender por qué un algoritmo falla o tiene éxito, debemos analizar la "topografía" del problema que intenta resolver.

#### 2.1 Funciones de Prueba 

* **Función de Rosenbrock:** Es una función no convexa utilizada para probar el rendimiento de los algoritmos de optimización.  
    * **Forma y Reto:** Tiene forma de un valle largo, estrecho y parabólico. Aunque encontrar el valle es fácil, converger al mínimo global (ubicado en el fondo del valle) es extremadamente difícil porque la superficie es casi plana en esa zona, lo que hace que los algoritmos "vagen" sin rumbo claro (Rosenbrock, 1960).  
    * **Representación:** Un problema de convergencia en valles de baja pendiente.
    

* **Función de Rastrigin:** Es una función altamente multimodal basada en la función de De Jong con la adición de modulación de coseno para crear múltiples mínimos locales.  
    * **Forma y Reto:** Visualmente parece una caja de huevos. Presenta un gran número de mínimos locales distribuidos de forma regular. Un algoritmo que solo busque "hacia abajo" quedará atrapado en el primer pozo que encuentre, siendo incapaz de ver que existe un pozo mucho más profundo en el centro (Rastrigin, 1974).  
    * **Representación:** Un problema de exploración global frente a trampas locales.
    

#### 2.2 Algoritmos de Optimización

* **Descenso por Gradiente (GD):** Método de primer orden que utiliza la derivada de la función para moverse en la dirección de mayor descenso. Es ultra-eficiente pero carece de memoria global (Cauchy, 1847).
* **Algoritmos Evolutivos (EA):** Utilizan mecanismos inspirados en la evolución biológica: selección, cruce y mutación. Son ideales para explorar grandes áreas del espacio de búsqueda (Holland, 1975).
* **Optimización por Enjambre de Partículas (PSO):** Basado en el comportamiento social de animales. Las "partículas" ajustan su trayectoria basándose en su mejor experiencia propia y la del grupo (Kennedy & Eberhart, 1995).
* **Evolución Diferencial (DE):** Optimiza mediante la suma de la diferencia ponderada de vectores de la población. Es excepcionalmente buena para mantener la diversidad y navegar valles complejos (Storn & Price, 1997).

---

### 3. Resultados y Análisis Estadístico

A continuación se presentan las métricas obtenidas tras **20 ejecuciones independientes** por cada caso.

| Experimento | Promedio | Mediana | Std | Mínimo | Máximo | Tasa Éxito | Tiempo Medio (s) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **GD Rastrigin 2D** | 17.5111 | 18.9041 | 8.5380 | 3.9798 | 28.8535 | 0.0000 | 0.0006 |
| **GD Rastrigin 3D** | 27.3611 | 27.8586 | 12.2604 | 4.9747 | 49.7474 | 0.0000 | 0.0005 |
| **GD Rosenbrock 2D** | 1.0698 | 0.6317 | 1.3993 | 0.0041 | 4.8054 | 0.1000 | 0.0005 |
| **GD Rosenbrock 3D** | 1.3080 | 1.3189 | 1.1161 | 0.0057 | 3.9595 | 0.1000 | 0.0007 |
| **DE Rastrigin 2D** | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 1.0000 | 0.0934 |
| **DE Rastrigin 3D** | 1.2715 | 1.1292 | 0.7849 | 0.2713 | 3.2107 | 0.0000 | 0.0938 |
| **DE Rosenbrock 2D** | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 1.0000 | 0.0969 |
| **DE Rosenbrock 3D** | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 1.0000 | 0.0970 |
| **EA Rastrigin 2D** | 0.0001 | 0.0000 | 0.0003 | 0.0000 | 0.0011 | 1.0000 | 0.2348 |
| **EA Rastrigin 3D** | 0.0003 | 0.0001 | 0.0007 | 0.0000 | 0.0025 | 1.0000 | 0.2371 |
| **EA Rosenbrock 2D** | 0.0558 | 0.0021 | 0.0771 | 0.0000 | 0.2048 | 0.6000 | 0.2394 |
| **EA Rosenbrock 3D** | 0.4883 | 0.5422 | 0.3390 | 0.0019 | 0.9297 | 0.2000 | 0.2426 |
| **PSO Rastrigin 2D** | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 1.0000 | 0.0268 |
| **PSO Rastrigin 3D** | 0.3010 | 0.0008 | 0.4542 | 0.0000 | 0.9949 | 0.6000 | 0.0270 |
| **PSO Rosenbrock 2D** | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 1.0000 | 0.0307 |
| **PSO Rosenbrock 3D** | 0.0585 | 0.0697 | 0.0422 | 0.0000 | 0.1179 | 0.3000 | 0.0308 |

#### Análisis de resultados:
1.  **Velocidad vs. Fiabilidad:** El GD es órdenes de magnitud más rápido, pero su tasa de éxito en Rastrigin es **cero**. Esto sucede porque el gradiente no "salta", solo se desliza hacia el fondo más cercano.
2.  **Superioridad de DE:** La Evolución Diferencial es el algoritmo más consistente, manteniendo éxito total en Rosenbrock incluso en 3D, donde otros (como EA y PSO) degradan su rendimiento significativamente.
3.  **Dificultad de la Dimensión:** Al pasar de 2D a 3D, la tasa de éxito del PSO en Rastrigin cae del 100% al 60%, lo que evidencia que la búsqueda por enjambre requiere más partículas o iteraciones a medida que el espacio crece.

---

### 4. Visualización y Análisis del Proceso

Esta sección analiza el comportamiento dinámico de los algoritmos. Las animaciones permiten observar cómo cada método explora el espacio de búsqueda y cómo gestiona los desafíos topográficos de las funciones.

#### 4.1 Función Rastrigin (El reto de la exploración global)
En la función Rastrigin, el éxito depende de la capacidad del algoritmo para "ignorar" los cientos de mínimos locales (pozos falsos) y converger al centro.

**Descenso por Gradiente (GD):**
En las animaciones se observa un único agente que se desliza rápidamente hacia la zona de menor altura más cercana.

Tabla 1. Visualización del comportamiento del GD en la función Rastrigin.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 1: GD en Rastrigin 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rastrigin.gif) |
| 3D | ![Figura 2: GD en Rastrigin 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rastrigin_3d.gif) |

* **Análisis de comportamiento:** El GD carece de mecanismos de escape. Una vez que entra en la cuenca de atracción de un mínimo local, el gradiente lo empuja al fondo y lo deja "atrapado" definitivamente. Esto explica por qué su tasa de éxito es 0; en un paisaje tan accidentado, la probabilidad de empezar justo en la cuenca del óptimo global es despreciable.

**Evolución Diferencial (DE):**
Se observa una población inicial dispersa que, mediante operaciones de mutación basada en vectores de diferencia, comienza a "saltar" entre las crestas de la función.

Tabla 2. Visualización del comportamiento del DE en la función Rastrigin.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 3: DE en Rastrigin 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rastrigin_de.gif) |
| 3D | ![Figura 4: DE en Rastrigin 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rastrigin_3d_de.gif) |

* **Análisis de comportamiento:** DE utiliza la diversidad de la población para estimar la dirección del mínimo global. En las animaciones, se nota cómo la nube de puntos se contrae de forma inteligente hacia el origen, demostrando una resiliencia superior a la multimodalidad.

**Algoritmos Evolutivos (EA):**
La animación muestra un proceso de "refinamiento" sucesivo. Los puntos rojos representan individuos que sobreviven y se reproducen en zonas prometedoras.

Tabla 3. Visualización del comportamiento del EA en la función Rastrigin.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 5: EA en Rastrigin 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rastrigin_ea.gif) |
| 3D | ![Figura 6: EA en Rastrigin 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rastrigin_3d_ea.gif) |

* **Análisis de comportamiento:** El EA mantiene una exploración constante gracias a la mutación. Aunque visualmente parece más errático y lento en converger, esta misma "lentitud" le permite inspeccionar mejor el terreno antes de comprometerse con una solución final, garantizando éxito total en 2D y 3D.

**Optimización por Enjambre de Partículas (PSO):**
Se visualiza una dinámica de "atracción". Las partículas vuelan hacia el líder del grupo pero conservan una inercia propia que las hace orbitar.

Tabla 4. Visualización del comportamiento del PSO en la función Rastrigin.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 7: PSO en Rastrigin 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rastrigin_pso.gif) |
| 3D | ![Figura 8: PSO en Rastrigin 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rastrigin_3d_pso.gif) |

* **Análisis de comportamiento:** Es el método más veloz en agruparse. Sin embargo, en la animación 3D se observa que algunas partículas "orbitan" demasiado rápido y no logran frenar en el punto exacto, lo que reduce su precisión final en dimensiones superiores.

---

#### 4.2 Función Rosenbrock (El reto de la convergencia en valles)
En Rosenbrock, el desafío no es encontrar la zona baja, sino avanzar por el fondo de un valle curvo donde la pendiente es casi inexistente.

**Descenso por Gradiente (GD):**
El agente encuentra el valle de forma casi instantánea, pero una vez dentro, su movimiento se vuelve imperceptible.

Tabla 5. Visualización del comportamiento del GD en la función Rosenbrock.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 9: GD en Rosenbrock 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rosenbrock.gif) |
| 3D | ![Figura 10: GD en Rosenbrock 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rosenbrock_3d.gif) |

* **Análisis de comportamiento:** El gradiente en el fondo del valle de Rosenbrock es muy pequeño (cercano a cero). El algoritmo sufre del fenómeno de "zig-zag" o estancamiento, donde avanza pasos minúsculos que no le permiten alcanzar el óptimo real en un tiempo razonable.

**Evolución Diferencial (DE):**
La población parece "fluir" a lo largo de la curva de la banana de forma fluida.

Tabla 6. Visualización del comportamiento del DE en la función Rosenbrock.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 11: DE en Rosenbrock 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rosenbrock_de.gif) |
| 3D | ![Figura 12: DE en Rosenbrock 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rosenbrock_3d_de.gif) |

* **Análisis de comportamiento:** DE es el algoritmo que mejor se adapta a esta topología. Al usar la diferencia entre puntos, genera pasos de búsqueda que se alinean naturalmente con la orientación del valle, permitiendo una convergencia perfecta (1.0) incluso en 3D.

**Algoritmos Evolutivos (EA):**
La población se concentra en el valle pero queda estirada a lo largo de este, sin lograr colapsar en un único punto.

Tabla 7. Visualización del comportamiento del EA en la función Rosenbrock.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 13: EA en Rosenbrock 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rosenbrock_ea.gif) |
| 3D | ![Figura 14: EA en Rosenbrock 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rosenbrock_3d_ea.gif) |

* **Análisis de comportamiento:** El mecanismo de mutación aleatoria del EA no es eficiente para seguir trayectorias curvas y estrechas. Los puntos "saltan" fuera del valle y deben volver a entrar, desperdiciando evaluaciones y reduciendo la precisión final.

**Optimización por Enjambre de Partículas (PSO):**
Las partículas parecen "rebotar" contra las paredes del valle mientras intentan seguir al líder.

Tabla 8. Visualización del comportamiento del PSO en la función Rosenbrock.
| Dimensión | Representación |
|----------|--------------|
| 2D | ![Figura 15: PSO en Rosenbrock 2D](/reporte-tecnico/opt-numerica/animaciones/animacion_rosenbrock_pso.gif) |
| 3D | ![Figura 16: PSO en Rosenbrock 3D](/reporte-tecnico/opt-numerica/animaciones/animacion_3d_rosenbrock_3d_pso.gif) |

* **Análisis de comportamiento:** El PSO lucha contra su propia inercia. Debido a que el valle es curvo, las partículas tienden a seguir una línea recta y salirse del camino óptimo, lo que explica por qué su tasa de éxito cae drásticamente al aumentar la complejidad del problema (3D).

---

### 5. Discusión y Conclusiones

#### 5.1 Comparativa Crítica de Métodos
A partir de la observación de los resultados y las animaciones, podemos concluir:

* **Descenso por Gradiente:** Es una herramienta de **explotación local**. Útil solo si se tiene una excelente estimación inicial o si la función es convexa. Su bajo costo computacional lo hace atractivo, pero su falta de visión global lo inhabilita para problemas reales complejos.
* **Algoritmos de Población (Heurísticos):** Son herramientas de **exploración global**.
    * **DE (Evolución Diferencial):** Es el más equilibrado. Su capacidad para adaptarse a la topología del terreno (como en Rosenbrock) lo sitúa como la opción más robusta para optimización numérica continua.
    * **EA (Evolución Aleatoria):** Es el más persistente. No es el más rápido, pero su tasa de éxito constante en Rastrigin demuestra que es casi imposible que quede atrapado permanentemente.
    * **PSO (Enjambre):** Es el más veloz de los heurísticos. Ideal para aplicaciones que requieren una respuesta rápida, siempre que se esté dispuesto a sacrificar algo de precisión en dimensiones altas.

#### 5.2 Conclusión General
La experimentación nos confirma que ningún algoritmo es superior en todos los escenarios. Sin embargo, para problemas de ingeniería con paisajes accidentados y valles estrechos, los métodos bioinspirados —específicamente la **Evolución Diferencial**— ofrecen una fiabilidad que el cálculo tradicional no puede garantizar. La transición de 2D a 3D evidenció que la robustez de un algoritmo es tan importante como su velocidad, ya que la complejidad del espacio de búsqueda crece de forma exponencial.

---

## 2) Optimización combinatoria 

### 1. Definición del problema
Un vendedor debe visitar todas las capitales de los 32 estados de México y regresar al origen (Ciudad de México en esta configuración).

### 2.2 Modelado de costo y trazabilidad de fuentes
El costo entre ciudades se modeló con una función compuesta de tiempo, peaje y combustible.

**Ecuación 3. Costo total por arco**
\[
C_{ij} = (valor\_hora \cdot tiempo_{ij}) + peajes_{ij} + combustible_{ij}
\]

**Ecuación 4. Costo de combustible por arco**

---

La Ecuación 3 integra costo de oportunidad temporal y costos directos de viaje, mientras que la Ecuación 4 explicita el aporte energético del vehículo según rendimiento.

**Fuente y metodología de extracción (corrección solicitada):**
- Se dejó un pipeline ETL reproducible en `opt. combinatoria/scripts/costs_etl_inegi.py`.
- Snapshot de matriz base: `opt. combinatoria/data/costos_matriz.csv`.
- Metadatos de extracción: `opt. combinatoria/data/costos_fuentes.json`.
- Snapshot de trabajo bloqueado (rama feat/inegi-full-rerun): SHA-256 6774e6907f37819d70a891db3e3f148dc03409c1703b114505241f1824c9b60a (timestamp UTC 2026-04-09T02:41:24.213206+00:00).
- Endpoints oficiales usados de INEGI SAKBÉ v3.1:
  - `buscalinea` (ajuste de cada capital a la red carretera),
  - `optima` (distancia, tiempo y costo de peaje por par origen-destino),
  - `combustible` (precio de referencia por tipo de combustible).
- Token usado por variable de entorno (`INEGI_RUTEO_TOKEN`), sin exponer credenciales en el repositorio.

Estas fuentes se seleccionaron por su carácter oficial y cobertura nacional para red vial y costos asociados (INEGI, 2026; CAPUFE, 2025; CNE, 2025).

**Comando de actualización del snapshot:**
```bash
$env:INEGI_RUTEO_TOKEN="<tu_token>"
python "opt. combinatoria/scripts/costs_etl_inegi.py" --ignore-existing
```

**Contingencia manual (si la API no está disponible):**
1. Mantener `costos_matriz.csv` como snapshot congelado.
2. Recolectar distancia/tiempo/peaje por pares desde fuentes oficiales (INEGI Ruteo o SICT/CAPUFE).
3. Documentar fecha, fuente y criterio de captura en `costos_fuentes.json`.
4. Validar matriz completa 32x32 antes de ejecutar ACO/GA.

### 2.3 Vehículo y parámetro estudiado
- Vehículo seleccionado: **Sedan Gasolina** (`vehicle_id=sedan_gasolina`).
- Rendimiento: **15.5 km/L**.
- Precio combustible configurado: **23.415 MXN/L** (referencia INEGI combustible tipo Regular).
- Parámetro analizado: `valor_hora` en **[100, 300] MXN/h** con paso **50**.

---

### 4. Métodos implementados
- Colonia de hormigas (ACO).
- Algoritmo genético (GA).

Configuración usada (archivo `opt. combinatoria/data/config.yaml`):
- Seeds: `[7, 13, 29]`
- ACO: `num_ants=55`, `iterations=120`, `alpha=1.0`, `beta=3.0`, `evaporation=0.35`, `q=120.0`.
- GA: `population_size=140`, `generations=220`, `crossover_rate=0.9`, `mutation_rate=0.22`, `elite_size=4`, `tournament_size=4`.

### 2.5 Resultados y visualización (mapa real)
En esta sección se presenta primero la comparación cuantitativa ACO vs GA y luego la evidencia visual del comportamiento del GA sobre mapa real.

#### Tabla 5. Comparativa ACO vs GA por valor-hora
| Valor hora (MXN/h) | Algoritmo | Mejor costo (MXN) | Promedio (MXN) | Desviación | Seed mejor |
|---:|---|---:|---:|---:|---:|
| 100 | ACO | 46698.84 | 47248.37 | 397.25 | 7 |
| 100 | GA | 51931.08 | 53497.88 | 1957.28 | 29 |
| 150 | ACO | 57038.91 | 57416.31 | 297.03 | 29 |
| 150 | GA | 60204.85 | 62179.38 | 1742.49 | 29 |
| 200 | ACO | 66286.27 | 66483.09 | 170.13 | 13 |
| 200 | GA | 69546.06 | 70632.68 | 907.06 | 13 |
| 250 | ACO | 75035.99 | 75788.82 | 542.52 | 13 |
| 250 | GA | 74821.81 | 79040.76 | 3051.05 | 7 |
| 300 | ACO | 83556.68 | 85088.15 | 1101.31 | 29 |
| 300 | GA | 86982.29 | 91954.07 | 3640.59 | 29 |

La Tabla 5 indica que ACO ofrece el mejor desempeño global en este experimento, mientras GA solo supera el mejor-caso de ACO en `valor_hora=250` y con mayor dispersión.

**Mejor solución global observada:** ACO con `valor_hora=100`, costo `46,698.84 MXN`.

Artefactos:
- **GIF de iteraciones GA sobre mapa real:** [ruta_ga_iteraciones_mapa_real.gif](/reporte-tecnico/opt-combinatoria/outputs/ruta_ga_iteraciones_mapa_real.gif)
- **Figura final GA sobre mapa real:** [ruta_ga_iteraciones_mapa_real.png](/reporte-tecnico/opt-combinatoria/outputs/ruta_ga_iteraciones_mapa_real.png)
- **GIF mejor solución global:** [mejor_ruta_global.gif](/reporte-tecnico/opt-combinatoria/outputs/mejor_ruta_global.gif)
- **Figura final mejor solución global:** [mejor_ruta_global.png](/reporte-tecnico/opt-combinatoria/outputs/mejor_ruta_global.png)
- **Rutas por método:** [mejor_ruta_aco.csv](/reporte-tecnico/opt-combinatoria/outputs/mejor_ruta_aco.csv), [mejor_ruta_ga.csv](/reporte-tecnico/opt-combinatoria/outputs/mejor_ruta_ga.csv)

**Figura 3.** Iteraciones del algoritmo genético sobre mapa real de México.

![Figura 3 - Iteraciones GA en mapa real](/reporte-tecnico/opt-combinatoria/outputs/ruta_ga_iteraciones_mapa_real.gif)

La Figura 3 permite verificar visualmente la evolución iterativa del GA sobre cartografía real, cumpliendo el requisito de seguimiento por iteración.

**Figura 4.** Mejor solución global en mapa real de México.

![Figura 4 - Mejor solución global en mapa real](/reporte-tecnico/opt-combinatoria/outputs/mejor_ruta_global.png)

La Figura 4 sintetiza la ruta final de menor costo observada, y complementa la conclusión cuantitativa reportada en la Tabla 5.

### 2.6 Recomendación al vendedor viajero (orden específico)
**Recomendación principal (global):**
Usar la ruta de ACO para `valor_hora=100`.

Orden recomendado de visita:
Ciudad de México -> Toluca -> Cuernavaca -> Puebla -> Tlaxcala -> Xalapa -> Villahermosa -> San Francisco de Campeche -> Mérida -> Chetumal -> Tuxtla Gutiérrez -> Oaxaca de Juárez -> Chilpancingo -> Morelia -> Santiago de Querétaro -> Guanajuato -> San Luis Potosí -> Aguascalientes -> Zacatecas -> Guadalajara -> Colima -> Tepic -> Durango -> Culiacán -> La Paz -> Hermosillo -> Mexicali -> Chihuahua -> Saltillo -> Monterrey -> Ciudad Victoria -> Pachuca -> Ciudad de México.

**Escenario alterno (cuando se prioriza un patrón GA competitivo):**
Para `valor_hora=250`, GA logra mejor mejor-caso que ACO (74,821.81 vs 75,035.99 MXN). Orden GA en ese escenario:
Ciudad de México -> Xalapa -> Villahermosa -> San Francisco de Campeche -> Mérida -> Chetumal -> Tuxtla Gutiérrez -> Oaxaca de Juárez -> Chilpancingo -> Toluca -> Cuernavaca -> Puebla -> Tlaxcala -> Pachuca -> Santiago de Querétaro -> Morelia -> Colima -> Guadalajara -> Tepic -> Durango -> Culiacán -> La Paz -> Hermosillo -> Mexicali -> Chihuahua -> Saltillo -> Monterrey -> Ciudad Victoria -> San Luis Potosí -> Zacatecas -> Aguascalientes -> Guanajuato -> Ciudad de México.

## 3) Metodología y justificación técnica
- Se usaron funciones benchmark clásicas para comparar métodos con distintas geometrías del paisaje de optimización (Rastrigin y Rosenbrock).
- Se estandarizó el número de iteraciones y se evaluó robustez mediante múltiples corridas con diferentes semillas.
- En TSP se modeló costo monetario total por arco (tiempo, peajes y combustible) con snapshot trazable (`costos_matriz.csv`) y metadatos de fuente (`costos_fuentes.json`), además de sensibilidad al valor-hora.
- Se priorizó reproducibilidad mediante configuración explícita y artefactos exportados (CSV/GIF/PNG).

## 4) Uso de IA
Registrar prompts principales y su impacto real en el resultado.

| ID | Prompt usado | Objetivo | Resultado obtenido | Impacto en calidad/final |
|---|---|---|---|---|
| P1 | Genera una visualización animada (GIF) del mejor recorrido TSP sobre un mapa de México: dibuja las capitales con lat/lon, traza la ruta iteración a iteración y guarda mejor_ruta_global.gif y mejor_ruta_global.png con anotaciones de costo, seed y algoritmo. | Generar gifs para acelerar tiempo de desarrollo y usar razonamiento en objetivos mas importantes. | Gifs representando el mejor recorrido en el mapa de México. | Medio |
| P2 | Propón hiperparámetros iniciales para ACO (alpha, beta, evaporación, q, número de hormigas) orientados a TSP de 32 nodos. | Definir una configuración inicial razonable de ACO para un TSP de 32 ciudades, que balancee exploración y explotación. | Se usó num_ants=55, iterations=120, alpha=1.0, beta=3.0, evaporation=0.35, q=120.0 (config actual), con desempeño competitivo en casi todo el barrido de valor_hora, incluyendo el mejor costo global del experimento (48,133.25 MXN a valor_hora=100) | medio |

## 5) Video de contribución individual (obligatorio)
Incluir URL del video final y aportes en primera persona.

- URL video: https://drive.google.com/file/d/1_WWLZ-UhydpCneBUtrUm_3TcAOnLVVLS/view?usp=sharing

## 6) Conclusiones
- En Parte 1, los métodos heurísticos (en especial DE y PSO) alcanzaron mejores mínimos promedio que GD en los escenarios evaluados.
- GD tuvo ventaja en costo de evaluación, pero menor calidad de solución en promedio para los casos más difíciles.
- En Parte 2, ACO obtuvo la mejor solución global (`valor_hora=100`), y GA solo superó el mejor-caso de ACO en el escenario `valor_hora=250`, con mayor variabilidad entre semillas.
- La modelación del costo con `valor_hora` cambió de forma significativa la ruta/costo óptimos, por lo que este parámetro es clave en análisis de sensibilidad.

## 7) Bibliografía

1. Rosenbrock, H. H. (1960). An automatic method for finding the greatest or least value of a function. The Computer Journal, 3(3), 175-184. https://doi.org/10.1093/comjnl/3.3.175
2. Nocedal, J., & Wright, S. J. (2006). Numerical optimization (2nd ed.). Springer. https://doi.org/10.1007/978-0-387-40065-5
3. Boyd, S., & Vandenberghe, L. (2004). Convex optimization. Cambridge University Press. https://stanford.edu/~boyd/cvxbook/
4. Holland, J. H. (1992). Adaptation in natural and artificial systems: An introductory analysis with applications to biology, control, and artificial intelligence. MIT Press. https://mitpress.mit.edu/9780262581110/adaptation-in-natural-and-artificial-systems/
5. Mitchell, M. (1998). An introduction to genetic algorithms. MIT Press. https://mitpress.mit.edu/9780262631853/an-introduction-to-genetic-algorithms/
6. Kennedy, J., & Eberhart, R. (1995). Particle swarm optimization. In Proceedings of ICNN'95 - International Conference on Neural Networks (Vol. 4, pp. 1942-1948). IEEE. https://doi.org/10.1109/ICNN.1995.488968
7. Storn, R., & Price, K. (1997). Differential evolution-a simple and efficient heuristic for global optimization over continuous spaces. Journal of Global Optimization, 11, 341-359. https://doi.org/10.1023/A:1008202821328
8. Dorigo, M., Maniezzo, V., & Colorni, A. (1996). Ant system: Optimization by a colony of cooperating agents. IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics), 26(1), 29-41. https://doi.org/10.1109/3477.484436
9. Jamil, M., & Yang, X.-S. (2013). A literature survey of benchmark functions for global optimisation problems. International Journal of Mathematical Modelling and Numerical Optimisation, 4(2), 150-194. https://doi.org/10.1504/IJMMNO.2013.055204
10. Surjanovic, S., & Bingham, D. (2013). Virtual library of simulation experiments: Test functions and datasets. Simon Fraser University. https://www.sfu.ca/~ssurjano/optimization.html
11. Surjanovic, S., & Bingham, D. (2013). Rosenbrock function. Simon Fraser University. https://www.sfu.ca/~ssurjano/rosen.html
12. Surjanovic, S., & Bingham, D. (2013). Rastrigin function. Simon Fraser University. https://www.sfu.ca/~ssurjano/rastr.html
13. Surjanovic, S., & Bingham, D. (2013). Schwefel function. Simon Fraser University. https://www.sfu.ca/~ssurjano/schwef.html
14. Surjanovic, S., & Bingham, D. (2013). Griewank function. Simon Fraser University. https://www.sfu.ca/~ssurjano/griewank.html
15. Surjanovic, S., & Bingham, D. (2013). Goldstein-Price function. Simon Fraser University. https://www.sfu.ca/~ssurjano/goldpr.html
16. Surjanovic, S., & Bingham, D. (2013). Six-Hump Camel function. Simon Fraser University. https://www.sfu.ca/~ssurjano/camel6.html
17. Applegate, D. L., Bixby, R. E., Chvatal, V., & Cook, W. J. (2007). The traveling salesman problem: A computational study. Princeton University Press. https://press.princeton.edu/books/hardcover/9780691129938/the-traveling-salesman-problem
18. Instituto Nacional de Estadistica y Geografia (INEGI). (2026). Servicio web del Catalogo Unico de Claves Geoestadisticas. https://www.inegi.org.mx/servicios/catalogounico.html
19. Secretaria de Infraestructura, Comunicaciones y Transportes (SICT). (2011). Carreteras V2 (incluye modulo Traza tu ruta). https://www.sct.gob.mx/index.php?id=1617
20. Caminos y Puentes Federales de Ingresos y Servicios Conexos (CAPUFE). (2025). Tarifas CAPUFE [Conjunto de datos]. datos.gob.mx. https://www.datos.gob.mx/dataset/tarifas_capufe
21. Comision Nacional de Energia (CNE). (2025). Historico de precios de gasolinas y diesel reportados por permisionario [Conjunto de datos]. datos.gob.mx. https://www.datos.gob.mx/dataset/historico_precios_gasolinas_y_diesel_reportados_por_permisionario
22. Instituto Nacional de Estadistica y Geografia (INEGI). (2026). API de Ruteo (SAKBE v3.1). https://www.inegi.org.mx/servicios/Ruteo/










