# lab3_instrumentacion
### Integrantes
Lina María Cortes Almonacid

Karen Dayanna Mora Segura

Sofia Alejandra Cardona Cruz
### Introduccion

En esta práctica se diseñó y construyó un modelo a escala de una incubadora neonatal, un dispositivo médico cuya finalidad es generar un ambiente controlado que se asemeje lo más posible a las condiciones del vientre materno y favorezca la supervivencia y el desarrollo del recién nacido [1], [2]. La variable más importante es la temperatura, que debe mantenerse alrededor de los 37 °C [3]. En esta práctica, el rango objetivo fue de 36 °C a 37,5 °C. La termorregulación es especialmente crítica en los neonatos prematuros, que tienen la piel más delgada y un sistema nervioso más inmaduro, lo que limita su capacidad para regular la temperatura corporal [4].

El modelo se construyó sobre una caja plástica transparente con tapa, que permite ver el interior y reduce las pérdidas de calor. A la caja se le hicieron modificaciones para el paso del cableado y un orificio para introducir la mano, de modo que se pueda manipular al neonato sin abrir por completo la cubierta y perturbar el ambiente interno. Para regular la temperatura se utilizó un bombillo como elemento calefactor y un ventilador que hace circular el aire y distribuye el calor dentro de la cabina, junto con un sensor de temperatura y un circuito de control de lazo cerrado. Además, se incorporó una celda de carga para estimar el peso del neonato, mostrado en una pantalla LCD, y un panel de LEDs que indica si la temperatura está dentro del rango, por encima o por debajo. Como paciente se empleó un muñeco que simula al recién nacido.

La práctica permitió aplicar conocimientos de instrumentación, electrónica y sistemas de control a un dispositivo biomédico real. También permitió reconocer las limitaciones de un prototipo frente a una incubadora comercial.

Objetivo general

*Diseñar y construir un modelo a escala de una incubadora neonatal capaz de mantener la temperatura interna entre 36 °C y 37,5 °C mediante un sistema de calefacción con circulación de aire, y de estimar el peso del recién nacido, mostrando ambas variables al usuario.

Objetivos específicos
*Identificar las partes principales de una incubadora neonatal y la función que cumple cada una.
*Construir una cabina a escala con dimensiones no menores a 44 cm × 20 cm × 22 cm, con cubierta transparente, aislante y con acceso para manipulación.
*Implementar un sistema de calefacción con un bombillo y un ventilador, controlado en lazo cerrado a partir de un sensor de temperatura.
*Implementar un panel de LEDs que indique si la temperatura está dentro, por encima o por debajo del rango establecido.
*Integrar una celda de carga y una pantalla LCD para estimar y mostrar el peso del neonato.
*Verificar el desempeño del sistema, evaluando la capacidad de mantener la temperatura en el rango y el error en la medición del peso con un patrón de masa conocida.
*Calcular el costo total y parcial del prototipo y compararlo con soluciones comerciales.
*Reconocer las limitaciones del prototipo frente a una incubadora neonatal real.

  
## 11. Procedimiento

### Montaje y prueba inicial del circuito
<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/ebe73d66-67a9-47ad-8cb6-091d512be53a" />


Para la construcción de la incubadora se utilizó como cabina una caja plástica transparente con tapa, que garantiza la visibilidad hacia el interior y reduce la pérdida de calor. Sus dimensiones fueron 44cm × 25cm × 30cm, superiores al mínimo de 44 cm × 20 cm × 22 cm indicado por el docente en el correo del 29 de septiembre para el modelo a escala. La tapa de la caja funciona como mecanismo de apertura y cierre.

Posteriormente, se le hicieron modificaciones a la caja. Se perforaron orificios para pasar el cableado de los sensores, el ventilador y el bombillo hacia el exterior, donde se ubicaron la electrónica y la pantalla LCD. También se abrió un hueco con forma de arco para poder introducir la mano y simular el acceso a un neonato dentro de una incubadora real sin necesidad de retirar la tapa.

Dentro de la cabina se instaló una base de madera cubierta con un colchón de espuma, sobre la cual se acostó un muñeco que simula al recién nacido. Esta base se montó sobre el sistema de pesaje, de modo que el peso del muñeco pudiera medirse. La celda de carga se conectó al microcontrolador mediante su módulo de acondicionamiento HX711, se calibró con masas conocidas y su valor se mostró en una pantalla LCD 16×2 con módulo I²C, alojada en una carcasa roja impresa en 3D.

