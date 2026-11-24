# Contexto del Proyecto: Clasificación Multiescala del Tejido Urbano de Puno

> **Nota para cualquier Asistente IA (ChatGPT, Claude, Gemini, Cursor, Antigravity, etc.):**  
> Lee este archivo antes de realizar cualquier acción en este repositorio. Evita escanear los archivos originales de la carpeta `Data/` (pesan ~1.5 GB y contienen todo el Perú). Trabaja siempre con los datos procesados en `data_puno/`.

---

## 1. Identificación y Objetivos Académicos
* **Curso:** Estructuras de Datos Avanzadas (EDA) — Unidad 2.
* **Área Geográfica de Estudio:** Distrito de Puno, Provincia de Puno, Departamento de Puno, Perú.
* **Problema a Resolver:** Modelado y clasificación supervisada multiescala del espacio urbano en el Distrito de Puno mediante algoritmos de partición del espacio (árboles de decisión y ensambles).
* **Enfoque Multiescala:**
  1. **Nivel Micro (Edificaciones):** Clasificar tipologías de construcción individual (`gis_osm_buildings_a_free_1`) según morfología y proximidad espacial.
  2. **Nivel Macro (Zonificación / Suelo):** Clasificar zonas de uso de suelo (`gis_osm_landuse_a_free_1` / cuadrantes urbanos) mediante agregación espacial.

---

## 2. Los 4 Modelos Obligatorios Requeridos
El proyecto exige implementar, comparar y evaluar rigurosamente los siguientes 4 modelos:

| # | Modelo | Tipo / Paradigma | Aspectos Clave a Evaluar |
|---|---|---|---|
| 1 | **DecisionTreeClassifier** | Árbol Simple (Partición del espacio) | Comparación explícita de criterios de división: **Gini** vs. **Entropía** (Information Gain). Poda de profundidad (`max_depth`), visualización gráfica del árbol de decisión. |
| 2 | **Random Forest** | Ensamble Bagging en paralelo | Reducción de varianza, promediado de árboles, `n_estimators`, análisis de *Feature Importance* (MDI). |
| 3 | **XGBoost** (`XGBClassifier`) | Boosting secuencial por gradiente | Corrección de errores residuales, `learning_rate`, regularización L1/L2, `max_depth`. |
| 4 | **LightGBM** (`LGBMClassifier`) | Boosting basado en histogramas | Crecimiento por hojas (*leaf-wise*), velocidad extrema, eficiencia en memoria. |

### Entregables del Modelado
1. **Tabla comparativa de métricas:** Accuracy, Precision (Macro/Weighted), Recall, F1-Score y Tiempo de cómputo.
2. **Matrices de confusión** para cada modelo.
3. **Análisis de Feature Importance** (¿qué variables geométricas o de distancia pesan más en cada escala?).
4. **Visualizaciones interactivas** en mapas (Folium / Geopandas) con los aciertos y discrepancias.

---

## 3. Estructura de Directorios

