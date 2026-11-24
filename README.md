# Clasificación Multiescala del Tejido Urbano en el Distrito de Puno mediante Modelos de Árboles y Ensambles

Este repositorio contiene el desarrollo del proyecto de la **Unidad 2** para el curso de **Estructuras de Datos Avanzadas (EDA)**.

## 🎯 Objetivo General
Analizar, modelar y clasificar el espacio urbano del **Distrito de Puno** a dos escalas de resolución espacial:
1. **Nivel Micro (Edificaciones):** Clasificación del uso y tipología de edificaciones a partir de variables morfológicas y métricas de proximidad vial/servicios.
2. **Nivel Macro (Zonificación Urbana):** Clasificación de zonas y uso de suelo mediante la agregación espacial de densidades, vías y equipamientos urbanos.

---

## 🌲 Modelos Implementados
En cada nivel de escala se implementa una comparativa exhaustiva de cuatro modelos basados en árboles y ensambles:
1. **DecisionTreeClassifier:** Evaluación de partición del espacio comparando criterios de impureza (**Gini**) vs. ganancia de información (**Entropía**).
2. **Random Forest Classifier:** Ensamble bagging en paralelo para reducción de varianza y robustez.
3. **XGBoost Classifier:** Boosting secuencial por gradiente con regularización.
4. **LightGBM Classifier:** Boosting optimizado con partición por hojas (*leaf-wise*) y agrupamiento por histogramas.

---

## 📂 Estructura del Repositorio
* **`PROJECT_CONTEXT.md`:** Documento de memoria técnica del proyecto y directrices para asistentes de IA.
* **`PROGRESS.md`:** Registro del estado actual y tareas completadas.
* **`Data/`:** Dataset original de OpenStreetMap (Perú).
* **`data_puno/`:** Datos vectoriales recortados y optimizados exclusivamente para el Distrito de Puno.
* **`figures/`:** Gráficas y mapas exportados en alta resolución (`300 DPI`) listos para el informe y la exposición.
* **`notebooks/`:** Cuadernos Jupyter explicados paso a paso (`01` a `04`) para el análisis y modelado.
* **`Clasificacion_Urbana_Puno_Colab.ipynb`:** Cuaderno unificado listo para ejecutar con 1 clic en **Google Colab** cargando **`data_puno_colab.zip`** (`614 KB`) desde **Google Drive**.
* **`Informe/`:** Informe académico completo en **LaTeX** (`main.tex`, figuras y `Informe_Overleaf.zip` listo para compilar en Overleaf) basado en `Template.pdf`.

---

## 🚀 Requisitos e Instalación
Requisitos en Python 3.13+:
```bash
pip install geopandas shapely pyogrio scipy scikit-learn xgboost lightgbm matplotlib seaborn folium ipykernel
```
