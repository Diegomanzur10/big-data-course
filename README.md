# Big Data Course

Repositorio con materiales de un curso introductorio/práctico de Big Data: fundamentos de SQL para análisis y transición hacia procesamiento distribuido con PySpark.

## Estructura

- `SQL/` Clases 1–4: Introducción a consultas, agregaciones (GROUP BY, funciones matemáticas), JOINS y manejo de datos con Pandas / series de tiempo.
- `PySpark/` Clases 5–8: Primeros pasos con Spark, filtrado y transformación, optimización/tuning básico y operaciones adicionales en RDD/DataFrames.

Cada carpeta de clase contiene notebooks (`.ipynb`) y, donde aplica, una pequeña base SQLite (`demo.db3`) para ejercicios prácticos.

## Objetivos del curso

1. Reforzar fundamentos de manipulación y consulta de datos con SQL.
2. Entender cuándo escalar de procesamiento local (Pandas) a distribuido (Spark).
3. Practicar transformaciones y acciones en PySpark, junto con nociones iniciales de performance.

## Requisitos sugeridos

- Python 3.9+ y Jupyter Lab/Notebook.
- Entorno con Spark (local con `pyspark` o imagen Docker `jupyter/pyspark-notebook`).
- Conocimientos básicos de SQL y Python.

## Uso rápido

Clonar el repo y abrir los notebooks:

```bash
git clone <URL_DEL_REPO>
cd big-data-course
jupyter lab
```