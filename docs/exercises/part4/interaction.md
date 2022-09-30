# Interactuando con contratos fuera de la terminal

Hasta este punto hemos realizado las llamadas a contratos desde nuestra terminal, pero también podemos realizarlas desde algún programa independiente, siempre gracias a Truffle.

## Creando un programa que interactue con nuestro contrato

En la raíz de nuestra carpeta desde la cual desplegamos nuestros contratos crearemos un nuevo archivo de Javascript.

```
/* interaction.js */

// Obtener instancia del contrato desplegado
// Necesitamos una línea adicional respecto a lo que hacíamos en la terminal
const contract = artifacts.require("Log");

// Truffle necesita que nuestras funciones vayan dentro de este module.exports
module.exports = function() {

    // Leemos todos los logs del contrato
    async function readLogs() {
        let instance = await contract.deployed();
        
        instance.getPastEvents(
            "LogScore", 
            {fromBlock: 0}
        ).then(
            async (logs) => {       // función asíncrona para poder usar await dentro

                for (const elem of logs) {      // usamos for en lugar de forEach para poder usar await dentro
                    console.log("Score:     " + elem.args._score.toNumber());
        
                    let blockNumber = elem.blockNumber;
                    let block = await web3.eth.getBlock(blockNumber);       // podemos usar la librería web3
        
                    console.log("Timestamp: " + block.timestamp);

                    let date = new Date(block.timestamp * 1000)                   // milisegundos a segundos

                    console.log("Date:      " + date);
                }
            }
        );
    }

    // Podemos definir todas las funciones que querramos en esta parte

    // Llamamos a alguna de nuestras funciones
    readLogs();
}
```

Para ejecutar este archivo usamos el siguiente comando desde la terminal normal (es decir, no desde la terminal de Truffle).

```
truffle exec --network goerli interaction.js
```

De esta manera podemos crear programas mucho más complejos que aprovechen la interacción con nuestros contratos.