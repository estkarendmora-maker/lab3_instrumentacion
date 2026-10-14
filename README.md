# lab3_instrumentacion
### Integrantes
Lina María Cortes Almonacid

Karen Dayanna Mora Segura

Sofia Alejandra Cardona Cruz
### Introduccion

En esta práctica se diseñó y construyó un modelo a escala de una incubadora neonatal, un dispositivo médico cuya finalidad es generar un ambiente controlado que se asemeje lo más posible a las condiciones del vientre materno y favorezca la supervivencia y el desarrollo del recién nacido [1], [2]. La variable más importante es la temperatura, que debe mantenerse alrededor de los 37 °C [3]. En esta práctica, el rango objetivo fue de 36 °C a 37,5 °C. La termorregulación es especialmente crítica en los neonatos prematuros, que tienen la piel más delgada y un sistema nervioso más inmaduro, lo que limita su capacidad para regular la temperatura corporal [4].

El modelo se construyó sobre una caja plástica transparente con tapa, que permite ver el interior y reduce las pérdidas de calor. A la caja se le hicieron modificaciones para el paso del cableado y un orificio para introducir la mano, de modo que se pueda manipular al neonato sin abrir por completo la cubierta y perturbar el ambiente interno. Para regular la temperatura se utilizó un bombillo como elemento calefactor y un ventilador que hace circular el aire y distribuye el calor dentro de la cabina, junto con un sensor de temperatura y un circuito de control de lazo cerrado. Además, se incorporó una celda de carga para estimar el peso del neonato, mostrado en una pantalla LCD, y un panel de LEDs que indica si la temperatura está dentro del rango, por encima o por debajo. Como paciente se empleó un muñeco que simula al recién nacido.

La práctica permitió aplicar conocimientos de instrumentación, electrónica y sistemas de control a un dispositivo biomédico real. También permitió reconocer las limitaciones de un prototipo frente a una incubadora comercial.

### Objetivo general

* Diseñar y construir un modelo a escala de una incubadora neonatal capaz de mantener la temperatura interna entre 36 °C y 37,5 °C mediante un sistema de calefacción con circulación de aire, y de estimar el peso del recién nacido, mostrando ambas variables al usuario.

### Objetivos específicos
* Identificar las partes principales de una incubadora neonatal y la función que cumple cada una.
* Construir una cabina a escala con dimensiones no menores a 44 cm × 20 cm × 22 cm, con cubierta transparente, aislante y con acceso para manipulación.
* Implementar un sistema de calefacción con un bombillo y un ventilador, controlado en lazo cerrado a partir de un sensor de temperatura.
* Implementar un panel de LEDs que indique si la temperatura está dentro, por encima o por debajo del rango establecido.
* Integrar una celda de carga y una pantalla LCD para estimar y mostrar el peso del neonato.
* Verificar el desempeño del sistema, evaluando la capacidad de mantener la temperatura en el rango y el error en la medición del peso con un patrón de masa conocida.
* Calcular el costo total y parcial del prototipo y compararlo con soluciones comerciales.
* Reconocer las limitaciones del prototipo frente a una incubadora neonatal real.

  
## Metodologia 

### Montaje y prueba inicial 


Para la construcción de la incubadora se utilizó como cabina una caja plástica transparente con tapa, que garantiza la visibilidad hacia el interior y reduce la pérdida de calor. Sus dimensiones fueron 44cm × 25cm × 30cm, superiores al mínimo de 44 cm × 20 cm × 22 cm indicado por el docente en el correo del 29 de septiembre para el modelo a escala. La tapa de la caja funciona como mecanismo de apertura y cierre.

Posteriormente, se le hicieron modificaciones a la caja. Se perforaron orificios para pasar el cableado de los sensores, el ventilador y el bombillo hacia el exterior, donde se ubicaron la electrónica y la pantalla LCD. También se abrió un hueco con forma de arco para poder introducir la mano y simular el acceso a un neonato dentro de una incubadora real sin necesidad de retirar la tapa.

