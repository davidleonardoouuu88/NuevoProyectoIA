# Informalidad laboral juvenil en Bogotá — Un análisis con IA y datos del DANE

Este repositorio mezcla economía, datos y realidad.  
La idea es simple, tomar los microdatos de la GEIH, limpiarlos como debe ser,
analizar qué está pasando con la juventud ocupada en Bogotá y aplicar modelos
de inteligencia artificial para ver qué variables explican la precarización.

No es un ejercicio decorativo.  
Aquí se modela en serio. Aquí se analiza el problema como un fenómeno estructural
y no como un “dato curioso”.  

Con este proyecto vas a encontrar:
- un EDA completo y sin maquillaje,  
- una definición sólida de informalidad,  
- modelos Logit y Random Forest listos para lectura econométrica y predictiva,  
- y una interpretación económica que apunta a política pública de verdad.

## 1. Propósito del proyecto

Este repositorio analiza la informalidad laboral juvenil en Bogotá usando datos
de la GEIH del DANE para 2024.  
Se construye un pipeline sólido que incluye:

- limpieza y depuración real de los microdatos,  
- creación de variables laborales clave,  
- análisis exploratorio profundo,  
- modelos de clasificación (Logit y Random Forest),  
- lectura crítica del mercado laboral juvenil.

La meta es clara: entender qué empuja a un joven a terminar informal
y qué factores predicen esta condición.  
Los resultados se conectan con desigualdad, educación, horas trabajadas y
precarización.

## 2. Estructura del repositorio
proyecto-informalidad-juventud/
│
├── notebooks/
│ ├── 01_EDA_informalidad.ipynb
│ └── 02_Modelos_informalidad.ipynb
│
├── data_raw/
│ ├── Caracteristicas.csv
│ ├── FuerzaTrabajo.csv
│ ├── Hogar.csv
│ └── Ocupados.csv
│
├── data_processed/
│ └── df_final_limpio.csv
│
├── outputs/
│ ├── graficos/
│ └── modelos/
│
└── README.md

```markdown
La idea es mantener siempre la separación entre:
- datos crudos  
- datos limpios  
- notebooks de análisis  
- resultados finales  
## 3. Datos utilizados y variables clave

Los microdatos provienen de la GEIH 2024 (DANE).  
Se usan los módulos:

- Características generales, salud y educación  
- Fuerza de trabajo  
- Hogar y vivienda  
- Ocupados  

La unidad de análisis es la persona ocupada residente en Bogotá.
Las llaves oficiales de unión son:

`DIRECTORIO + SECUENCIA_P + ORDEN (+ HOGAR en algunos módulos)`

Variables centrales detectadas:

- Sexo → P4000  
- Edad → P6040  
- Educación → P6070  
- Ocupación actual → P6240  
- Cotización salud → P6920  
- Cotización pensión → P6940  
- Horas trabajadas → P6800

Estas permiten construir la variable dependiente de informalidad.
## 4. Definiciones y filtros utilizados

### Filtros poblacionales
- Bogotá: `DPTO == 11`
- Jóvenes: `18 ≤ edad ≤ 28`
- Ocupados: `P6240 == 1`

### Construcción de la informalidad

Se define informalidad de acuerdo con criterios del DANE y la OIT:

- No cotiza a salud → `P6920 == 2`
- No cotiza a pensión → `P6940 == 2` o `NaN`

Se crea:
- `informal = 1` si NO cotiza salud y NO cotiza pensión  
- `informal = 0` en caso contrario

### Variables categóricas creadas

- `sexo_cat`: Hombre / Mujer / Otro-NS  
- `edu_cat`: Primaria o menos, Secundaria, Media, Técnica, Universitaria  
- `informal_cat`: Informal / Formal
## 5. Notebook de EDA — Qué se hizo y qué se encontró

El EDA verifica la integridad de la base y luego explora:

- distribución de informalidad  
- informalidad por sexo  
- informalidad según nivel educativo  
- mapa de calor sexo × educación  
- boxplots de horas trabajadas  
- revisión de NA en variables críticas  
- detección de outliers laborales

### Hallazgos principales

- cerca de un tercio de los jóvenes ocupados es informal  
- los hombres presentan mayor informalidad que las mujeres  
- el grupo Otro/NS está en situación de riesgo extremo  
- menor educación implica mayor probabilidad de informalidad  
- incluso jóvenes con educación universitaria tienen informalidad visible  
  → evidencia de subempleo juvenil profesional  
- los informales trabajan más horas y con mayor dispersión  
  aparecen jornadas extremas de más de 80 horas semanales

El EDA muestra un mercado laboral juvenil que absorbe mano de obra en condiciones
de precarización estructural y baja protección social.
## 6. Notebook de modelos — Logit y Random Forest

Después del EDA se entrena el pipeline de IA usando Scikit-learn:

### Variables usadas
- Categóricas: `sexo_cat`, `edu_cat`
- Numéricas: `edad`, `P6800`

### Pipeline
- OneHotEncoder para categóricas  
- StandardScaler para numéricas  
- train–test split con estratificación  

### 6.1 Regresión Logística
Modelo base para inferencia.  
AUC obtenido: ~0.66  
Sirve para interpretar odds y efectos marginales.

### 6.2 Random Forest
Modelo principal: captura relaciones no lineales y patrones complejos.  
AUC obtenido: ~0.73  

Importancia de variables:
1. Horas trabajadas  
2. Edad  
3. Nivel educativo  
4. Sexo  

Lectura clave:  
el riesgo de informalidad se explica más por cómo se trabaja y cuánto se trabaja,
que únicamente por cuánta educación tenga la persona.

Este resultado es consistente con estudios de precarización laboral juvenil
en ciudades latinoamericanas.

## 7. Reproducibilidad

1. Descarga los módulos de la GEIH desde el DANE.  
2. Ubícalos en `data_raw/`.  
3. Ejecuta el notebook `01_EDA_informalidad.ipynb`.  
   - este notebook produce `df_final_limpio.csv`  
4. Ejecuta el notebook `02_Modelos_informalidad.ipynb`.  
   - genera métricas, curvas ROC, importancias y conclusiones  

Todo el flujo está diseñado para ser completamente reproducible.

## 8. Conclusiones del proyecto

- La informalidad juvenil en Bogotá no es marginal: es estructural.  
- La educación reduce riesgo, pero ya no garantiza formalidad.  
- Las brechas por sexo se mantienen: los hombres son más informales.  
- Las identidades no binarias sufren la peor exclusión laboral.  
- Los informales trabajan más horas y con peor estabilidad laboral.  
- Los modelos de IA muestran que las condiciones de trabajo pesan más  
  que la educación misma al explicar la informalidad.  
- Las políticas públicas deben mirar más allá del "estudie y todo mejora":  
  se requieren intervenciones sobre horas, regulación y acceso a seguridad social.  

## 9. Autor

**David Leonardo Martínez Pinzón**  
Economía — Universidad Externado de Colombia  
Proyecto final del curso *Inteligencia Artificial con Aplicaciones en Economía*

Este trabajo combina análisis técnico y lectura crítica del mercado laboral juvenil.
