# Problema 1 — Producción de petróleo, gas y agua (campo Volve, Equinor)

**Notebook:** `Problema1_Volve_Produccion.ipynb`. Se ejecuta en Google Colab o en local; descarga los datos solo con `kagglehub`.

## Datos
- Archivo público `Volve production data.xlsx`, con 34 variables: 24 en la hoja diaria y 10 en la hoja mensual.
- Hay 7 pozos. 5 son productores (`WELL_TYPE = OP`) y 2 son inyectores de agua (`WELL_TYPE = WI`: F-4 y F-5).
- Targets: `BORE_OIL_VOL`, `BORE_GAS_VOL` y `BORE_WAT_VOL`.

## Selección de variables
- **Entradas:** horas de operación, presión y temperatura de fondo, presión diferencial del tubing, presión del anular, apertura del estrangulador, presión y temperatura en cabeza, y ΔP del estrangulador.
- **Variables creadas:**
  - Producción de días anteriores: rezagos de 1, 2 y 7 días y media móvil de 7 días.
  - Cambios diarios de las variables operativas.
  - Corte de agua y GOR del día anterior.
  - Días en producción.
  - Código del pozo.
  - Agua inyectada en el campo (`BORE_WI_VOL` de los inyectores, agregada por día).
- **Descartadas:**
  - Códigos y nombres de campo y plataforma, porque son constantes.
  - `AVG_CHOKE_UOM`, porque es constante.
  - Los identificadores, que solo se usan para agrupar.
  - La hoja mensual, porque repite la información diaria agregada por mes.
- `FLOW_KIND` y `WELL_TYPE` se usan para quedarse solo con los pozos productores.

## Modelo
- **Ensemble** (promedio) de LightGBM, Random Forest y Extra Trees, con un modelo para cada target.
- **Objetivo transformado:** se predice el cambio respecto al día anterior, `log(1+y_t) − log(1+y_{t−1})`. Así se evita que los árboles tengan que extrapolar a niveles de producción que no vieron al entrenar.
- **Validación temporal:** el 20 % final de la historia de cada pozo se reserva como test, de modo que no hay fuga de información del futuro.

## Resultados (test temporal, ensemble)

| Target | R² | SMAPE (%) |
|---|---|---|
| BORE_OIL_VOL | 0.978 | 4.44 |
| BORE_GAS_VOL | 0.976 | 4.57 |
| BORE_WAT_VOL | 0.985 | 4.27 |

Los tres targets cumplen **R² > 0.9 y SMAPE < 10**. El ensemble también supera a la línea base de persistencia ("hoy = ayer"), que obtiene R² ≈ 0.95 y SMAPE ≈ 6.2.

**Limitaciones:**
- Es un pronóstico a un día vista.
- Los días con el pozo cerrado (`ON_STREAM_HRS = 0`) se resuelven con una regla: si el pozo no opera, la producción es 0.
