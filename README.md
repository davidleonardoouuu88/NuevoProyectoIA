# README – Proyecto “Informalidad juvenil en Bogotá / Chapinero”

## 1. Descripción general  
Este proyecto analiza la persistencia de la informalidad laboral entre jóvenes en Bogotá, con énfasis en Chapinero. Se combinan fuentes como la **GEIH del DANE (2024)** y la **Encuesta de Demanda Laboral (2020–2021)** para contrastar oferta educativa con demanda empresarial.  

La hipótesis central:  
**Si incluso en Chapinero, con su concentración universitaria, la informalidad persiste, entonces el problema no es la educación sino la estructura del mercado laboral.**

---

## 2. Variables del modelo  

### Variable dependiente (target)  
- **`informal`**  
  - Binaria:  
    - `1 = informal` (no cotiza salud ni pensión).  
    - `0 = formal`.  

### Variables independientes (features)  
1. **Sexo – `P4000`**  
   - Categórica nominal:  
     - `1 = Hombre`  
     - `2 = Mujer`  
     - `3, 4 = valores residuales / outliers`.  
   - Codificada en dummies → `P4000_1`, `P4000_2`, etc.  

2. **Nivel educativo – `P6070` (recodificada en `educ_cat`)**  
   - Categórica ordinal:  
     - Sin/Primaria baja  
     - Primaria completa  
     - Secundaria  
     - Media  
     - Técnica/Tecnológica  
     - Universitaria o más  
   - Codificada en dummies → `educ_cat_Secundaria`, `educ_cat_Media`, etc.  

---

## 3. Estructura de carpetas
ERstructuras desde Colab/Drive y este github


---

## 4. Proceso de limpieza
1. Se unificaron los módulos GEIH (características generales, fuerza de trabajo y hogar).
2. Se filtró Bogotá (`DPTO = 11`) y población joven (18–28 años).
3. Se identificaron ocupados (`CLASE` / `P6240`).
4. Se creó la variable `informal` usando cotización a salud y pensión.
5. Se seleccionaron las variables `P4000` (sexo) y `P6070` (educación).
6. Se recodificaron categorías y se aplicó `OneHotEncoder`.
7. Se entrenaron modelos de Regresión Logística y Random Forest.
8. Se evaluaron con curvas ROC, Odds Ratios, Importancias Gini y SHAP.

---

## 5. Ecuaciones de los modelos

### Regresión Logística
\[P(\text{informal}=1|X) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 P4000 + \beta_2 P6070)}}\]
Los coeficientes β1 y β2 representan el efecto del sexo y la educación sobre la probabilidad de informalidad.  
El **Odds Ratio** se obtiene como: OR = exp(β).

### Random Forest
\[\hat{y} = \frac{1}{T} \sum_{t=1}^T h_t(X)\]
Cada árbol \(h_t(X)\) predice informalidad o formalidad; el modelo promedia todos los árboles para decidir la clase final.

### Curva ROC / AUC
\[
AUC = \int_0^1 TPR(FPR^{-1}(x)) \, dx
\]
Mide la capacidad del modelo para distinguir entre trabajadores formales e informales.

---

## 6. Resultados clave
- La informalidad cae con la educación, pero incluso los universitarios presentan más del 60%.
- Las mujeres muestran mayores tasas de informalidad en todos los niveles educativos.
- Logit y Random Forest confirman que sexo y educación influyen, pero no explican todo.
- Los valores SHAP muestran que el sexo y la educación interactúan: la brecha de género se mantiene incluso en niveles educativos altos.

---

## 7. Calidad de los datos
- En `P4000` se detectaron valores residuales (3, 4), considerados outliers mínimos.
- En `P6070` hubo algunos valores nulos (`NaN`), recodificados adecuadamente.
- Las inconsistencias se mantuvieron visibles por transparencia, sin afectar resultados.

---

## 8. Próximos pasos
- Comparar la oferta (GEIH) con la demanda empresarial (EDL Chapinero).
- Realizar un EDA detallado por variable: sexo, educación e informalidad.
- Extender el análisis a estrato socioeconómico y sector económico.

---

## 9. Créditos
Proyecto desarrollado por **David Leonardo Martinez Pinzon**  
Universidad Externado de Colombia – 2025  
Fecha de última versión: **2 de Octubre del 2025**

---



