# Utilizando los logs como almacenamiento barato

Crear variables para almacenar datos puede resultar costoso, por lo tanto puede ser conveniente utilizar logs para guardar nuestra información.

## Comparando costos en despliegue de contrato

Iniciamos un nuevo proyecto, nos aseguramos de instalar las librerías necesarias en esta carpeta, y crearemos varios contratos.

```
mkdir storage
cd storage
truffle init

npm install dotenv
npm install @truffle/hdwallet-provider

truffle create contract Storage
truffle create contract Log
truffle create contract Migrations
```

Dentro de los contratos en la carpeta `contracts` colocaremos el siguiente código.

```
/* Storage */

// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

contract Storage {
  address private owner;

  int public score;

  constructor() public {
    owner = msg.sender;
  }

  function reportScore (int _score) public {
    require(msg.sender == owner, "Only the owner is authorized to report events");

    score = _score;
  }
}
```

```
/* Log */

// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

contract Log {
  address private owner;

  constructor() public {
    owner = msg.sender;
  }

  event LogScore(address _sender, int indexed _score);

  function reportScore (int _score) public {
    require(msg.sender == owner, "Only the owner is authorized to report events");

    emit LogScore(msg.sender, _score);
  }
}
```

```
/* Migrations */

// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

contract Migrations {
  address public owner = msg.sender;
  uint public last_completed_migration;

  modifier restricted() {
    require(
      msg.sender == owner,
      "This function is restricted to the contract's owner"
    );
    _;
  }

  function setCompleted(uint completed) public restricted {
    last_completed_migration = completed;
  }
}
```

Adicionalmente debemos crear dos archivos con el siguiente código en la carpeta `migrations`.

```
// 1_initial_migrations.js

const Migrations = artifacts.require("Migrations");

module.exports = function (deployer) {
  deployer.deploy(Migrations);
};
```

```
// 2_deploy_contracts.js

const Storage = artifacts.require("Storage");
const Log = artifacts.require("Log");

module.exports = function(deployer) {
  deployer.deploy(Storage);
  deployer.deploy(Log);
};
```

Para finalizar, colocamos nuestra llave dentro del archivo `.env` y modificamos el archivo `truffle-config.js` como realizamos en ejercicios anteriores. Tras preparar todo esto, estamos listos para realizar la migración.

```
truffle migrate --network goerli
```

!!! question "Compara"
    Desplegar **Storage** tuvo un costo de 213661 unidades de gas. Desplegar **Log** debió costar 224881 unidades de gas.

    De momento parece que **Storage** es más barato, pero esto cambiará cuando empecemos a realizar transacciones.

## Comparando costos en transacciones

Ahora usaremos la terminal de Truffle para realizar una transacción para almacenar información en cada contrato. Iniciaremos con **Storage**.

```
let storage = await Storage.deployed()
let storageResult = await storage.reportScore(12345)
storageResult.receipt.gasUsed
```

Luego almacenaremos información en **Log**. Aunque ahora estamos usando log en lugar de variables, la llamada al contrato se ve igual.

```
let log = await Log.deployed()
let logResult = await log.reportScore(12345)
logResult.receipt.gasUsed
```

!!! question "Compara"
    Almacenar en la variable de **Storage** tuvo un costo de 45881 unidades de gas. Generar un log en **Log** tuvo un costo significativamente más bajo de solo 25384 unidades de gas.

    A pesar que desplegar **Log** fue ligeramente más costoso, sus operaciones siguientes son mucho más baratas.

## Accediendo a la información guardada en un log

Leer una variables es bastante sencillo, como vimos en ejercicios anteriores. Usamos el siguiente código para leer y transformar a número el BigNumber leído.

```
let bn = await storage.score()
bn.toNumber()
```

La lectura de logs se realiza de forma diferente. Primero obtenemos un arreglo que contiene todos los logs con el siguiente código. Si quisieramos obtener solo los más recientes, podemos indicar algún bloque en específico con `fromBlock: 0`.

```
let logs = await log.getPastEvents("LogScore", {fromBlock: 0}) 
```

Luego tenemos varias formas de visualizar el valor que guardamos. Las siguientes dos líneas permiten obtenerlo como hexadecimal o como decimal respectivamente.

```
logs[0].raw.topics[1]
logs[0].args._score.toNumber()
```

Adicionalmente, también podemos obtener información de cuándo se realizó el almacenamiento si usamos este código. Debemos recordar que las transacciones no se realizan de forma inmediata, sino que se ejecutan cuando son aceptadas como parte de un bloque. Aquí obtendremos el timestamp del bloque en formato Unix.

```
let blockNumber = logs[0].blockNumber
let block = await web3.eth.getBlock(blockNumber)
block.timestamp
```

En conclusión, los logs pueden usarse como una forma de almacenamiento más poderosa y más barata que las variables.