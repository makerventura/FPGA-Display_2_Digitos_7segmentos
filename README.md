![](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/Display_7seg_DEC_UNID.png)



# Bloque 7seg-DEC-UNID

* **Introducción :**

Se trata de una bloque especial preparado para controlar **dos displays de 7 segmentos de tipo ánodo común en paralelo** , uno para indicar la cifra de **DECENAS** y otro para las **UNIDADES** ,  utilizando la técnica del multiplexado .

* **Forma de conectar los displays a la placa :**

  Con el fin de poder conectar de manera correcta este bloque de circuito directamente a los bloques creados por @Obijuan en Icestudio y que podéis encontrar en la esquina superior derecha en  **Comb > Decodificadores > 7seg** ( bloques con decodificadores ) , **Const > 7seg** ( bloques de constantes ) , las conexiones físicas de este circuito se deben hacer de la siguiente manera , según la placa que tengáis :

  - **Placa Icezum Alhambra** :

    La entrada de datos a los leds ( a-b-c-d-e-f-g ) se realizará utilizando las salidas GPi  de 3.3. Volts ( del GP6 al GP0) de la placa Icezum Alhambra , mediante la **salida datos[6:0] ** del bloque , es decir , < Datos6[GP6 (a)] - Datos5[GP5 (b)] - Datos4[GP4 (c)] - Datos3[GP3 (d)] - Datos2[GP2 (e)] - Datos1[GP1 (f)] - Datos0[GP0 (g)] >  . 

    **Nota importante : Este bloque no está preparado para controlar el dato del punto decimal "dp" . Si alguien lo necesita , el circuito físico que más delante os detallo tiene la conexión "dp" preparada para conectar al pin GP7 , y para utilizarlo habría que realizar su cableado "fuera" del bloque mismo .**

    El control de encendido y apagado de los dos displays ( DECENAS / UNIDADES )  se realiza mediante otras dos salidas cualquiera de 5 Volts ( por ejemplo D13-D12 ) conectadas a la **salida On-Off[1:0]** del bloque. 

    

  - **Placa Alhambra II** :

    La entrada de datos a los leds ( a-b-c-d-e-f-g ) se realizará utilizando 7 pines cualesquiera de la placa mediante la **salida datos[6:0] ** del bloque, pero yo recomiendo esta sucesión de los mismos : 

    < Datos6[DD3 (a)] - Datos5[D1 (b)] - Datos4[DD2 (c)] - Datos3[D2 (d)] - Datos2[DD1 (e)] - Datos1[D3 (f)] - Datos0[DD0 (g)] >   . 

    **Nota importante : Este bloque no está preparado para controlar el dato del punto decimal "dp" . Si alguien lo necesita , el circuito físico tiene la conexión "dp" preparada para conectar al pin D0 , y para utilizarlo habría que realizar su cableado "fuera" del bloque mismo .**

    El control de encendido y apagado de los dos displays ( DECENAS / UNIDADES )  se realiza mediante otras dos salidas cualquiera de 5 Volts ( por ejemplo D5-D4 ) conectadas a la **salida On-Off[1:0]** del bloque. 

* **Control del bloque y de la información :**

Para que un led cualquiera de cualquiera de los dos displays ( DECENAS/UNDADES) se encienda , por la conexión correspondiente del bus datos[6:0] tendremos que enviar un **"cero"** , mientras que simultáneamente  por la salida **On-Off[1:0]** correspondiente al display en el que queremos que aparezca encendido ( On-Off(1) > Decenas , On-Off(0) > Unidades ) tendrá que salir una señal de 5 Volts ( señal = 1 ) .

La información de los leds que tienen que iluminarse o permanecer apagados se introducen en el bloque de multiplexado a través de los buses de datos denominados **unid[6:0]** y **dec[6:0]** . Recordad , que al tratarse de un circuito construido con displays de  segmentos de **ánodo común** , los leds que queramos iluminar tendrán que aparecer en el bloque de datos o la tabla de datos como un **cero** .

**Nota**: 

Si queremos que para una aplicación en concreto uno de los dos dígitos permanezca **apagado** deberemos conectarle como **entrada una constante K de valor decimal 127 ( binario 1111111) ** .

