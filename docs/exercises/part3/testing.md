# Realizando pruebas con Truffle

Truffle nos permite hacer pruebas de forma sencilla a los contratos que hemos creado. Continuaremos trabajando con el contrato **Concurso** del ejercicio pasado.

## Creando un archivo de pruebas

Cuando inicializamos nuestro proyecto se creó una carpeta llamada **test**, allí adentro crearemos una archivo nuevo llamado `concurso_test.js` y escribiremos el siguiente código dentro de él.

```
// Para acceder a los métodos de nuestro contrato necesitamos su ABI
// ABI significa Application Binary Interfaces y es un archivo json
// que contiene información de nuestros campos y métodos
// Los ABI se encuentran en la carpeta 'build' de nuestro proyecto
const Concurso = artifacts.require("Concurso");

contract ("Concurso", (accounts) => {

    // Aqui podemos colocar múltiples pruebas
    // Cada una debería llevar un mensaje descriptivo sobre su funcionamiento
	it("owner should be the 0th account", async () => {

        // Obtenemos una referencia al contrato desplegado
		const instance = await Concurso.deployed();

        // El método owner se creo automáticamente al crear 
        // el campo con ese nombre
        // Nos permite obtener el valor actual
		const owner = await instance.owner();

        // Los tests se basan en comparaciones de igualdad
        // Se incluye un mensaje descriptivo en caso de que la prueba falle
		assert.equal(
            owner, 
            accounts[0],
            "Owner of contract is not our 0th account"
        );
	});

});
```

!!! abstract "Significado de la prueba anterior"
    Al desplegar nuestro contrato no indicamos que cuenta usar, entonces por defecto se usó la primera cuenta que Ganache nos provee (a la cuál corresponde el índice cero `accounts[0]`).

    En nuestra prueba queremos corroborar esto, comparamos `owner` con `accounts[0]` y esperamos que sean iguales

Una vez hayamos escrito nuestras pruebas, podemos ir a la terminal y usar el siguiente comando para ejecutarlas.

```
truffle test
```

Truffle ejecutará estas pruebas de forma interna y al final nos mostrará si estas fueron exitosas.

IMAGEN

## Realizando más pruebas

A continuación encontrarás algunas ideas de pruebas a realizar. Agregalas en tu archivo `concurso_test.js`.

Comenzamos probando los depósitos, que pueden ser realizados por cualquier usuario.

```
await instance.deposit({from : accounts[1], value: 1000});
```

Esta línea indica que llamaremos al método `deposit()` con la dirección `accounts[1]`, es decir, una dirección que no es owner. Finalmente `value: 1000` indica que depositaremos 1000 wei.

!!! question "Experimenta"
    Pruebas para los depósitos.

    ```
    it("should allow a deposit from anyone", async () => {
        const instance = await Concurso.deployed();
        await instance.deposit({from : accounts[1], value: 1000});
        const deposited = await instance.deposited();

        assert.equal(
            deposited, 
            1000,
            "Must allow any user to deposit"
        );
    });

    it("should correctly track the deposited ammount after multiple deposits", async () => {
        const instance = await Concurso.deployed();
        await instance.deposit({from : accounts[2], value: 1000});
        const deposited = await instance.deposited();

        assert.equal(
            deposited, 
            2000,
            "Two 1000 wei deposits have been made, the total ammount must be 2000 wei"
        );
    });
    ```

Luego probaremos los retiros. Compara el siguiente código con el de los depósitos.

```
await instance.withdraw(500, {from : accounts[0]});
``` 

La función `withdraw()` recibe un argumento que corresponde a la cantidad a retirar. Ya no colocamos ningún `value` pues esto es exclusivo para depositar en un contrato.

!!! question "Experimenta"
    Pruebas para los retiros.
    ```
    // Usar después de haber depositado 2000 wei
    it("should allow the owner to withdraw", async () => {
		const instance = await Concurso.deployed();
		await instance.withdraw(500, {from : accounts[0]});
		const deposited = await instance.deposited();

		assert.equal(
            deposited, 
            1500,
            "The contract owner must be allowed to withdraw funds"
        );
	});

	it("should disallow everyone else to withdraw", async () => {
		const instance = await Concurso.deployed();
		await instance.withdraw(500, {from : accounts[1]})
			.then(assert.fail)
			.catch(function (error) {
				// console.log(error.message);
			});
		const deposited = await instance.deposited();

		assert.equal(
            deposited, 
            1500,
            "Only the contract owner should be able to withdraw funds"
        );
	});
    ``` 