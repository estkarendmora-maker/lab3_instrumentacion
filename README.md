# lab3_instrumentacion
hola

## 12. Resultados de la práctica

### Metodología de captura
Con el sensor MAX30102 se adquirio la señal PPG con una fs de 100 Hz durante 150 s, divididos en tres etapas:primero se estabilizo el sensor en el tiempo 0 - 5 s, despues se realiza una calibración donde se fija un rango de normalización del SPI en el tiempo 5 - 30 s, y por ultimo se monitorio el SPI en timepo real en el tiempo 30 - 150 s con una duracion de monitoria de 120 s (2 min)

Los valores basales obtenidos al final de la calibración fueron:
- PPGA basal = 602.43
- HBI basal = 1.060 s
- FC basal = 56.6 bpm

### Estadísticas generales de la captura

| Variable | Promedio | Mediana | Mínimo | Máximo |
|---|---|---|---|---|
| FC (bpm) | 63.9 | 60.6 | 46.5 | 92.3 |
| HBI (s) | 0.973 | 0.990 | — | — |
| PPGA (u.a.) | 458.97 | 371.22 | — | — |
| SPI instantáneo | 63.5 | 72.6 | 0.0 | 100.0 |
| SPI (mediana móvil 15 s) | 65.0 | 72.6 | 8.9 | 100.0 |

Se detectaron:
- 151 picos sistólicos
- 151 valles
- 148 intervalos HBI válidos

### Evolución temporal del SPI
Segun las graficas podemos observar tres fases:

1. **Reposo inicial 30-65 s:** el SPI se mantiene relativamente bajo y variable, oscilando mayormente entre 0 y 65 con PPGA alto de 400-1100 con esto se puede decir que conicide con  un estado basal sin estrés nociceptivo.
2. **Estímulo 70-110 s:** el SPI es alto con un valor de 100 indicando una mayor respuesta nociceptica causado una respuesta simpatica como el estres, el PPGA baja a casi 100
3. **Recuperación (113-150 s):** el PPGA progresivamente va aumentando incluso superando el valor basal inicial llegando a 1400, lo que se interpreta como una vasodilatación aumentando el flujo sanguineo tras retirar el estímulo. El SPI se vuelve mas negativo llegando a valores cercanos a 0.

La correlación entre los bajos valores de PPGA y altos valores de SPI durante el estimulo y su recuperación cambaindo los valores. Es el comportamiento esperado, el cual coincide con el rango de 20 - 50 objetivo para una anagesia intraoperatoria adecuada.


### Detección de picos
Las Imágenes 3 y 5 muestran los máximos sistólicos (rojo) y mínimos/valles (verde) detectados sobre la señal PPG filtrada — la detección es consistente latido a latido incluso durante los cambios de amplitud, gracias al método de detección adaptativo/independiente de amplitud usado.