```text
Zonas_Puno/
│
├── Data/                              <- [NO MODIFICAR] Shapefiles crudos de Perú descargados de Geofabrik/OSM (~1.5 GB).
│
├── data_puno/                         <- Shapefiles, GeoJSON, CSVs y Mapas HTML exclusivos del Distrito de Puno.
│   ├── puno_distrito_boundary.geojson <- Polígono administrativo de Puno (admin_level8, osm_id: 1913513).
│   ├── puno_buildings.geojson         <- Edificaciones recortadas en Puno (652 registros).
│   ├── puno_landuse.geojson           <- Usos de suelo recortados en Puno (273 registros).
│   ├── puno_roads.geojson             <- Red vial dentro de Puno (4,276 tramos, 1,114.2 km).
│   ├── puno_pois.geojson              <- Puntos de interés (935 registros).
│   ├── dataset_micro_edificaciones.csv<- Dataset tabular Nivel Micro (652 filas, 11 features + etiquetas).
│   ├── puno_buildings_clasificados.geojson <- Las 652 edificaciones con clase final (184 verificadas + 468 predichas).
│   ├── resultados_micro_modelos.csv   <- Tabla de métricas comparativas de los 4 modelos (Nivel Micro).
│   ├── mapa_micro_edificaciones.html  <- Mapa interactivo Nivel Micro (edificaciones clasificadas).
│   ├── puno_pois_a.geojson            <- Polígonos de POIs en Puno (330 registros).
│   ├── puno_transport.geojson         <- Paraderos y transporte en Puno (48 registros).
│   ├── puno_water_a.geojson           <- Cuerpos de agua / Lago Titicaca (9 registros).
│   ├── puno_waterways.geojson         <- Quebradas y ríos en Puno (30 registros).
│   ├── dataset_macro_zonificacion.csv <- Dataset tabular Nivel Macro (2,100 celdas activas, 17 features + etiquetas).
│   ├── puno_zonificacion_macro.geojson<- Grilla 250m×250m clasificada (1,009 verificadas + 1,091 predichas).
│   ├── resultados_macro_modelos.csv   <- Tabla de métricas comparativas de los 4 modelos (Nivel Macro).
│   ├── mapa_macro_zonificacion.html   <- Mapa interactivo Nivel Macro (zonificación urbana y rural).
│   ├── comparativa_global_modelos.csv <- Tabla consolidada Micro vs. Macro (CV-5, Test y tiempos).
│   ├── comparativa_global_detallada.csv <- Métricas detalladas en Test (Macro y Weighted) en ambas escalas.
│   └── mapa_final_multiescala_puno.html <- Mapa interactivo integrado (Zonificación + Edificaciones + Aciertos/Discrepancias).
│
├── figures/                           <- Figuras de alta resolución (300 DPI) listas para el informe y la exposición.
│   ├── 01_esquema_datasets_puno.png   <- Ficha técnica visual de variables (X) y variable objetivo (y) Micro/Macro.
│   ├── 01_radiografia_datos_puno.png  <- Panel 2×2 de diagnóstico y distribución de datos crudos OSM.
│   ├── 01_mapa_cartografico_puno.png  <- Mapa cartográfico 2 paneles (Distrito 460.4 km² + zoom núcleo urbano crudo).
│   ├── 02_flujo_validacion_micro.png  <- Diagrama visual Train 80% / Test 20% / 5-Fold CV vs. Imputación (468).
│   ├── 02_arbol_gini_completo.png     <- Árbol completo DecisionTree Gini (prof. 4, 11 hojas, sin recortes).
│   ├── 02_arbol_entropia_completo.png <- Árbol completo DecisionTree Entropía (prof. 4, 11 hojas, sin recortes).
│   ├── 02_arbol_rf_001_completo.png   <- Árbol #1 completo de Random Forest (prof. 7, 22 hojas).
│   ├── 02_arbol_rf_200_completo.png   <- Árbol #200 completo de Random Forest (último del bosque, 23 hojas).
│   ├── 02_arbol_xgboost_ronda1_completo.png   <- Árbol completo XGBoost Ronda 1 (level-wise, 11 hojas).
│   ├── 02_arbol_xgboost_ronda150_completo.png <- Árbol completo XGBoost Ronda Final #150 (convergencia, 6 hojas).
│   ├── 02_arbol_lightgbm_ronda2_completo.png  <- Árbol completo LightGBM Ronda 2 (leaf-wise, 8 hojas).
│   ├── 02_arbol_lightgbm_ronda150_completo.png<- Árbol completo LightGBM Ronda Final #150 (leaf-wise, 7 hojas).
│   ├── 02_matrices_confusion_micro.png<- Matrices de confusión en Test (n=37) con Accuracy y F1.
│   ├── 02_feature_importance_micro.png<- Importancia relativa de las 11 variables (Nivel Micro).
│   ├── 03_grilla_espacial_macro.png   <- Mapa de grilla 250m×250m (2,100 celdas) + desbalanceo de las 4 clases.
│   ├── 03_flujo_validacion_macro.png  <- Diagrama visual Train 80% / Test 20% + balanceo vs. Imputación (1,091).
│   ├── 03_arbol_gini_completo.png     <- Árbol completo DecisionTree Gini Macro (prof. 6, 27 hojas).
│   ├── 03_arbol_entropia_completo.png <- Árbol completo DecisionTree Entropía Macro (prof. 6, 28 hojas).
│   ├── 03_arbol_rf_001_completo.png   <- Árbol #1 completo de Random Forest Macro (77 hojas).
│   ├── 03_arbol_rf_250_completo.png   <- Árbol #250 completo de Random Forest Macro (último del bosque, 73 hojas).
│   ├── 03_arbol_xgboost_ronda1_completo.png   <- Árbol completo XGBoost Macro Ronda 1 (level-wise, 16 hojas).
│   ├── 03_arbol_xgboost_ronda200_completo.png <- Árbol completo XGBoost Macro Ronda #200 (13 hojas).
│   ├── 03_arbol_lightgbm_ronda2_completo.png  <- Árbol completo LightGBM Macro Ronda 2 (leaf-wise, 23 hojas).
│   ├── 03_arbol_lightgbm_ronda200_completo.png<- Árbol completo LightGBM Macro Ronda #200 (leaf-wise, 10 hojas).
│   ├── 03_matrices_confusion_macro.png<- Matrices de confusión 4×4 en Test (n=202) con Accuracy y F1.
│   ├── 03_feature_importance_macro.png<- Importancia relativa de las 17 variables (Nivel Macro).
│   ├── 04_comparativa_global_modelos.png   <- Panel 1×3: CV-5 F1, Test Accuracy y Trade-off Precisión vs. Velocidad.
│   ├── 04_gini_vs_entropia_curvas.png      <- Curvas Train vs. CV-5 (max_depth 2..12) marcando zona de overfitting.
│   ├── 04_acuerdo_modelos_kappa.png        <- Matrices de concordancia entre paradigmas (Cohen's Kappa κ).
│   ├── 04_aciertos_discrepancias_test.png  <- Distribución de aciertos unánimes (4/4), discrepancias y errores (0/4).
│   ├── 04_coherencia_multiescala.png       <- Cruce espacial Micro ↔ Macro (652 edificios × 4 zonas).
│   └── 04_mapa_final_multiescala.png       <- Lámina cartográfica final clasificada (Macro distrital + zoom Micro+Macro).
│
├── notebooks/                         <- Cuadernos Jupyter (.ipynb) documentados paso a paso.
│   ├── 01_delimitacion_puno.ipynb     <- Extracción, explicación Shapefile, fichas de datos y mapas base (pulido).
│   ├── 02_micro_edificaciones.ipynb   <- Feature engineering, matemática, árboles visuales de los 4 modelos y validación (pulido).
│   ├── 03_macro_zonificacion.ipynb    <- Grilla 250m×250m, balanceo matemático, 8 árboles completos y mapa Folium mejorado (pulido).
│   └── 04_evaluacion_comparativa.ipynb<- Comparativa global, Gini vs Entropía, acuerdos, lámina final y mapa integrado (pulido).
│
├── Clasificacion_Urbana_Puno_Colab.ipynb <- Cuaderno unificado ejecutable con 1 clic en Google Colab (carga desde Google Drive).
├── data_puno_colab.zip                <- Paquete comprimido ligero (614 KB) con las 9 capas GeoJSON de Puno para subir a Drive.
├── Informe/                           <- Informe final en LaTeX basado en Template.pdf (sin código, tablas adjustbox, APA).
│   ├── main.tex                       <- Documento LaTeX completo (Sec. 1–6 + Referencias, 100% compilable en Overleaf).
│   ├── figures/                       <- Copia autocontenida de las 37 figuras (300 DPI + capturas Micro y Macro).
│   └── Informe_Overleaf.zip           <- Paquete listo para subir directamente a Overleaf (1 clic).
├── Template.pdf                       <- Plantilla académica de referencia para el informe.
├── PROJECT_CONTEXT.md                 <- Este archivo (memoria y contexto principal).
├── PROGRESS.md                        <- Bitácora de avance y estado actual.
└── README.md                          <- Portada general del proyecto.
```