Para el sistema de calefacción, en uno de los extremos de la cabina se ubicó un bombillo que actúa como fuente de calor y, junto a él, un ventilador pequeño que hace circular el aire y distribuye la temperatura en el interior. Cerca del muñeco se ubicó un sensor de temperatura que mide la temperatura del aire. El bombillo es controlado por el circuito de lazo cerrado diseñado en la Parte A, que lo enciende o lo apaga según la lectura del sensor para mantener la temperatura entre 36 °C y 37,5 °C. Además, un panel de 3 LEDs indica el estado de la temperatura: se enciende un LED verde cuando está dentro del rango y un LED rojo, uno para cada caso, cuando está por encima o por debajo.

Finalmente, se realizaron las pruebas de funcionamiento. Se encendió el sistema y se registró la evolución de la temperatura hasta alcanzar y sostener el rango establecido, se verificó el cambio de los LEDs según el estado y se midió el error del peso empleando un patrón de masa conocida. Con los componentes utilizados se calculó el costo parcial de cada parte y el costo total del prototipo, para compararlo con equipos comerciales.

### Circuito final utilizando el sensor MAX30102
Para la adquisición final de la señal se utilizó un ESP32 conectado a un sensor óptico MAX30102. La comunicación entre ambos dispositivos se realizó mediante el protocolo I²C.

<p align="center">
  <img width="380" height="535" alt="image" src="https://github.com/user-attachments/assets/2939e751-9bc8-49c3-a4ab-7254bee1db3a" width="500">
</p>


De acuerdo con el código implementado, se utilizaron los siguientes pines del ESP32:

| Elemento                      | ESP32          |
| ----------------------------- | -------------- |
| SDA del MAX30102              | GPIO 21        |
| SCL del MAX30102              | GPIO 22        |
| Comunicación                  | I²C            |
| Velocidad serial hacia MATLAB | 115200 baudios |
| Frecuencia de muestreo        | 100 Hz         |

La comunicación I²C se inicializó mediante:

Wire.begin(SDA_PIN, SCL_PIN);

Posteriormente, se verificó la comunicación con el sensor mediante:

particleSensor.begin(Wire, I2C_SPEED_FAST)

Una vez inicializado el MAX30102, se configuraron los parámetros de adquisición. El sensor se estableció con una frecuencia de muestreo de 100 Hz, un ancho de pulso de 411 μs, un rango ADC de 4096 y un nivel de brillo LED de 25. El modo utilizado corresponde a la adquisición mediante los canales rojo e infrarrojo (RED + IR), aunque para el procesamiento realizado en MATLAB únicamente se envió la señal infrarroja (IR).

El código utilizado para la adquisición fue configurado para enviar únicamente el valor IR mediante el puerto serial:

uint32_t ir = particleSensor.getFIFOIR();

Serial.println(ir);

De esta manera, el ESP32 funcionó como sistema de adquisición, mientras que MATLAB recibió los valores de la señal IR para realizar posteriormente el filtrado, detección de pulsos y cálculo de las variables fisiológicas.

### Adquisición y procesamiento de la señal

La señal IR obtenida mediante el MAX30102 fue enviada desde el ESP32 hacia MATLAB mediante comunicación serial a una velocidad de 115200 baudios. El programa de MATLAB recibió las muestras y las procesó en tiempo real.

La frecuencia de muestreo utilizada fue:
Fs​=100 Hz
por lo que el intervalo temporal entre muestras corresponde a:

$$ T_s=\frac{1}{F_s}=\frac{1}{100}=0.01\ s $$

El tiempo de adquisición total utilizado en el código fue de: 150 s=2 min 30 s

La adquisición se dividió en tres etapas:

0–5 s: estabilización inicial.

5–30 s: referencia o calibración basal.

30–150 s: monitoreo y cálculo del SPI en tiempo real.

Por tanto, después de los primeros 30 segundos se contó con aproximadamente 120 segundos de monitoreo.
### Filtrado de la señal PPG
Una vez recibida la señal IR, MATLAB realizó un filtrado pasa banda para conservar principalmente las componentes asociadas con la actividad pulsátil.

