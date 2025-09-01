# **Análisis musical**

# Competition

### 🔹 Glosario de columnas - tabla `competition`

- **track_id** → Identificador único de la canción en Spotify. Permite referenciar la canción de manera inequívoca.
- **in_apple_playlists** → Número de listas de reproducción de Apple Music en las que aparece la canción. Indica difusión o exposición de la canción.
- **in_apple_charts** → Número de charts o rankings de Apple Music donde aparece la canción. Refleja popularidad relativa en Apple.
- **in_deezer_playlists** → Número de listas de reproducción de Deezer en las que aparece la canción.
- **in_deezer_charts** → Número de charts o rankings de Deezer donde aparece la canción.
- **in_shazam_charts** → Número de charts de Shazam donde aparece la canción. Los valores `NULL` representan información desconocida.

## Revisión de valores nulos en **competition**

```
SELECT track_id FROM `laboratoria-470421.data_music.competition`
WHERE track_id IS NULL ; // No hubo registros

SELECT in_apple_playlists FROM `laboratoria-470421.data_music.competition`
WHERE in_apple_playlists IS NULL ;  // No hubo registros

SELECT in_apple_charts FROM `laboratoria-470421.data_music.competition`
WHERE in_apple_charts IS NULL ; // No hubo registros

SELECT in_deezer_playlists FROM `laboratoria-470421.data_music.competition`
WHERE in_deezer_playlists IS NULL ; // No hubo registros

SELECT in_deezer_charts FROM `laboratoria-470421.data_music.competition`
WHERE in_deezer_charts IS NULL ; // No hubo registros

SELECT in_shazam_charts FROM `laboratoria-470421.data_music.competition`
WHERE in_shazam_charts IS NULL ; // 50 registros con NULL

SELECT count(*) FROM `laboratoria-470421.data_music.competition` ; // 953

```

### ✅ Resumen de nulos en la tabla `competition`

| Columna               | Nulos detectados | Observaciones                                                                    |
| --------------------- | ---------------- | -------------------------------------------------------------------------------- |
| `track_id`            | 0                | Todos los registros tienen identificador.                                        |
| `in_apple_playlists`  | 0                | Todos los registros tienen información de playlists de Apple.                    |
| `in_apple_charts`     | 0                | No hay valores faltantes.                                                        |
| `in_deezer_playlists` | 0                | No hay valores faltantes.                                                        |
| `in_deezer_charts`    | 0                | No hay valores faltantes.                                                        |
| `in_shazam_charts`    | 50               | 50 registros con `NULL`, representa información desconocida sobre Shazam Charts. |
| **Total registros**   | 953              | Cantidad total de canciones en la tabla.                                         |

**Observaciones importantes:**

- Todas las columnas de Apple y Deezer están completas.
- Solo `in_shazam_charts` tiene nulos, que se interpretan como “desconocido” en el análisis.
- Mantener los nulos en `in_shazam_charts` permite diferenciar entre canciones que **no aparecieron** (`0`) y canciones con **información faltante** (`NULL`).
- Dejar consultas separadas está bien para documentación o seguimiento histórico; más adelante se pueden combinar en un solo query con `COUNTIF`.

Otra manera de observar cuántos datos tienen valor, cuántos son 0 y cuántos NULL:

```
SELECT
  COUNTIF(in_shazam_charts > 0) AS en_shazam,
  COUNTIF(in_shazam_charts = 0) AS no_en_shazam,
  COUNTIF(in_shazam_charts IS NULL) AS nulos
FROM `laboratoria-470421.data_music.competition`

// La salida: 559 valores diferentes a 0, 344 iguales a cero y 50 nulos;
```

Para imputar los nulos con ceros (revisar opción), se puede crear otra tabla con esta consulta:

```
CREATE OR REPLACE TABLE data_music.competition_clean AS
SELECT
  track_id,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_playlists,
  in_deezer_charts,
  IFNULL(in_shazam_charts, 0) AS in_shazam_charts
FROM `laboratoria-470421.data_music.competition`;
```

Se decidió que los valores nulos en in_shazam_charts se interpretan como información no disponible. Por eso se mantienen como NULL y no se imputan.

# Spotify