Finalmente , este bloque permite aprender el efecto de la **frecuencia de barrido del multiplexado** en la presentación de la información en ambos displays introduciendo cualquier número entero al parámetro exterior **Hz** . Se puede comenzar dando como parámetro una frecuencia baja de 1 o 2 Hzs , para ir aumentando paulatinamente a 10 Hzs o 20 Hzs , alcanzando finalmente 100 Hzs o 200 Hzs , frecuencia de refresco suficiente para engañar a la vista y que "parezca" que encendemos **simultáneamente** ambos displays .

Esta misma técnica y la estructura de este mismo bloque podría ser utilizada para gobernar sistemas de un número mayor de displays , sin más que incrementar el número de canales del multiplexor e incrementar el bus de control On-Off con los canales necesarios . El bus de datos permanecerá siempre inalterado .

* **Circuito de conexión eléctrica y elementos electrónicos a utilizar :**

Adjunto imagen con el cableado utilizado :

![1545844654086](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/diagrama 2 displays 7 segmentos.png)

Elementos electrónicos empleados :

- **Dos displays 7 segmentos de ánodo común**
- Un rac de **8 resistencias de 330 Ohm**, referencia **4116R LF 1-331 **o en formato individual .
- Dos transistores **BC547**

**Nota importante** : 

Revisar cuidadosamente la hoja de especificaciones técnicas de los displays concretos empleados , para saber en cada caso concreto el patillaje exacto que corresponde a cada uno de los segmentos y las entradas AC de los mismos porque es muy probable que no se identifiquen con los expuestos en la imagen anterior , que debe servir **solamente** como guía de conexionado y que requiere adaptarla a cada caso particular .

Aquí adjunto un par de fotos para que veáis como queda el display haciendo las conexiones en una placa perforada :

![](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/Display_img01.png)

![](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/Display_img02.png)



- **Ejemplo de utilización del bloque y comprobación de la influencia de la frecuencia de barrido :**

En el circuito del ejemplo adjunto vamos a ver de una manera sencilla cómo utilizar de manera práctica este bloque de circuito en la placa Alhambra II , así como la influencia de la frecuencia de barrido en el multiplexado .

Primero veamos cómo está construido el bloque internamente :

![](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/Display_7seg_DEC_UNID_img00.png)



En la imagen anterior podéis apreciar en líneas generales la base del funcionamiento del bloque . Hay un corazón que bombea "1"s y "0"s a una frecuencia variable y controlable como parámetro exterior en Hzs , y cuya señal llega simultáneamente a dos subcircuitos :

El **subcircuito superior controlado por una puerta AND se encarga de encender cada medio ciclo solamente uno de los dos displays **.

Mientras , el **subcircuito inferior controlado por un bloque multiplexor , se encarga de dejar paso hacia ambos displays alternativamente el dato que tengamos conectado en la entrada dec[6:0] o unid[6:0] . Obviamente solamente seremos capaces de ver dicho dato en los leds del display que se encuentre encendido en uno de los dos medios ciclos de la onda periódica que envía el corazón Hz .**

A continuación veamos la imagen de un ejemplo práctico donde vamos a conectar a las entradas de datos dec[6:0] y unid[6:0] dos datos constantes de 7 bits : Las letras A y F , respectivamente .

La frecuencia de barrido de los displays ( es decir , la frecuencia de encendido y apagado de los mismos) la iremos incrementando para ver cómo influye en la "apariencia" con la que se ve dicha información finalmente . **Comenzaremos con 2 Hzs , subiremos a 20 Hzs y terminaremos con 200 Hzs **, donde comprobaremos que la imagen **engaña ya a la vista y parece quedar fija en los dos displays a la vez **.

El punto decimal (dp) de los displays los vamos a controlar con un sencillo interruptor externo .

![](https://raw.githubusercontent.com/makerventura/Mundo_FPGA_libre/master/Imagenes/Img_Display7seg_DEC_UNID/Display_7seg_DEC_UNID_img01.png)



Para terminar , aquí os dejo un enlace al video donde se puede comprobar el funcionamiento del bloque a distintas frecuencias de barrido ( 2 Hz , 20 Hz , 200 Hz ) así como del control del punto decimal mediante un interruptor :

**Video de ejemplo :** https://youtu.be/jDfVmzxlp2Q

