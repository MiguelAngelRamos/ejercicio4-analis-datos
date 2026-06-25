# TurboEnvíos — Dataset de logística de última milla

**Lógica de negocio.** TurboEnvíos reparte paquetes en una ciudad con 5 zonas. Cada fila es una entrega de un mes (junio 2026). El negocio quiere entender por qué unas zonas son confiables y otras no, qué encarece un envío y qué arruina la nota del cliente.

**Qué responde cada análisis.**
- *Media/mediana/σ/CV por zona* → ¿qué zona es consistente (Centro) y cuál errática (Periferia)? Tienen casi la misma media (~43 min) pero σ muy distinta (≈7 vs ≈22): la dispersión, no el promedio, distingue el servicio.
- *Sesgo + IQR/outliers* → detectar entregas extremas (paquetes retrasados/perdidos) que inflan la media y disparan la cola derecha.
- *Correlación/covarianza* → la distancia encarece el envío (r≈0,78) y alarga el tiempo; a más demora, peor calificación (r≈−0,64).

## Cómo regenerarlo
```bash
python generar_turboenvios.py        # crea turboenvios_entregas.csv (N=50.000)
```
Cambia la constante `N` (probado hasta 1.000.000) y vuelve a ejecutar. `np.random.seed(42)` garantiza reproducibilidad.

## Fenómenos verificados (N=50.000)
| Concepto | Resultado | Objetivo |
|---|---|---|
| Centro: media / σ | 43,5 / **6,8** | media≈Periferia, σ baja |
| Periferia: media / σ | 43,5 / **21,7** | misma media, σ alta |
| Sesgo (skew) tiempo | **4,7** (media 47 > mediana 43) | > 1, cola derecha |
| Outliers IQR (tiempo) | **~5,5%** (2% inyectados + ~3,5% estructurales) | detectables |
| r(distancia, tiempo) | **+0,39** | positiva |
| r(tiempo, calificación) | **−0,64** | ≈ −0,6 |
| r(costo, distancia) | **+0,78** | ≈ 0,8 |
| Nulos en calificación | ~1% | practicar faltantes |

## Nota técnica honesta (vale la pena enseñarla)
Dos requisitos del enunciado están en **tensión matemática** y no pueden cumplirse a la vez de forma literal; el dataset prioriza el primero y documenta el segundo:

1. **r(distancia, tiempo) ≈ 0,8 vs. σ_Periferia ≈ 22.** Para que Periferia tenga σ≈22 hace falta mucha varianza *no explicada por la distancia*; eso topa el r global de distancia↔tiempo. Aquí la dispersión de Periferia se construyó haciendo el tiempo *más sensible a la distancia* (no con ruido puro), lo que es más realista y deja r en **+0,39** en vez de 0,8. La correlación fuerte (≈0,8) sí existe limpia en **costo↔distancia**. Lección: los outliers y la heterogeneidad entre grupos *atenúan* las correlaciones.
2. **Outliers "2%" vs. detección IQR.** Se inyecta exactamente 2% de entregas extremas (120–300 min, solo en zonas secundarias para no contaminar la lección Centro/Periferia), pero el IQR detecta ~5,5% porque la mezcla de zonas con medias y dispersiones distintas genera colas estructurales adicionales. Lección: el IQR sobre una población heterogénea marca más que las anomalías "verdaderas".
