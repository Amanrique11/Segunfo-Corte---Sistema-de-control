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
  | PI | $$0.45*K_u$$ | $$\frac{P_u}{1.2} | X |
  | PID | $$0.6*K_u$$ 
