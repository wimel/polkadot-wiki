# Cómo ver y desplegar parachains

La guía ha sido actualizada para funcionar con la red de pruebas de Alexander.

## Cómo ver las parachains

En la [interfaz de Polkadot](https://polkadot.js.org/apps/#/explorer), vaya a la pestaña `Chain State`. Selecciona el módulo `parachains` y `parachains()` y pulsa el botón `+`. Devolverá una lista de las parachains actualmente activas.

## Cómo desplegar parachain de Adder

**Necesitará tener el depósito mínimo necesario para crear un referéndum. Actualmente este mínimo es de 5 DOTs.**

El parachain `adder` es un parachain simple que mantendrá un valor en el almacenamiento y añadirá a este valor a medida que se le envíen mensajes. Puede encontrarse en el repositorio de Polkadot bajo la carpeta `test-parachains`.

> Una versión de video ligeramente desactualizada de esta guía presentada por Adrian Brink está disponible [aquí](https://www.youtube.com/watch?v=pDqkzvA4C0E). Cuando las dos guías difieran, por favor refiérase a este texto escrito como definitivo y actualizado.

### Desarrollando el código

El primer paso es descargar localmente el código de Polkadot.

```bash
git clone https://github.com/paritytech/polkadot.git
```

Ahora asegúrese de tener Rust instalado.

```bash
curl https://sh.rustup.rs -sSf | sh
sudo apt install make clang pkg-config libssl-dev
rustup update
```

Ahora navegue hasta la carpeta `test-parachains` en el repositorio de código de Polkadot y ejecute el script de compilación.

```bash
cd polkadot/test-parachains
./build.sh
```

Esto creará el ejecutable Wasm `adder` simple de parachain contenido en esta carpeta. Esta parachain simplemente agregará los mensajes que se le envíen. El ejecutable Wasm se generará en la ruta `parachains/test/res/adder.wasm`, así que asegúrese de que puede encontrarlo allí.

Necesitará construir y ejecutar el nodo collator para obtener el estado de génesis de esta parachain.

Navegue hasta el directorio `test-parachains/adder/collator` y ejecute los comandos `build` y `run`.

```bash
cargo build
cargo run
[ctrl-c]
```

Siéntase libre de detener el nodo collator de inmediato. Obtendrá una salida que se parece a ésta:

```
Starting adder collator with genesis:
Dec: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 27, 77, 3, 221, 140, 1, 241, 4, 145, 67,
207, 156, 76, 129, 126, 75, 22, 127, 29, 27, 131, 229, 198, 240, 241, 13, 137, 186, 30, 123, 206]
Hex: 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000011b4d03dd8c01f1049143cf9c4c817e4b167f1d1b83e5c6f0f10d89ba1e7bce
```

La información importante es la cadena hexadecimal. Este es su estado de génesis y necesitará guardarlo para los próximos pasos.

### Desplegando la parachain

Vaya a la [interfaz de usuario de Polkadot]((https://polkadot.js.org/apps/#/extrinsics)) en la pestaña `Extrinsics`. Seleccione la cuenta desde la que desea desplegar la parachain. Necesitará crear un referéndum para desplegar la parachain.

Haga click en `democracy` -> `propose(proposal,value)` -> `parachains` -> `registerParachain(id,code,initial_head_data)`.

En la entrada `id`, introduzca el id de la parachain. En el caso adder simple será de `100`. En el campo `code` haga clic en el botón de la página y luego suba el binario `adder.wasm` que fue compilado antes. En los datos `initial_head_data` copiaremos y pegaremos los datos hexadecimales que obtuvimos al ejecutar el nodo collator. En el campo `value` deberá introducir el valor mínimo requerido para crear un referéndum. En el momento de redactar el presente informe, se trata de 5 DOTs en la red de pruebas de Alexander.

![registering a parachain](../../img/parachain/register.png)

Si navega a la pestaña `Democracy` podrá ver su propuesta en la sección de propuestas.

Una vez que esperes a que la propuesta se convierta en un referéndum, podrás votar `Nay` o `Aye`. Asumiblemente, usted votará a favor ya que esto será un voto para el despliegue de su parachain.

![parachain referendum](../../img/parachain/referendum.png)

Después de que el período de votación de su referéndum pase, usted podrá consultar el estado de su parachain.

Puedes ir a la pestaña `Chain State` y consultando el estado de las `parachains` deberías poder ver alguna información sobre su parachain.

![parachain info](../../img/parachain/info.png)

### Interactuando con la parachain

_Coming soon_
Wasm
