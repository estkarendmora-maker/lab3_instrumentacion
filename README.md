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

## 13. Análisis de Resultados

### Análisis 1: Compare los valores del SPI obtenidos durante la práctica con los que frecuentemente se observan durante una cirugía para proporcionar el nivel óptimo de anestesia.  
Para analizar la respuesta obtenida se dividió el registro en tres periodos: una etapa previa al estímulo entre 30 y 70 s, la aplicación del Cold Pressor Test (CPT) entre 70 y 110 s y una etapa de recuperación entre 110 y 150 s. El SPI promedio pasó de aproximadamente 32,2 antes del CPT a 96,7 durante la aplicación del frío y posteriormente disminuyó hasta aproximadamente 51,8 durante la recuperación.  

En pacientes adultos bajo anestesia general se utiliza habitualmente un intervalo de SPI entre 20 y 50 como referencia de un balance adecuado entre nocicepción y analgesia; valores superiores a 50 pueden indicar que el estímulo nociceptivo predomina sobre el efecto analgésico [3]. Por lo tanto, el SPI promedio de 32,2 registrado antes del CPT se encontraba dentro de este intervalo, mientras que el valor de 96,7 observado durante el estímulo frío se situó claramente por encima de dicho rango. El valor promedio de 51,8 durante la recuperación muestra que, aunque la respuesta disminuyó después de retirar el estímulo, todavía no había regresado completamente al nivel previo al CPT [3].  

El incremento del SPI fue acompañado por cambios importantes en las variables utilizadas para obtenerlo. La frecuencia cardiaca promedio aumentó de aproximadamente 55,0 bpm antes del CPT a 75,4 bpm durante el estímulo, mientras que el intervalo entre latidos disminuyó de 1,10 s a 0,81 s. De manera más marcada, la amplitud de la onda pletismográfica (PPGA) disminuyó de aproximadamente 706,6 a 121,0. El SPI se construye a partir de la PPGA normalizada y del intervalo entre latidos normalizado, de manera que una disminución de ambas variables produce un incremento del índice [2].  

Este comportamiento es consistente con la respuesta fisiológica esperada ante un estímulo nociceptivo. El aumento de la actividad simpática incrementa la frecuencia cardiaca y el tono vascular periférico, reduciendo tanto el HBI como la PPGA y produciendo un aumento del SPI [3]. Además, el CPT ha sido utilizado precisamente como una técnica para provocar activación simpática y disminución relativa de la actividad parasimpática [7].  

Durante gran parte del CPT el SPI experimental alcanzó el límite máximo de 100. Este resultado debe interpretarse como una respuesta autonómica muy superior a la observada durante la condición basal y no como una indicación de que el participante experimentó un “dolor de 100”. El SPI fue desarrollado como un indicador del balance nocicepción-antinocicepción y no como una escala subjetiva de intensidad del dolor [2], [3].  

También debe considerarse que el sistema implementado utiliza una normalización experimental basada en la referencia basal obtenida para el participante, mientras que el algoritmo utilizado en monitores comerciales emplea procedimientos propios de normalización. Por esta razón, la comparación con el intervalo clínico de 20–50 permite estudiar el comportamiento y la tendencia del índice, pero los valores obtenidos no deben considerarse equivalentes a los de un dispositivo clínico validado [3].  

### Análisis 2: Evalúe el alcance y las posibles limitaciones de emplear el sistema desarrollado para cuantificar el dolor que percibe una persona.   
El sistema desarrollado demostró capacidad para detectar en tiempo real modificaciones fisiológicas asociadas con un estímulo nociceptivo. La disminución de la PPGA, el aumento de la frecuencia cardiaca y la reducción del HBI observados durante el CPT fueron reflejados mediante un incremento marcado del SPI, lo que permite utilizar este tipo de sistema como herramienta experimental para estudiar la respuesta autonómica frente a estímulos potencialmente dolorosos [2], [3].  

Sin embargo, el sistema no permite cuantificar directamente el dolor percibido por una persona. La International Association for the Study of Pain define el dolor como una experiencia sensorial y emocional desagradable asociada, o similar a la asociada, con daño tisular real o potencial; además, establece que el dolor y la nocicepción son fenómenos diferentes y que el dolor es una experiencia personal influida por factores biológicos, psicológicos y sociales [4].  

Por esta razón, dos individuos sometidos al mismo estímulo pueden presentar respuestas autonómicas y experiencias subjetivas diferentes. El SPI proporciona información relacionada principalmente con la respuesta autonómica y con el balance nocicepción-antinocicepción, pero no incorpora directamente factores emocionales, cognitivos o psicológicos que forman parte de la experiencia del dolor [3], [4].

