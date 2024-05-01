# Workshop 6 - Yeyson Esteban Pulido Parra
### Automatización y Control de Procesos

## Introducción 

Este workshop aborda la creación de un controlador de dos niveles en una configuración maestro-esclavo utilizando comunicación I2C con dos placas Arduino UNO, sobre las cual se implementará un sistema en el que el Arduino esclavo estará equipado con un sensor de temperatura análogo LM35DZ, el cual enviará datos de temperatura al controlador maestro cada segundo. El controlador maestro tiene la tarea de monitorear estas lecturas de temperatura y está programado para activar un actuador LED cuando la temperatura exceda los 30°C, sirviendo como una alerta visual inmediata.

Además, se mostrará el proceso de integración de la configuración con la plataforma ThingSpeak, la cual permitirá el monitoreo de las lecturas actuales de temperatura en tiempo real, lo cual permitirá un acercamiento al prototipado usando tecnologías IoT.

## Marco Teórico

* Internet de las Cosas - IoT:
  El término de Internet de las Cosas nace con la necesidad de interconexión para simplificar numerosos aspectos de nuestra vida diaria, facilitando no sólo tareas cotidiana, sino que también procesos en el ámbito industrial mediante la automatización y el control remoto [1]. Es por esto que, IoT se refire a la red de equipos, máquinas, productos y dispositivos conectados entre sí, los cuales pueden comunicarse tanto con la nube como entre los propios dispositivos [2], lo cual es soportado gracias al hardware y software que reune y procesa todos los datos provenientes de múltiples dispositivos; usualmente empleando tecnologías de machine learning e inteligencia artificial con el objetivo de realizar decisiones bien fundamentadas ante los usuarios [3].
* Controladores de Dos Niveles:
* Comunicación I2C:
* ThingSpeak:
* Sensores de Temperatura:

## Diagrama de Actividad

## Circuito

## Código

## Implementación Física

## Implementación IoT

## Video

Todo el proceso de diseño, programación, prototipado y demostración presentado a lo largo de este workshop es presentado en el siguiente video:

## Conclusiones

### Referencias 

[1] Grupo de investigación BISITE. (s. f. ). "¿Cómo surgió el internet de las cosas?". [En línea]. Disponible en: https://bisite.usal.es/es/blog/formacion/21/04/21/como-surgio-el-internet-de-las-cosas-BISITE#:~:text=Se%20podr%C3%ADa%20decir%20que%2C%20fue,de%20internet%20en%20cualquier%20cosa. (Accedido Mayo 1, 2024).

[2] Azure. (s. f. ). "¿Qué es IoT?". [En línea]. Disponible en: https://azure.microsoft.com/es-es/resources/cloud-computing-dictionary/what-is-iot (Accedido Mayo 1, 2024).

[3] AWS. (2023). "¿Qué es IoT (Internet de las cosas)?". [En línea]. Disponible en: https://aws.amazon.com/es/what-is/iot/#:~:text=El%20t%C3%A9rmino%20IoT%2C%20o%20Internet,como%20entre%20los%20propios%20dispositivos. (Accedido Mayo 1, 2024).
