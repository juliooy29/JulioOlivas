# Proyecto de calculo de riesgo y rendimiento y optimizacion de portafolios
Optimización de Portafolios (Mean-Variance)

Analizador y optimizador de portafolios de inversión basado en la Teoría Moderna de Portafolios (Markowitz). El proyecto descarga precios históricos reales, calcula las métricas de riesgo y rendimiento de un portafolio, y encuentra las combinaciones óptimas de pesos según distintos objetivos.

¿Qué hace?

A partir de un conjunto de acciones, el notebook:

Descarga precios históricos reales usando la API de yfinance.
Calcula los rendimientos diarios y los anualiza (rendimiento esperado y volatilidad).
Construye la matriz de covarianza para capturar cómo se mueven los activos entre sí.
Calcula las tres métricas fundamentales de un portafolio:
Rendimiento esperado (suma ponderada).
Riesgo / volatilidad, mediante la fórmula matricial √(wᵀΣw).
Ratio de Sharpe, usando una tasa libre de riesgo obtenida dinámicamente de los bonos del Tesoro de EE.UU.
Optimiza los pesos del portafolio con scipy.optimize bajo tres enfoques:
Máximo Sharpe: la mejor relación riesgo-rendimiento.
Mínimo riesgo dado un retorno objetivo: el portafolio menos volátil que alcanza un rendimiento deseado.
Máximo retorno dado un riesgo máximo: el mayor rendimiento posible sin exceder un límite de volatilidad.
Conceptos aplicados
Anualización asimétrica: el rendimiento escala por ×252 (días de mercado) y la volatilidad por ×√252, porque la varianza (no la desviación estándar) se acumula linealmente en el tiempo.
Covarianza y diversificación: por qué el riesgo de un portafolio suele ser menor que el promedio ponderado de los riesgos individuales.
Optimización con restricciones: uso de restricciones de igualdad (los pesos suman 1) y desigualdad (límites de riesgo/retorno), además de bounds para portafolios long-only (sin ventas en corto).
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
Abre el notebook:
bash
   jupyter notebook optimizacion_portafolios.ipynb
En la segunda celda, edita la lista de activos y el rango de fechas según lo que quieras analizar:
python
   acciones = ['AAPL', 'JPM', 'XOM', 'JNJ', 'PG', 'CAT', 'NEE', 'AMT', 'DIS', 'HD']
   fecha_inicio = '2020-1-1'
   fecha_final  = '2026-7-28'
Ajusta los parámetros de las optimizaciones con restricción:
python
   re_minimo    = 0.20   # retorno mínimo exigido (para minimizar riesgo)
   riesgo_maximo = 0.20  # riesgo máximo permitido (para maximizar retorno)
Ejecuta todas las celdas en orden (en Jupyter: Kernel → Restart & Run All).
Nota sobre reproducibilidad

Los datos se descargan en vivo desde Yahoo Finance, por lo que los resultados dependen de la fecha en que se ejecute el notebook: correrlo en una fecha distinta incluye más días de datos y produce números ligeramente diferentes. Los tickers también pueden cambiar o dejar de estar disponibles con el tiempo; si alguno falla en la descarga, basta con sustituirlo por otro.

Posibles extensiones
Trazar la frontera eficiente completa (repetir la optimización de mínimo riesgo para un rango de retornos objetivo).
Visualizar el plano riesgo-rendimiento con matplotlib, marcando los activos individuales y los portafolios óptimos.
Añadir límites de concentración por sector mediante restricciones de desigualdad.
Descargo de responsabilidad

Este proyecto tiene fines educativos y de aprendizaje. No constituye asesoría financiera ni una recomendación de inversión. Los rendimientos históricos no garantizan resultados futuros.