Dentro de la cabina se instaló una base de madera cubierta con un colchón de espuma, sobre la cual se acostó un muñeco que simula al recién nacido. Esta base se montó sobre el sistema de pesaje, de modo que el peso del muñeco pudiera medirse. La celda de carga se conectó al microcontrolador mediante su módulo de acondicionamiento HX711, se calibró con masas conocidas y su valor se mostró en una pantalla LCD 16×2 con módulo I²C, alojada en una carcasa roja impresa en 3D.

Para el sistema de calefacción, en uno de los extremos de la cabina se ubicó un bombillo que actúa como fuente de calor y, junto a él, un ventilador pequeño que hace circular el aire y distribuye la temperatura en el interior. Cerca del muñeco se ubicó un sensor de temperatura que mide la temperatura del aire. El bombillo es controlado por el circuito de lazo cerrado diseñado en la Parte A, que lo enciende o lo apaga según la lectura del sensor para mantener la temperatura entre 36 °C y 37,5 °C. Además, un panel de 3 LEDs indica el estado de la temperatura: se enciende un LED verde cuando está dentro del rango y un LED rojo, uno para cada caso, cuando está por encima o por debajo.

Finalmente, se realizaron las pruebas de funcionamiento. Se encendió el sistema y se registró la evolución de la temperatura hasta alcanzar y sostener el rango establecido, se verificó el cambio de los LEDs según el estado y se midió el error del peso empleando un patrón de masa conocida. Con los componentes utilizados se calculó el costo parcial de cada parte y el costo total del prototipo, para compararlo con equipos comerciales.

### Medición y control de temperatura

### 1. Montaje del sistema de medición

Para medir la temperatura en la encubadora a escala, se utilizó; un termistor NTC, donde la resistencia disminuye cuando la temperatura aumenta. El sensor se conectó a la ESP32 con un divisor de voltaje por el termistor y una resistencia fija de 3,3 kΩ.

El voltaje del punto medio del divisor se conectó al pin GPIO 34 de la ESP32.

<img width="400" alt="CircuitoTemp" src="https://github.com/user-attachments/assets/fc4ef49a-3601-45e3-8026-ee0e968123e2" />

**Figura 1. Circuito de medición de temperatura con termistor NTC y ESP32.**


### 2. Adquisición y procesamiento de la señal

En Arduino IDE. Para cada ciclo la ESP32 tomó 100 muestras de voltaje y calculó su promedio para reducir las fluctuaciones de la lectura.
A partir de este voltaje promedio, se calculó la resistencia del termistor.

\[
R_{NTC}=R_F\left(\frac{V_{CC}-V_{NTC}}{V_{NTC}}\right)
\]

Donde:

- \(R_{NTC}\): resistencia calculada del termistor.
- \(R_F\): resistencia fija de 3,3 kΩ.
- \(V_{CC}\): voltaje de alimentación de 3,3 V.
- \(V_{NTC}\): voltaje medido en el punto medio del divisor.

La resistencia obtenida se utilizó para estimar la temperatura en grados Celsius, basada en puntos de calibración experimentales.

### 3. Calibración del termistor

La calibración se hizo utilizando mediciones de temperatura de referencia y sus valores de resistencia. Se emplearon puntos de referencia aproximados de 18 °C, 24 °C, 36 °C y 37 °C.
La temperatura calculada se consideró una estimación experimental.

### 4. Implementación del control de temperatura

La temperatura estimada se utilizó como variable de entrada para la lógica de control implementada en la ESP32. El sistema acciona un módulo relé conectado al GPIO 26, que permite controlar el elemento calefactor del prototipo.

El relé funciona con una lógica activa en nivel bajo: una señal `LOW` lo activa y una señal `HIGH` lo desactiva.

La lógica de control se configuró de la siguiente manera:

- **Temperatura inferior a 36 °C:** se solicita calentamiento.
- **Temperatura igual o superior a 37 °C:** se desactiva el calentamiento.
- **Temperatura igual o superior a 37,5 °C:** se mantiene el calentamiento desactivado y se genera una alerta de temperatura alta.


