# Lab-SPI

Este informe trata sobre el desarrollo de una práctica de laboratorio enfocada en la medición del índice pletismográfico quirúrgico (SPI) en condiciones ambulatorias. La idea principal fue construir un sistema capaz de captar cambios en el volumen sanguíneo periférico, procesar esa señal y, a partir de ella, calcular el SPI en tiempo real.

Durante la práctica se trabajó inicialmente con un circuito analógico diseñado para capturar la señal pletismográfica. Sin embargo, en nuestro caso no se logró obtener una señal clara y estable usando únicamente ese montaje. Por esta razón, fue necesario utilizar un módulo sensor para adquirir la señal de forma más confiable y continuar con el desarrollo de la práctica.



Objetivo general

- Desarrollar un sistema de medición continua del índice pletismográfico quirúrgico (SPI) en condiciones ambulatorias.

Objetivos específicos


- Reconocer las características principales de la onda de pulso a partir de las cuales se obtiene el SPI.
- Construir un sistema que permita calcular el SPI en tiempo real en condiciones ambulatorias.
- Validar el funcionamiento del sistema mediante una maniobra fisiológica que produzca una respuesta similar a la del dolor agudo.

- 
¿Qué es el SPI?

El índice pletismográfico quirúrgico (SPI) es un parámetro que se usa para evaluar cambios relacionados con la actividad del sistema nervioso autónomo, especialmente en situaciones de estrés o dolor. Este índice se calcula a partir de variables obtenidas de la señal pletismográfica, como:

La amplitud de pulso
El intervalo entre latidos

El SPI intenta cuantificar cómo responde el cuerpo frente a un estímulo, usando información obtenida del pulso periférico.


Fundamento de la práctica

La práctica se basó en la captura de variaciones del volumen sanguíneo periférico usando un sistema óptico. Este tipo de medición se relaciona con la técnica de fotopletismografía (PPG), donde una fuente de luz ilumina el tejido y un sensor detecta cambios en la cantidad de luz absorbida o reflejada, los cuales dependen del flujo sanguíneo.

A partir de esta señal es posible: detectar cada latido, identificar máximos y mínimos, medir amplitudes de pulso, calcular intervalos entre pulsos,
y finalmente estimar el SPI.


Sobre el circuito implementado

En la guía original se propone un circuito analógico conformado por varias etapas:

Etapa de detección óptica
Filtro pasaaltas (HPF)
Filtro pasa bajas activo (LPF)
Etapa de ajuste de offset y ganancia, mediante amplificadores operacionales y potenciómetros.

La intención de este circuito era obtener una señal suficientemente limpia para visualizar en Arduino y luego procesarla, aunque se realizó el montaje y se intentaron varios ajustes, no fue posible obtener una señal pletismográfica clara y estable con el circuito analógico. 

Debido a esto, se tomó la decisión de usar un módulo sensor de pulso que permitiera adquirir la señal de manera más estable, el cual va conectado a una placa esp32. Este módulo facilitó la captura de la señal, ya que integra internamente una etapa de sensado más estable y, en algunos casos, parte del acondicionamiento necesario.

Gracias a esta modificación, se logró obtener una señal adecuada para visualizarla en el Serial Plotter, registrar los pulsos, detectar máximos y mínimos, y calcular el SPI de manera continua.


Materiales utilizados
Arduino UNO 
Sensor óptico / módulo de pulso
Computador con Arduino IDE
MATLAB
Protoboard y jumperrs
Fuente de alimentación
Agua con hielo para aplicar la técnica Cold Pressor Test (CPT)
Procedimiento realizado


Parte A. Adquisición de la señal
1. Montaje inicial del circuito

Primero se intentó trabajar con el circuito analógico propuesto en la guía. Se realizó la conexión de sus etapas y se conectó la salida a una entrada analógica del Arduino.

2. Verificación de la señal

Se usó el Serial Plotter del Arduino IDE para observar la señal obtenida. En esta etapa se hicieron varios ajustes en los potenciómetros buscando aumentar la ganancia, reducir el ruido, mejorar el nivel de la señal, y corregir el offset.

Debido a la imposibilidad de su desarrollo se conectó el módulo al Arduino y se verificó nuevamente la salida en el Serial Plotter. Con esta configuración sí se logró observar una señal periódica relacionada con el pulso del dedo colocado sobre el sensor.

Parte B. Cálculo del SPI

Con la señal ya disponible desde el módulo, se realizó la captura de datos en un tiempo definido por el usuario. Para la prueba principal se configuró una duración de  minutos.

2. Procesamiento en MATLAB

Se desarrolló un código en MATLAB capaz de leer o importar la señal capturada, detectar máximos y mínimos de cada pulso, calcular la amplitud de cada latido, estimar el intervalo entre pulsaciones, y calcular el SPI para cada latido.


3. Visualización de resultados

El código mostró los resultados en la ventana de comandos y, al final de la captura, generó una gráfica con la evolución del SPI en función del tiempo.


