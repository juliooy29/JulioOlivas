Análisis y Optimización de Portafolios

Librería de proyectos de finanzas cuantitativas en Python, basada en la Teoría Moderna de Portafolios (Markowitz). El repositorio contiene dos proyectos complementarios que van de lo básico a lo avanzado: primero medir el riesgo y rendimiento de un portafolio dado, y luego encontrar los pesos óptimos automáticamente.

Proyectos
1. Analizador de portafolio (riesgo y rendimiento)

Evalúa un portafolio con pesos definidos por el usuario. Descarga precios históricos reales, calcula las métricas fundamentales y valida las entradas.

Qué hace:

Descarga precios históricos reales con la API de yfinance.
Calcula los rendimientos diarios y los anualiza (rendimiento esperado y volatilidad).
Construye la matriz de covarianza de los activos.
Pide los pesos al usuario, validando que sumen 1 (con manejo robusto de punto flotante mediante math.isclose).
Calcula las tres métricas del portafolio:
Rendimiento esperado (suma ponderada).
Riesgo / volatilidad, mediante la fórmula matricial √(wᵀΣw).
Ratio de Sharpe, con la tasa libre de riesgo obtenida dinámicamente de los bonos del Tesoro de EE.UU.
2. Optimización de portafolios (mean-variance)

Toma como base el analizador y da el siguiente paso: en lugar de evaluar un portafolio dado, busca los pesos óptimos usando scipy.optimize bajo tres enfoques:

Máximo Sharpe: la mejor relación riesgo-rendimiento.
Mínimo riesgo dado un retorno objetivo: el portafolio menos volátil que alcanza un rendimiento deseado.
Máximo retorno dado un riesgo máximo: el mayor rendimiento posible sin exceder un límite de volatilidad.
Conceptos aplicados
Anualización asimétrica: el rendimiento escala por ×252 (días de mercado) y la volatilidad por ×√252, porque la varianza (no la desviación estándar) se acumula linealmente en el tiempo.
Covarianza y diversificación: por qué el riesgo de un portafolio suele ser menor que el promedio ponderado de los riesgos individuales.
Álgebra matricial con NumPy para el cálculo del riesgo (wᵀΣw), que escala a cualquier número de activos sin cambiar el código.
Alineación de datos: los pesos, rendimientos y la matriz de covarianza se derivan todos del mismo orden de columnas para evitar errores silenciosos de indexación.
Optimización con restricciones: restricciones de igualdad (los pesos suman 1) y desigualdad (límites de riesgo/retorno), más bounds para portafolios long-only (sin ventas en corto).
El truco de minimizar el negativo de una función para maximizarla con un optimizador que solo minimiza.
Tecnologías
Python 3
yfinance — descarga de datos de mercado
NumPy — álgebra matricial
pandas — manejo de series de tiempo
SciPy — optimización (scipy.optimize.minimize)
Jupyter Notebook
Instalación
bash
pip install yfinance numpy pandas scipy notebook
Uso

Ambos notebooks siguen la misma lógica de configuración. Se recomienda revisar primero el analizador y luego el de optimización.

Abre el notebook que quieras ejecutar con Jupyter.
Edita la lista de activos y el rango de fechas en las primeras celdas:
python
   acciones = ['AAPL', 'JPM', 'XOM', 'JNJ', 'PG']
   fecha_inicio = '2020-1-1'
   fecha_final  = '2026-7-28'
En el notebook de optimización, ajusta además los parámetros de las restricciones:
python
   re_minimo     = 0.20   # retorno mínimo exigido (para minimizar riesgo)
   riesgo_maximo = 0.20   # riesgo máximo permitido (para maximizar retorno)
Ejecuta todas las celdas en orden (en Jupyter: Kernel → Restart & Run All).

En el analizador, el notebook te pedirá los pesos de cada activo de forma interactiva; deben sumar 1.

Nota sobre reproducibilidad

Los datos se descargan en vivo desde Yahoo Finance, por lo que los resultados dependen de la fecha en que se ejecute el notebook: correrlo en una fecha distinta incluye más días de datos y produce números ligeramente diferentes. Los tickers también pueden cambiar o dejar de estar disponibles con el tiempo; si alguno falla en la descarga, basta con sustituirlo por otro.

Posibles extensiones
Trazar la frontera eficiente completa (repetir la optimización de mínimo riesgo para un rango de retornos objetivo).
Visualizar el plano riesgo-rendimiento con matplotlib, marcando los activos individuales y los portafolios óptimos.
Añadir límites de concentración por sector mediante restricciones de desigualdad.
Descargo de responsabilidad

Este proyecto tiene fines educativos y de aprendizaje. No constituye asesoría financiera ni una recomendación de inversión. Los rendimientos históricos no garantizan resultados futuros.