<img width="400" alt="Completotemp" src="https://github.com/user-attachments/assets/ae29806b-d59d-4223-b1cc-946413e70ed3" />

**Figura 2. Montaje físico del sistema de control de temperatura.**


### 5. Indicadores visuales y supervisión

Se utilizaron dos LED para indicar el estado del sistema. El LED verde representa el estado de calentamiento desactivado y una temperatura de 37 °C, y el LED rojo indica una solicitud de calor o indicador de una alta temperatura.

### 6. Peso  
La guía de laboratorio establece que la incubadora debe incorporar una galga o sensor de fuerza para estimar el peso del recién nacido y que el resultado debe visualizarse mediante una pantalla LCD o un sistema equivalente [9]. 

Para cumplir este requerimiento se implementó una celda de carga tipo barra, un módulo amplificador y conversor HX711, un microcontrolador ESP32 y una pantalla LCD 16×2 con interfaz I2C. La celda de carga trabaja mediante galgas extensométricas organizadas eléctricamente como un puente de Wheatstone. Las pequeñas variaciones eléctricas producidas al deformarse la celda son acondicionadas por el HX711, permitiendo que el microcontrolador pueda obtener una lectura digital proporcional a la fuerza aplicada [10].  

Para obtener mediciones adecuadas, la celda tipo barra no se instaló completamente apoyada sobre la superficie. Este tipo de sensor debe fijarse por uno de sus extremos mientras el extremo contrario recibe la carga, permitiendo que la zona central pueda deformarse ligeramente. El uso de tornillos y separadores evita restringir la deformación mecánica y mejora la repetibilidad de la medición [10]. Durante las primeras pruebas se observó una variación considerable de la lectura después de colocar y retirar una carga. Por esta razón se revisó el montaje mecánico, evitando que toda la barra quedara apoyada directamente sobre la mesa. Este ajuste fue importante porque factores como vibración, deriva, temperatura y montaje mecánico pueden afectar la lectura de una celda de carga y hacen necesaria una correcta calibración [10].   

La celda empleada contaba con cuatro conductores: rojo (E+), negro(E-), verde (A+) y blanco (A-). La conexión utilizada coincide con una codificación común para celdas de cuatro conductores, aunque los colores pueden variar según el fabricante, por lo que siempre es recomendable comprobar el modelo específico [10]. El HX711 se conectó al ESP32 utilizando dos líneas digitales. En el montaje desarrollado, DOUT se conectó al GPIO 19 y SCK al GPIO 18. El módulo se alimentó a 3,3 V y compartió una tierra común con el ESP32. El HX711 permite trabajar con microcontroladores y está diseñado específicamente para obtener datos de celdas de carga basadas en galgas extensométricas [10].  

La visualización se realizó mediante una LCD 16×2 provista de un módulo I2C, mediante un escáner I2C se identificó la dirección 0x27. La LCD se conectó al ESP32 utilizando SDA = GPIO 21 y SCL = GPIO 22.
La pantalla fue programada para mostrar el peso directamente en kilogramos y la utilización de I2C permitió reducir el número de conductores necesarios entre la pantalla y el ESP32, dejando disponibles otros GPIO para los demás elementos de la incubadora. La calibración se realizó comenzando con la celda completamente descargada. Durante el inicio del programa se ejecutó una función de tara, tomando la condición sin carga como referencia de cero. Posteriormente se colocó una masa conocida y se registró la lectura cruda entregada por el HX711. Este procedimiento es el recomendado para determinar experimentalmente el factor de calibración de una balanza basada en HX711 [10]. El factor se obtuvo mediante:  

<img width="434" height="81" alt="image" src="https://github.com/user-attachments/assets/f068c4fc-e400-4d04-9031-639f8bed4633" />  

Cuando el sistema trabaja en kilogramos, la masa patrón debe introducirse en kilogramos. El factor calculado se incorporó posteriormente al programa mediante scale.set_scale(), mientras que scale.tare() permitió establecer nuevamente el cero cada vez que se encendía el sistema.Finalmente, el programa realiza varias lecturas consecutivas y emplea un pequeño promedio con el propósito de disminuir las fluctuaciones sin introducir un retraso excesivo en la visualización.  


