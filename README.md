# Variabilidad interanual de la temperatura del aire en Piura (2000–2025) y su relación con El Niño y La Niña

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/k-montoya/1GEO20_Grupo-3/blob/main/Entrega_final.ipynb)

Entrega final del curso **1GEO20 – Grupo 3**.

**Integrantes**

- Angele Pérez (20201988)
- Alexandra Caballero (20222790)
- Karla Montoya (20196066)

---

## Descripción

El proyecto analiza la variabilidad interanual y espacial de la temperatura del aire a 2 metros en el departamento de Piura (noroeste del Perú) durante el periodo 2000–2025 (312 meses) y evalúa su relación temporal y estadística con El Niño y La Niña.

Los datos térmicos provienen del reanálisis ERA5-Land y se promedian sobre un rectángulo delimitador (*bounding box*) que cubre Piura. La condición oceánica se sigue con dos índices independientes:

- **ONI** (Índice Oceánico El Niño): Pacífico central, región Niño 3.4.
- **ICEN** (Índice Costero El Niño): mar frente a Perú y Ecuador, región Niño 1+2.

## Objetivos

**General.** 

Analizar la variabilidad interanual y la distribución espacial de la temperatura del aire a 2 m en Piura entre 2000 y 2025, y evaluar su relación con las fases de El Niño y La Niña.

**Específicos**

1. Caracterizar la climatología mensual, la variabilidad interanual y la distribución espacial de la temperatura en el dominio de Piura.
2. Evaluar la relación temporal entre las anomalías de temperatura y los índices ONI e ICEN mediante correlación (lineal y no paramétrica), desfases (*lags*) y distribución por fases.

**Hipótesis exploratoria.** Las anomalías de temperatura en Piura son positivas durante El Niño y negativas durante La Niña, con un acoplamiento más estrecho con el ICEN que con el ONI por la cercanía de Piura a la región Niño 1+2. Se considera que la hipótesis se cumple si (i) la mediana de la anomalía es positiva en El Niño y negativa en La Niña, y (ii) la correlación con el ICEN es mayor, en valor absoluto, que con el ONI.

## Datos

