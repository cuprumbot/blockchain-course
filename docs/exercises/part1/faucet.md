# Uso de faucet

Normalmente las monedas se adquieren a través de minería y comprándolas con efectivo. Sin embargo, como usaremos la testnet Goerli, podemos obtener monedas de forma totalmente gratuita a través de un **faucet**.

Los faucets nos regalan una pequeña cantidad de monedas, GoerliETH en nuestro caso. La mayoría tienen alguna restricción de tiempo para que un usuario malintencionado no pueda llevarse todas las monedas gratuitas.

!!! warning "Hay pocos faucets disponibles"
    Recientemente algunas otras testnet como Ropsten y Rinkeby dejaron de funcionar, por la tanto la popularidad de Goerli aumentó. Actualmente son pocas las faucets que siguen funcionando debido a que muchas se han quedado sin reservas de ETH.

## Obteniendo mi dirección

Al hacer click sobre el ícono de Metamask se deplegará una ventana. En la parte de arriba de esta aparece por defecto el nombre **Account 1** y abajo el inicio de un número hexadecimal, el cuál es nuestra dirección, en este caso `0x752...`. Para llevar un mejor control podemos hacer click sobre los tres puntos y luego sobre **Account details** para modificar el nombre de esta cuenta, la dirección no puede ser modificada pero más adelante crearemos direcciones adicionales.

<figure markdown>
  ![Wallet](../../img/part1-faucet0.png)
  <figcaption>Datos de mi primera cuenta</figcaption>
</figure>

Si nos colocamos sobre el nombre de la cuenta, aparecerá un mensaje indicando que podemos hacer click para copiar la dirección, esto nos será de gran utilidad en todas las operaciones siguientes.

<figure markdown>
  ![Faucet](../../img/part1-faucet00.png)
  <figcaption>Click para copiar la dirección</figcaption>
</figure>

## Goerli Faucet

[Goerli Faucet](https://goerlifaucet.com/) nos permite pedir hasta 0.25 ETH cada día. Debido a la alta popularidad que Goerli ha tenido recientemente, este faucet requiere que creemos y nos identifiquemos con una cuenta de Alchemy. Al crearla, debemos elegir la opción gratuita.

<figure markdown>
  ![Faucet](../../img/part1-faucet1.png)
  <figcaption>Se requiere una cuenta de Alchemy</figcaption>
</figure>

Una vez te hayas registrado e ingresado podrás colocar la dirección que Metamask te da y pedir tu ETH gratuito.

<figure markdown>
  ![Faucet](../../img/part1-faucet2.png)
  <figcaption>Después de ingresar ya podemos pedir</figcaption>
</figure>

## Goerli PoW Faucet

[Goerli PoW Faucet](https://goerli-faucet.pk910.de/) es un faucet que requiere que realicemos minería a cambio de entregarnos ETH.

Debemos colocar nuestra dirección para recibir las monedas y luego hacer click en el botón de Start Mining.

<figure markdown>
  ![Faucet](../../img/part1-faucet3.png)
  <figcaption>Previo a iniciar minería</figcaption>
</figure>

Al hacerlo nuestro navegador web empezará a calcular hashes, es decir, a realizar minería. Debemos esperar algunos minutos mientras se realiza el trabajo suficiente para obtener el pago mínimo.

<figure markdown>
  ![Faucet](../../img/part1-faucet4.png)
  <figcaption>Se requiere un mínimo de 0.05 ETH para poder retirar</figcaption>
</figure>

Después de la espera, el botón de abajo cambiará y nos indicará que ya podemos cobrar lo obtenido.

<figure markdown>
  ![Faucet](../../img/part1-faucet5.png)
  <figcaption>Al alcanzar el mínimo, ya podremos retirar</figcaption>
</figure>

Evita este faucet si estás trabajando en una laptop únicamente con batería, o si tu computadora no tiene muy buen enfriamiento. Realizar minería hace que la computadora realice bastante trabajo.