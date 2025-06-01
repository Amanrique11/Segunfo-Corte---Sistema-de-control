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

$$U(s)=K_p*E(s)+K_i \frac{E(s)}{s}+k_d*s*E(s)$$