<img width="1600" height="1336" alt="image" src="https://github.com/user-attachments/assets/fcc87213-640e-4e74-8136-cb5a046d93c5" />  

**Figura 3. Montaje físico del circuito para medir peso.**  



## Anális de resultados  

### Análisis 1. Eficacia del sistema desarrollado para controlar la temperatura de la cabina, así como también el error en el cálculo del peso, empleando un patrón con peso conocido  

La guía establece que el prototipo debe mantener la temperatura de la incubadora entre 36 °C y 37,5 °C mediante un sistema de calentamiento y circulación de aire [9]. Durante las pruebas experimentales, el sistema inició aproximadamente a la temperatura ambiente del laboratorio, cercana a 17 °C, y posteriormente alcanzó una temperatura máxima de aproximadamente 43 °C. Estos resultados demostraron que el sistema de calentamiento implementado posee la capacidad suficiente para elevar considerablemente la temperatura interna de la cabina; sin embargo, también se observó un sobrepaso respecto al límite superior de 37,5 °C establecido para la práctica.  

El incremento desde aproximadamente 17 °C hasta 43 °C corresponde a una variación cercana a 26 °C, lo que evidencia una respuesta térmica considerable del sistema. No obstante, alcanzar 43 °C indica que la regulación todavía requiere un ajuste más preciso para evitar que la temperatura exceda el rango establecido. Este aspecto resulta especialmente importante en aplicaciones neonatales, ya que los recién nacidos, particularmente los prematuros, poseen una capacidad limitada de termorregulación y requieren un ambiente térmico cuidadosamente controlado [17].  

En consecuencia, el sistema desarrollado fue eficaz para generar y conservar calor dentro de la cabina, pero durante la prueba realizada no logró limitar completamente el máximo de temperatura al intervalo de 36 °C–37,5 °C solicitado. Una posible mejora consiste en ajustar los puntos de activación y desactivación del elemento calefactor y reducir la inercia térmica del sistema, evitando que el calor acumulado continúe elevando la temperatura después de desconectar el bombillo. También puede mejorarse la posición del sensor de temperatura y la circulación producida por el ventilador para obtener una medición más representativa del aire presente en toda la cabina.  

Para evaluar el sistema de medición se utilizó como patrón una masa conocida de 1,9 kg. Después de realizar la tara y calibración de la celda de carga, la pantalla LCD indicó aproximadamente 1,8 kg. El error absoluto se calculó E=|1.8-1.9| = 0.1 kg. Por tanto, el sistema presentó un error de aproximadamente 5,26 % respecto al patrón utilizado. Para un prototipo académico este resultado permitió comprobar que el conjunto formado por la celda de carga, el HX711 y el ESP32 fue capaz de estimar el peso aplicado y presentarlo directamente en la pantalla LCD. Sin embargo, la diferencia de 100 g demuestra que todavía existe margen para mejorar la exactitud mediante una calibración con varios puntos de referencia, un montaje mecánico más rígido y repetible y un mejor acondicionamiento de la señal de la celda [10].  


También debe considerarse que una calibración realizada con un único patrón permite ajustar principalmente la pendiente de la conversión entre lectura digital y masa. Utilizar varios patrones distribuidos dentro del rango de trabajo permitiría verificar la linealidad del sistema y determinar si el error se mantiene aproximadamente constante para diferentes masas [10].  

### Análisis 2: Compare el sistema desarrollado con los que ofrecen los fabricantes sugeridos en términos de capacidades, limitaciones y precio. 

A continuación, se presenta el presupuesto estimado de los materiales y componentes utilizados para la construcción de la incubadora neonatal a escala. Los precios están expresados en pesos colombianos (COP).