Otra limitación se encuentra en la adquisición de la señal fotopletismográfica. Los movimientos, cambios en el contacto entre el dedo y el sensor, modificaciones de la perfusión periférica y artefactos pueden alterar la forma de la onda PPG y afectar la detección de máximos y mínimos. Aunque el método del alpinista fue diseñado para detectar picos y valles en tiempo real y adaptarse a variaciones de amplitud, sus autores señalan que todavía presenta limitaciones frente a artefactos de movimiento [1].  

Además, el SPI puede modificarse por factores como la edad, el volumen circulante efectivo, la posición corporal, los medicamentos administrados, el tipo de anestesia y el nivel de conciencia del paciente [3]. Esto es especialmente importante en esta práctica, ya que el participante se encontraba consciente durante el CPT, mientras que el SPI fue desarrollado principalmente para evaluar la respuesta nociceptiva de pacientes sometidos a anestesia general [2], [3].  

Por estas razones, el sistema desarrollado puede considerarse una herramienta útil para detectar y estudiar cambios fisiológicos relacionados con la nocicepción, pero no como un instrumento independiente para medir de forma objetiva la intensidad del dolor subjetivo de una persona [3], [4].  

## Conclusión
El desarrollo del sistema permitió abordar el problema de estimar cambios relacionados con la nocicepción a partir de una señal fotopletismográfica obtenida de forma no invasiva. Mediante el procesamiento de la PPG fue posible extraer variables como la amplitud pletismográfica, el intervalo entre latidos y la frecuencia cardiaca, las cuales fueron empleadas para calcular un SPI experimental en tiempo real [1], [2].

Los resultados obtenidos mostraron que el sistema fue capaz de identificar una respuesta fisiológica clara durante la aplicación del Cold Pressor Test. El SPI aumentó de un promedio aproximado de 32,2 antes del estímulo a 96,7 durante su aplicación, acompañado por un aumento de la frecuencia cardiaca, una disminución del intervalo entre latidos y una reducción marcada de la amplitud pletismográfica. Este comportamiento es consistente con una mayor activación simpática y una respuesta nociceptiva frente al estímulo frío [2], [3], [7].

A partir de estos resultados también se evidenció que nocicepción y dolor no son equivalentes. La nocicepción corresponde al procesamiento fisiológico de estímulos potencialmente dañinos, mientras que el dolor constituye una experiencia subjetiva que involucra componentes sensoriales, emocionales y cognitivos [4]. Por esta razón, aunque el SPI permite detectar modificaciones autonómicas asociadas con un estímulo nociceptivo, no debe interpretarse como una medida directa de la intensidad del dolor percibido por una persona [3], [4].

El uso de este tipo de índices puede complementar la evaluación del balance entre nocicepción y analgesia, especialmente en situaciones en las que el paciente no puede comunicar de forma directa su percepción. Sin embargo, su interpretación debe realizarse junto con otras variables fisiológicas y con el contexto clínico, debido a la influencia de factores como movimiento, perfusión periférica, medicamentos y variabilidad individual [3].

Como siguiente paso, sería conveniente validar el sistema en un mayor número de participantes y comparar los valores obtenidos con un equipo clínico o un monitor comercial de SPI. También podría evaluarse la respuesta frente a diferentes intensidades de estímulo y analizar la repetibilidad de las mediciones, con el fin de determinar qué tan cercana es la estimación experimental al comportamiento de un sistema clínicamente validado.

## Discusión
Pregunta 1. ¿Cómo se relacionan las variaciones del volumen sanguíneo periférico con el balance autonómico?  

Las variaciones de la señal pletismográfica periférica están relacionadas con modificaciones en el tono vascular producidas por el sistema nervioso autónomo. Un aumento de la actividad simpática puede generar vasoconstricción periférica y reducir la amplitud pulsátil registrada mediante fotopletismografía [1], [3]. La PPGA se calcula a partir de la diferencia entre un máximo y el valle correspondiente de la señal PPG, por lo que su disminución permite identificar cambios asociados con vasoconstricción periférica [1].  

En el CPT se ha demostrado un aumento de indicadores de actividad simpática y una reducción relativa de la actividad parasimpática [7]. Esto coincide con los resultados experimentales obtenidos, ya que durante el estímulo frío la PPGA disminuyó de aproximadamente 706,6 a 121,0, mientras que la frecuencia cardiaca aumentó de 55,0 a 75,4 bpm. Estos cambios sugieren un desplazamiento del balance autonómico hacia una mayor actividad simpática durante el estímulo [7].  

