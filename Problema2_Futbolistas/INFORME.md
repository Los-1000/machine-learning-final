# Problema 2 — Clasificación de imágenes de futbolistas (Golden Foot)

**Notebook:** `Problema2_Clasificacion_Futbolistas.ipynb`. Se ejecuta en Google Colab con GPU T4 y tarda unos 20–30 minutos. Descarga el dataset de Kaggle con `kagglehub`.

## Datos
- 22 futbolistas y unas 7 190 imágenes, organizadas en un directorio por jugador.
- El CSV (`folder_name`, `filename`) relaciona cada imagen con su jugador.
- **Limpieza:**
  - Se descarta 1 fila vacía del CSV.
  - Se eliminan 7 imágenes idénticas que estaban en carpetas de jugadores distintos (etiqueta contradictoria).
  - Se eliminan los duplicados exactos, comparando por hash MD5.
  - Quedan 7 174 imágenes.
- **Partición estratificada:** 70 % entrenamiento, 15 % validación y 15 % prueba (5 021 / 1 076 / 1 077 imágenes).

## Modelo en conjunto (ensemble)
1. **CLIP ViT-L/14 + regresión logística.** El codificador de imágenes de CLIP está preentrenado con 400 M de pares imagen-texto y se usa congelado. Se extrae un embedding por imagen, promediado con el de su reflejo horizontal, y sobre esos embeddings se entrena una regresión logística multinomial. El parámetro C se elige en validación.
2. **ConvNeXt-Small (ImageNet-22k) con fine-tuning completo.** Se entrena con data augmentation, AdamW, ciclo de LR coseno, label smoothing y precisión mixta. Se conserva la época con mejor F1 macro en validación.
3. **Soft voting ponderado:** `P = w·P_CLIP + (1−w)·P_ConvNeXt`. El peso `w` se elige en validación.

Como referencia, también se reporta CLIP zero-shot, que clasifica solo con texto, sin entrenar.

## Evaluación
- Se reportan Accuracy, Precision, Recall y F1 en promedio macro sobre el conjunto de prueba.
- También se incluyen el reporte por clase, la matriz de confusión y ejemplos mal clasificados.
- Requisito: **Recall macro > 0.9 y F1 macro > 0.9**. El notebook imprime CUMPLE / NO CUMPLE para cada uno.

## Resultados (conjunto de prueba, 1 077 imágenes; ejecutado en Colab con GPU Tesla T4)

| Modelo | Accuracy | Precision (macro) | Recall (macro) | F1 (macro) |
|---|---|---|---|---|
| A. CLIP ViT-L/14 + LogReg (C=50) | 0.961 | 0.960 | 0.959 | 0.959 |
| B. ConvNeXt-Small con fine-tuning (mejor época: 8) | 0.936 | 0.937 | 0.933 | 0.934 |
| **ENSEMBLE (w = 0.55)** | **0.970** | **0.970** | **0.968** | **0.968** |
| Referencia: CLIP zero-shot | 0.913 | 0.917 | 0.910 | 0.910 |

- El ensemble **cumple los dos requisitos**: Recall = 0.968 y F1 = 0.968, ambos > 0.9.
- El ensemble supera a cada modelo por separado. Esto confirma que CLIP y ConvNeXt cometen errores distintos y se complementan.
- Por clase, el recall va de 0.889 (Pavel Nedved) a 1.000 (Iniesta, Cavani, Casillas, Modric, Salah, entre otros).
- Las clases más difíciles son Nedved, Ronaldo Nazário, Pelé y Maradona. Son las que tienen menos imágenes, y muchas son fotos antiguas o en blanco y negro.
- Hubo 32 errores en 1 077 imágenes de prueba (3 %).
