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


### Evolución temporal del SPI
Segun las graficas podemos observar tres fases:

1. **Reposo inicial 30-65 s:** el SPI se mantiene relativamente bajo y variable, oscilando mayormente entre 0 y 65 con PPGA alto de 400-1100 con esto se puede decir que conicide con  un estado basal sin estrés nociceptivo.
2. **Estímulo 70-110 s:** el SPI es alto con un valor de 100 indicando una mayor respuesta nociceptica causado una respuesta simpatica como el estres, el PPGA baja a casi 100
3. **Recuperación (113-150 s):** el PPGA progresivamente va aumentando incluso superando el valor basal inicial llegando a 1400, lo que se interpreta como una vasodilatación aumentando el flujo sanguineo tras retirar el estímulo. El SPI se vuelve mas negativo llegando a valores cercanos a 0.

La correlación entre los bajos valores de PPGA y altos valores de SPI durante el estimulo y su recuperación cambaindo los valores. Es el comportamiento esperado, el cual coincide con el rango de 20 - 50 objetivo para una anagesia intraoperatoria adecuada.


### Detección de picos y valles

**Método utilizado:** Para identificar los picos sistólicos y los valles (diastólicos) de la señal PPG se implementó un algoritmo de detección adaptativo, inspirado en el "método del alpinista" *Mountaineer's Method for Peak Detection*, MMPD [1]. Este método no depende de un umbral fijo de amplitud, encambio depende de la forma de la señal, por lo que la hace más personalizada. Este metodo consiste en contar cuántas muestras consecutivas van subiendo antes de que la pendiente cambie. Cuando ese conteo alcanza o supera un umbral, esto se identifica como un pico sistolico, al identificar el pico cuando empieza a desender este se detecta como un valle el cual presede del pico sistolico.


**Resultados de la detección:** Sobre los 150 s de captura, 15035 muestras a una fs de 100 Hz, se detectó:
- 151 máximos (picos sistólicos)
- 151 mínimos (valles diastólicos)
- 148 intervalos HBI válidos


**Validación visual:** Las Imágenes 3 y 5 muestran los máximos y mínimos. Se observa que la detección se mantiene con un pico y un valle por cada latido incluso donde la amplitud de la señal cae en ele timepo 70-110 s, el cual es el tiempo donde se genera el estimulo.

## Bibliografia
[1] E. J. Argüello-Prada, "The mountaineer's method for peak detection in photoplethysmographic signals," Revista Facultad de Ingeniería, Universidad de Antioquia, no. 90, pp. 42–50, 2019. https://doi.org/10.17533/udea.redin.n90a06
