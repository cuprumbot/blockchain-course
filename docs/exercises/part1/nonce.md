# Encontrando un nonce

Cuando realizamos un cambio a un archivo o string, por más pequeño que sea, el hash resultante cambiará por completo.

Al realizar minería se busca obtener un hash en específico, que comience con una cantidad de ceros dada. Para lograr esto, los mineros prueban una gran cantidad de **nonces** distintos hasta hallar el hash deseado.

## Buscando un cero en el hash

!!! example "Ejercicio"
    Escribe la función `findOneZero(str)`. `str` es una cadena cualquiera a la cuál dentro de la función se le agregará un nonce y se le calculará su hash sha256. 
    
    La función probará nonces distintos hasta que encuentre un hash que comienza con cero, al hacerlo desplegará la cantidad de intentos realizados y el hash obtenido.

!!! tip "Sugerencias"
    El nonce puede ser cualquier valor e incluso podría elegirse al azar, sin embargo puede ser más eficiente solo usar un número correlativo ya que haremos muchos intentos.

!!! question "Experimenta"
    Aplica la función a distintas cadenas.

    ¿Cuál es el promedio de intentos antes de encontrar el hash que buscamos?

## Ajustando la cantidad de ceros

En la mayoría de blockchains es posible ajustar la dificultad según la cantidad de mineros, esto quiere decir cambiar la cantidad de ceros que buscamos al inicio del hash.

!!! example "Ejercicio"
    Basándote en el ejercicio anterior, escribe la función `findManyZeroes(str, ammount)`. `str` es una cadena cualquiera a la cuál le agregaremos un nonce y calcularemos su hash sha256. 
    
    El objetivo es encontrar al inicio del hash la cantidad de ceros que `ammount` nos indica. Al encontrar la cantidad de ceros deseada, nuestra función debe desplegar la cantidad de intentos realizados y el hash obtenido.

!!! question "Experimenta"
    Aplica la función a la misma cadena, pero cada vez busca una mayor cantidad de ceros. Repite el proceso para varias cadenas, cada una con distintas cantidades de ceros.
    
    ¿Que tanto crece la cantidad de intentos con cada cero adicional que buscamos?