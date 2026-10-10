# Informe: decisiones de diseño y funcionamiento del sistema

## Objetivo

Sistema de gestión académica que analiza las notas de estudiantes en seis
materias: calcula promedios, determina la situación académica (aprobado /
reprobado) y muestra estadísticas generales. Se implementó de dos formas en
paralelo, **Python vanilla orientado a objetos** y **pandas**, para poder
comparar enfoques y resultados.

## Funcionamiento general

1. **Generación de datos**: se crea `estudiantes.csv` con nombres y apellidos
   aleatorios (`Faker`, localización `es_ES`) y seis notas enteras entre 1 y 10
   (`random`). Si el archivo existe, se sobreescribe.
2. **Lectura y modelado**: cada implementación lee el mismo CSV.
3. **Cálculos**: promedio por estudiante, situación académica, promedio por
   materia, porcentaje de aprobación general, materias de mayor y menor
   rendimiento.
4. **Visualización**: gráfico de barras con el promedio por materia, ordenado
   de mayor a menor (`matplotlib`).

## Decisiones de diseño

- **Clase `Constantes`**: concentra los valores configurables (nota mínima de
  aprobación = 6, ruta del archivo, cantidad de filas, decimales, nombres de
  columnas). Evita "números mágicos" y permite cambiar reglas en un solo lugar.
- **`GeneracionDatos`**: separa la creación de datos de su análisis, de modo
  que el sistema puede trabajar con cualquier CSV con el mismo formato.
- **`EstadoAcademico` (Enum)**: representa la situación académica con valores
  cerrados, en lugar de cadenas sueltas propensas a errores.
- **`Estudiante`**: encapsula nombre, apellido y notas (atributos privados con
  getters). Sabe calcular su propio promedio y su situación, y se imprime con
  un formato legible (`__str__`).
- **`GestionAcademicaVanilla`**: administra la colección de estudiantes y
  resuelve las estadísticas del grupo con estructuras básicas de Python
  (listas, diccionarios, `csv`).
- **`GestionAcademicaPandas`**: replica la misma interfaz (mismos nombres de
  métodos) usando un `DataFrame`, con operaciones vectorizadas (`mean`,
  comparaciones, `idxmax` / `idxmin`). Mantener la misma interfaz facilita la
  comparación directa.

## Comparación de enfoques

| Aspecto | Vanilla (POO) | Pandas |
|---|---|---|
| Claridad del modelo | Alta: cada entidad es un objeto con responsabilidades | Menor: todo se expresa sobre una tabla |
| Cantidad de código | Mayor (bucles y acumuladores explícitos) | Menor (operaciones de una línea) |
| Escalabilidad | Más lento con muchos datos | Eficiente con grandes volúmenes |
| Control sobre las reglas | Total | Depende de la API de la librería |

Con el mismo CSV, ambas implementaciones arrojan los **mismos resultados**
(promedios por materia, porcentaje de aprobación y materias de mayor y menor
rendimiento), lo que valida la corrección de ambas.

## Observaciones

- En la versión pandas, `promedio_asignaturas()` incluye la columna
  `promedio` porque se calcula con `describe()` sobre todo el `DataFrame`.
- Al generarse los datos de forma aleatoria, los valores cambian en cada
  ejecución del notebook.
