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
   Se sustituye la accion proporcional por un señal que alterna un valor alto (+d) y uno bajo (-d) 
