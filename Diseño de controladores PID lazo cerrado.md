# Diseño de controladores PID lazo cerrado
## 1. Metodo de ziegler & Nichols
### 1.1 Metodo paso a paso.
1. Preparacion del controlador:
   - Inicialemnete, P = Varieable ajustable, mientras que I y D estan en cero (o muy bajos), para que solo actue la parte proporciona.
2. Incremento de la ganancia P:
   - Se aumenta la ganancia proporcional lentamente, observando la salida del sistema en lazo cerrado hasta que se produzcan oscilaciones sostenodas de amplitudes constantes, es decir, sin amortiguadores ni creciminetos.
3. Indentificacion de parametros criticos:
   - El valor de ganancia proporcional en ese punto se llama ganancia ultima ($k_u$ o $K_{cr}$.
   - Tambien se mide el periodo de oscilaciones constantes, llamado $P_u$ o $T_{cr}$.
4. Calculo de los parametros PID:
   - A partir de $K_u$ y $P_u$, se usa las siguientes formulas:
     
  | Controlador | $$K_p$$ | $$T_i$$ | $$T_d$$ |
  |-------------|-------|---------|---------|
  | P | $$0.5*K_u$$ | X | X |
  | PI | $$0.45*K_u$$ | $$\frac{P_u}{1.2}$$ | X |
  | PID | $$0.6*K_u$$  | $$\frac{P_u}{2}$$ | $$\frac{P_u}{8}$$ |
  
5. Implemantacion y afinacion fino:
   - La sintonizacion resultante suele generar una relacion de decaimineto del 25%.
   - Sin embargo, puede ser nesecaria un ajuste adicional manual para bajar el sobreimpulso, mejorar la respuesta o adaptarse a restricciones del sistema.

### 1.2 Ventajas y Desventajas.

- Ventajas:
   - Sencillo de aplicar: Solo se varia la ganancia proporcional.
   - Se captura la dinamica real del lazo completo, incluyendo la planta y sensores.

- Desventajas:
  - Se lleva el sistema al borde de la inestabilidad, lo que puede ser arriesgado en ciertos procesos.
  - Pueden causar oscilaciones excivas o salir del rango seguro para sistemas delicados.

## 2. Metodo de rele.

El metodo de rele es otro tecnica clasica de sintonizacion en lazo cerrado para PID, mas segura y autamatizable que es el metodo de Ziegler & Nichols.

### 2.1 ¿En que consiste?

1. Reemplazar el controlador PID (o al menos P) por un rele:
   Se sustituye la accion proporcional por un señal que alterna un valor alto (+d) y uno bajo (-d), segun el error sea positivo o negativo.
2. Generacion de oscilaciones sostenidas:
   El sistema en lazo cerrado entra en un ciclo limite oscilante con amplitud y periodo constante.
3. Medicion de parametros criticos:
   - Amplitud de la oscilacion del proceso(a).
   - Amplud del cambio de señal del rele (2-d).
   - Periodo de oscilacion ($P_u$ o $T_u$). Estos valores permiten estimar la ganancia critica $K_u$ mediante:
  
     $$K_u =\frac{4*d}{\pi * a}$$

### 2.2 Calculo de parametros.

| Controlador | $$K_p$$ | $$T_i$$ | 
|-------------|---------|---------|
| PI | $$\frac{5 * K_c}{6}(\frac{12+K * K_u}{15+14K * KI_c})$$ | $$\frac{P_u}{5}(1+\frac{4}{15} * K * K_c)$$ |

### 2.3 Ventajas y consideraciones.


  - Menor rango que zieler-Nichols puro: El rele produce oscilaciones controladas, evitando llevar al sistema justo al borde de inestabilidad.
  - Automatizable y preciso: Permite obtner rapidamente $K_u$ y $P_u$ sin intervecion manual intenciva.
  - Versatil: V
  - Variantes como rele con histeresis mejoran la robustez frente al ruido y pertubaciones.

## 3. Fenomeno wind-up.

Tambien es conocido como integral wind-up o reset wind-up, ocurre cuando la accion integral del controlador PID acumul demaciado error durante periodos en que el actuador esta saturado y sno puede ejecutar la salida requerida.
Esto puede provocar varias consecuencias negativas.

### 3.1 ¿Que es y por que sucede?.

- Cuando el actuador llega a su limite, ( valvula totalmente abierta o cerrada), pero sigue existiendo error, la parte integral PID sigue sumando ese errors sin que haya un efecto real en la salida.
- Esta acumulacion excesiva hace que, una vez que el error comience a corregirse, la integral "sobrecarga" empuje al sistema mucho mas alla del punto deseaado, provocando sobreimpulsos, oscilaciones o respuestas lentas.

![download](https://github.com/user-attachments/assets/eb40cf90-6085-48c5-84ec-bd6e8d49c6cb)

### 3.2 Sintomas comunes.
- Overshoot excesivo tras cambios en el setpoint o pertubaciones.
- Respuestas lentas y perezaosa, incluso despues de que el sistema deberia haber convergido, debido a que la iintegral necesita descargarse.
- En casos graves, puede generar oscilaciones prolongadas o inestables.

### 3.3 Estrategias anti-wind-up.

1. Clamping (limitacion de integrador(.
   - Ss fija un tope (maximo y minimo) para el valor del integrador, cuando el controlador se satura, la integral se "congela".
2. Integracion condicionl.
   - Solo acomula la integral si el error esta dentro de un rango controlable o el actuador no esta saturado.
3. Back-Calculation ( retro-calculo).
   - Se corrige la suma integral basandose en la diferencia entre la salida deseada y la salida real del actuador, desacelarando o invirtiendo la acumalacion.
4. Reset o freeze al modo manual o durante cambios.
   - Cuando el controlador cambia de modo en arranques, se reinica o detiene la integral para aevitar acumulaciones previas.
5. Limitaciones en la rampa de setpoint
   - Al limtar la velocidad del cambio en el setpoint, se evita que se acumule un gran error repentinamnete.

## 4. Anti wind-up por saturacion integral.

ES un a estrategia que detecta cuando el integrador se esta saturando y detiene o modula su accion para evitar su sobreacumulacion.

En palabras simples: 
- Se monitorea la salida del PID y el nivel de saturacion del actuador.
- Si la salida integral a P o D excederia el limite fisico, se congela o limita el acumulador integral.
- Al volver al rango operacion, simplemente se reactiva la integracion desde ese punto, evitando que la integral se haya sobrecargado.

![images](https://github.com/user-attachments/assets/5c318f09-7598-43aa-8d93-d41ad6eaf8c9)

### 4.1 Beneficios y cuando usarlo.
- Evita transitorios larhgos y sobreimpulsos: El sistema no " se queda dormido " Corrigiendo aculuaciones pasadas.
- Mejora la respuesta post-saturacion: Al reactivar la integral justo cuando es efectiva, se mejora el control.
- Ideal en sistemas coon limites fisicos definidos ( Valvulas, motores, bombas).