| N.º | Material o componente | Cantidad | Costo (COP) |
|---:|---|---:|---:|
| 1 | ESP32  | 1 | $28.000 |
| 2 | Termistor NTC | 1 | $12.000 |
| 3 | Módulo relé de 5 V | 1 | $11.000 |
| 4 | Bombillo de 100 W | 1 | $4.200 |
| 5 | Ventilador pequeño de corriente continua | 1 | $12.875 |
| 6 | Caja Plastica | 1 lote | $70.000 |
| 7 | Resistencia fija de 3,3 kΩ | 1 | $500 |
| 8 | LED indicadores | 3 | $1.500 |
| 9 | Protoboard | 1 | $8.000 |
| 10 | Cables jumper | 1 juego | $6.000 |
| 11 | Portalámparas y cableado | 1 juego | $10.000 |
| 12 | Interruptor y fusible | 1 juego | $8.000 |
| 13 | Cable USB para programación | 1 | $5.000 |
| 14 | Base y estructura de soporte | 1 lote | $25.000 |
| 15 | Pegamento, tornillos y elementos de fijación | 1 lote | $10.000 |
| | **Costo total estimado** | | **$212.075 COP** |

En el mercado colombiano existen incubadoras neonatales comerciales con un nivel de integración considerablemente mayor. Por ejemplo, Instrumentalia YP920 se ofrece públicamente por $28.758.350 COP y dispone de modos de control por temperatura del aire y por temperatura del bebé [11].  

La Instrumentalia YP2000 tiene un precio publicado de $53.391.350 COP e incorpora una cabina cerrada de doble pared y modos de control por aire y por temperatura del paciente [12]. Otro ejemplo es la COMEN B3, comercializada en Colombia por IMEDS con un precio publicado de $30.710.000 COP antes de IVA. Este equipo incorpora control preciso de temperatura, humidificación activa, doble pared, cortina de aire, pantalla interactiva y opción de balanza digital [14].   

La incubadora LEEX B3 presenta características clínicas similares, entre ellas control automático de temperatura, humidificación activa, doble pared, cortina de aire y una balanza digital opcional que permite pesar al paciente sin retirarlo del ambiente térmico controlado [13].  

Esto significa que el prototipo construido tiene un costo aproximadamente 99,3 % menor que la YP920, 99,3 % menor que la B3 y 99,6 % menor que la YP2000. Sin embargo, esta diferencia no significa que ambos sistemas tengan prestaciones equivalentes. Las incubadoras comerciales incorporan sistemas redundantes de seguridad, control de humedad, modos de control por temperatura de piel, alarmas, mejores mecanismos de aislamiento térmico y, dependiendo del modelo, medición de peso y otros sistemas de monitoreo [11]–[14].  

Por lo tanto, el prototipo desarrollado resulta adecuado como plataforma académica para demostrar principios de instrumentación, medición de peso y regulación térmica, pero no puede considerarse equivalente a una incubadora de uso clínico. La gran diferencia económica se explica principalmente por el nivel de seguridad, precisión, redundancia, certificación, diseño sanitario y funciones adicionales requeridas en equipos destinados al cuidado real de pacientes neonatales.  

## Discusión
**Pregunta 1. ¿Qué otras variables (y por qué) además de las aquí mencionadas son críticas en el monitoreo neonatal?**
Además de temperatura, humedad, flujo de aire y peso, es importante monitorear variables fisiológicas directamente relacionadas con la condición cardiorrespiratoria del recién nacido. Entre ellas se encuentran la frecuencia cardíaca, frecuencia respiratoria, saturación de oxígeno y presión arterial, variables que forman parte de la evaluación clínica neonatal y permiten identificar alteraciones cardiovasculares o respiratorias [16].  

La saturación de oxígeno (SpO₂) es especialmente importante porque permite identificar situaciones de hipoxemia y evaluar la respuesta del paciente cuando requiere oxigenoterapia. Las guías neonatales recomiendan utilizar pulsioximetría para supervisar la oxigenación cuando existe dificultad respiratoria o necesidad de oxígeno suplementario [16].   

La frecuencia respiratoria permite detectar episodios de respiración anormal, apnea o dificultad respiratoria, mientras que la frecuencia cardíaca proporciona información inmediata sobre el estado cardiovascular del neonato. En la evaluación del recién nacido se consideran ambas variables junto con la saturación de oxígeno como parámetros relevantes del seguimiento clínico [16].   