---

## 4. Origen y Capas de Datos (OpenStreetMap - Geofabrik)
* **Límite Administrativo de Puno:** Capa `gis_osm_adminareas_a_free_1`, filtro: `name == 'Puno'` y `fclass == 'admin_level8'` (OSM ID: 1913513).
* **Edificaciones:** `gis_osm_buildings_a_free_1` (652 edificaciones en Puno: viviendas, escuelas, iglesias, hoteles, etc.).
* **Uso de Suelo:** `gis_osm_landuse_a_free_1` (273 polígonos: residencial, parques, áreas verdes, comercial, etc.).
* **Red Vial:** `gis_osm_roads_free_1`.
* **Servicios / POIs:** `gis_osm_pois_free_1` y `gis_osm_pois_a_free_1`.

---

## 5. Entorno de Ejecución
* **Python:** 3.13+
* **Librerías principales instaladas:**
  * `geopandas`, `shapely`, `pyogrio` (Procesamiento geoespacial).
  * `scipy` (`cKDTree` para consultas espaciales en $O(\log N)$).
  * `scikit-learn` (DecisionTreeClassifier, RandomForestClassifier, métricas).
  * `xgboost` (XGBClassifier).
  * `lightgbm` (LGBMClassifier).
  * `matplotlib`, `seaborn` (Gráficos estadísticos y árboles).
  * `folium` (Mapas interactivos usando `tiles="OpenStreetMap"` — nota: no usar CartoDB sin API key).
  * `ipykernel` (Soporte para Jupyter Notebooks).

