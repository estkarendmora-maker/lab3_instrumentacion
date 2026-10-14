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

  
## Metodologia

### Montaje y prueba inicial del circuito


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

## Costos del prototipo

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

## Discusión
**Pregunta 1. ¿Qué otras variables (y por qué) además de las aquí mencionadas son críticas en el monitoreo neonatal? **


**Pregunta 2. ¿Qué haría falta para convertir el sistema desarrollado en una incubadora neonatal real? **
El prototipo ilustra el principio de operación (calefacción, control de temperatura y pesaje), pero está lejos de ser un dispositivo médico. Para convertirlo en una incubadora neonatal real harían falta varios cambios.

Primero, un control de temperatura más completo: una incubadora real combina el control de la temperatura del aire con el control servo por temperatura de la piel del neonato, medida con una sonda cutánea [3], [4]. El calefactor también debería ser un elemento resistivo con ventilador, ya que el bombillo del prototipo genera calor localizado e iluminación constante, lo que no es adecuado para un neonato. Además, se requiere control de humedad, que reduce las pérdidas de agua por la piel en prematuros, y la posibilidad de suministrar oxígeno suplementario [1], [4].

Segundo, un sistema de alarmas y seguridad independiente, con alertas audibles y visuales para sobretemperatura, falla del sensor, falla del ventilador y corte de energía, junto con un termostato de seguridad que corte el calefactor ante una falla, pues la hipertermia es un riesgo grave [5]. Todo esto debe cumplir la norma de seguridad de equipos electromédicos IEC 60601-1 y la norma particular de incubadoras IEC 60601-2-19 [6], [7], además del registro sanitario ante el INVIMA en Colombia [8].

Por último, se necesitan materiales de grado médico y fáciles de desinfectar, una cubierta de doble pared con filtración de aire y accesos seguros en lugar de una tapa de caja, una celda de carga calibrada con resolución de gramos, y una validación clínica del equipo.

**Pregunta 3. ¿Qué semejanzas hay entre una incubadora neonatal y una servo-cuna? **
Tanto la incubadora neonatal y la servo-cuna son dispositivos que proporcionan apoyo térmico al recién nacido, para mantener una temperatura corporal adecuada. Ambos equipos incorporan sensores de temperatura, sistemas de calentamiento y mecanismos de control que permiten regular el aporte de calor según las condiciones necesitadas [18], [19].

La principal diferencia está en su estructura y forma de transferencia de calor. La incubadora neonatal proporciona un ambiente cerrado y controlado, y la servo-cuna abierta y permite un acceso más directo al recién nacido durante los procedimientos médicos. [19], [20].

## Bibliografia
[1] C. G. K. Tran et al., “Designing a low-cost multifunctional infant incubator,” J. Lab. Autom., vol. 19, no. 3, pp. 332–337, Jun. 2014. https://doi.org/10.1177/2211068214530391

[2] L. Restrepo-Pérez, N. Durango-Londoño, N. E. Gómez-Suárez, F. González-Ramírez y N. Rivera-Bonilla, “Prototipo de incubadora neonatal,” Revista Ingeniería Biomédica, vol. 1, no. 1, pp. 55–59, 2007.

[3] M. van Leeuwen et al., European Standards of Care for Newborn Health: Temperature management in newborn infants, EFCNI, Nov. 2018. https://newborn-health-standards.org

[4] R. B. Knobel-Dail, “Role of effective thermoregulation in premature neonates,” Research and Reports in Neonatology, vol. 4, pp. 147–156, 2014. https://doi.org/10.2147/RRN.S52377

[5] R. E. Black et al., “Global, regional, and national causes of child mortality in 2008: a systematic analysis,” Lancet, vol. 375, pp. 1969–1987, Jun. 2010. https://doi.org/10.1016/S0140-6736(10)60549-1

[6] International Electrotechnical Commission, IEC 60601-2-19: Medical electrical equipment – Particular requirements for the basic safety and essential performance of infant incubators.

[7] International Electrotechnical Commission, IEC 60601-1: Medical electrical equipment – General requirements for basic safety and essential performance.

[8] Ministerio de la Protección Social de Colombia, Decreto 4725 de 2005, por el cual se reglamenta el régimen de registros sanitarios, permiso de comercialización y vigilancia sanitaria de los dispositivos médicos para uso humano.

[18] Scottish Government, NHS Greater Glasgow and Clyde, “Thermal care of the neonate,” Clinical Guidelines

[19] Royal Children's Hospital Melbourne, “Thermoregulation for neonates and infants,” Clinical Practice Guidelines

[20] Colombia, Ministerio de Salud y Protección Social, “Decreto 438 de 2020,” Gestor Normativo de Función Pública
