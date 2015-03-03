## Proyecto 2 - Interfaz de una nave espacial:  
En este capítulo vamos a construir algo que podría ser la interfaz de una nave espacial en una película de ciencia ficción de los años 70.  
Crearemos un panel de control con un pulsador y luces que se encenderán al accionar dicho pulsador. Un LED verde estará encendido hasta que pulsemos el botón. Cuando Arduino reciba la señal del pulsador, la luz verde se apagará y otras dos empezarán a parpadear.  
Los pines digitales de Arduino sólo pueden leer **dos estados**:  
  * Cuando hay tensión en un pin de entrada.  
  * Cuando no la hay.  

Este tipo de entrada es normalmente conocida como digital (o binaria, por tener dos estados). A estos dos estados se les refiere como `Alto` o `Bajo` *(`HIGH` y `LOW` en inglés)*. `HIGH` es lo mismo que decir hay tensión, y `LOW` lo mismo que no hay tensión.  
Cuando le indicamos a un pin de salida un nivel `HIGH` utilizando el comando **`digitalWrite()`**, lo estamos activando, pero cuando le indicamos un nivel `LOW`, lo estamos desactivando.  
Los pines digitales de Arduino pueden actuar tanto como **entradas** como **salidas**. En nuestro código, podemos configurarlos para que funcionen dependiendo de la acción que deseemos que hagan. Cuando dichos pins funcionan como salidas, podemos activar componentes tales como LEDs. Si los configuramos como entradas, podemos comprobar por ejemplo, si un pulsador ha sido activado o no.  
Los pines `0` y `1` se utilizan para la comunicación con el ordenador, por ello, es mejor utilizar los pines a partir del número `2`.  
### Montar el circuito:  
Pasemos ahora al montaje del circuito. Para ello seguiremos los pasos siguientes:  
  1.  Conecta la alimentación de `5V` y la tierra de la placa Arduino a la protoboard.  
  2.  Colocamos un LED verde y dos rojos en dicha protoboard.  
  3.  Cogemos una resistencia de `220Ω` y conectamos uno de sus terminales al cátodo del LED verde (la patilla corta) y el otro, lo conectamos a masa. Cogemos otras dos resistencias de `220Ω` y repetimos el proceso para los otros dos LEDs.  
  4.  A continuación, realizamos una conexión entre el ánodo del LED verde (la patilla larga) y el pin 3 del Arduino.  
  5.  Los ánodos de los LEDs rojos, los conectamos uno al pin 4 y el otro al pin 5.  
  6.  Colocamos un pulsador en la protoboard, tal y como se muestra en la figura de más abajo. Conectamos un lado del pulsador a la alimentación y el otro lado al pin 2 del Arduino. Necesitaremos añadir una resistencia de 10kΩ entre la tierra y la conexión del pulsador con el pin del Arduino.  
  
