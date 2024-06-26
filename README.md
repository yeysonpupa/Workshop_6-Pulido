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

![Diagrama sin título drawio](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/39f3f606-2dd9-49f5-af62-869f58d732e8)

Figura 3: Diagrama de Actividad del sistema.

## Circuito

Basado en este entendimiento del problema, se prosigue a realizar la construcción digital del circuito haciendo uso de TinkerCad mostrado en la Figura 4, donde se conectaron lso 2 Arduinos UNO según las especificaciones de pines de las que se habló en el marco teórico, donde para realizar el protocolo I2C es necesario conectar el pin de reloj en serie (SCL), correspondiente al pin A5 y el pin de datos en serie (SDA), correspondiente al pin A4, además, de las respectivas GND y VCC [7].

Por otro lado, se conecta el sensor de temperatura, que para el caso de TinkerCad es de referencia TMP36, sin embargo, cumple la misma función que el sensor LM35DZ que será usado en la implementación real. Finalmente, se conecta la alerta visual LED junto con su debida resistencia de 220 Ohms [6].

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/ae07f47d-cd60-4aaa-95cb-59f484a48907)

Figura 4: Circuito del sistema en TinkerCad.

## Código

Con la configuración realizada, es momento de relizar la lógica de todo el proceso, para esto, además del lenguaje de Arduino que conmunmente se usa, se debe importar la librería Wire de la cual se habló anteriormente, pues solo con esta librería se puede configurar el envío y recibimiento de datos entre el Arduino esclavo y el Arduino Maestro. Además, se hace uso de la documentación de I2C brindada por Arduino con tal de saber qué comandos utilizar para configurar cada puerto [7].

Primero, se presenta la configuración del Arduino esclavo, con el código expuesto en la Figura 5.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/f43ce3a4-5e49-4490-96ea-7a7a41da02f1)

Figura 5: Código Arduino esclavo [7] [10] [11].

En este código se inicia con la importación de la librería Wire, mediante *#include <Wire.h>*, posteriormente se une el esclavo al bus I2C con la dirección #8 con *Wire.begin*, luego se establece el *requestEvent* para ser llamado cada vez que el maestro solicite el dato de temperatura con *Wire.onRequest(requestEvent)*.

En la función de *requestEvent*, primero se lee el valor analógico obtenido mediante el pin A0 con *analogRead*, donde está conectado el sensor de temperatura LM35DZ, para posteriormente ser leido como un voltaje usando la referencia de 5V de la placa y  siendo convertido a digital usando la resolución de conversión análoga-digital [11]. Finalmente, se calcula la temperatura en grados centígrados, donde se sabe que 1ºC es igual a 10mV [11].

Por otro lado, se incluye la función básica de monitor serial para ver el valor de temperatura capturado, y posteriormente este se envía al Arduino maestro haciendo uso del comando *Wire.write*.

Ahora, se presenta la configuración del Arduino maestro, con el código expuesto en la Figura 6.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/0dbd72f7-5544-4b53-8f26-5829c6ce555d)

Figura 6: Código Arduino maestro [7] [10] [11].

En el segundo código, se importa de igual manera la librería Wire y se configura el pin digital 13 como salida, correspondiente al LED, además, se inicia la biblioteca Wire sin parámetros, es decir, *Wire.begin()*, lo que configura ese dispositivo Arduino como maestro en el bus I2C [7].

Ahora, haciendo uso del bucle, se llame a *Wire.requestFrom(8, 1)*, el cual solicita los datos del Arduino esclavo que se dejó configurado on la dirección #8 en el bus I2C. Después, se genera el bucle *while (Wire.available())*, que ejecutará la recolección de temperatura siempre y cuando haya datos disponibles para leer del esclavo, los cuales son capturados con *Wire.read()*.

Al capturar la temperatura se realizan 2 acciones, la de imprimirla en el monitor serial y la de verificar si es mayor de 30°C, para así acivar o desactivar la alerta LED que se tiene en el pin 13, esto se hace con una lógica simple de condicional y el *digitalWrite*. Finalmente, se define con *delay* que la captura de datos se realice cada 1 segundo, tal como lo pide el enunciado del workshop.

Con el código realizado para cada parte del bus de datos I2C, se realizan las pruebas pertinentes para saber qué funciona, cuyas evidencias se muestran en la Figura 7.

![Imagen1](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/940e3c5c-00f5-4b5b-8944-81280342c202)

Figura 7: Funcionamiento del sistema en TinkerCad.