| Variable | Fuente | Detalles |
|---|---|---|
| Temperatura del aire a 2 m | ERA5-Land mensual (`ECMWF/ERA5_LAND/MONTHLY_AGGR`, banda `temperature_2m`) vía Google Earth Engine | Resolución nativa de ~9–11 km, solo superficie terrestre. Se promedia sobre el bounding box `[-81.5, -6.5, -78.5, -3.5]` y se convierte de K a °C. |
| ONI | [NOAA Climate Prediction Center](https://cpc.ncep.noaa.gov/products/analysis_monitoring/enso/oni/v6/) (ERSSTv6) | El Niño ≥ +0,5 °C y La Niña ≤ −0,5 °C. |
| ICEN | [ENFEN]([https://enfen.imarpe.gob.pe/](https://siofen.imarpe.gob.pe/nivel2/indice-costero-el-nino-icen))| El Niño ≥ +0,5 °C y La Niña ≤ −0,7 °C. |

Datos crudos: [carpeta en Google Drive](https://drive.google.com/drive/folders/1ij63gMxIznzkXioFCEjUQID0N6r0wCm7?usp=sharing).

## Metodología

1. **Extracción en la nube.** Se obtiene la serie mensual de ERA5-Land (promedio espacial sobre el bounding box) desde Earth Engine y se genera la serie mensual en .cvs y el mapa de temperatura media 2000–2025.
2. **Limpieza y anomalías (Python/Pandas).** Se realiza la conversión de unidades (de Kelvin a grados Celcius), se genera un promedio histórico por mes calendario y se calcula la anomalía térmica.
3. **Armonización e integración (pandas).** Conversión de trimestres ONI y estandarización ICEN, se normalizan las fechas al primer día de cada mes.
4. **Análisis estadístico (Scipy/NumPy).** Se realiza el calculo de correlación con el coeficiente lineal de Pearson (r) y de randos Kendall (t), se añade una correlación cruzada desplazada, y se agrupan los umbrales del ENSO.


## Resultados principales

- La temperatura media mensual va de **20,99 °C en febrero** a **18,47 °C en julio**.
- Año más cálido: **2023 (+0,91 °C)**. Año más frío: **2000 (−0,68 °C)**. Tendencia lineal: **+0,32 °C por década**.

| Métrica | ICEN | ONI |
|---|---|---|
| Pearson r (lag 0) | 0,693 | 0,685 |
| Kendall τ (lag 0) | 0,443 | 0,457 |
| Pearson r sin tendencia | 0,709 | 0,724 |
| r máxima (lag −1 mes) | 0,736 | 0,697 |
| Mediana de anomalía en El Niño (°C) | +0,34 | +0,38 |
| Mediana de anomalía en La Niña (°C) | −0,37 | −0,34 |
| Kruskal-Wallis (H) | 92,92 | 102,87 |

- La temperatura del aire responde alrededor de **un mes después** del océano.
- **La hipótesis se cumple de forma parcial.** El criterio (i) se cumple con ambos índices. El criterio (ii) se cumple, pero solo por 0,008, y la ventaja se invierte con Kendall y al quitar la tendencia. Los datos confirman el signo esperado de las anomalías en cada fase, pero no permiten afirmar que la temperatura del dominio dependa más del índice costero.

## Limitaciones

- El dominio es un rectángulo que mezcla costa, desierto y sierra.
- ERA5-Land (9–11 km) puede no representar bien la temperatura local en la cordillera Occidental.
- La climatología de referencia incluye la tendencia, y la serie empieza con La Niña (2000) y termina tras El Niño 2023-2024, por lo que la pendiente puede estar inflada.
- Las fases se asignan mes a mes con umbrales, sin el requisito de persistencia de los episodios oficiales.
- Las series están autocorrelacionadas y hay pocos eventos por fase, así que los p-valores son orientativos y las conclusiones, exploratorias.
- La diferencia entre las correlaciones con ICEN y ONI no se sometió a una prueba estadística.

## Cómo reproducir

El notebook está pensado para ejecutarse en **Google Colab**.

1. Abre `Entrega_final.ipynb` con el botón de Colab de arriba.
2. Necesitas una cuenta de Google con acceso a [Google Earth Engine](https://earthengine.google.com/) y un proyecto de Google Cloud registrado. En la celda de inicialización reemplaza el valor de `PROJECT_ID` por el ID de tu proyecto:
   ```python
   PROJECT_ID = 'tu-proyecto'
   ee.Initialize(project=PROJECT_ID)
   ```
3. Ejecuta las celdas en orden. Se te pedirá autenticarte en Earth Engine y montar Google Drive, donde se guardan los CSV de salida.
4. Los CSV de ONI e ICEN se leen desde enlaces de Google Drive. Si cambian de ubicación, actualiza las variables `url_oni` y `url_icen`.

**Librerías:** `earthengine-api`, `pandas`, `numpy`, `matplotlib`, `scipy`, `xarray`, `rioxarray`, `cartopy`, `requests`.

## Archivos principales generados

- `ONI_2000_2025.csv` y `ICEN_2000_2025.csv`: índices filtrados y estandarizados.
- `ERA5_T2M_Anomalias_ENSO_Piura_2000_2025.csv`: tabla final con temperatura, anomalía, ONI e ICEN (312 filas).

## Referencias

- Climate Prediction Center. (s. f.). *Cold & warm episodes by season (ERSSTv6)*. NOAA. https://cpc.ncep.noaa.gov/products/analysis_monitoring/enso/oni/v6/
- Comisión Multisectorial ENFEN. (2024). *Definición operacional de los eventos El Niño costero y La Niña costera en el Perú* (Nota Técnica ENFEN 01-2024).
- Instituto Geofísico del Perú. (2024). *Informe técnico Nº PpR/El Niño-IGP/2024-05*.
- Rodríguez, I., Villarreal, E. y Acosta, J. (2011). *Memoria sobre la geología económica de la región Piura*. INGEMMET.
- Takahashi, K. (2004). The atmospheric circulation associated with extreme rainfall events in Piura, Peru, during the 1997–1998 and 2002 El Niño events. *Annales Geophysicae, 22*(11), 3917–3926. https://doi.org/10.5194/angeo-22-3917-2004
- Takahashi, K. et al. (2018). The 2017 coastal El Niño. *Bulletin of the American Meteorological Society, 99*(8), S210–S242. https://doi.org/10.1175/2018BAMSStateoftheClimate.1
- Yglesias-González, M. et al. (2023). Reflections on the impact and response to the Peruvian 2017 coastal El Niño event. *PLOS ONE, 18*(9), e0290767. https://doi.org/10.1371/journal.pone.0290767
