# Resumen de Limpieza (Parte 4)

Este proceso implementa las operaciones de limpieza solicitadas en el proyecto para dejar el conjunto **listo para análisis**, manteniendo la trazabilidad y sin eliminar información válida (especialmente considerando que **un centro educativo puede tener más de un código** por **nivel, plan o jornada**).

## Decisiones clave y justificación
- **CODIGO**: se conserva como texto y se eliminan **duplicados exactos** de fila (mismo contenido en todas las columnas) para evitar redundancias. **No** se fusionan ni eliminan filas con **códigos distintos**, ya que un mismo centro puede tener varios servicios autorizados (nivel/plan/jornada).
- **ESTABLECIMIENTO**: normalización (mayúsculas, sin acentos, homogeneización de abreviaturas) para **detectar variaciones tipográficas**. Se reportan **sospechas** de duplicados por nombre normalizado, pero **no** se eliminan automáticamente si difieren en código/servicio.
- **DIRECCION**: se homogeneizan abreviaturas (p. ej., `AV.`→`AVENIDA`, `CALZ.`→`CALZADA`) y se corrige el error frecuente **“ZONA O”→“ZONA 0”**. Se eliminan caracteres no informativos.
- **TELEFONO**: se dejan solo **dígitos**; se validan **8 dígitos** (formato Guatemala); múltiples teléfonos se separan con `; `. Los inválidos quedan como `NaN` para tratamiento posterior.
- **DEPARTAMENTO/MUNICIPIO**: mayúsculas y sin acentos. `DEPARTAMENTO` se contrasta con el **catálogo oficial**; se corrigen equivalencias comunes.
- **Categóricas** (`NIVEL`, `SECTOR`, `AREA`, `STATUS`, `MODALIDAD`, `JORNADA`, `PLAN`, `DISTRITO`, `DEPARTAMENTAL`): homogeneización de valores (p. ej., `MAT.`→`MATUTINA`), conservando la semántica institucional.

## Métricas del resultado
- Filas finales: **11985**
- Columnas: **17**
- Duplicados exactos eliminados: **21846**
- Teléfonos no válidos o vacíos (tras limpieza): **1888**

## Chequeos y reportes generados
- **Sospechas de duplicados** por nombre normalizado (`n>1`): `sospechas_duplicados` (CSV y hoja en Excel).
- **Resumen post-limpieza** (nulos, únicos por columna): `resumen_post_limpieza` (CSV y hoja en Excel).
- **Teléfonos inválidos** (NaN tras limpieza): `telefonos_invalidos` (CSV y hoja en Excel).
- **Value counts** de variables categóricas: `value_counts` (CSV y hoja en Excel).
- **Pares raros** DEPARTAMENTO–MUNICIPIO (conteo ≤ 2): `pares_dpto_muni_raros` (CSV y hoja en Excel).

## Notas
- Departamentos fuera de catálogo oficial detectados: CIUDAD CAPITAL
- Los archivos limpios finales se guardaron en:
  - **CSV**: `DATA_COMPLETA_LIMPIA.csv`
  - **XLSX**: `DATA_COMPLETA_LIMPIA.xlsx`