En la Figura 7.a, se ve como el Arduino esclavo envía la temperatura por debajo de los 30°C y por consiguiente el LED está apagado, mientras que en la Figura 7.b, el Arduino esclavo está enviando una temperatura mayor a 30°C, haciendo que el LED se encienda. Para el otro lado del bus I2C, en la Figura 7.c, se ve como el Arduino maestro recibe una temperatura por debajo de los 30°C y por consiguiente no enciende el LED, mientras que en la Figura 7.d, el Arduino maestro está recibiendo una temperatura mayor a 30°C, haciendo que encienda el LED.

## Implementación IoT

Para pasar los datos a la nube, se debe reconfigurar la implemetación realizada en TinkerCad, pues se busca que el dato que llega al Arduino maestro pase a la plataforma IoT ThingSpeak, sin embargo, no es posible hacerlo a menos de que se conecte a la red, para esto, simplemente se reemplaza el Arduino maestro por un módulo ESP32 mostrado en la Figura 8 y el cual permite conectarse vía wifi a ThingSpeak.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/32f11159-3dc6-4361-a397-095f65084e1c)

Figura 8: Arduino ESP32 [12].

En ThingSpeak, se crea una cuenta y se genera un nuevo canal de visualización, donde se configura para que monitoreé la variable de temperatura, como se ve en la Figura 9.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/dc35e540-50b2-42a1-af30-0e712012aaea)

Figura 9: Configuración de canal en ThingSpeak.

Además, se genera un ID del canal y una API Key de escritura (Figura 10), las cuales serán útiles para el siguiente paso.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/2f3e82b2-90b4-4385-9761-59346d81e287)

Figura 10: ID Channel y API Key ThingSpeak.

Teniendo ThingSpeak configurado, se modifica el código del Arduno maestro, incluyendo ahora las librería de WiFi y ThingSpeak para realizar el intercambio de datos entre el ESP32 y ThingSpeak [13], tal como se observa en el código de la Figura 11, especificando datos importantes para la conexión como el SSID del WiFi mediante el cual se realizará la conexión, la contraseña del wifi del mismo, el ID de canal de ThingSpeak y la API key de escritura [14].

![Imagen1](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/5eecb338-6219-4f3e-9854-b8908413b48c)

Figura 11: Código Arduino maestro usando ESP32 [10] [13] [14].

Ya que el envío a ThingSpeak sucede únicamente por parte del maestro, no es necesario cambiar el código del esclavo, manteniéndose igual al mostrado en la Figura 5.

## Implementación Física

Ahora, es momento de realizar la conexión en físico, pero teniendo en cuenta que a diferencia de la versión en TinkerCad, toda la lógica del Arduino maestro será contenida en el ESP32, el cual también tiene pines de SCL y SDA que deben ser conectados al Arduino esclavo [15], quedando el montaje como se ve en la Figura 14.

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/472fe3b1-ae1d-46fe-ae96-cae95ddea661)

Figura 14: Prototipo físico del sistema.

Se conecta la placa de Arduino esclavo al computador y usando Arduino IDE se carga el código dispuesto, por lo que al correrlo, se puede observar como el monitor serial recoge el valor de manera local proveniente del sensor de temperatura (Figura 15).

