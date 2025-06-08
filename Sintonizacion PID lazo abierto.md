# Sintomizacion PID en lazo abierto.
## 1.¿Que es un controladro PID?
Es un mecanismo de control ampliamente utilizado en sistemas de automatizacion para mantener variables como temperatura, presion velociada o flujo en un valotr deseado, conocido como setpoit. Opera en un lazo de realimentacion, ajustando continuamente la salida del sistema en funcoin del error entre el valor medido y el setpoint.
## 2. ¿Como funciona un controlador PID?
El controlador PID combina trse acciones para corregir:
- Proporcional (P): Responde al error actual. Como mayor sea el error, mayor sera la accion correctiva.
- Integral (I): Considera la suma acumalada de error, ayudanto a elimar el error persistente.
- Derivativo (D): Anticipada errores basandose en la tasa de cambio, mejorarndo la estabiliad del sistema.
## 3. Arquitecturas PID
### 3.1 PID Paralelo.
En estsa configuracion, las acciones proporcional, integral y derivativa se calculan de manera independiente y luego se suma para obtener la señal de control:

$$ U(s) = K_p * E(s) + K_i \frac{E(s)}{s} + K_d * s * E(s)$$

- Ventajas:
  - Facilita la comprension y la implementacion, especialmente en sistemas digitale.
  - Permite ajustar cada accion de control por separado.
- Desventajas:
  - El ajuste puede ser menos intuitivo, ya que cada parametro afecta solo su componente correspondiente y requiere adaptaciones.
  - Las reglas de sintonizacion clasicas, como Ziegler-Nichols, no se aplicacan directamente y requiere adaptaciones.

### 3.2 PID ideal.
Tambien conocida como forma estandar o no interactiva, en esta arquitectura la ganancia proporcional $K_p$ multiplica la suma de las acciones I y D.

$$ U(s) = K_p ( E(s) + \frac{1}{T_i} \frac{E(s)}{s} + T_d * s * E(s) )$$

- Ventajas:
  - El ajuste de $K_p$ afecta simultaneamnete a las dos acciones, lo que puede simplificar la sintomizacion.
  - Es compatible con muchas reglas de sintomizacion estandar.
- Desventajas:
  - Menor idependecia entre las acciones de control, lo que puede limitar la flexibilidad en cierto casos.   
   
## 3.3 PID serie

En la forma serie, tambien conocida como interactiva, las acciones de control estan interrelacionadas y la ganancia proporcional afecta a todasa ellas:

$$ U(s) = ((E(s)(1+ T_d * s)) K_p)(1+\frac{1}{T_i * s })$$

- Ventajas:
  - Refleja el comportamineto de los controladores analogicas y neumatico tradicionales.
  - Puede ofrecer una respuesta mas natural en ciertos sistemas fisicos.
- Desventajas:
  - La interaccion entre las accioines de control puede complicar el ajuste y la prediccion del comportamiento del sistema.

## 4. Sintonizacion por prueba y error

Es un metodo empirico y directo para ajustar los parametros de un controlador PID cuando no se dispone de un modelo matematico preciso del sistema o cuando se busaca una solucion rapida y practica. Este metodo se basa en la observacion y ajuste iterativo de los parametros del controlador para lograr una respuesta desada del sistema.

### 4.1 ¿Como aplicar la sintonizacion por prueba y error?
1. Establecer valores inicales:
   - Comenzar con una ganacia proporcional baja.
   - Configurara el tiempo integral en un valor alto.
   - Establecer el tiempo derivativo en cero.
2. Ajustar la ganancia proporcional:

    - Incrementar gradualmente el $K_p$ hasta que el sistema comience a responder de manera mas rapida.
    - Observar si aparecen oscilaciones; Si son excesivas, reducir $K_p$
3. Ajustar el tiempo integral:

   - Reducir $T_i$ Para eliminar el error en un estado estacionario.
   - Si el sistema comienza a oscilar o se vuelve inestable, aumenta $T_i$ nuevamente.
4. Ajustar el tiemo derivativo.
   - Introducir $T_d$ para mejorar la estabilidad y reducir el sobrepulso.
   - Ajustar $T_d$ cuidadosamente, ya que valores altos pueden amplificar el ruido y causar inestabilidad.
5. Iterar y observar:
   - Repertir los pasos anteriores, realizando pequeños ajustes y observando la respuesta del sistema hasta alcanzar un comportamiento satisfactorio.

- Ventajas:
  -  Simplicidad: No requiere conocimineto matematicos avanzados ni modelos del sistemas.
  -  Aplicabilidad: Util para sitemas donde otro metodos de sintomizacion no son practicos 
## 5. Criterios de desempeño para diseño de controladores PID.
### 5.1 Indices integrales de error.
#### 5.1.1 ISE (Integral of Squered Error)
$$ IAE = \int_0^T [e(t)]^2 dt $$

- Penaliza con fuerza los errores grandes, ya que los eleva al cuadrado.
- Favorece respuestas rapidas, pero puede inducir oscilaciones y poca estabilidad relativa.

#### 5.1.2 IAE (Integral of Absolute Error)
$$ IAE = \int_0^T |e(t)| dt $$

- Penaliza todos los errores por igual.
- Produce respuesta mas amortiguadas con sobre impulso modesto, pero es mas complejo de calcular analiticamente.
#### 5.1.3 ITAE (Integral Time-weighted Absolute Error)
$$ ITAE = \int_0^T t*|e(t)| dt $$

- Aumenta el peso de errores que persisten mas tiempo.
- Genera sistemas con bajo sobreimpulso y buenas caracteristicas de amortiguamiento.

#### 5.1.4 ITSE (Integral Time-weighted Squared Error)
$$ ITSE = \int_0^T t*[e(s)]^2 dt $$