Tras seguir los pasos anteriores, debería de quedarnos un circuito como el de la figura siguiente:  
![](https://elgatoinquieto.files.wordpress.com/2014/01/protoboard.jpg "Protoboard")  
Su esquema eléctrico correspondiente sería:  
![](http://elgatoinquieto.files.wordpress.com/2014/01/esquema-electrico.jpg "Esquema")  
### El código:  
Todo programa de Arduino posee **dos funciones** principales. Las funciones son partes de un programa informático que ejecutan comandos específicos. Dichas funciones tienen nombres **únicos**, y son utilizadas cuando se las necesita. Las obligatorias son las llamadas **`setup()`** y **`loop()`**.  
Estas funciones necesitan ser **declaradas**, lo que significa que hay que decirle a Arduino la acción que van a llevar a cabo dichas funciones.  
Éstas, **`setup()`** y **`loop()`**, se declaran de la siguiente manera:  

    void setup(){
    
    }
    
    void loop(){
    
    }

En este programa, vamos a crear una variable antes de adentrarnos en la parte principal del programa. Las variables son nombres que se colocan en la memoria de Arduino y de las que podemos saber el valor de su contenido. Dicho contenido puede cambiar dependiendo de las instrucciones que le demos al programa.  
Los nombres de variables suelen describir que tipo de valor están guardando. Por ejemplo, una variable cuyo nombre sea `switchState`, nos estará diciendo que almacena el estado de un pulsador. Por otro lado, una variable cuyo nombre sea `x`, no nos estará diciendo mucho acerca de que valor está guardando.  
Para crear una variable necesitamos declarar de qué **tipo** es. El tipo de dato **`int`** se refiere a un número entero (también llamado `integral` o `integrer`). Éste es cualquier número sin coma decimal. Cuando declaramos una variable, normalmente debemos indicarle un valor inicial, a esto se le llama **inicializar** una variable. La declaración de dicha variable debe acabar siempre con un punto y coma `;`.  

    int switchState=0;

La función **`setup()`** solo se ejecuta una vez, cuando Arduino se pone en funcionamiento. Este es el punto donde configuraremos los pines digitales para que sean `INPUT` o `OUTPUT`, utilizando una función llamada **`pinMode()`**. Los pines conectados a los LEDs serán de salida y el pin del pulsador será de entrada:

    void setup(){
      pinMode(3, OUTPUT);
      pinMode(4, OUTPUT);
      pinMode(5, OUTPUT);
      pinMode(2, INPUT);
    }

La función **`loop()`** se ejecuta de forma continua después de que la función **`setup()`** haya concluido. En **`loop()`** es donde comprobaremos el voltaje de la entrada, y cambiaremos el estado de las salidas en `HIGH` o en `LOW`. Para comprobar el nivel de voltaje en la entrada, deberemos utilizar la función **`digitalRead()`**, la cual comprueba el voltaje del pin escogido. Para escoger el pin que queremos comprobar, **`digitalRead()`** necesita un argumento:

    void loop(){
      switchState = digitalRead(2);
        //Esto es una línea de comentario

Los argumentos son información que aplicamos a las funciones, diciéndoles a éstas cómo deben realizar su trabajo. Por ejemplo, **`digitalRead()`** necesita saber el pin que debe comprobar. En nuestro programa, **`digitalRead()`** comprobará el estado del pin `2` y guardará dicho valor en la variable *`switchState`*. Si hay tensión en el pin cuando ejecutamos a la función **`digitalRead()`**, la variable *`switchState`* almacenará el valor `HIGH`. Si no hay tensión en dicho pin, *`switchState`* guardará el valor `LOW`.  
En programación, la declaración **`if()`** comprara dos cosas, y determina si dicha comparación es **cierta o falsa**: `true` o `false`. A continuación, en función del resultado de la comparación, realizará las acciones que le hayamos indicado. Cuando en programación comparamos dos cosas, utilizamos el símbolo `==`. Si solo utilizáramos un signo igual, estaríamos asignando un valor a la variable en lugar de realizar dicha comparación:  

      if(switchState == LOW) {
        //El pulsador NO ha sido presionado

Entonces, **`digitalWrite()`** es el comando que nos permite enviar tensión `HIGH` o `LOW` a un pin de salida. Dicha función tiene dos parámetros: a qué pin debe enviar el voltaje y qué voltaje enviará, si `HIGH` o `LOW`:

        digitalWrite(3, HIGH);    //Led verde.
        digitalWrite(4, LOW);    //Led rojo.
        digitalWrite(5, LOW);    //Led rojo.
      }

La condición **`if()`** tiene un componente opcional llamado **`else`** que permite ejecutar código cuando no se cumple la primera condición. En nuestro caso, tras haber comprobado si el pulsador estaba en un nivel `LOW`, escribiremos el código para un nivel `HIGH` dentro de la condición **`else`**:  
Para hacer que los LEDs rojos parpadeen cuando el pulsador sea presionado, necesitamos apagar y encender dichos LEDs dentro de la condición **`else`**. El código sería el siguiente:  

      else{    //el pulsador es activado.
          digitalWrite(3, LOW);
          digitalWrite(4, LOW);
          digitalWrite(5, HIGH);
          delay(250);    //esperamos un cuarto de segundo.
          //cambiamos el estado de los LEDs.
          digitalWrite(4, HIGH);
          digitalWrite(5, LOW);
          delay(250);    //esperamos un cuarto de segundo.
        }
    }//volvemos al principio del bucle.

Después de asignarles a los LEDs un estado concreto, queremos que Arduino haga una pausa antes de que vuelva a cambiar el estado de los mismos. Si no esperamos, las luces cambiarán de estado tan rápido que nos parecerá que siempre están igual. Esto es debido a que Arduino ejecuta el bucle **`loop()`** miles de veces por segundo, y los LEDs cambian de estado tan rápido que no nos daríamos cuenta. La función **`delay()`** nos permite indicarle al Arduino que deje de ejecutar lo que esta haciendo durante el lapso de tiempo que le indiquemos. Por lo general, **`delay()`** tiene un argumento que indica el número de milisegundos antes de que siga ejecutando el código siguiente. Hay `1000` milisegundos en un segundo, por tanto, `delay(250)` realizará una pausa en la ejecución del código de un cuarto de segundo de duración.  
### Puesta en marcha:  
Una vez que hemos programado nuestro Arduino, veremos como el LED verde está encendido. Cuando apretemos el pulsador, los LEDs rojos empezarán a parpadear y el verde se apagará. Probemos a cambiar el tiempo de las dos funciones **`delay()`** prestando un poco de atención a lo que ocurre con las luces y cómo cambia la respuesta del sistema en función del tiempo de parpadeo. Cuando ejecutamos la función **`delay()`** en nuestro programa, ésta detiene cualquier otra funcionalidad. No se leerá ningún sensor hasta que no haya concluido el periodo de tiempo establecido. Los delays se utilizan con frecuencia, pero cuando diseñemos nuestros propios proyectos tenemos que tener cuidado con que no interfieran con nuestra interfaz de manera innecesaria.  
Bien, en este proyecto hemos creado nuestro primer programa de Arduino para controlar el estado de unos LEDs en función de un pulsador. Hemos utilizado variables, el condicional **`if()`** **`else`** y funciones para leer el estado de una entrada y cambiar el estado de las salidas.  
Espero que os haya gustado y que todas las dudas que tengáis al respecto puedan ser aclaradas mediante este turorial del proyecto.
