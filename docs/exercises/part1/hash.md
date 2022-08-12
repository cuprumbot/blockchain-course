# Cálculo de un hash

La operación más comunmente realizada en blockchain es calcular un **hash**. Esta se usa al relacionar un bloque con otro, previo a firmar una transacción, al realizar minería, entre otros momentos.

Para realizar el cálculo usaremos la libreria `crypto` de Node. Si no la tienes instalada, utiliza el siguiente comando para instalarla en tu carpeta actual.

```
npm install crypto
```

## Nuestro primer hash

Para nuestro primer hash utilizaremos el siguiente código.

```
const crypto = require('crypto');

const content = "Mi string a hashear";
const stringBuffer = Buffer.from(content);

const hashSum = crypto.createHash('sha256').update(stringBuffer);
const hex = hashSum.digest('hex');
console.log(hex);
```

La función `createHash()` requiere un tipo de hash, usaremos **sha256** el cuál es usado en la mayoría de blockchains. Luego utilizamos `update()` para indicar a quién le aplicaremos la función hash, el argumento que enviemos debe ser un **buffer**. Finalmente `digest()` nos permite obtener el hash en el formato deseado, en este caso como hexadecimal.

## Aplicando la función hash a un archivo

!!! example "Ejercicio"

    Modifica el ejemplo anterior para aplicar la función hash a un archivo.

!!! tip "Sugerencias"
    La librería `fs` nos permite acceder a archivos.
    ```
    const fs = require('fs');
    ```

    La función `readFileSync()` es la forma más sencilla de leer un archivo y obtener un buffer.
    ```
    const fileBuffer = fs.readFileSync(filename);
    ```

!!! question "Experimenta"
    Si aplicas la función varias veces al mismo archivo, siempre obtendrás el mismo resultado. Compruébalo.

!!! question "Experimenta"
    Aplica la función hash a un archivo y anota el resultado. Haz una pequeña modificación al archivo (cambia un caracter o un pixel por ejemplo) y vuelve a aplicar el hash. 
    
    ¿Qué le sucede a nuestro hash al hacer un pequeño cambio en el archivo?



## Agregando un nonce al archivo leído

Un **nonce** es un valor cualquiera que se concatena a un mensaje, transacción, etc. con el objetivo de obtener un hash distinto.

!!! example "Ejercicio"
    Modifica el código anterior para convertirlo en la función `hashWithNonce(filename, nonce)`. `filename` es un string que representa el nombre del archivo y `nonce` es un número entero cualquiera.

!!! tip "Sugerencias"
    La función `Buffer.from()` crea un buffer a partir de algún string dado.
    ```
    const stringBuffer = Buffer.from(someString);
    ```

    La función `Buffer.concat()` recibe un **arreglo** que contiene varios **buffers** y los concatena.
    ```
    const newBuffer = Buffer.concat([fileBuffer, stringBuffer]);
    ```