![image](https://github.com/yeysonpupa/Workshop_6-Pulido/assets/101272542/f10b1eab-4088-4cd5-aa5e-c5c54a5ce302)

Figura 15: Funcionamiento del Arduino esclavo del prototipo.

Aunque el objetivo final era que el Arduino esclavo pasara los datos al ESP32 de manera física I2C y posteriormente a ThingSpeak en la nube, la placa ESP32 utilizada para la implementación de la Figura 15 no se pudo conectar correctamente al computador con el Arduino IDE, por lo que el código maestro de la Figura 11 no pudo ser cargado, dejando la implementación física únicamente con el funcionamiento del esclavo.

## Video

Todo el proceso de diseño, programación, prototipado y demostración presentado a lo largo de este workshop es presentado en el siguiente video: https://youtu.be/glLmryZBP2U

## Conclusiones

Se puede concluir que el sistema fue capaz de responder de manera adecuada a las variaciones de temperatura en tiempo real, activando el LED cuando la temperatura excedía los 30°C, lo que sirve como una alerta visual efectiva para los usuarios. Esto indica que el diseño del sistema y de la lógica de programación implementada en las placas fue la adecuada para manejar los escenarios al momento de las pruebas en el prototipo real.

A su vez, la utilización del protocolo I2C permitió una comunicación eficiente y estructurada entre las 2 placas Arduino, destacando la flexibilidad que se tiene con este tipo de configuración al tener un maestro y un esclavo que se comunican entre sí. Además, la exploración de la librería Wire brindó un primer acercamiento a este tipo de programaciones que se pueden realizar en una placa Arduino, expandiendo el concoimiento de programación que se tenía hasta el momento sobre estos microcontroladores.

Se adquirió conocimiento valioso sobre como realizar una conexión IoT usando elementos de prototipado rápido a pesar de no resultar como se quería, conociendo la forma de cómo ThingSpeak sirve como herramienta para realizar un monitoreo en tiempo real de variables tiempo real, lo cual queda precisamente como una deuda técnica de este proyecto.

Finalmente, la articulación de planear una idea desde el diagrama de actividades bridnó un entendimiento claro desde el inicio sobre los requerimientos del sistema y el camino que debía seguir la lógica de programación, facilitando la construcción del código y su posterior implementación exitosa en el ambiente simulado de TinkerCad y en el prototipo físico.

### Referencias 

[1] Grupo de investigación BISITE. (s. f. ). "¿Cómo surgió el internet de las cosas?". BISITE. [En línea]. Disponible en: https://bisite.usal.es/es/blog/formacion/21/04/21/como-surgio-el-internet-de-las-cosas-BISITE#:~:text=Se%20podr%C3%ADa%20decir%20que%2C%20fue,de%20internet%20en%20cualquier%20cosa. (Accedido Mayo 1, 2024).

[2] Azure. (s. f. ). "¿Qué es IoT?". [En línea]. Azure. Disponible en: https://azure.microsoft.com/es-es/resources/cloud-computing-dictionary/what-is-iot (Accedido Mayo 1, 2024).

[3] AWS. (2023). "¿Qué es IoT (Internet de las cosas)?". AWS. [En línea]. Disponible en: https://aws.amazon.com/es/what-is/iot/#:~:text=El%20t%C3%A9rmino%20IoT%2C%20o%20Internet,como%20entre%20los%20propios%20dispositivos. (Accedido Mayo 1, 2024).

[4] Oracle. (s. f. ). "¿Qué es el IoT?". Oracle. [En línea]. Disponible en: https://www.oracle.com/co/internet-of-things/what-is-iot/ (Accedido Mayo 1, 2024).

[5] Paguayo. (2022, Agosto 23). "Protocolo I2C (Inter Integrated Circuit)". MCI Educacion. [En línea]. Disponible en: https://cursos.mcielectronics.cl/2022/08/23/i2c/ (Accedido Mayo 1, 2024).

[6] Aranda, J. (2024). "Automatización & Control de Procesos - Lecture#7 - Automatizacion_4.0 - Part 1". (Accedido Mayo 1, 2024). 

[7] Zambetti, N., Söderby, K., & Hylén, J. (2024, Enero 16). "Protocolo de circuito interintegrado (I2C)". Arduino Docs. [En línea]. Disponible en: https://docs.arduino.cc/learn/communication/wire/ (Accedido Mayo 1, 2024).

[8] ThingSpeak. (s. f. ). "ThingSpeak para proyectos de IoT". ThingSpeak. [En línea]. Disponible en: https://thingspeak.com/ (Accedido Mayo 1, 2024).

[9] Electronica Caribe. (s. f. ). "LM35DZ Sensor de temperatura analógico". Electronica Caribe. [Imagen]. Disponible en: https://electronicacaribe.com/product/lm35dz-sensor-de-temperatura-analogico/ (Accedido Mayo 1, 2024).

[10] OpenAI. (2015). ChatGPT [En línea]. Disponible en: https://openai.com/chatgpt (Accedido Mayo 1, 2024).

[11] Castaño, S. (s. f. ). "ADC Arduino – Entradas Analógicas". Control Automático Educación. [En línea]. Disponible en: https://controlautomaticoeducacion.com/arduino/entradas-analogicas-adc/ (Accedido Mayo 1, 2024).

[12] Colombianizate. (s. f. ). "Tarjeta ESP32 wifi + bluetooth 4.2". Colombianizate. [Imagen]. Disponible en: https://www.colombianizate.com.co/tienda/arduino/wifi-bluetooth-rf/tarjeta-esp32-wifi-bluetooth-4-2/ (Accedido Mayo 2, 2024).

[13] TodoMaker. (2022, Enero 10). "ESP32 + THINGSBOARD: ENVIO DE DATOS". TodoMaker. [Video]. Disponible en: https://www.youtube.com/watch?v=0l_5hioeBAE (Accedido Mayo 2, 2024).

[14] Carranza, S. (2022, Enero 4). "ENVÍO DE DATOS A THINGSPEAK USANDO ESP32". TodoMaker. [En línea]. Disponible en: https://todomaker.com/blog/envio-de-datos-a-thingspeak-usando-esp32/ (Accedido Mayo 2, 2024).

[15] Lozano, R. (2021, Junio 13). "Programar ESP32 con IDE Arduino". Talos Electronics. [En línea]. Disponible en: https://www.taloselectronics.com/blogs/tutoriales/programar-esp32-con-ide-arduino (Accedido Mayo 2, 2024).