También debe considerarse la concentración de oxígeno dentro del entorno de la incubadora cuando se administra oxígeno suplementario. Mantener una concentración adecuada es importante porque tanto una oxigenación insuficiente como una exposición excesiva pueden representar riesgos para el recién nacido. Por esta razón, las incubadoras clínicas cuentan con sistemas de monitoreo y alarmas que van mucho más allá del control básico de temperatura desarrollado en el prototipo [13], [16].   

En consecuencia, una versión más completa del sistema podría integrar sensores para SpO₂, frecuencia cardíaca, frecuencia respiratoria y presión arterial, además de mantener las variables ambientales ya implementadas. Esto permitiría que el sistema no solo controlara el ambiente de la incubadora, sino que también ofreciera información sobre el estado fisiológico del paciente [15], [16].  
 

**Pregunta 2. ¿Qué haría falta para convertir el sistema desarrollado en una incubadora neonatal real?**
El prototipo ilustra el principio de operación (calefacción, control de temperatura y pesaje), pero está lejos de ser un dispositivo médico. Para convertirlo en una incubadora neonatal real harían falta varios cambios.

Primero, un control de temperatura más completo: una incubadora real combina el control de la temperatura del aire con el control servo por temperatura de la piel del neonato, medida con una sonda cutánea [3], [4]. El calefactor también debería ser un elemento resistivo con ventilador, ya que el bombillo del prototipo genera calor localizado e iluminación constante, lo que no es adecuado para un neonato. Además, se requiere control de humedad, que reduce las pérdidas de agua por la piel en prematuros, y la posibilidad de suministrar oxígeno suplementario [1], [4].

Segundo, un sistema de alarmas y seguridad independiente, con alertas audibles y visuales para sobretemperatura, falla del sensor, falla del ventilador y corte de energía, junto con un termostato de seguridad que corte el calefactor ante una falla, pues la hipertermia es un riesgo grave [5]. Todo esto debe cumplir la norma de seguridad de equipos electromédicos IEC 60601-1 y la norma particular de incubadoras IEC 60601-2-19 [6], [7], además del registro sanitario ante el INVIMA en Colombia [8].

Por último, se necesitan materiales de grado médico y fáciles de desinfectar, una cubierta de doble pared con filtración de aire y accesos seguros en lugar de una tapa de caja, una celda de carga calibrada con resolución de gramos, y una validación clínica del equipo.

**Pregunta 3. ¿Qué semejanzas hay entre una incubadora neonatal y una servo-cuna?**
Tanto la incubadora neonatal y la servo-cuna son dispositivos que proporcionan apoyo térmico al recién nacido, para mantener una temperatura corporal adecuada. Ambos equipos incorporan sensores de temperatura, sistemas de calentamiento y mecanismos de control que permiten regular el aporte de calor según las condiciones necesitadas [18], [19].

La principal diferencia está en su estructura y forma de transferencia de calor. La incubadora neonatal proporciona un ambiente cerrado y controlado, y la servo-cuna abierta y permite un acceso más directo al recién nacido durante los procedimientos médicos. [19], [20].  

## Conclusión  

El desarrollo del prototipo permitió comprobar que es posible integrar, en un sistema a escala, funciones básicas de una incubadora neonatal como el control de temperatura, la medición de peso y la visualización de variables mediante una interfaz sencilla. El sistema térmico logró elevar la temperatura interna desde aproximadamente 17 °C hasta 43 °C, lo que demostró que el mecanismo de calefacción y circulación de aire tiene capacidad suficiente para modificar las condiciones de la cabina. Sin embargo, el sobrepaso del límite superior establecido evidenció que el control todavía necesita ajustes para mantener la temperatura de forma más estable dentro del rango de 36 °C a 37,5 °C requerido para la práctica.   

En cuanto al sistema de pesaje, la integración de la celda de carga, el módulo HX711, el ESP32 y la pantalla LCD permitió estimar el peso del neonato simulado de manera funcional. Para un patrón de 1,9 kg se obtuvo una lectura cercana a 1,8 kg, correspondiente a un error de aproximadamente 5,26 %, lo que muestra que el sistema es capaz de realizar una medición útil, aunque todavía puede mejorarse mediante una calibración multipunto y un montaje mecánico más rígido y repetible.    