---

## 6. Decisiones Técnicas y Resultados Registrados

### Convenciones Geoespaciales
* **Proyección métrica oficial:** `EPSG:32719` (WGS 84 / UTM Zone 19S) para calcular áreas ($m^2$) y distancias en metros.
* **Proyección de exportación/mapas:** `EPSG:4326` (WGS 84) para GeoJSON y Folium.
* **Punto central de referencia:** Plaza de Armas de Puno (`lat = -15.8402`, `lon = -70.0280`).

### Nivel Micro — Edificaciones (`02_micro_edificaciones.ipynb`) ✅ Completado
* **Estrategia de Etiquetado (3 Clases Urbanas):** De 652 edificaciones, se verificaron **184** combinando `name`, `type` y POIs internos (`Residencial`: 73, `Institucional`: 60, `Comercial_Servicios`: 51). Las **468** restantes (sin etiqueta o ambiguas como `roof`) se imputaron con el modelo ganador.
* **11 Features ($X$):** `area_m2`, `perimetro_m`, `compacidad` (Polsby-Popper), `relacion_aspecto`, `num_vertices`, `dist_plaza_armas_m`, `dist_via_cercana_m`, `jerarquia_via`, `densidad_edificios_150m` (`cKDTree`), `dist_vecino_cercano_m` (`cKDTree`), `pois_entorno_300m` (`cKDTree`, excluyendo POIs internos con distancia $\le 1\text{ m}$ para evitar *data leakage*).
* **Benchmark de Resultados (Train/Test 80/20 estratificado + 5-Fold Stratified CV):**

| Modelo | Test Acc | Test F1 (Macro) | CV-5 Acc | CV-5 F1 (Macro) | Tiempo Train (ms) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **LightGBM** *(Ganador CV)* | 0.8108 | 0.7874 | **0.7667** | **0.7496** | 92.63 |
| **Random Forest** *(Ganador Test)* | **0.8378** | **0.8115** | 0.7664 | 0.7465 | 1224.15 |
| **XGBoost** | 0.7838 | 0.7545 | 0.7395 | 0.7160 | 606.56 |
| **DecisionTree (Entropía)** | 0.6757 | 0.6204 | 0.7009 | 0.6744 | 8.96 |
| **DecisionTree (Gini)** | 0.8108 | 0.7831 | 0.6793 | 0.6517 | **7.46** |

* **Distribución Final Predicha (652 edificios):** `Institucional`: 287 (60 verif. + 227 pred.), `Comercial_Servicios`: 195 (51 verif. + 144 pred.), `Residencial`: 170 (73 verif. + 97 pred.).
* **Insumo para el Paso 3 (Nivel Macro):** `data_puno/puno_buildings_clasificados.geojson` ya contiene todas las edificaciones clasificadas y sus áreas en $m^2$ para enriquecer la agregación zonal.

