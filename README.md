# EDA de juegos de Steam (2023-2025): ¿cuándo y con qué género lanzar un juego?

Análisis exploratorio de datos (EDA) sobre los juegos publicados en Steam entre 2023 y 2025. El objetivo es responder a una pregunta práctica: **Que juego, de qué genero y lanzado en qué mes, podría tener mas posibilidades de éxito.**

## Resumen de resultados

- **Enero es el mejor mes, pero el efecto es moderado.** En 2023 el 24,1 % de los juegos lanzados en enero superan los 10k propietarios estimados, frente al 19,0 % del resto de meses (p = 0,0006). En 2024 la diferencia es de 18,4 % frente a 13,8 % (p < 0,001). Son unos 5 puntos porcentuales a favor de enero.
- **El análisis conjunto de los tres años está sesgado por la antigüedad.** Los juegos de 2025 han tenido menos tiempo para acumular propietarios, y su tasa de éxito se hunde a partir de junio. Por eso las conclusiones sobre el mes se apoyan en 2023 y 2024, y 2025 se muestra como referencia.
- **RPG es el género con mayor tasa de éxito (19,8 %); Casual, el que menos (8,7 %)**, a pesar de ser el segundo con más lanzamientos. El ranking de géneros se mantiene en cada uno de los tres años.

## Datos

- **Fuente:** [Steam Games Dataset (daily updates), Kaggle](https://www.kaggle.com/datasets/hubertsidorowicz/steam-games-dataset-daily-updates/data).
- **Filtro aplicado:** juegos con fecha de lanzamiento entre 2023 y 2025 (41.677 juegos tras la limpieza: 11.633 en 2023, 15.436 en 2024 y 14.608 en 2025).
- **Variables usadas:** fecha de lanzamiento, propietarios estimados (`estimated_owners`, un rango) y géneros.

>[!NOTE]
> El CSV original es muy pesado y **no está incluido en el repositorio**. Hay que descargarlo de Kaggle y guardarlo en `data/archive/steam_games.csv` para ejecutar el notebook de limpieza.

## Estructura del repositorio

```
.
├── notebooks/
│   ├── 00_Limpieza_Datos.ipynb          # Carga, filtrado, limpieza y exportación
│   ├── 01_Analisis_Univariante.ipynb    # Distribuciones de cada variable
│   ├── 02_Analisis_Bivariante.ipynb     # Mes y género frente al éxito
│   ├── 03_Analisis_Bivariante_II.ipynb  # Contraste estadístico: ¿real o casualidad?
|   └── EDA_Memoria.ipynb                # Conjunto del EDA, combina todos los notebooks y añado comentarios y observaciones.
├── utils/
│   └── utils.py                         # Funciones auxiliares
├── data/
│   ├── juegos_steam.parquet             # datos limpios (generado por el notebook 00)
│   └── juegos_steam.csv                 # datos limpios en CSV (generado por el notebook 00)
└── README.md
```

## Contenido de los notebooks

| Notebook | Qué hace |
|---|---|
| `00_Limpieza_Datos` | Selecciona columnas y años, separa el rango de propietarios en mínimo y máximo, arregla formatos inconsistentes, extrae los géneros (algunos guardados como JSON) a columnas `genre_0`, `genre_1`... y crea la columna `month`. |
| `01_Analisis_Univariante` | Cardinalidad y tipos, frecuencias de géneros, medidas de tendencia central y dispersión, y distribución de los tramos de propietarios. |
| `02_Analisis_Bivariante` | Compara el % de éxito por mes (también con la media del logaritmo de propietarios) y cruza género con mes de lanzamiento. |
| `03_Analisis_Bivariante_II` | Separa por año, contrasta con chi-cuadrado si el mes y el éxito están relacionados, compara enero con el resto y calcula el éxito por género. |

## Metodología

1. **Limpieza y acotado** de los datos (notebook 00).
2. **Análisis univariante** para entender cada variable (notebook 01).
3. **Análisis bivariante** para detectar patrones entre mes, género y éxito (notebook 02).
4. **Contraste de hipótesis** con el test chi-cuadrado de independencia en cada año por separado, comprobando que las frecuencias esperadas son mayores que 5 y reportando el tamaño de la diferencia además del p-valor (notebook 03).

## Limitaciones

- `estimated_owners` es una **estimación en tramos**, no una cifra exacta.
- Los datos de 2025 están **inmaduros**: los juegos recientes no han acumulado propietarios, así que no son comparables con los de años anteriores.
- Un juego puede tener **varios géneros** y por tanto cuenta una vez por cada uno en el análisis por género.
- No se controlan otras variables relevantes (precio, marketing, reseñas). Los resultados describen **asociaciones, no causas**.

## Cómo ejecutarlo

1. Clona el repositorio y descarga el CSV de Kaggle en `data/archive/steam_games.csv`.
2. Instala las dependencias:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy pyarrow jupyter
   ```
3. Ejecuta los notebooks **en orden** (`00` → `03`) desde la carpeta `notebooks/`. El `00` genera los archivos limpios que usan los demás.

---
Amber Cebrián · Bootcamp Data Science 2026