Para validar el comportamiento del sistema se utilizó la maniobra Cold Pressor Test (CPT), donde el sujeto de prubea le fue sometido un brazo a una bolsa helada,  durante un tiempo corto. Esto genera una respuesta del sistema nervioso simpático, produciendo cambios cardiovasculares que pueden reflejarse en la señal pletismográfica.

La captura duró 2 minutos, divididos así:

Primeros 40 segundos: condición basal

![WhatsApp Image 2026-03-26 at 9 48 29 PM](https://github.com/user-attachments/assets/83a40d17-cfbd-44e8-8212-4007e3170e4b)

Se observa la señal ppg y ligeramente en algunas ondas el nodo dicrótico y el SPI ronda valores de 40-45
Siguientes 40 segundos: aplicación del CPT

![WhatsApp Image 2026-03-26 at 9 48 29 PM (1)](https://github.com/user-attachments/assets/4de52d21-1a38-4d56-b23f-539ad2533998)

Al aplicar el Cold Pressor Test que en nuestro caso fue la aplicación de una bolsa de gel congelada en el antebrazo, se observa como disminuye la amplitud de la señal y al observar los valores del SPI se nota como este aumenta a un nivel mayor que el "normal" superando 50
Últimos 40 segundos: recuperación

![WhatsApp Image 2026-03-26 at 9 48 29 PM (1)](https://github.com/user-attachments/assets/b0a362e9-2ba0-4efe-8465-4282c15b182b)

Al quitar el gel congelado del antebrazo se observa como la señal va volviendo a los niveles de amplitud normales y se controla nuevamente, en el momento de recuperación los valores de SPI rondan los 45-50 

Durante cada etapa se registraron los valores del SPI para comparar cómo cambiaba antes, durante y después de la maniobra.

![WhatsApp Image 2026-03-26 at 9 48 30 PM (1)](https://github.com/user-attachments/assets/d568f382-d74a-4da3-b9ea-831c4f8a4d60)

En la gráfica del SPI se observa el momento en el que aumenta debido a la aplicación del CPT, aunque baja rápidamente por un motivo que se asume fue una presión aplicada diferente, respiración o habla al momento de la toma del CPT
Resultados esperados

Al finalizar la práctica se esperaba lograr:

La captura de variaciones del volumen sanguíneo periférico,
La observación de pulsos en tiempo real,
La extracción de características de la señal,
El cálculo cuantitativo del SPI, y la comparación del comportamiento del índice durante una maniobra fisiológica como el CPT.

Preguntas para discusión
-Pregunta 1:¿Cómo se relacionan las variaciones del volumen sanguíneo periférico con el balance autonómico?

Las variaciones del volumen se relacionan con el sistema de balance autonómico debido a que cuando el sistema simpático es activado genera vasoconstricción haciendo que el volumen se reduzca, mientras que la activación parasimpática aumenta el volumen debido a la vasodilatación.

-Pregunta 2:¿Cómo se compara el SPI con otros índices comúnmente empleados en cirugía, como el índice nocicepción-analgesia (ANI) y el índice de perfusión?

Al momento de cirugía con el SPI se basa en la PPG y los intervalos entre latidos, esto se interpreta como niveles analgésicos controlados o estrés durante la cirugía. Mientras que el ANI (Analgesia Nociception Index) se basa en la HRV reflejando actividad parasimpática lo que representa en este caso el control de dolor, a mayor ANI indica un alto tono vagal y un control del dolor, un ANI bajo indica dolor o estrés. El PI (Índice de perfusión) se basa en la relación pulsátil de la PPG lo que permite observarla de manera más periférica indicando activación simpática (vasoconstricción) o activación parasimpática (vaso dilatación).
Análisis


Uno de los puntos más importantes de esta práctica fue entender que, en laboratorio, no siempre el diseño teórico funciona exactamente como se espera. El circuito analógico tenía una base correcta y estaba pensado para acondicionar la señal del sensor óptico, pero en la implementación real aparecieron dificultades que impidieron obtener una señal útil.


Se comprendió el principio básico de adquisición de una señal pletismográfica a partir de cambios en el volumen sanguíneo periférico.
Aunque se intentó implementar el sistema usando un circuito analógico, no se logró obtener una señal adecuada para el análisis.
Como solución práctica, se utilizó un módulo sensor que permitió adquirir la señal de manera más estable.
Con la señal obtenida fue posible aplicar un algoritmo en MATLAB para detectar máximos y mínimos, estimar características del pulso y calcular el SPI, al principio tratamos de aplicar la detección de máximos y mínimos mediante el algoritmo pan tompkins, comprendimos que este tipo de detección de máximos y mínimos era erróneo para el método de ppg por lo que nos daba valores y señal SPI erróneos por lo que decidimos pasar a una detección mediante la primera derivada obteniendo mucho mejores resultados.
La maniobra Cold Pressor Test permitió observar cambios fisiológicos que ayudan a validar el comportamiento del sistema.
La práctica mostró la importancia de combinar adquisición experimental, procesamiento de señales y análisis fisiológico en aplicaciones biomédicas.
