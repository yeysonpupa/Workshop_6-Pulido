# Workshop 6 - Yeyson Esteban Pulido Parra
### Automatización y Control de Procesos

## Introducción 

Este workshop aborda la creación de un controlador de dos capas en una configuración maestro-esclavo utilizando comunicación I2C con dos placas Arduino UNO, sobre las cual se implementará un sistema en el que el Arduino esclavo estará equipado con un sensor de temperatura análogo LM35DZ, el cual enviará datos de temperatura al controlador maestro cada segundo. El controlador maestro tiene la tarea de monitorear estas lecturas de temperatura y está programado para activar un actuador LED cuando la temperatura exceda los 30°C, sirviendo como una alerta visual inmediata.

Además, se mostrará el proceso de integración de la configuración con la plataforma ThingSpeak, la cual permitirá el monitoreo de las lecturas actuales de temperatura en tiempo real, lo cual permitirá un acercamiento al prototipado usando tecnologías IoT.

## Marco Teórico

* Internet de las Cosas - IoT:

El término de Internet de las Cosas nace con la necesidad de interconexión para simplificar numerosos aspectos de nuestra vida diaria, facilitando no sólo tareas cotidiana, sino que también procesos en el ámbito industrial mediante la automatización y el control remoto [1]. Es por esto que, IoT se refire a la red de equipos, máquinas, productos y dispositivos conectados entre sí, los cuales pueden comunicarse tanto con la nube como entre los propios dispositivos [2], lo cual es soportado gracias al hardware y software que reune y procesa todos los datos provenientes de múltiples dispositivos; usualmente empleando tecnologías de machine learning e inteligencia artificial con el objetivo de realizar decisiones bien fundamentadas ante los usuarios [3].

Dentro de la industria 4.0, IoT tiene diversos campos de acción, desde las redes eléctricas inteligentes, pasando por ciudades inteligentes, cadenas de suministro digitales inteligentes, hasta proceso de fabricación inteligene y mantenimiento preventivo y predictivo; todo esto genera un volúmen de datos inimaginable, es por esto que se utlizan paneles de control, interfaces y alertas en tiempo real con tal de obtener claridad sobre indicadores de rendimiento, estadísticas de tiempo medio entre averías y otros datos relevantes [4].

* Inter-Integrated Circuit (I2C) Protocol:

El I2C es un protocolo que permite que varios circuitos integrados digitales perfiéricos o esclavos, se comuniquen con uno o más chips controladores o maestros [5]. Para el caso de este workshop ya se sabe que al usar 2 placas Arduino UNO, cada cual tomará su rol respectivo de esclavo y maestro al hacer la conexión, la cual es representada en la Figura 1.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/c7a90f25-c726-4b8f-a052-4ee5b0c0f48e)

Figura 1: Conexión para el protocolo I2C [6].

Según la imágen, el protocolo I2C requiere una conexión en dos sentidos, donde primero se tiene un pin de reloj en serie (SCL), correspondiente al pin A5 en cada placa de Arduino UNO, el cual se encarga de pulsar datos a intervalos regulares; y un pin de datos en serie (SDA), correspondiente al pin A4 en cada placa de Arduino UNO, a través del cual se envían datos entre los dos dispositivos [7]. En secciones posteriores se mostrará explicitamente esta conexión descrita.

Por otro lado, para activar el protocolo I2C y poder generar sistemas con él, es importante hacer uso de la biblioteca Wire, la cual usa Arduino para generar la configuración entre los dispositivos, a través del uso de diferentes comandos específicos de este paquete [7], los cuales serán utilizados para la creación de la lógica de este workshop.
  
* ThingSpeak:

ThingSpeak es una plataforma de análisis enfocada en el Internet de las Cosas, la cual busca faciltar la agregación, visualización y análisis de flujos de datos en tiempo real a través de sus servicios basados en la nube [8]. Con esta solución, el workshop se podrá beneficiar pues da la posibilidad de transmitir los datos cada segundo desde las placas, así como lo requieren las especificaciones, monitoreando y gestionando directamente el correcto funcionamiento del sistema propuesto a través de la representación gráfica de los datos en tiempo real sobre una interfaz o dashboard, facilitando la interpretación y el seguimiento de la temperatura.

