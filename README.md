# Alarm-Clock-State-Machine
Despertador arduino basado en las máquinas de estado

La idea es desarrollar un reloj despertador con:
* Arduino NANO
* RTC DS3231
* Display I2C OLED 0.96" donde pintaremos una interface del tipo

          ===================================          
          #     # #     #     #     # #     # 
          #     # #     #     ##   ## ##   ## 
          #     # #     #  #  # # # # # # # # 
          ####### #######     #  #  # #  #  # 
          #     # #     #  #  #     # #     # 
          #     # #     #     #     # #     # 
          #     # #     #     #     # #     # 
          
           Alarm: HH:MM      Day: YYYY-MM-DD
          ===================================

Botón que servirá para entrar en el SetUp, e irá pasando por HH, MM, HH, MM, YYYY, MM, DD
Potenciómetro para seleccionar el valor de cada elemento. Podrá tener diversos valores según el dato cambiante:
          Hora 
              HH -> 00-23
              MM -> 00-59
          Alarma
              HH -> 00-23
              MM -> 00-59
          Día:
              YYYY -> 2003-2100
              MM -> 01-12
              DD -> 01-31
Sensor PIR para detectar la no presencia de personas y apagar el display OLED para ahorrar batería que podría estar alimentado por una pata digital y así poder apagarlo totalmente.

Botón Apagado de la alarma que servirá para parar la alarma mientras suena. Para pararla definitivamente habrá que pulsar el otro botón de setup.

Una vez suene la alarma, si no se apaga, se parará al de un minuto, de cualquier forma esperará un tiempo (puede ser un minuto, 3 y 10) hasta reactivarse (esto puede repetirse varias veces). Estaría bien indicar que el despertador está esperando volver a sonar por si nos despertamos y no nos ha dado tiempo a apagar la alarma ya que podría volver a sonar cuando estemos en la ducha molestando a otras personas.

Tendremos una variables globales HORA_HH,HORA_MM, ALARMA_HH, ALARMA_MM, DIA_YY, DIA_MM, DIA_DD
Desarrollaremos una función para pintar la hora, etc y que tomará los datos de las variables globales
Además habrá una variable que indica que parámetro se pinta en video inverso en el caso de poder cambiar con el potenciometro. {HH,HM,AH,AM,DY,DM,DD}
La función que pinta la pantalla podrá pintar también una alerta de batería o si está en modo Wait esperando a que se repita la alarma.

Creo que no es necesario pintar continuamente, sino cuando cambie el display o pasemos de apagado a encendido.


MAQUINA DE ESTADOS
==================

Estado NULL: Es un estado en el que el reloj está funcionando y cada minuto se cambia el display. 
Se podría saber si ha cambiado el display si los cambios se realizan a través de funciones que indiquen en una variable el cambio.

NULL + BS (Botón Setup) -> SETUP_HH
-----------------------------------
Al punsar el botón de Setup pasaremos a marcar la hora como posible valor a cambiar. Esto se haría pintandola en video inverso.

SETUP_HH + Cambios en el Potenciómetro -> SET_HH
------------------------------------------------
El valor del Potenciómetro se establece en el valor de HORA_HH y se pinta la pantalla. Los valores del potenciometro se corresponderán entre 00 y 23.

SETUP_HH + BS -> SETUP_MM
-------------------------
SET_HH + BS -> SETUP_MM
-----------------------
Al igual que con la hora (HORA_HH) se selecciona para modificar los minutos y se pinta la pantalla. 

SETUP_MM + Potenciometro -> SET_MM
----------------------------------
Igual que con los minutos (HORA_MM) se lee el potenciómetro y se establece el valor correspondiente en la variable HORA_MM. Los valores del potenciómentro se correspondrán entre 00 y 59.

Lo mismo para ALARMA_HH (00-23), ALARMA_MM (00-59), DIA_YY (2003-2103), DIA_MM (00-12), DIA_DD (00-31). Habrá que tener en cuenta el año y mes para los posibles valores de la variable DIA_DD (28/29, 30, 31).

NULL + Alarma = Hora Actual + Wait -> ALARMA
--------------------------------------------
Con un Wait inicial = 0, cuando la HH:MM coincida con la Alarma HH:MM pasaremos al estado ALARMA donde sonará una nota cada vez del tono seleccionado. Esto permitirá seguir procesando botones, etc mientras suena para poder parar de sonar.
Además se establece Wait = 2 * Wait + 3 -> Se espera en volver a sonar 3 minutos, luego 9, ... Habrá que establecer un máximo.

ALARMA + BS -> NULL
--------------------
Se establece Wait = 0 por lo que no volverá a sonar hasta el próximo día.

ALARMA + Botón Alarma -> NULL
-----------------------------
Al no establecer Wait a 0 volvería a sonar cuando Alarma = Hora + Wait.

-----------------------------------
Tengo pendiente pensar lo de sumar a la hora el tiempo Wait para comparar con la hora
Y poder desactivar la alarma (Igual podemos pensar que el potenciómetro comience en -1 hasta 23 para poder desactivar la alarma.
