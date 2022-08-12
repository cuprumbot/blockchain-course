# Causando una colisión

Cuando aplicamos una función hash a dos datos distintos y obtenemos el mismo hash, decimos que ha ocurrido una **colisión**.

Queremos que el algoritmo de hash que usamos sea resistente a colisiones, es decir, que sea casi imposible que estas ocurran.

## Algunas pequeñas colisiones

!!! example "Ejercicio"
    Escribe la función `miniCollision(str1, str2, ammount)` que recibirá dos cadenas cualquiera y una cantidad de ceros. 
    
    Para la cadena `str1` calcula su hash y despliegalo. El objetivo es encontrar un hash para `str2` cuyos primeros dígitos sean iguales a los del primer hash que calculamos, `ammount` nos indica cuántos dígitos iguales queremos.

    Para lograrlo, se deberán agregar distintos nonces a `str2` hasta encontrar uno que nos genere el hash deseado. Al encontrarlo despliega el hash y la cantidad de intentos que se necesitaron.

!!! question "Experimenta"
    Aplica la función a la misma pareja de cadenas, pero con distintas cantidades de dígitos.

    ¿Cuánto aumenta la cantidad de intentos necesarios por cada dígito adicional que queremos que coincida? 
    
    Si este patrón se mantiene, ¿cuántos intentos se necesitarían para que todos los digitos coincidan?
