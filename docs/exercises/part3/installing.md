# Instalación de herramientas

## Truffle

Truffle es un framework para desarrollo de contratos inteligentes y dapps (descentralized apps). Funciona sobre Node, por lo que nos permite utilizar JavaScript para desplegar los contratos e interactuar con ellos.

Para instalar Truffle ejecutamos el siguiente comando:

```
npm install -g truffle
```

Adicionalmente necesitaremos HD Wallet para acceder a nuestra billetera desde Truffle. Lo instalamos con el siguiente comando:

```
npm install @truffle/hdwallet-provider
```

## Ganache

Ganache nos permite crear fácilmente un blockchain personal de Ethereum, es decir, un ambiente donde podemos realizar pruebas de transacciones y contratos con facilidad.

Para instalar Ganache en Ubuntu seguimos los siguientes pasos:

* [Descargar Ganache](https://trufflesuite.com/ganache/) desde su página oficial.
* Abrir una terminal y dirigirse hacia la carpeta donde descargamos el archivo.
* Usar el comando `chmod` para dar permiso de ejecución al archivo descargado. Cambiar `VERSION` según corresponda al archivo descargado.
```
chmod +x ganache-VERSION-linux-x86_64.AppImage
```
* Luego de darle permiso, podemos ejecutar Ganache haciendo doble click, o desde la terminal. Cambiar `VERSION` según corresponda al archivo descargado.
```
./ganache-2.5.4-linux-x86_64.AppImage
```
* La primera vez que ejecutemos Ganache nos preguntará si deseamos compartir datos para análisis. Podemos desactivar esta opción.
* Ganache está ahora listo para usarse en los próximos ejercicios.

## Infura

Infura es un proveedor Web3, es decir, un servicio que consiste en múltiples nodos de Ethereum que provee a nuestras aplicaciones descentralizadas acceso a información del blockchain. Los proveedores Web3 también son los responsables de permitir el funcionamiento de billeteras como Metamask y exploradores como Etherscan.

Para poder usar Infura, debemos [crear una cuenta](https://infura.io/register). Más adelante crearemos un proyecto en Infura.