El filtro utilizado fue un filtro Butterworth de tercer orden, con frecuencias de corte:


 $$f_{inferior}=0.7\ Hz y f_{superior}=2.34\ Hz $$

Por tanto, la señal utilizada para el análisis correspondió a la señal IR filtrada dentro del intervalo:

                0.7\ Hz \leq ≤ f ≤\leq 2.34\ Hz 

Posteriormente, la señal filtrada fue utilizada para identificar los máximos y mínimos correspondientes a los pulsos de la señal PPG.

### Detección de los latidos
Para identificar los latidos cardíacos se implementó en MATLAB un detector de máximos basado en la evolución de la señal PPG, sin utilizar la función findpeaks.

El algoritmo analiza la pendiente de la señal para identificar una fase ascendente seguida de una fase descendente. Cuando se confirma la cima de la señal, esta se considera un máximo sistólico.

Además, se identificó el valle asociado a cada pulso. La diferencia entre el máximo y el valle permitió calcular la amplitud de la onda de pulso pletismográfica (PPGA):

                     PPGA=Pico-Valle 

A partir de la separación temporal entre dos máximos consecutivos se calculó el intervalo entre latidos (HBI):
<p align="center">
  <img width="247" height="88" alt="image" src="https://github.com/user-attachments/assets/a29a2942-a906-442d-879f-3cf1214ec0ed"width="500">
</p>

y posteriormente la frecuencia cardiaca:

<p align="center">
  <img width="190" height="83" alt="image" src="https://github.com/user-attachments/assets/9f1cfc39-2692-46a6-8b24-ab10d36b1011"width="500">
</p>

donde:

\(HBI\) está expresado en segundos.
\(FC\) corresponde a la frecuencia cardiaca en latidos por minuto.
### Referencia basal y cálculo del SPI

Durante el intervalo de 5 a 30 segundos se obtuvieron los valores utilizados como referencia basal. El código almacena los valores de PPGA y HBI obtenidos durante este período y posteriormente realiza una limpieza de valores extremos mediante la mediana y la desviación absoluta respecto a la mediana.

Cuando se dispone de una cantidad suficiente de latidos válidos, se establece la referencia basal y se utiliza para normalizar los valores obtenidos durante el período de monitoreo.

El código implementado utiliza la siguiente expresión para el cálculo del índice:


<p align="center">
  <img width="532" height="62" alt="image" src="https://github.com/user-attachments/assets/3f9ab2d9-08f6-4c94-9acd-bd48f9c04d28"width="500">
</p>
  
Esta expresión corresponde a la formulación descrita por Huiku et al. (2007) para el índice de estrés quirúrgico, posteriormente denominado Surgical Pleth Index (SPI). El índice combina información proveniente de la amplitud de la onda pletismográfica y del intervalo entre latidos[8].

### ¿Qué es el SPI?

El SPI (Surgical Pleth Index) es un índice no invasivo derivado principalmente de la señal de fotopletismografía (PPG) y de la información relacionada con los intervalos entre latidos. Fue desarrollado inicialmente por Huiku et al. como Surgical Stress Index (SSI) para cuantificar cambios relacionados con el estrés quirúrgico y la respuesta a estímulos nociceptivos durante anestesia general.

Matemáticamente, la formulación utilizada en este trabajo es:

<p align="center">
  <img width="527" height="63" alt="image" src="https://github.com/user-attachments/assets/822a6fd2-4e52-413a-9d74-4dae48ed2882"width="500">
</p>

donde  PPGA_{norm} representa la amplitud normalizada de la onda pletismográfica y HBI_{norm} representa el intervalo entre latidos normalizado.

El índice es adimensional y se expresa en una escala de 0 a 100. En el contexto de su utilización original, los cambios del índice se relacionan con la respuesta nociceptiva y la respuesta autonómica durante anestesia; por ello, no debe interpretarse directamente como una medición subjetiva del dolor de una persona[8].

En este proyecto, el SPI se utiliza como un indicador experimental de cambios en la respuesta fisiológica ante el estímulo aplicado, obtenido a partir de la señal PPG registrada mediante el MAX30102.

### Prueba experimental y estímulo frío
Una vez verificado el funcionamiento del circuito final, se realizó la adquisición de la señal fisiológica durante el protocolo experimental establecido en la práctica.

