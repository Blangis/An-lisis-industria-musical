# Análisis de Datos Musicales

## Descripción del proyecto

Este proyecto analiza un conjunto de datos de música con el objetivo de explorar patrones de popularidad, características técnicas de las canciones y su relación con reproducciones y presencia en playlists.  
Se utilizan datos de Spotify, Deezer y Apple Music para estudiar tendencias y correlaciones.

---

## Objetivos

1. Explorar la **distribución de métricas musicales** como streams, danceability, BPM, y otras características técnicas.
2. Analizar la **dispersión de los datos** mediante desviación estándar.
3. Visualizar la **evolución de reproducciones y número de canciones a lo largo del tiempo**.
4. Calcular y analizar **correlaciones entre variables continuas** para responder preguntas de negocio sobre popularidad musical.

---

## Preguntas que se buscan responder

- ¿Las canciones con mayor BPM tienen más reproducciones en Spotify?
- ¿Las canciones más populares en Spotify también lo son en otras plataformas como Deezer?
- ¿Estar en más listas de reproducción se relaciona con mayor cantidad de reproducciones?
- ¿Los artistas con más canciones disponibles tienen más reproducciones?
- ¿Las características técnicas de una canción influyen en su número de reproducciones?

---

## Metodología

1. **Carga de datos y limpieza**: se importaron datasets en CSV con información de tracks, artistas, streams y playlists.
2. **Exploración de datos**: se analizaron distribuciones con histogramas y boxplots, incluyendo métricas porcentuales como danceability, energy, valence, etc.
3. **Medidas de dispersión**: se calcularon desviaciones estándar para evaluar la variabilidad de streams, danceability y otras métricas.
4. **Visualización temporal**: se crearon gráficos de línea para mostrar la evolución de streams y número de canciones a lo largo de los años.
5. **Correlaciones**: se calcularon correlaciones de Pearson entre variables continuas usando BigQuery y se validaron con scatterplots en Python.

---

## Resultados principales

- **Streams vs Playlists**: correlación positiva fuerte (≈0.78). Las canciones presentes en más playlists tienden a tener más streams.
- **Streams vs Danceability**: correlación cercana a 0 (-0.10). La bailabilidad no determina la popularidad medida en streams.
- **Streams vs BPM**: correlación cercana a 0 (-0.002). El tempo de la canción no influye significativamente en el número de streams.
- **Playlists Spotify vs Deezer**: correlación positiva fuerte (≈0.83). Las canciones populares en Spotify tienden a serlo también en Deezer.
- **Artistas con más canciones**: correlación positiva fuerte (≈0.78) entre número de canciones por artista y total de streams acumulados.

---

## Visualizaciones

Se generaron **scatterplots** y gráficos de línea para ilustrar la relación entre variables y la evolución temporal de streams y número de canciones.

- Scatterplots: Streams vs Playlists, Streams vs Danceability, Streams vs BPM, Spotify vs Deezer, Total Songs vs Total Streams por artista.
- Gráficos de línea: evolución de streams y número de canciones por año/década.

---

## Tecnologías utilizadas

- **Python**: Pandas, Matplotlib, Seaborn.
- **BigQuery**: consultas SQL para cálculo de mediana, desviación estándar y correlaciones.
- **Looker Studio**: visualizaciones de dispersión y series temporales.

---

## Conclusión

El análisis sugiere que la **exposición en playlists y la cantidad de canciones de un artista** son factores determinantes en la popularidad medida por streams, mientras que **características técnicas como BPM o bailabilidad** no presentan correlación significativa con la cantidad de reproducciones.
