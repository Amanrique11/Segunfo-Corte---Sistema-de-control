# Indetificacion de sistemas  por curva de reaccion 
## 1. Indetificacion de sistemas en lazo abierto 
La identificacion de sistemas por curva de reacion es un tecnica utilizada en el campo de control de sistemas para obtener un modeol aproximado del comportamieno de un sistema a partir de su respuesta a una entrada escalon. Esta tecnica es muy util cuaudno no se tiene un modelo matematico exacto del sistema, pero si se puede medir como responde ante cirtos estimulos.
>🔑 *Curva de reaccion:* Es la respuesta temporal del sistema a un entrada escalon.
## 2. Procedimineto Basico 
1. Aplicar una entrada escalon al sistema.
2. Medir la salida del sistema con respecto al tiempo.
3. Obtener parametros clave de la curva:
  - Tiempo Muerto(L)
  - Constante de tiempo($\tau$)
  - Ganancia del sistema(K)
>🔑 *Tiempo Muerto:* Tiempo entre la aplicacion del escalon y cuadno la salida comienza a cambiar.

>🔑 *Constante de tiempo:* Relacionado con la rapidez con que la salida alncanza  su valor final.

>🔑 *Ganancia del sistema:* Cambio en la salida divido entre el cambio en la entrada.

## 3. Modelo tipico: Sistema de primer orden con retardo
$$G(s) = \frac{K*e^{-Ls}}{\tau s+1}$$

Donde:
- K: Ganancai estatica.
- $\tau$: Costante de tiempo.
- L: Tiempo muerto.

## 4. Otros metodos de optener un modelo de planta
- Modelo matematico (Ecuaciones diferenciales).
- Identificacon de parametros en lazo cerrado.
- Analisis en frecuencia.
- Tecnica de inteligencia computacional.

## 5. Metodo empirico 
Se basa en obtener un modelo matematico aproximado de un proceso a partir de datos experimentales, sin necesidad de conocer su estructura interna. Este enfoque es especialemnte util en sistemas industriales donde la dinamica completa es compleja o desconociada.

### 5.1 ¿En que consiste el metodo empirco?
El procedimineto general para obtener un modelo empiroco que caracterice adecuadamente el comportamiento dinamico de un proceso consta de los siguientes pasos:

1. Diseñar las experencias a realizar: Planificar las pruebas necesarias para obtener datos representavos del sistema.
2. Ejecutar las experencias programadas en las condiciones adecuadas: Realizar las pruebas bajo condiciones controladas para asegurar la validez de los datos.
3. Determinar el tipo de modelo adecuado: Seleccionar una estructura de mole que represente el  comportamiento del sistema.
4. Estimar los parametros: Ajustar los palametros del modelo utilizando los datos experimientales obtenidos.
5.  Evaluar el modelo: Comparar las predicciones del modelo con los datos experiminetales para verificar su precision.
6.  Verificar el modelo: Realizar pruebas adicionales para confirmar que el modelos es valido bajo diferentes condiciones de operacion.

## 6. Metodo de Ziegler-Nichols
Es una tecnica clasica y empirica para sintonizar controles PID a  partir de la curva de reaccion de  un preceso. Este metodo es especialmente util cuando se dispone de datos experimentales y se desea obtener una configuracion inicial del controlador sin un modelo matematico detallado del sistema.

### 6.1 ¿En que consiste este metodo?
Este metodo se aplica a sistemas estables cuya respuesta a una entrada esaclon presenta una forma de "S", es decir, para sistemas de orden de sistemas de primer orden y sistemas de segundo orden, solo cuando son sobre amortiguado y criticamente amortiguiado. La idea es aproximar la dinimaca de los procesos a un modelo con retardo, utilizando paramentros obtenidos graficamnete de la curva de reaccion.

### 6.2 Procedimiemto paso a paso
1. Aplicar una entrada escalon al sistema en lazo abierto y registrar la respuesta del proceso.
2. Indentifacar la curva de reaccion.
3. Trazar la tangente en el punto de infelexion de la curva de reaccion. Este puntno es donde la pendiente de la curva es maxima.
4. Determina los parametros clave:
   - Tiempo muerto
   - Constate de tiempo: Es el tiempo desde el final del tiempo muerto hasta que la tangente alcanza el valor final del proceso.
  