* Sensor de Temperatura:

Para la realización de este workshop, se hizo uso del sensor de temperatura analógico LM35DZ, el cual se puede observar en la Figura 2, este será el sensor encargado de medir la temperatura en el momento y mandarla a la configuración I2C, para posteriormente ser enviada a ThingSpeak.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/c545cdb4-a5ea-4e0c-919e-695c9efc0b6d)

Figura 2: Sensor de temperatura analógico LM35DZ [9].

## Diagrama de Actividad

Para entender la lógica de diseño que debe seguir todo el sistema, se generó el diagrama de actividades mostrado en la Figura 3, el cual está dividido en carriles con cada agente involucrado para el correcto funcionamiento del sistema. En este se evidencia el camino que deben seguir los datos desde la toma analógica en el sensor de temperatura LM35DZ, pasando por su conversión digital en el Arduino esclavo y posterior envío al Arduino maestro, donde además de hacer la conexión con ThinkSpeak, se evalúa el valor de temperatura actual para saber si es necesario o no activar la alerta LED.

![Diagrama sin título drawio](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/6311d2fb-5615-4e8d-a7a4-00827c46f1d9)

Figura 3: Diagrama de Actividad del sistema [9].

## Circuito

## Código

## Implementación Física

## Implementación IoT

## Video

Todo el proceso de diseño, programación, prototipado y demostración presentado a lo largo de este workshop es presentado en el siguiente video:

## Conclusiones

### Referencias 

[1] Grupo de investigación BISITE. (s. f. ). "¿Cómo surgió el internet de las cosas?". BISITE. [En línea]. Disponible en: https://bisite.usal.es/es/blog/formacion/21/04/21/como-surgio-el-internet-de-las-cosas-BISITE#:~:text=Se%20podr%C3%ADa%20decir%20que%2C%20fue,de%20internet%20en%20cualquier%20cosa. (Accedido Mayo 1, 2024).

[2] Azure. (s. f. ). "¿Qué es IoT?". [En línea]. Azure. Disponible en: https://azure.microsoft.com/es-es/resources/cloud-computing-dictionary/what-is-iot (Accedido Mayo 1, 2024).

[3] AWS. (2023). "¿Qué es IoT (Internet de las cosas)?". AWS. [En línea]. Disponible en: https://aws.amazon.com/es/what-is/iot/#:~:text=El%20t%C3%A9rmino%20IoT%2C%20o%20Internet,como%20entre%20los%20propios%20dispositivos. (Accedido Mayo 1, 2024).

[4] Oracle. (s. f. ). "¿Qué es el IoT?". Oracle. [En línea]. Disponible en: https://www.oracle.com/co/internet-of-things/what-is-iot/ (Accedido Mayo 1, 2024).

[5] Paguayo. (2022, Agosto 23). "Protocolo I2C (Inter Integrated Circuit)". MCI Educacion. [En línea]. Disponible en: https://cursos.mcielectronics.cl/2022/08/23/i2c/ (Accedido Mayo 1, 2024).

[6] Aranda, J. (2024). "Automatización & Control de Procesos - Lecture#7 - Automatizacion_4.0 - Part 1". (Accedido Mayo 1, 2024). 

[7] Zambetti, N., Söderby, K., & Hylén, J. (2024, Enero 16). "Protocolo de circuito interintegrado (I2C)". Arduino Docs. [En línea]. Disponible en: https://docs.arduino.cc/learn/communication/wire/ (Accedido Mayo 1, 2024).

[8] ThingSpeak. (s. f. ). "ThingSpeak para proyectos de IoT". ThingSpeak. [En línea]. Disponible en: https://thingspeak.com/ (Accedido Mayo 1, 2024).

[9] Electronica Caribe. (s. f. ). "LM35DZ Sensor de temperatura analógico". Electronica Caribe. [Im<gen]. Disponible en: https://electronicacaribe.com/product/lm35dz-sensor-de-temperatura-analogico/ (Accedido Mayo 1, 2024).