- Penalizaa tanto errores grandes como errores persistentes a lo largo del tiempo.
- Proporciona un quilibrio entre rapidez, amortiguamineto y estabilidad.

### 5.2 Profundizacion
- ISE es ideal para suprimir errores grandes pero suele derivar en oscilaciones, debido a la rapidez en la correcion.
- IAE genera una respuesta mas baleanceada, evitando sobresaturacion del actuador.
- ITAE actua errores a largo plazo, resultando en un control mas conservador y con excelente amortiguamiento.
- ITSE combina lo mejor de ISE y ITAE, penalizando errores grandes y tardios, aunque puede complocar la sincronizacion.
## 6. Metodos de sintonizacion en lazo abierto 
### 6.1 Metodo de Ziegler-Nichols
Es un procedimineto empiricio diseñado para sintonnizar controladores PID cuando la planta no tiene un modelo matematico conocido, solo se necesita una respuesta tipo S cuando se aplica un escalon en laza abierto.
#### 6.1.1 Pasos del procedimiento
1. Configurar el sistema en lazo abieto.
   - Desconectar el control PID o ponerlo en modo manual.
   - Asugurarse de que el sistema este estable y en estado estacionario.
2. Aolicar un escalon en la entrada.
   - Introducir un cambio de entrada y registrar la respuesta del proceso hasta que se estabilice.
3. Observar la curva de reaccion-
   - La respuesta deberia tener forma de S. Encontart el punto de inflexion.
   - Traza una tangente en ese punto: donde se obtendra el tiempo muerto (L) y la constate de tiempo $\tau$.
4. Calcular los parametros PID.
   - Las formular de Ziegler-Nichols para control clasico PID son:

   | Controlador | $$K_p$$ | $$T_i$$ | $$T_d$$ |
   -------------|------------|---------|-----|
   | P | $$\frac{\tau}{K_p*L}$$ | x | x |
   | PI | $$0.9(\frac{\tau}{K_p*L}$$ | $$3.3*L$$ | x |
   | PID | $$1.2(\frac{\tau}{K_p*L})$$ | $$2*L$$ | $$0.5*L$$ |
#### 6.1.2 Ventajas y limitaciones.

Ventajas: 
- Rapido y facil de aplicar sin modelo previo.
- Permite obtner una respuesta amortiguada $\frac{1}{4}$.

  
Limitaciones:
- Solo es aplicable a sistemas estables en lazo abierto con respuesta tipo S.
- Puede generar valores de control agresivos y sobre impulso alto.
- Las aproximaciones pueden subir inestabale en ciertos precesos.

### 6.2 Metodo Cohen-Coon.
Es un planteamineto empirico diseñado para sintomizar controladores PID ( en forma paralela) en plamtas que pueden modelarse como un sistema de primer orden con retardo. Fue desarrollado por Cohen y Coon en 1953 para mejorar el rendimiento de sistemas con retardos mayores.

#### 6.2.1 Pasos para aplicarlo.
1. Realiza un ensayo de escalon en lazo abierto.
   - Poner el controlador en manual, intrucir un cambio de escalon en la entrada y registar la salida hasta estabilizarce.
2. Obten los paramtros:
   - Ganancia del proceso: Cambio en la salida dividido por cambio de la entrada.
   - Retardo: Tiempo hasta el inicio de la respuesta, medidio desde el escalon hasta que la tangente a la curva intercepta con la base.
   - Constante de tiempo: Tiempo que tarda la respuesta en alcanzar el 63% del cambio total, medido desde donde termina el retardo.
#### 6.2.2 Formula para la sintonizacion.
| Tipo de controlador | $$K_p$$ | $$T_i$$ | $$T_d$$ |
|---------------------|----------|--------|---------|
| P | $$\frac{1}{K}* \frac{\tau}{L}* \frac{3*\tau + L}{3*\tau}$$| X | X |
|PI | $$\frac{1}{K}* \frac{\tau}{L}* \frac{10.8*\tau + L}{12*\tau}$$ | $$\frac{30+3(\frac{L}{\tau})}{9+20(\frac{L}{\tau})}*L$$ | X |
| PID | $$\frac{1}{K}* \frac{\tau}{L}* \frac{16*\tau + 3 * L}{12* \tau}$$ | $$2*L$$ | $$0.5*L$$ |

#### 6.2.3 Ventajas y limitaciones.

Ventajas:
- Espesifico para sistemas con retardo significativo, funcionando bien cuando $\frac{L}{\tau}$ este entre aproximadamente 0.6 y 4.5.
-  Busca una respuesta rapida con buen amortiguamiento.
-  Ofrece una mejor base inicial que Ziegler-Nichols para sistemas con retardo mas grande.
Limitaciones:
- Es un metodo offline, require prueba en lazo abierto antes de aplicar el controlador.
- Pueden generar ajustes agresivos; se recomienda reducir la ganancia resultante en 50% para mejorar la estabilidad.
- Solo es valido para procesos de tipo FOPTD; no aplica bien a procesos de orden superior o no lineales.
### 6.3 Metodo por coeficinete de ajustabilidad.

| y | $$K_p$$ | $$T_in$$ | $$T_d$$ |
|-----|-------|-----------|--------|
| 0 a 0.1 | $$\frac{5}{K}$$ | $$\tau$$ | X |
| 0.1 a 0.2 | $$\frac{0.5}{K}$$ | $$\tau$$ | X | 
| 0.2 a 0.5 | $$\frac{0.5(1+0.5 * y)}{K* y}$$ | $$\tau(1+0.5* y)$$ | $$\tau * \frac{0.5* y}{0.5*y+1}$$ |

Donde y = $\frac{L}{\tau}$