Los resultados obtenidos también permitieron identificar claramente la diferencia entre un prototipo académico y una incubadora neonatal comercial. Aunque el sistema construido tuvo un costo significativamente menor, las incubadoras clínicas incorporan funciones adicionales de seguridad, control de humedad, alarmas, monitoreo fisiológico, materiales de grado médico y sistemas redundantes que son necesarios para su uso real con pacientes.  

Como siguiente paso, sería necesario mejorar la precisión del control térmico, optimizar la calibración del sistema de peso e integrar variables adicionales como saturación de oxígeno, frecuencia cardíaca y frecuencia respiratoria. Estas mejoras permitirían acercar el prototipo a un sistema de monitoreo neonatal más completo, manteniendo su utilidad como plataforma de aprendizaje en instrumentación biomédica y control.
## Bibliografia
[1] C. G. K. Tran et al., “Designing a low-cost multifunctional infant incubator,” J. Lab. Autom., vol. 19, no. 3, pp. 332–337, Jun. 2014.   h

[2] L. Restrepo-Pérez, N. Durango-Londoño, N. E. Gómez-Suárez, F. González-Ramírez y N. Rivera-Bonilla, “Prototipo de incubadora neonatal,” Revista Ingeniería Biomédica, vol. 1, no. 1, pp. 55–59, 2007.

[3] M. van Leeuwen et al., European Standards of Care for Newborn Health: Temperature management in newborn infants, EFCNI, Nov. 2018.   

[4] R. B. Knobel-Dail, “Role of effective thermoregulation in premature neonates,” Research and Reports in Neonatology, vol. 4, pp. 147–156, 2014.   

[5] R. E. Black et al., “Global, regional, and national causes of child mortality in 2008: a systematic analysis,” Lancet, vol. 375, pp. 1969–1987, Jun. 2010.  

[6] International Electrotechnical Commission, IEC 60601-2-19: Medical electrical equipment – Particular requirements for the basic safety and essential performance of infant incubators.

[7] International Electrotechnical Commission, IEC 60601-1: Medical electrical equipment – General requirements for basic safety and essential performance.

[8] Ministerio de la Protección Social de Colombia, Decreto 4725 de 2005, por el cual se reglamenta el régimen de registros sanitarios, permiso de comercialización y vigilancia sanitaria de los dispositivos médicos para uso humano.
[9] Universidad Militar Nueva Granada. (2025). Guía de preparación práctica de laboratorio: Diseño y construcción de una incubadora neonatal a escala. Laboratorio de Instrumentación Biomédica y Biosensores.

[10] SparkFun Electronics. (s. f.). Load Cell Amplifier HX711 Breakout Hookup Guide. SparkFun Learn.  

[11] Instrumentalia S.A.S. (s. f.). Incubadora YP920.  

[12] Instrumentalia S.A.S. (s. f.). Incubadora neonatal YP2000. Instrumentalia.   

[13] LEEX Medical. (s. f.). Incubadora neonatal B3.  

[14] IMEDS Colombia. (s. f.). Incubadora neonatal B3.   

[15] World Health Organization. (2025). Essential newborn care course, second edition. World Health Organization.  

[16] World Health Organization. (2021). National guidelines for comprehensive newborn care. WHO Platform for Maternal, Newborn, Child and Adolescent Health. 


[17] Knobel-Dail, R. B. (2014). Role of effective thermoregulation in premature neonates. Research and Reports in Neonatology, 4, 147–156.  

[18] Scottish Government, NHS Greater Glasgow and Clyde, “Thermal care of the neonate,” Clinical Guidelines

[19] Royal Children's Hospital Melbourne, “Thermoregulation for neonates and infants,” Clinical Practice Guidelines

[20] Colombia, Ministerio de Salud y Protección Social, “Decreto 438 de 2020,” Gestor Normativo de Función Pública
