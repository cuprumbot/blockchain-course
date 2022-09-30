# Errores por falta de gas

El mayor limitante al momento de desplegar contratos sobre Ethereum suele ser el gas. Si colocamos muy pocas unidades de gas al momento de realizar una transacción, esta fallará y perderemos lo pagado. Si pagamos muy poco por cada unidad de gas, nuestra transaccción podría demorarse demasiado tiempo.

## Gas insuficiente

Realizaremos una variación al ejercicio anterior. Estando en la carpeta de nuestro contrato ya desplegado, abrimos la terminal de Truffle. Comenzamos obteniendo una instancia de nuestro contrato y preparando la transacción.

```
let instance = await Concurso.deployed()
let ammount = web3.utils.toWei('0.01', 'ether')
```

Luego realizaremos una estimación del gas a usar. Podemos notar que el siguiente código es muy similar al usado en el ejercicio anterior, pero ahora agregamos `estimateGas()`. En mi caso obtengo un resultado de **31745**, este puede variar ligeramente dependiendo del contrato y función a llamar.

```
await instance.deposit.estimateGas({from: accounts[0], value: ammount})
```

Ahora intentaremos realizar un depósito, pero especificaremos una cantidad de gas menor a la estimada.

```
let result = await instance.deposit({from: accounts[0], value: ammount, gas: 30000})
```

!!! question "Experimenta"
    ¿Qué resultado obtuvimos? Como la cantidad de gas fue menor a la estimada, mi transacción debió haber fallado. Obtendremos un mensaje de error similar al siguiente (su hash será distinto).

    ```
    StatusError: Transaction: 0x0123456789abcdef exited with an error (status 0) after consuming all gas.
    ```

Volveremos a causar un error, pero ahora con una cantidad de gas todavía más baja.

```
let result = await instance.deposit({from: accounts[0], value: ammount, gas: 5000})
```

!!! question "Experimenta"
    Ahora nuestro mensaje de error fue distinto. La cantidad de gas fue tan baja, que la transacción ni siquiera llego a ser enviada.

    ```
    Uncaught { code: -32000, message: 'intrinsic gas too low' }
    ```

!!! danger "No cambies la cantidad de gas"
    Truffle es capaz de calcular la cantidad correcta de gas a usar. Un error común y grave entre nuevos desarrolladores es querer bajar esta cantidad.

    No cambies la cantidad de gas para evitar este tipo de errores.

## Precio muy bajo por unidad de gas

Ahora reintentaremos la transacción con la cantidad correcta de gas, pero un pago muy bajo por cada unidad. Nuestra transacción tardará mucho tiempo en realizarse, y si bajamos lo suficiente el precio es posible que nunca se ejecute.

Primero, usaremos el siguiente comando para conocer el precio actual del gas. Obtendremos un valor en wei, si removemos nueve cifras tendríamos el valor en gwei. En el testnet Goerli el gas normalmente se encuentra entre 2 y 4 gwei por unidad.

```
web3.eth.getGasPrice()
``` 

Ahora que conocemos el precio por unidad de gas, intentaremos pagar menos.

```
let result = await instance.deposit({from: accounts[0], value: ammount, gasPrice: 1000})
```

!!! question "Experimenta"
    ¿Qué resultado obtuvimos? Como el precio del gas fue demasiado bajo, recibiremos un mensaje como el siguiente.

    ```
    {
        code: -32000,
        message: 'err: max fee per gas less than block base fee: address 0x0123456789abcdef, maxFeePerGas: 1000 baseFee: 2050705937 (supplied gas 15010499)',
        reason: undefined
    }
    ```

Ya que el precio que colocamos fue muy bajo, lo aumentaremos. Revisamos el mensaje de error anterior y usaremos un valor ligeramente mayor al que `baseFee` que nos indica. En mi caso la transacción queda de esta manera.

```
result = await instance.deposit({from: accounts[0], value: ammount, gasPrice: 2100000000})
```

!!! question "Experimenta"
    ¿Qué sucedió esta vez? Como el precio del gas fue bajo, es posible que nuestra transacción haya tardado más de lo usual en ejecutarse.

    En Goerli por su bajo uso este efecto no es tan notable, pero puede llegar a ser algo más grave en mainnet.