![image](https://github.com/user-attachments/assets/7ca41ba4-91f3-495a-8f98-92a9bafb7ba4)

Figuara 1: Metodo de Ziegler-Nichols
###  6.3 Consideraciones
- Aplicabilidad: Este metodo es adecuado para procesos estables cuya respuesta al escalon presenta una forma en "S".
- Limetaciones:
   - Puede resultar en un sobre impulso significativo en la respuesta del sistema.
   - No es adecuado para proceoss con retardo denimiante o sistemas integrales.
- Recomendaciones:
   - Utilizar este metodo como punto de partida y realizar ajustes adicionales segun el comportamiento observado del sistema.

## 7. Metodo de Miller
Es una tecnica empirica utilizada en la indentificacion de sistemas, especialmente en el ambito de control de procesos. Aunque no es tan ampliamnete conocido como el metodo de Ziegler-Nichols, ofrece una alternativa para estimar los parametros de un sistema a partir de su respusta a una entarda escalon.

### 7.1 ¿En que consiste el metodo de Miller?
Se basa en analizar la respuesta temporal de un sistema ante una entrada escalon para deter,inar los parametros caracteristicos del modelo del proceso. A diferencia de otros metodos que pueden requerir conocimientos detallados del sistema o realizar pruebas de oscilaciones sostenida, el metodo de Miller utiliza directamente la curva de reaccion obtenida.

### 7.2 Procedimineto general
1. Aplicar una entrada esaclon al sistema y registrar la respuesta temporal del proceso.
2. Analizar la curva de reaccion obtenida, identificando caracteristicas clave como:
    - Tiempo muerto.
    - Constante de tiempo: Tiempo que tarda la respuesta en alcanzar un cierto porcentaje de su valor final, tipicamente el 63.2%.
    - Ganancia del sistema.
3. Ajustar un modelo de primer orden con retardo utilizando los parametros estimados.
4. Validar el modelo comparando la respuesta simulada del modelo con respuesta real al sistema.

![download](https://github.com/user-attachments/assets/6445711a-624b-4715-a654-171e434b5a79)


Figura 2: Modelo de Miller.

### 7.3 Aplicaciones y consideraciones
- Sintonizacion de controladores: Los parametros obtenidos medienat el modelo de Miller pueden utilizarse para sintonizar controladores PID, proporcionando una configuracion inicial que puede ser refinada posteriormente.
- Limitaciones:
   - La precision del metodo depende de la calidad de los datos experimentales y de la correcta identificacion de  los parametros en la curva de reaccion.
   - Pueden no ser adecuado para sistemas con comportamientos no lineales o con multiples retardos.
## 8. Metodo de indentificacion de dos puntos
Tambien es conocido como meetodo de Smith, es una tecnica empirica utilizada para estimar los parametros de un modelo de primer orden con retardo a partir de la respuesta a una entrada escalon. A diferencia de metodos como el de Ziegler-Nichols, que requiere trazar una tangennte en el punto de inflexion de la curva de reaccion, el metodo de dos puntos se basa en indentificar dos puntos espesfificos en la curva de respueta, lo que lo hace menos sensible al ruido y mas adecuado para sistemas sobreaamortiguado.

### 8.1 Fundamento del metodo  
El motodo de Smith se basa en seleccionar dos puntos caracteristicios en la curva de reaccion del proceso:
- $t_{28}$: Tiempo en el que la respuesta alcnza el 28.3% del cambio total.
- $t_{63}$: Tiempo en el que la respuesta alnza el 63.2% del cambio total.

Estos puntos corresponde a valores especificos en la respuesta de un sistem de primer orden y permiten establecer dos ecucaciones con dos incognitas: El tiempo muerto y la constante de tiempo.

### 8.2 Procedimiento paso a paso 
1. Aplicar una entrada escalon al sistema en lazo abierto y registrar la respuesta del proceso.
2. Determinar los tiempos $t_{28}$ y $t_{63}$ en la que la respuesta alcanza el 28.3% y 63.2% del cambio total, respectivamente.
3. Calcular los parametos del metodo utilizando las siguientes formulas:
   
   $$\tau = (-1.5)(t_{28})+(1.5)(t_{63})$$

   $$L = (1.5)(t_{28})+(-0.5)(t_{63})$$
4. Determinar la ganancia del proceso (K) como la relacion entre el cambio en la salida y el cambio en la entrada.

   $$K = \frac{\Delta y}{\Delta u}$$

Donde $\Delta y$ es el cambio total en la salida y $\Delta u$ es el cambio en la entrada.

5. Obtener el modelo de primer orden con retardo utilizando la funcion de transferencia.
   
![download](https://github.com/user-attachments/assets/51134b33-f6a4-483a-b495-e91de7765da3)

Figura 3: Modelo de dos puntos de Smith

### 8.3 Ventajas del metodo
- Simplicidad: No requiere trazar tangentes ni indentificar puntos de inflexion en la curva de reaccion.
- Robustez: Menos sensible al ruido eb la señal de respuesta, ya que se basa en puentos especificos de la curva.
- Aplicabilidad: Adecuado para sistemas sobreamortiguados y procesos dodne la respuesta nno presenta un punto de inflexion claro.

### 8.4 Consideraciones 
- Precision: La exactitud de los parametros estimasdos depende de la precision en la determinacion de los tiempo $t_{28}$ y $t_{63}$.
- Limitaciones: No es adecuado para sistemas con respuestas oscilatorias o no monotonas.

### 8.5 Otros metodos de dos puntos

| **Metodo** | **% $P_1$ ($t_1$)** |  **% $P_2$ ($t_2$)** | **A** | **B** | **C** | **D**|
|------------|-------------------|--------------------|-------|-------|-------|------|
| **Alfaro** | 25.0 | 75.0 | -0.910 | 0.910 | 1.262 | -0.262 |
| **Bröida** | 28.0 | 40.0 | -5.500 | 5.500 | 2.800 | -1.800 | 
| **Chen y Yang** | 33.0 | 67.0 | -1.400 | 1.400 | 1.540 | -0.540 |
| **Ho et al.** | 35.0 | 85.0 | -0.670 | 0.670 | 1.300 | -0.290 |
| **Smith** | 28.3 | 63.2 | -1.500 | 1.500 | 1.500 | -0.500 |

Tabla 1: Constantes para la indentidicacion de los modelos de primer orden mas tiempo muerto.
Donde A, B, C y D son constantes. 
## 9. Como se puede indentificar un sistema inestable (Lazo abierto)
Identificar un sistema inestable en lazo abierto requiere precaución, ya que aplicar una entrada escalón directamente puede provocar respuestas crecientes que dañen el sistema o los equipos de medición. 

## 10. Aproximacion FOPDTI
La aproximación FOPDTI (First Order Plus Dead Time Integrator) es una extensión del modelo FOPDT (Primer Orden con Tiempo Muerto) que incorpora un comportamiento integrador en el proceso. Este modelo es especialmente útil para representar sistemas que exhiben una respuesta creciente sin alcanzar un estado estacionario, como aquellos con acumulación de masa o energia.

### 10.1  ¿Qué es un modelo FOPDTI?
Un modelo FOPDTI describe procesos que combinan un comportamiento integrador con una dinámica de primer orden y un tiempo muerto. La función de transferencia típica de un sistema FOPDTI es:

$$ G(s) = \frac{K*e^{-Ls}}{(\tau *s+1)s}$$

### 10.2 Indentificacion de parametrs en un modelo FOPDTI
#### 10.2.1 Método de la línea tangente
Este método consiste en aplicar una entrada escalón al sistema y registrar la respuesta. Luego, se traza una tangente en el punto de inflexión de la curva de respuesta. Los parámetros se estiman de la siguiente manera:

- Tiempo muerto.
- Constante de tiempo.
- Ganancia.
Este método es sencillo y proporciona una estimación rápida de los parámetros del modelo.

![9b7d6bf417ad85fcdef814593a5004c6](https://github.com/user-attachments/assets/c38a0413-039d-47b2-960d-51edecce4ea8)

Figura 4: Respuestas en el sistema.

#### 10.2.2  Método de dos puntos
Este enfoque utiliza dos puntos específicos en la curva de respuesta, típicamente donde la salida alcanza el 28.3% y el 63.2% del cambio total. A partir de estos puntos, se calculan los parámetros del modelo utilizando fórmulas empíricas. Este método es menos sensible al ruido y no requiere identificar el punto de inflexión.

#### 10.2.3  Métodos computacionales
Se pueden emplear algoritmos de optimización, como el método de mínimos cuadrados no lineales, para ajustar el modelo FOPDTI a los datos experimentales. Estos métodos buscan minimizar la diferencia entre la respuesta del modelo y los datos reales, proporcionando una estimación precisa de los parámetros.

## 11.  Aproximacion IPDT
a aproximación IPDT (Integrating Plus Dead Time) es una técnica de modelado utilizada en sistemas de control para representar procesos que exhiben un comportamiento integrador junto con un retardo de tiempo. Este modelo es especialmente útil para describir sistemas donde la salida continúa cambiando indefinidamente ante una entrada constante, y existe un retraso significativo entre la entrada y la respuesta del sistema.

### 11.1 ¿Qué es un modelo IPDT?

Un modelo IPDT se caracteriza por tener una ganancia integradora y un tiempo muerto. La función de transferencia típica de un sistema IPDT es:

$$G(s) = \frac{K*e^{-Ls}}{s}$$

Este modelo es adecuado para procesos donde la salida se acumula con el tiempo, como en sistemas de nivel de tanques o control de temperatura, y donde hay un retardo antes de que la salida comience a responder a una entrada.

![fopdt_graphical_fit](https://github.com/user-attachments/assets/ebff2b6c-dafc-4c3a-ac3f-8122bbd75751)

Figura 5: Respusta de aproximacion IPDT.

### 11.2  Identificación de parámetros en un modelo IPDT
#### 11.2.1  Método de la línea tangente
Este método consiste en aplicar una entrada escalón al sistema y registrar la respuesta. Luego, se traza una tangente en el punto de inflexión de la curva de respuesta. Los parámetros se estiman de la siguiente manera:
- Tiempo muerto.
- Ganancia.
Este método es sencillo y proporciona una estimación rápida de los parámetros del modelo.

#### 10.2.2 Métodos computacionales
Se pueden emplear algoritmos de optimización, como el método de mínimos cuadrados no lineales, para ajustar el modelo IPDT a los datos experimentales. Estos métodos buscan minimizar la diferencia entre la respuesta del modelo y los datos reales, proporcionando una estimación precisa de los parámetros.
