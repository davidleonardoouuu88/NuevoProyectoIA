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

