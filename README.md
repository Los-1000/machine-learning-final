# Proyecto final de Machine Learning

Este repositorio contiene dos problemas resueltos como notebooks de Jupyter, preparados para ejecutarse en Google Colab. Cada notebook descarga sus datos automáticamente con `kagglehub`.

| Problema | Notebook | Modelo | Resultado (test) | Requisito |
|---|---|---|---|---|
| 1. Producción de petróleo, gas y agua (campo Volve, Equinor) | [Problema1_Volve_Produccion.ipynb](Problema1_Volve/Problema1_Volve_Produccion.ipynb) | Ensemble: LightGBM + Random Forest + Extra Trees | R² 0.976–0.985 · SMAPE 4.3–4.6 % | R² > 0.9 · SMAPE < 10 ✅ |
| 2. Clasificación de futbolistas (Golden Foot, 22 jugadores) | [Problema2_Clasificacion_Futbolistas.ipynb](Problema2_Futbolistas/Problema2_Clasificacion_Futbolistas.ipynb) | Ensemble: CLIP ViT-L/14 + LogReg y ConvNeXt-Small con fine-tuning | Recall 0.968 · F1 0.968 | Recall > 0.9 · F1 > 0.9 ✅ |

## Contenido
- `Problema1_Volve/` contiene el notebook ya ejecutado y su `INFORME.md`, con la selección de variables, el modelo y los resultados.
- `Problema2_Futbolistas/` contiene el notebook ejecutado en Colab (GPU T4) y su `INFORME.md`.
- `entrega/` contiene los dos archivos comprimidos que se entregan.

## Cómo ejecutar
1. Abre el notebook en Google Colab (*Archivo → Subir notebook*, o desde GitHub).
2. Solo para el problema 2: activa la GPU en *Entorno de ejecución → Cambiar tipo → T4 GPU*.
3. Selecciona *Ejecutar todo*. El problema 1 tarda unos 2 minutos y el problema 2 unos 25 minutos.

## Fuentes de datos
- Volve: https://www.equinor.com/energy/volve-data-sharing. El notebook usa el espejo de Kaggle `lamyalbert/volve-production-data`.
- Golden Foot: https://www.kaggle.com/datasets/balabaskar/golden-foot-football-players-image-dataset
