
Proyecto para desarrollar un despertador basado en Arduino.

Utilizaremos técnicas de programación de Máquinas de Estado.

En principio pienso usar:
Arduino NANO v3. ATmega328
RTC DS3231 I2C
OLED 128x64 I2C
Buzzer
Sensor movimientos (para apagar el display o pasar a sleep si no hay nadie)
Potenciómetro para cambiar la hora y los minutos tanto del día actual como de las alarmas.

Usamos la estructura de directorios siguiente:

\Alarm-Clock-State-Machine
----\doc -> Documentación del proyecto, enlaces de interés, manuales, ...
----\lib -> Irán las librerías en la versión que funcionaron
----\schematics -> Aquí irán los esquemas hechos con Fritz
----\Alarm-Clock-State-Machine -> Aquí irá el código fuente para Arduino

Este directorio estará bajo el directorio git y estará sincronizado con el proyecto en GitHub. 

El estar en GitHub, nos permitirá versionar el código, librerías, manuales, etc.
Posibles versiones:
V.0.0 Configuración inicial de Arduino SDK. No hace más que pasar el código del programa de ejemplo que enciende el led a la NANO.
V.1.0 Agregamos el display OLED y pintamos lo que será la interface del despertador.
V.2.0 Agregamos el RTC DS3231 permitiendo establecer el día/hora y pintandolo en el display. Hará falta un botón y el potenciómetro.
V.3.0 Agregamos la lógica de negocio de despertador, …


V.0 Programar el Arduino NANO