Por lo tanto, las variaciones del volumen sanguíneo periférico observadas indirectamente mediante la señal PPG pueden utilizarse como una aproximación a cambios en el tono vascular autonómico, aunque no constituyen por sí solas una medición completa de la actividad del sistema nervioso autónomo [1], [3].  

Pregunta 2. ¿Cómo se compara el SPI con otros índices comúnmente empleados en cirugía, como el índice nocicepción-analgesia (ANI) y el índice de perfusión?  

El SPI combina información proveniente del intervalo entre latidos (HBI) y de la amplitud de la señal fotopletismográfica (PPGA). Una disminución del HBI, una disminución de la PPGA o la combinación de ambas producen un aumento del SPI, por lo que valores elevados se asocian con una mayor respuesta nociceptiva [2], [3].  

El Analgesia Nociception Index (ANI) utiliza principalmente información obtenida de la variabilidad de la frecuencia cardiaca para valorar cambios relacionados con la actividad parasimpática. El índice se ha propuesto para evaluar en tiempo real el balance entre antinocicepción y nocicepción durante anestesia general [5]. A diferencia del SPI, donde valores mayores indican generalmente una respuesta nociceptiva más elevada, en el ANI los valores más bajos se asocian con una menor actividad parasimpática y una mayor respuesta al estrés nociceptivo [5].  

El índice de perfusión (PI) se obtiene a partir de la señal del oxímetro de pulso y representa cambios en la perfusión periférica. Su comportamiento está estrechamente relacionado con modificaciones del flujo sanguíneo periférico y puede utilizarse como indicador no invasivo de cambios en la perfusión [6].  

En consecuencia, los tres índices estudian componentes fisiológicos diferentes: el SPI combina información cardiaca y vascular periférica, el ANI se basa principalmente en la variabilidad cardiaca asociada con la regulación parasimpática y el índice de perfusión refleja principalmente las modificaciones de la perfusión periférica [2], [5], [6]. Ninguno de estos parámetros debe interpretarse por sí solo como una medición directa del dolor subjetivo, ya que el dolor y la nocicepción corresponden a fenómenos distintos [4].  

## Bibliografia
[1] E. J. Argüello-Prada, "The mountaineer's method for peak detection in photoplethysmographic signals," Revista Facultad de Ingeniería, Universidad de Antioquia, no. 90, pp. 42–50, 2019. https://doi.org/10.17533/udea.redin.n90a06  

[2] Huiku, M., Uutela, K., van Gils, M., Korhonen, I., Kymäläinen, M., Meriläinen, P., Paloheimo, M., Rantanen, M., Takala, P., Viertiö-Oja, H., & Yli-Hankala, A. (2007). Assessment of surgical stress during general anaesthesia. British Journal of Anaesthesia, 98(4), 447–455.
https://doi.org/10.1093/bja/aem004  

[3] Oh, S. K., Won, Y. J., & Lim, B. G. (2024). Surgical pleth index monitoring in perioperative pain management: Usefulness and limitations. Korean Journal of Anesthesiology, 77(1), 31–45.
https://doi.org/10.4097/kja.23158  

[4] Raja, S. N., Carr, D. B., Cohen, M., Finnerup, N. B., Flor, H., Gibson, S., Keefe, F. J., Mogil, J. S., Ringkamp, M., Sluka, K. A., Song, X. J., Stevens, B., Sullivan, M. D., Tutelman, P. R., Ushida, T., & Vader, K. (2020). The revised International Association for the Study of Pain definition of pain: Concepts, challenges, and compromises. Pain, 161(9), 1976–1982.
https://doi.org/10.1097/j.pain.0000000000001939  

[5] Jeanne, M., Clément, C., De Jonckheere, J., Logier, R., & Tavernier, B. (2012). Variations of the analgesia nociception index during general anaesthesia for laparoscopic abdominal surgery. Journal of Clinical Monitoring and Computing, 26(4), 289–294.
https://doi.org/10.1007/s10877-012-9354-0  

[6] Lima, A. P., Beelen, P., & Bakker, J. (2002). Use of a peripheral perfusion index derived from the pulse oximetry signal as a noninvasive indicator of perfusion. Critical Care Medicine, 30(6), 1210–1213.
https://doi.org/10.1097/00003246-200206000-00006  

[7] Wirch, J. L., Wolfe, L. A., Weissgerber, T. L., & Davies, G. A. L. (2006). Cold pressor test protocol to evaluate cardiac autonomic function. Applied Physiology, Nutrition, and Metabolism, 31(3), 235–243.
https://doi.org/10.1139/h05-018  
