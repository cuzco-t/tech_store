# TechStore: Inferencia Estadística y Modelado Predictivo para la Adopción de un Chatbot con IA

[![Python](https://img.shields.io/badge/Python-3.10-blue)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Este repositorio contiene un análisis estadístico exhaustivo y un proyecto de modelado predictivo para **TechStore**, una empresa de comercio electrónico especializada en productos tecnológicos. El objetivo fue ir más allá de la analítica descriptiva y utilizar la estadística inferencial para validar suposiciones comerciales, comprender el comportamiento del cliente y construir un modelo predictivo para los montos de las transacciones, centrándose en el impacto de un chatbot con IA recientemente implementado.

---

## Tabla de Contenido
- [Contexto del Negocio](#contexto-del-negocio)
- [Descripción de los Datos](#descripción-de-los-datos)
- [Metodología](#metodología)
- [Hallazgos Clave](#hallazgos-clave)
- [Recomendaciones](#recomendaciones)
- [Estructura del repositorio](#estructura-del-repositorio)
---

## Contexto del Negocio

TechStore es un minorista de comercio electrónico que lanzó recientemente un **chatbot con IA** piloto para mejorar la experiencia del cliente. Después de tres meses, recopilaron datos de **500 transacciones**. La gerencia ahora requiere una validación estadística rigurosa de los indicadores clave de rendimiento (KPIs), comprender la adopción del chatbot en las distintas regiones y un modelo predictivo para pronosticar los montos de las transacciones basado en el comportamiento de navegación de los clientes.

Las principales preguntas de negocio fueron:
- ¿El monto promedio de compra es realmente de **$350** como lo asume la gerencia?
- ¿El uso del chatbot depende de la **región geográfica**?
- ¿Qué factores influyen más en el monto final de compra?
- ¿Podemos predecir el gasto de un cliente basándonos en sus patrones de navegación?

---

## Descripción de los Datos

El conjunto de datos `techstore_data.csv` contiene 500 transacciones con las siguientes variables clave:

- **Cuantitativas**: `monto_compra` (objetivo), `tiempo_navegacion`, `productos_vistos`, `satisfaccion`
- **Categóricas**: `region`, `uso_chatbot_ia`, `tipo_cliente`

El diccionario de datos y el contexto completo están disponibles en el [PDF del caso de estudio](./Inferencia%20Estadística%20y%20Modelado%20-%20caso%20de%20estudio.pdf).

---

## Metodología

El análisis se dividió en tres módulos principales, detallados en el [Jupyter Notebook](./analysis.ipynb) adjunto:

### A. Estimación y Tamaño de Muestra
- Cálculo de los tamaños de muestra requeridos para estudios futuros (media y proporción) utilizando niveles de confianza y márgenes de error.
- Construcción de intervalos de confianza del 95% y 90% para métricas clave (compra promedio de clientes premium, proporción de adopción del chatbot).

### B. Pruebas de Hipótesis (α = 0.05)
- **Prueba t de una muestra**: Se evaluó la afirmación de la gerencia de que la compra promedio global es de $350.
- **Prueba de independencia Chi-cuadrado**: Se analizó si la adopción del chatbot es independiente de la región geográfica.
- **ANOVA de dos factores**: Se examinaron los efectos de la región y el uso del chatbot sobre el monto de compra.

### C. Modelado Predictivo (Regresión Lineal Múltiple)
- Construcción de un modelo de regresión para predecir `monto_compra` usando `tiempo_navegacion`, `productos_vistos` y `satisfaccion`.
- Verificación de multicolinealidad (VIF) y realización de selección de características por pasos (stepwise).
- Validación de los supuestos del modelo (linealidad, normalidad, homocedasticidad).
- Cálculo de coeficientes estandarizados para identificar el factor más influyente.
- Predicción del gasto para un "usuario promedio del chatbot" (valores medianos de los predictores).

---

## Hallazgos Clave

### 1. La Adopción del Chatbot es Nacional y Uniforme
- **Prueba Chi-cuadrado**: p‑valor = 0.618 → No hay asociación significativa entre el uso del chatbot y la región.
- La tasa de adopción es homogénea en todo el país, validando una estrategia de implementación nacional estandarizada.

### 2. Se Rechaza la Afirmación de una Compra Promedio de $350
- **Prueba t de una muestra**: t = −19.82, p‑valor ≈ 0.0 → La verdadera media poblacional es **significativamente menor** a $350.
- Este hallazgo fue respaldado posteriormente por el modelo predictivo (ver más abajo).

### 3. La Cantidad de Productos Vistos Impulsa el Gasto
- Los **coeficientes de regresión estandarizados** revelaron que `productos_vistos` (β ≈ 0.29) tiene la mayor influencia sobre `monto_compra`, superando ampliamente a `satisfaccion` (β ≈ 0.27) y `tiempo_navegacion` (β ≈ 0.12).
- El modelo de regresión final explicó solo el **17.9% de la varianza** (R²aj = 0.179), lo que indica que otros factores no medidos también juegan un papel, pero la cantidad de productos vistos sigue siendo el predictor más fuerte entre los estudiados.

### 4. Gasto Predicho para el Usuario Promedio del Chatbot
- Para un cliente con **valores medianos** de tiempo de navegación, productos vistos y satisfacción, el modelo predice un monto de compra de **$161.54**.
- Esta cifra está muy por debajo del KPI asumido de $350, lo que resalta la necesidad de recalibrar las expectativas del negocio.

### 5. ANOVA: Sin Efecto Regional o del Chatbot en el Monto de Compra
- El ANOVA de dos factores en los compradores reales mostró **que no hay efectos principales significativos** ni interacción entre la región y el uso del chatbot (p > 0.05).

---

## Recomendaciones

Basándonos en estos hallazgos, proponemos las siguientes estrategias accionables:

- **Mejorar la visibilidad de los productos** en el sitio web: Dado que la cantidad de productos vistos es el predictor más fuerte del gasto, invierta en mejoras de UI/UX que sugieran artículos relacionados, realicen venta cruzada y hagan que la navegación sea intuitiva.
- **Ajustar las previsiones de ingresos**: Reemplace el supuesto de $350 con expectativas basadas en datos (por ejemplo, los $161.54 predichos para los usuarios promedio del chatbot) para establecer objetivos de ventas realistas.
- **Mantener un despliegue nacional uniforme del chatbot**: Dada la homogeneidad regional, no son necesarias adaptaciones específicas por región; concéntrese en refinar la funcionalidad e integración del chatbot.
- **Explorar características predictivas adicionales**: El R² bajo sugiere que se deben recopilar e incorporar otros factores (ej. tipo de cliente, hora del día, promociones) en modelos futuros.

---

## Estructura del repositorio
```
.
├── analysis.ipynb # Cuaderno Jupyter completo con código y resultados
├── Inferencia Estadística y Modelado - caso de estudio.pdf # Descripción original del caso
├── README.md # Este archivo
├── requirements.txt # Dependencias de Python
├── tech_store_report.pdf # Informe final con resumen ejecutivo
└── techstore_data.csv # Dataset
```
---