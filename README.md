# Análisis de Factores que Inciden en los Resultados del ICFES Saber 11

Proyecto académico desarrollado para la asignatura **Procesamiento de Datos a Gran Escala** — Pontificia Universidad Javeriana, Ingeniería de Sistemas.

**Integrantes:** Carlos Caicedo · Esteban Salazar · Adrián Rojas  
**Docente:** Fabián Pallares Jaimes

---

## Contexto

El puntaje de las pruebas Saber 11 es uno de los indicadores más visibles del desempeño educativo en Colombia, pero rara vez se analiza como lo que realmente es: el resultado acumulado de condiciones territoriales, socioeconómicas y sociales que varían enormemente de un municipio a otro. La brecha entre colegios oficiales y privados es de aproximadamente 31 puntos en promedio nacional, y la diferencia entre Bogotá (276 pts) y Chocó (215 pts) ilustra que el problema no es solo pedagógico.

Este proyecto parte de esa premisa: entender qué factores territoriales determinan el rendimiento en Saber 11, con el fin de construir una base analítica que permita anticipar qué municipios están en riesgo de deterioro académico. El período de análisis cubre de 2015 a 2023.

---

## Objetivo

Identificar los factores territoriales que inciden en el rendimiento académico medido por las pruebas Saber 11, y construir un modelo predictivo que permita al Ministerio de Educación focalizar intervenciones de forma diferenciada según el perfil de cada municipio.

---

## Datos utilizados

El proyecto integra siete fuentes de datos públicas, articuladas por el código DANE o el nombre del municipio:

| Dataset | Fuente | Registros aprox. | Descripción |
|---|---|---|---|
| Resultados Únicos Saber 11 | [datos.gov.co](https://www.datos.gov.co/Educaci-n/Resultados-nicos-Saber-11/kgxf-xxbe) | 7,1 millones | Puntajes por estudiante en cada área (matemáticas, lectura crítica, inglés, ciencias, sociales) y puntaje global. Variable objetivo del proyecto. |
| Estadísticas MEN por Municipio | [datos.gov.co](https://www.datos.gov.co/Educaci-n/MEN_ESTADISTICAS_EN_EDUCACION_EN_PREESCOLAR-B-SICA/nudc-7mev) | 15,7 mil | Indicadores de cobertura, deserción, aprobación y repitencia por nivel educativo, agregados a nivel municipal. |
| Estadísticas MEN por Departamento | [datos.gov.co](https://www.datos.gov.co/Educaci-n/MEN_ESTADISTICAS_EN_EDUCACION_EN_PREESCOLAR-B-SICA/ji8i-4anb) | 462 | Mismos indicadores del MEN municipal pero agregados a nivel departamental, útil para comparaciones de alto nivel. |
| Establecimientos Educativos MEN | [datos.gov.co](https://www.datos.gov.co/Educaci-n/MEN_ESTABLECIMIENTOS_EDUCATIVOS_PREESCOLAR_B-SICA_/cfw5-qzt5) | 588 mil | Registro de instituciones educativas con ubicación, sector (oficial/privado), carácter, matrícula total y número de sedes. |
| Población Matriculada Víctima de Conflicto | [datos.gov.co](https://www.datos.gov.co/Educaci-n/POBLACI-N-MATRICULADA-VICTIMA-DE-CONFLICTO-ARMADO-/k5aq-t8t2) | 693 | Número de estudiantes víctimas del conflicto armado matriculados, desagregado por municipio y género. |
| Módulo de Trabajo Infantil MTI-GEIH 2019 | [microdatos.dane.gov.co](https://microdatos.dane.gov.co/index.php/catalog/664) | 38 mil | Microdatos del DANE sobre actividades laborales y no laborales de niños, niñas y adolescentes, incluyendo horas dedicadas por tipo de actividad. |
| Beneficiarios Estrategia UNIDOS | [datos.gov.co](https://www.datos.gov.co/Inclusi-n-Social-y-Reconciliaci-n/Beneficiarios-Estrategia-UNIDOS/snvf-epj8) | 1,51 millones | Información de hogares en pobreza extrema: puntaje SISBEN, estrato, grupo étnico, acceso a educación, seguridad alimentaria y condiciones de vivienda. |
 
---

## Estructura del repositorio

```
├── Pruebas_saber.py                 # Limpieza, filtros y análisis exploratorio de Saber 11
├── Estadisticas_Municipio.py        # Procesamiento de estadísticas MEN nivel municipal
├── Estadisticas_Departamento.py     # Procesamiento de estadísticas MEN nivel departamental
├── Establecimientos_educativos.py   # Limpieza y agregación de establecimientos por depto
├── Victimas_matriculadas.py         # Transformación del dataset de víctimas matriculadas
├── Beneficiarios.py                 # Curación del dataset UNIDOS
└── Trabajo_Infantil.py              # Procesamiento del módulo MTI-GEIH 2019
```


---

## Pipeline de procesamiento

Cada notebook sigue la misma estructura general:

1. **Lectura** desde la capa BRONZE en formato CSV
2. **Reporte de calidad de datos**: conteo de nulos por columna, detección de duplicados e inconsistencias case-sensitive o por tíldes
3. **Limpieza**: eliminación de columnas irrelevantes, corrección de tipos de dato, tratamiento de nulos (eliminación, imputación por ventana longitudinal, o imputación por forward/backward fill según corresponda)
4. **Transformaciones**: renombrado de columnas con nombres representativos, creación de variables derivadas, filtrado al período 2015–2023 (periodo en que todos los datos se intersectan)
5. **Análisis exploratorio**: agrupaciones, rankings, gráficas



---

## Tecnologías

- **Databricks** (notebooks en Python)
- **pandas + matplotlib** — análisis y visualizaciones exploratorias
- Almacenamiento en capas **BRONZE → SILVER** (arquitectura medallion)