### Nivel Macro — Zonificación Urbana (`03_macro_zonificacion.ipynb`) ✅ Completado
* **Estrategia de Grilla y Etiquetado (4 Clases):** Grilla espacial regular de $250\text{ m} \times 250\text{ m}$ ($6.25\text{ ha}$ por celda) sobre el distrito (7,657 celdas totales; **2,100 celdas activas**). Se etiquetaron **1,009** celdas verificadas únicamente desde capas poligonales (`puno_landuse` y `puno_pois_a`): `Urbano_Residencial` (566), `Rural_Agricola_Natural` (167), `Equipamiento_Productivo` (149) y `Areas_Verdes_Recreacion` (127). Las **1,091** celdas activas sin etiqueta en OSM fueron imputadas con el modelo ganador.
* **17 Features ($X$, cero fuga de datos):**
  * **Puente Nivel Micro (Paso 2):** `num_edificios`, `pct_area_construida`, `edificios_residenciales`, `edificios_comerciales`, `edificios_institucionales`.
  * **Red Vial:** `longitud_vias_m`, `num_tramos_via`, `pct_vias_arteriales`, `pct_vias_rurales_peat`, `dist_via_principal_m`.
  * **Servicios y Transporte (`cKDTree`):** `num_pois_celda`, `num_pois_500m`, `dist_poi_cercano_m`, `dist_transporte_m`.
  * **Contexto Geográfico e Hídrico:** `dist_plaza_armas_m`, `dist_lago_agua_m`, `dist_quebrada_rio_m`.
* **Benchmark de Resultados (Train/Test 80/20 estratificado + 5-Fold Stratified CV, con balanceo de clases):**

| Modelo | Test Acc | Test Bal. Acc | Test F1 (Macro) | CV-5 Acc | CV-5 F1 (Macro) | Tiempo Train (ms) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Random Forest** *(Ganador CV)* | 0.7426 | 0.7023 | 0.6803 | 0.7681 | **0.6980** | 449.01 |
| **XGBoost** *(Ganador Test)* | **0.7772** | **0.7189** | **0.7120** | **0.7760** | 0.6977 | 754.24 |
| **LightGBM** | 0.7574 | 0.6728 | 0.6670 | 0.7691 | 0.6767 | 282.91 |
| **DecisionTree (Gini)** | 0.6535 | 0.6221 | 0.5996 | 0.6809 | 0.6318 | **5.24** |
| **DecisionTree (Entropía)** | 0.6634 | 0.6205 | 0.5841 | 0.6671 | 0.5999 | 9.25 |

* **Distribución Final (2,100 celdas activas):** `Urbano_Residencial`: 970 (566 verif. + 404 pred.), `Rural_Agricola_Natural`: 799 (167 verif. + 632 pred.), `Equipamiento_Productivo`: 194 (149 verif. + 45 pred.), `Areas_Verdes_Recreacion`: 137 (127 verif. + 10 pred.).
* **Insumo para el Paso 4 (Comparativa Global):** `data_puno/resultados_macro_modelos.csv`, `data_puno/puno_zonificacion_macro.geojson` y `data_puno/mapa_macro_zonificacion.html`.

### Evaluación Comparativa Global (`04_evaluacion_comparativa.ipynb`) ✅ Completado
* **Gini vs. Entropía (`max_depth` óptimo):** En Nivel Micro ambos alcanzan óptimo en `max_depth = 4` (11 hojas); en Nivel Macro ambos en `max_depth = 6` (27–28 hojas), con Gini ~2.4× más rápido (`5.5 ms` vs `13.0 ms`) y mejor F1 en Test (`0.5996` vs `0.5841`).
* **Aciertos y Discrepancias en Test (4 familias: DT Gini, RF, XGB, LGBM):**
  * **Nivel Micro (`n = 37`):** `73.0%` Acierto Unánime (27), `21.6%` Discrepancia entre Modelos (8), `5.4%` Error Unánime (2).
  * **Nivel Macro (`n = 202`):** `56.9%` Acierto Unánime (115), `29.2%` Discrepancia entre Modelos (59), `13.9%` Error Unánime (28).
* **Coherencia Multiescala (652/652 edificaciones cruzadas con la grilla Macro):** `Equipamiento_Productivo` concentra 444 edificios (42.6% Institucional, 30.0% Comercial/Servicios), `Areas_Verdes_Recreacion` 140, `Urbano_Residencial` 43 y `Rural_Agricola_Natural` 25.
* **Entregables Finales:** `comparativa_global_modelos.csv`, `comparativa_global_detallada.csv` y `mapa_final_multiescala_puno.html`.
