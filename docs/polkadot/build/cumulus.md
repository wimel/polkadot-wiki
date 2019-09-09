# Cumulus

> Cumulus clouds tienen forma de lunares y están en el aire, como este proyecto (ya que es un prototipo inicial - esperar un cambio de nombre cuando se enfríe).

[Cumulus](https://github.com/paritytech/cumulus) es una extensión de Substrate que facilita la integración de cualquier tiempo de ejecución de Substrate en una parachain compatible con Polkadot.

## Componentes

### Consenso Cumulus

*cumulus-consensus* es un motor de consenso para Substrate que sigue una relay chain de Polkadot. Esto ejecutará un nodo Polkadot internamente y dictará al cliente y a los algoritmos de sincronización qué cadena seguir, finalizar y tratar de la mejor manera.

### Cumulus Runtime

Una envoltura alrededor de los tiempos de ejecución de Substrate para permitir que sean validados por los validadores de Polkadot y proporcionar rutinas de generación de testigos. Añade una API `validate_block` a la interfaz externa Substrate que será llamada por los validadores.

Integrarlo en el tiempo de ejecución de su substrate será tan fácil como importar la caja y añadir esta macro de una línea a su código.

``` rust
runtime::register_validate_block!(Block, BlockExecutor);
```

### Cumulus Collator

Una emparejadora Polkadot planeada para una parachain.

## Recursos 

- [La charla de Rob de EthCC presentando Cumulus](https://www.youtube.com/watch?v=thgtXq5YMOo)
