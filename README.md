# Proyecto final de Machine Learning

Este repositorio contiene dos problemas resueltos como notebooks de Jupyter, preparados para ejecutarse en Google Colab. Cada notebook descarga sus datos automáticamente con `kagglehub`.

| Problema | Notebook | Modelo | Resultado (test) | Requisito |
|---|---|---|---|---|
| 1. Producción de petróleo, gas y agua (campo Volve, Equinor) | [Problema1_Volve_Produccion.ipynb](Problema1_Volve/Problema1_Volve_Produccion.ipynb) | Ensemble: LightGBM + Random Forest + Extra Trees | R² 0.976–0.985 · SMAPE 4.3–4.6 % | R² > 0.9 · SMAPE < 10 ✅ |
| 2. Clasificación de futbolistas (Golden Foot, 22 jugadores) | [Problema2_Clasificacion_Futbolistas.ipynb](Problema2_Futbolistas/Problema2_Clasificacion_Futbolistas.ipynb) | Ensemble: CLIP ViT-L/14 + LogReg y ConvNeXt-Small con fine-tuning | Recall 0.968 · F1 0.968 | Recall > 0.9 · F1 > 0.9 ✅ |

**Autores:** Carlos Flores, Leonardo de Casanova y Miguel Ángel Mori.

## Informes (formato IEEE Transactions)
- [Informe IEEE: Problema 1 (Volve)](Informes_IEEE/Problema1/informe_volve.pdf)
- [Informe IEEE: Problema 2 (Futbolistas)](Informes_IEEE/Problema2/informe_futbolistas.pdf)

El código fuente LaTeX y las figuras están en `Informes_IEEE/`. Los archivos `Overleaf_*.zip` se pueden subir directamente a Overleaf en *New Project → Upload Project*.

## Contenido
- `Problema1_Volve/` contiene el notebook ya ejecutado, su `INFORME.md` y el PDF IEEE.
- `Problema2_Futbolistas/` contiene el notebook ejecutado en Colab (GPU T4), su `INFORME.md` y el PDF IEEE.
- `Informes_IEEE/` contiene el código `.tex`, las figuras, los PDF y los zips para Overleaf.
- `entrega/` contiene los dos archivos comprimidos que se entregan (notebook, informe y PDF IEEE).

## Cómo ejecutar
1. Abre el notebook en Google Colab (*Archivo → Subir notebook*, o desde GitHub).
2. Solo para el problema 2: activa la GPU en *Entorno de ejecución → Cambiar tipo → T4 GPU*.
3. Selecciona *Ejecutar todo*. El problema 1 tarda unos 2 minutos y el problema 2 unos 25 minutos.

## Fuentes de datos
- Volve: https://www.equinor.com/energy/volve-data-sharing. El notebook usa el espejo de Kaggle `lamyalbert/volve-production-data`.
- Golden Foot: https://www.kaggle.com/datasets/balabaskar/golden-foot-football-players-image-dataset