### 🔹 Glosario de columnas - tabla `spotify`

- **track_id** → Identificador único de la canción en Spotify. Permite referenciar la canción de manera inequívoca.

- **track_name** → Nombre de la canción. Es útil para mostrar títulos y para unir con otras tablas de información.

- **artists_name** → Nombre(s) del o los artistas que interpretan la canción.

- **artist_count** → Número de artistas asociados a la canción. Por ejemplo, un dueto tendría 2.

- **released_year** → Año de lanzamiento de la canción. Sirve para análisis temporales o de tendencias.

- **released_month** → Mes de lanzamiento (1-12). Permite hacer análisis estacionales o de estacionalidad en reproducciones.

- **released_day** → Día de lanzamiento (1-31). Útil si quieres analizar patrones de lanzamiento exactos.

- **in_spotify_playlists** → Número de listas de reproducción en Spotify donde aparece la canción. Indica difusión o exposición de la canción.

- **in_spotify_charts** → Número de charts o rankings de Spotify donde aparece la canción. Refleja popularidad relativa.

- **streams** → Número total de reproducciones de la canción en Spotify. Es la métrica principal de éxito.

## Revisión de valores nulos en **spotify**

```
SELECT
COUNTIF(track_id IS NULL) AS nulos_id,
COUNTIF(track_name IS NULL) AS nulos_trackname,
COUNTIF(artists_name IS NULL) AS nulos_artistname,
COUNTIF(artist_count IS NULL) AS nulos_artistcount,
COUNTIF(released_year IS NULL) AS nulos_realeasedyear,
COUNTIF(released_month IS NULL) AS nulos_releasedmonth,
COUNTIF(released_day IS NULL) AS nulos_releasedday,
COUNTIF(in_spotify_playlists IS NULL) AS nulos_spotiyplay,
COUNTIF (in_spotify_charts IS NULL) AS nulos_spotycharts,
COUNTIF(streams IS NULL) AS nulos_streams
FROM `laboratoria-470421.data_music.spotify`

// NO hubo valores nulos en ninguna columna wuu
```

# Technical_info

### 🔹 Glosario de columnas - tabla `technical_info` (según Spotify Audio Features)

- **track_id** → Identificador único de la canción.

- **bpm** → Beats Per Minute: indica la velocidad de la canción. Valores altos → canción rápida.

- **key** → La tonalidad de la canción, representada como un número (0=C, 1=C♯/D♭, 2=D, …, 11=B). Esto indica la nota base sobre la que está la canción.

- **mode** → Modo musical: 0 = menor, 1 = mayor.

- **danceability\_%** → Qué tan bailable es la canción (0-100%). Spotify calcula esto según ritmo, estabilidad, etc.

- **valence\_%** → Positividad de la canción (0 = triste, 100 = feliz).

- **energy\_%** → Intensidad, fuerza o energía de la canción (0-100%).

- **acousticness\_%** → Qué tan acústica es la canción (0-100%).

- **instrumentalness\_%** → Qué tan instrumental es (0 = con voz, 100 = instrumental puro).

- **liveness\_%** → Probabilidad de que la canción haya sido grabada en vivo (0-100%).

- **speechiness\_%** → Presencia de palabras habladas (0 = poca, 100 = habla pura/rap).

## Revisión de valores nulos en **technical info**

```
SELECT
COUNTIF(track_id IS NULL) AS nulos_id,
COUNTIF(bpm IS NULL) AS nulos_bpm,
COUNTIF(`key` IS NULL) AS nulos_key, // 95 nulos
COUNTIF(mode IS NULL) AS nulos_mode,
COUNTIF(`danceability_%` IS NULL) AS nul_dance,
COUNTIF(`valence_%` IS NULL) AS nulos_valence,
COUNTIF(`energy_%` IS NULL) AS nulos_energy,
COUNTIF(`acousticness_%` IS NULL) AS nulos_acous,
COUNTIF(`instrumentalness_%` IS NULL) AS nulos_instr,
COUNTIF(`liveness_%` IS NULL) AS nulos_liv,
COUNTIF(`speechiness_%` IS NULL) AS nulos_speech,
FROM `laboratoria-470421.data_music.technical_info`
```

# Identificar duplicados