Durante la prueba se mantuvo el dedo en contacto con el sensor MAX30102, procurando reducir el movimiento para evitar alteraciones en la señal PPG. Posteriormente se aplicó el estímulo frío correspondiente al Cold Pressor Test (CPT), mientras se continuó registrando la señal.

La señal obtenida durante el experimento permitió comparar las variables fisiológicas antes y durante el estímulo, especialmente la frecuencia cardiaca, el HBI, la PPGA y la evolución temporal del SPI.

<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/f7ad0483-fcd0-471c-a6b6-ba00e5187a36" />


## 12. Resultados de la práctica

### Metodología de captura
Con el sensor MAX30102 se adquirió la señal PPG con una fs de 100 Hz durante 150 s, divididos en tres etapas:primero se estabilizó el sensor en el tiempo 0 - 5 s, despues se realiza una calibración donde se fija un rango de normalización del SPI en el tiempo 5 - 30 s, y por ultimo se monitoreó el SPI en tiempo real en el tiempo 30 - 150 s con una duración de monitoreó de 120 s (2 min)

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

### Estadísticas por fase de la captura

| Variable | Reposo inicial (30-65s) | Estímulo (70-110s) | Recuperación (113-150s) |
|---|---|---|---|
| n° de muestras | 31 | 49 | 34 |
| FC promedio (bpm) | 54.3 | 75.4 | 59.1 |
| FC mediana (bpm) | 53.1 | 75.9 | 56.1 |
| FC mín-máx (bpm) | 48.0 - 67.4 | 47.6 - 92.3 | 46.5 - 81.1 |
| HBI promedio (s) | 1.10 | 0.80 | 1.00 |
| HBI mediana (s) | 1.10 | 0.80 | 1.10 |
| PPGA promedio (u.a.) | 728.1 | 121.0 | 585.7 |
| PPGA mediana (u.a.) | 751.1 | 99.5 | 480.2 |
| PPGA mín-máx (u.a.) | 339.4 - 1112.5 | 32.4 - 342.3 | 223.7 - 1448.4 |
| SPI promedio | 29.5 | 96.7 | 50.4 |
| SPI mediana | 30.4 | 100.0 | 64.2 |
| SPI mín-máx | 0.0 - 65.5 | 72.9 - 100.0 | 0.0 - 87.8 |


### Evolución temporal del SPI
Segun las graficas podemos observar tres fases:
<img width="926" height="617" alt="Imagen 1" src="https://github.com/user-attachments/assets/7558b761-a572-4ab3-a1b7-629db12f8a51" />
<img width="934" height="612" alt="Imagen 2" src="https://github.com/user-attachments/assets/918ef032-c95a-4a2b-adae-b79417437fa6" />

1. **Reposo inicial 30-65 s:** el SPI se mantiene relativamente bajo y variable, oscilando mayormente entre 0 y 65 con PPGA alto de 400-1100 con esto se puede decir que conicide con  un estado basal sin estrés nociceptivo.
2. **Estímulo 70-110 s:** el SPI es alto con un valor de 100 indicando una mayor respuesta nociceptica causado una respuesta simpatica como el estres, el PPGA baja a casi 100
3. **Recuperación (113-150 s):** el PPGA progresivamente va aumentando incluso superando el valor basal inicial llegando a 1400, lo que se interpreta como una vasodilatación aumentando el flujo sanguineo tras retirar el estímulo. El SPI disminuye llegando a valores cercanos a 0.

Con esto se puede observar una correlación entre los valores de PPGA y SPI, los cuales son inversos. Este es el comportamiento esperado, el cual coincide con el rango de 20 - 50 objetivo para una analgesia intraoperatoria adecuada.


### Detección de picos y valles
<img width="963" height="622" alt="Imagen 3" src="https://github.com/user-attachments/assets/4a8ac151-85c1-47b4-922c-fe2257dbfed9" />
<img width="956" height="617" alt="Imagen 4" src="https://github.com/user-attachments/assets/144eba35-17fd-424a-a790-f6f4b6e58493" />

**Método utilizado:** Para identificar los picos sistólicos y los valles (diastólicos) de la señal PPG se implementó un algoritmo de detección adaptativo, inspirado en el "método del alpinista" *Mountaineer's Method for Peak Detection*, MMPD [1]. Este método no depende de un umbral fijo de amplitud, en cambio depende de la forma de la señal, por lo que la hace más personalizada. Este metodo consiste en contar cuántas muestras consecutivas van subiendo antes de que la pendiente cambie. Cuando ese conteo alcanza o supera un umbral, esto se identifica como un pico sistolico, al identificar el pico cuando empieza a descender este se detecta como un valle el cual anticipa del pico sistolico.


**Resultados de la detección:** Sobre los 150 s de captura, 15035 muestras a una fs de 100 Hz, se detectó:
- 151 máximos (picos sistólicos)
- 151 mínimos (valles diastólicos)
- 148 intervalos HBI válidos


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
**Pregunta 1. ¿Qué otras variables (y por qué) además de las aquí mencionadas son críticas en el monitoreo neonatal? **


**Pregunta 2. ¿Qué haría falta para convertir el sistema desarrollado en una incubadora neonatal real? **
El prototipo ilustra el principio de operación (calefacción, control de temperatura y pesaje), pero está lejos de ser un dispositivo médico. Para convertirlo en una incubadora neonatal real harían falta varios cambios.

Primero, un control de temperatura más completo: una incubadora real combina el control de la temperatura del aire con el control servo por temperatura de la piel del neonato, medida con una sonda cutánea [3], [4]. El calefactor también debería ser un elemento resistivo con ventilador, ya que el bombillo del prototipo genera calor localizado e iluminación constante, lo que no es adecuado para un neonato. Además, se requiere control de humedad, que reduce las pérdidas de agua por la piel en prematuros, y la posibilidad de suministrar oxígeno suplementario [1], [4].

Segundo, un sistema de alarmas y seguridad independiente, con alertas audibles y visuales para sobretemperatura, falla del sensor, falla del ventilador y corte de energía, junto con un termostato de seguridad que corte el calefactor ante una falla, pues la hipertermia es un riesgo grave [5]. Todo esto debe cumplir la norma de seguridad de equipos electromédicos IEC 60601-1 y la norma particular de incubadoras IEC 60601-2-19 [6], [7], además del registro sanitario ante el INVIMA en Colombia [8].

Por último, se necesitan materiales de grado médico y fáciles de desinfectar, una cubierta de doble pared con filtración de aire y accesos seguros en lugar de una tapa de caja, una celda de carga calibrada con resolución de gramos, y una validación clínica del equipo.

**Pregunta 3. ¿Qué semejanzas hay entre una incubadora neonatal y una servo-cuna? **
## Bibliografia
[1] C. G. K. Tran et al., “Designing a low-cost multifunctional infant incubator,” J. Lab. Autom., vol. 19, no. 3, pp. 332–337, Jun. 2014. https://doi.org/10.1177/2211068214530391

[2] L. Restrepo-Pérez, N. Durango-Londoño, N. E. Gómez-Suárez, F. González-Ramírez y N. Rivera-Bonilla, “Prototipo de incubadora neonatal,” Revista Ingeniería Biomédica, vol. 1, no. 1, pp. 55–59, 2007.

[3] M. van Leeuwen et al., European Standards of Care for Newborn Health: Temperature management in newborn infants, EFCNI, Nov. 2018. https://newborn-health-standards.org

[4] R. B. Knobel-Dail, “Role of effective thermoregulation in premature neonates,” Research and Reports in Neonatology, vol. 4, pp. 147–156, 2014. https://doi.org/10.2147/RRN.S52377

[5] R. E. Black et al., “Global, regional, and national causes of child mortality in 2008: a systematic analysis,” Lancet, vol. 375, pp. 1969–1987, Jun. 2010. https://doi.org/10.1016/S0140-6736(10)60549-1

[6] International Electrotechnical Commission, IEC 60601-2-19: Medical electrical equipment – Particular requirements for the basic safety and essential performance of infant incubators.

[7] International Electrotechnical Commission, IEC 60601-1: Medical electrical equipment – General requirements for basic safety and essential performance.

[8] Ministerio de la Protección Social de Colombia, Decreto 4725 de 2005, por el cual se reglamenta el régimen de registros sanitarios, permiso de comercialización y vigilancia sanitaria de los dispositivos médicos para uso humano.
