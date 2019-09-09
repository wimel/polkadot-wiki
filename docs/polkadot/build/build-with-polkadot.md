# Cómo empezar a desarrollar en Polkadot

!!! info
    _Polkadot y Substrate están en desarrollo activo y las cosas están cambiando rápidamente. Debería calmarse en un par de semanas con la [versión 1.0](https://github.com/paritytech/substrate/milestone/9) de Substrate, pero tenga en cuenta que las APIs aún están evolucionando._

Polkadot se describe en el [whitepaper](https://github.com/w3f/polkadot-white-paper/raw/master/PolkaDotPaper.pdf) como un banco de pruebas de desarrollo, despliegue e interacción de cadenas de bloques totalmente extensible y escalable. Los desarrolladores tendrán la capacidad de utilizar esta nueva e interesante tecnología como infraestructura para sus propias aplicaciones.

Esta guía le guiará a través del [estado actual](#where-are-we-now) del desarrollo de Polkadot, le explicará la [diferencia entre una parachain y un contrato inteligente](#what-is-the-difference-between-a-parachain-and-smart-contract), le explicará los pasos necesarios para [construir una parachain](#so-you-want-to-build-a-parachain) desde el [inicio de un nuevo proyecto](#starting-a-new-project) hasta la [inclusión de su cadena en Polkadot](#how-to-include-your-parachain-in-polkadot); o para [desarrollar un contrato inteligente](#so-you-want-to-build-a-smart-contract) desde la elección de una plataforma hasta el [pago de la implementación](#paying-for-your-smart-contract).

## ¿Dónde estamos ahora?

Polkadot ha alcanzado el hito PoC 3 y ha estado funcionando con éxito la red de pruebas Alexander durante tres meses. Se está avanzando constantemente hacia la próxima versión de PoC-4, que incluirá la mensajería entre cadenas. El desarrollo está en marcha para el lanzamiento de la Mainnet a finales de año.

Mientras tanto, Substrate, que es la biblioteca de módulos en tiempo de ejecución sobre la que se está construyendo Polkadot, se está acercando a su primera versión estable. Substrate permite a los desarrolladores reutilizar muchas de las partes que se están utilizando para crear Polkadot y componerlas en su propia cadena. También es fácilmente extensible escribiendo módulos de tiempo de ejecución personalizados que dictan la lógica de tiempo de ejecución de la cadena.

Si usted es un desarrollador, constructor, empresario, o un visionario que considera a Polkadot como el ecosistema para su próximo proyecto, esta guía le ayudará a entender sus opciones de desarrollo y a decidir qué stack es el adecuado para usted y para su proyecto. Le mostrará los recursos disponibles que puede utilizar para comenzar a planificar y ejecutar su idea hoy mismo.

## ¿Cuál es la diferencia entre una parachain y un contrato inteligente?

Antes de que pueda empezar a trabajar en su idea, es importante que entienda las ventajas y desventajas entre las dos opciones de desarrollo que tiene para construir sobre Polkadot: parachains o contratos inteligentes.

Las parachains son las cadenas individuales que se conectarán a la red Polkadot para beneficiarse de la seguridad compartida del conjunto de validadores de la relay chain de Polkadot. Polkadot se está construyendo con las mismas herramientas disponibles para empezar a crear parachains. Las parachains permiten flexibilidad y menos restricciones sobre lo que es posible en la lógica de la cadena, pero vienen con una sobrecarga adicional que hace que no siempre sean la opción preferible cuando se comparan con los contratos inteligentes.

Los contratos inteligentes son programas ejecutables que existen en una cadena específica. Ellos a menudo tienen una buena interoperabilidad compatible con otras aplicaciones de la plataforma en la misma cadena, pero no puede realmente interactuar con algo en otra cadena. Ellos vienen con menos gastos de desarrollo que la construcción de una parachain completa, pero implican más restricciones sobre lo que permite la lógica de la aplicación. Son buenos medios con los que probar el prototipado inicial, pero que con el tiempo pueden superarse a sí mismos debido a restricciones de escalabilidad o sobrecarga de complejidad.

En términos generales, si quieres tener control sobre la mecánica de protocolo de tu red, entonces una parachain será la mejor opción para ti.

Construir una parachain significa que se puede diseñar la economía del sistema monetario desde cero. Puede implementar una estructura de tarifas personalizada en sus transacciones o un sistema de tesorería completo que podría actuar como una DAO nativa y utilizar porciones de la recompensa en bloque para permitir a las partes interesadas asignar fondos.

Esto abre la flexibilidad para construir una lógica compleja que puede ser demasiado costosa para ser ejecutada como un contrato inteligente. Si hay una característica específica que su cadena utilizará como piedra angular, puede incluso implementar un tiempo de ejecución optimizado para que sea más barato y rápido.

Si hay un caso de uso específico en el que está pensando, como la implementación de un tipo completamente nuevo de máquina virtual, podrá hacerlo construyendo una parachain.

En cambio, si desea una barrera de entrada más baja y ciclos de iteración más rápidos para la creación rápida de prototipos y la implementación, se sentirá más atraído por el medio de contrato inteligente.

Los contratos inteligentes suelen ser más sencillos que la lógica de la parachain y por lo tanto tardan menos tiempo en desarrollarse. Tendrán herramientas fuertes como los IDEs que le permiten sacar su idea al mundo mucho más rápido.

Como se ha mencionado anteriormente, tendrán acceso al entorno de la cadena para la que están desplegados. Si su aplicación requiere interacción con un sistema que ya está desplegado en una cadena como un contrato inteligente, en la mayoría de los casos tendrá más sentido escribir su aplicación como un contrato inteligente en la misma cadena.

Un ejemplo de la composición de los contratos inteligentes es evidente en el reciente fenómeno de las aplicaciones DeFi (finanzas descentralizadas). Proyectos como [Maker](https://makerdao.com/) abrieron el camino para que se construyeran nuevas aplicaciones sobre ellos, como los fondos de cobertura descentralizados y los sistemas de gestión de préstamos.

Habrá más gastos generales para crear una parachain que simplemente no existirá para desplegar su aplicación como contratos inteligentes. Esto incluye ejecutar nodos collators para su parachain, o incentivarlos de alguna manera, y vincular DOTs o convencer al mecanismo de gobierno de Polkadot para que incluya su cadena en el registro de la parachain.

A menos que esté absolutamente seguro de que su proyecto necesita ser una parachain o si sólo está deseando experimentar con nuevas tecnologías, es aconsejable que empiece por crear primero un MVP de su aplicación descentralizada como un contrato inteligente.

Aquí hay una tabla de comparación rápida para ayudarle a comprender la información:

| Parachain | Smart Contract |
|-----------|----------------|
| más complejo para desarrollar (-) | fácil para desarrollar (+) |
| requiere un nodo collator para el desarrollo (-) | fácil para desarrollar (+)  |
| intereacción entre parachains (+) | interacción entre aplicaciones (+) |
| requiere mantenimiento de la red (-) | fácil de mantener (+) |
| posible implementar lógica de red compleja y criptografía avanzada (+) | más difícil de implementar lógica compleja o criptografía (-) |
| es su propia red (+) | existe en una red (-) |

Esta guía ahora se divide en dos dependiendo de la decisión que usted haya tomado de desarrollar su proyecto como una parachain o como un contrato inteligente.

- [Quiero desarrollar una parachain](#so-you-want-to-build-a-parachain)
- [Quiero desarrollar un contrato inteligente](#so-you-want-to-build-a-smart-contract)

## Entonces, quieres construir una parachain

Una vez que haya determinado que la creación de una parachain es el enfoque correcto para su proyecto, el siguiente paso es decidir qué herramientas y marco de trabajo utilizará. Ahora mismo la elección es fácil, ya que sólo tienes una opción: Substrate de Parity.

> En realidad, no es estrictamente cierto que Substrate sea su única opción, ya que puede escribir toda la lógica de la cadena desde cero siempre y cuando esté compilada en Wasm y tenga la interfaz de interacción correcta para Polkadot. Pero este enfoque no se beneficiará de la reutilización modular y la facilidad de desarrollo que permiten las estructuras como Substrate.

### Substrate

Substrate es un framework para los innovadores de la cadena de bloques. Proporciona los bloques de construcción básicos que se necesitan para construir una cadena y proporciona una biblioteca fácil de conectar y modular de módulos de tiempo de ejecución a partir de los cuales se puede componer la lógica de la cadena. La motivación de construirlo fue reducir el tiempo de desarrollo de una nueva cadena de bloques de años a semanas y días o incluso horas para cadenas simples.

El recurso más definitivo con respecto al desarrollo de Substrate es el [Centro de Desarrollo de Substrate](https://docs.substrate.dev/) mantenido por Parity, que cubre el material desde el comienzo de su primera parachain desde una plantilla hasta la construcción de Dappchains como Cryptokitties.

Se recomienda que investigue un poco hasta que se familiarice con los patrones de cómo se construirá una cadena de Substrate. Una vez que tenga una comprensión sólida, puede ejecutar los tutoriales de [Token Curated Registry](https://docs.substrate.dev/docs/building-a-token-curated-registry-dappchain-using-substrate) o [Substratekitties](https://shawntabrizi.github.io/substrate-collectables-workshop/).

#### Empezando un nuevo proyecto

Es probable que desee utilizar el script proporcionado por los desarrolladores de Substrate para iniciar una nueva plantilla de proyecto.

Primero descargue el script ejecutando este comando:

```bash
curl https://getsubstrate.io -sSf | bash
```

A continuación, cree su proyecto ejecutándo:

```bash
substrate-node-new <myProject> <myName>
```

!!! attention
    _Aunque Substrate todavía es anterior a la versión 1.0, se recomienda utilizar en su lugar este [paquete estable](https://github.com/shawntabrizi/substrate-package) que contiene el nodo y ui en lugar del script anterior._

### Configurando su cadena

Después de haber creado su lógica de cadena usando un framework como Substrate, lo compilarás en un Wasm blob que contiene su función de transición de estado. Este es el núcleo de su cadena de bloques y es a lo que los validadores de la relay chain de Polkadot validarán todas las transiciones de estado. Pero antes de que pueda enviar su cadena a la red de Polkadot, todavía hay un par de cosas de las que debe ocuparse.

Las dos grandes cosas que necesitará resolver cuando termine de desarrollar su lógica de cadena son: 1) necesita configurar al menos un nodo collator y 2) necesitará asegurar su disponibilidad en la relay chain adquiriendo un lugar en el registro de parachain.

El primero necesita ser hecho porque los validadores en la relay chain necesitan alguna forma de estar al tanto de las nuevas transiciones de estado que vienen de su cadena. Esta funcionalidad es manejada por el tipo de nodo especializado conocido como [collator](../learn/terms-and-definitions.md#collator). Básicamente, los collators son los nodos que manejarán las transiciones de estado de su cadena y entregarán esas transiciones de estado con pruebas a los validadores para que las validen.

Su cadena puede tener un validador o varios. Pueden ser administrados como servicios públicos por los desarrolladores o puede haber una estructura de incentivos implantada en la parachain para animar a la comunidad a que los opere.

En este momento es todavía muy temprano en el desarrollo de los nodos collator definitivos que pueden ser usados para la interacción con Polkadot. Hay una demo temprana que está disponible en el repositorio de Polkadot, esto puede ser ejecutado siguiendo una demo temprana como se muestra en este [video tutorial](https://www.youtube.com/watch?v=pDqkzvA4C0E). A medida que continúe el desarrollo, habrá bibliotecas que harán que la configuración de una parachain compatible con Polkadot sea muy sencilla. Una de estas bibliotecas se llama Cumulus.

#### Cumulus

[Cumulus](https://github.com/paritytech/cumulus) está todavía en desarrollo y no está listo para su uso. Su objetivo es ser una extensión de la librería de Substrate que hará que cualquier tiempo de ejecución de Substrate sea compatible con Polkadot.

Manejará parte de la sobrecarga de cualquier parachain que necesite ser compatible con Polkadot. Estos incluyen:

 - gestión de mensajes entre parachains
 - nodo collator out-of-the-box
 - seguimiento de la relay chain con un cliente ligero integrado
 - compatible con las complejidades específicas de Polkadot de la autoría en bloque

Comenzar con Cumulus una vez que esté listo será tan fácil como:

 - Mínima modificación de la cadena de Substrate ya escrita
 - Cumulus lo adapta con poco esfuerzo

Para lo último sobre Cumulus, véase una charla reciente de Rob Habermeier a continuación.

[![img](http://img.youtube.com/vi/thgtXq5YMOo/0.jpg)](https://www.youtube.com/watch?v=thgtXq5YMOo)

#### Garantizar una elección justa del validador

En Polkadot los validadores se seleccionan automáticamente para validar para cada parachain utilizando la aleatoriedad de la relay chain. Cada era en la relay chain estos validadores se rotarán para asegurar que haya una elección justa de validador y que ningún validador individual continúe validando para una cadena específica. Como desarrollador u operador de parachains, esta función de seguridad se incluirá una vez que haya hecho su cadena compatible con Polkadot y haya adquirido un lugar en el registro de parachains.

### Cómo incluir su parachain en polkadot

El segundo paso muy importante que tendrá que hacer una vez que esté listo para integrar su cadena en Polkadot es asegurar un lugar en el registro de parachain.

En el white paper se afirma que las parachains sólo se añadirán a la red a través de un proceso de votación en referéndum pleno por parte del mecanismo de gobernanza. Además, la cadena existiría hasta que sea nuevamente expulsada por un mecanismo similar en el que un referéndum concedería un período de gracia para que los usuarios migren de las cadenas

Se sigue creyendo que el mecanismo de gobernanza integrará de esta manera algunas cadenas especialmente útiles y de valor añadido en beneficio de la red Polkadot en su conjunto. Pero esta no será la única manera de que usted adquiera un lugar para su parachain. Si usted no quiere persuadir al mecanismo de gobierno de que su cadena es útil, entonces hay otra manera.

El pensamiento actual es que habrá un mecanismo de subasta que distribuirá los anuncios en el registro de parachain. Esta subasta será probablemente una subasta [Vickrey](https://es.wikipedia.org/wiki/Subasta_Vickrey), también conocida como subasta de segundo precio. La subasta se utilizaría para asignar entradas de registro a proyectos de parachain para duraciones de tiempo diferentes pero constantes (por ejemplo, 6 meses, 12 meses, 24 meses). Para participar en la subasta, los participantes deben depositar DOTs. El participante que apueste por un número suficiente de DOTs para ser el mejor postor deberá bloquear el número de DOTs de la segunda oferta más alta mientras dure la inclusión en el registro.

Los desarrolladores pueden empezar a pensar ahora en cómo obtener suficientes DOTs para asegurar que puedan obtener una entrada en el registro de parachain. Algunas ideas incluyen una campaña de financiación colectiva en la que los participantes depositarán sus propios DOTs individualmente para una sola cadena, una venta en masa para la cual los participantes comprarán algunas fichas de una cadena de modo que la cadena pueda tener suficientes DOTs para depositar su propia entrada, o a través de medios de recaudación de fondos privados.

#### ¿Qué pasa cuando se acaba el tiempo?

Una vez que asegure su entrada en el registro de parachain, se le garantiza ese lugar hasta que 1) la duración del tiempo vinculado a esa entrada haya transcurrido o 2) el mecanismo de gobierno vote para eliminarlo.

La opción 2 probablemente sólo ocurrirá en circunstancias extremas y no será algo que ocurra comúnmente. La opción 1 será la forma en que la mayoría de las cadenas expirarán de la red de Polkadot.

Cuando su cadena se está acercando al final de su duración, algunas cosas que podrían suceder incluyen: un referéndum podría ser realizado por las partes interesadas de la cadena para extender el tiempo de vida al continuar con los DOTs (si su cadena implementa el gobierno en la cadena), los usuarios pueden migrar con seguridad fuera de la plataforma a una alternativa, una campaña podría ser llevada a cabo para financiar a los DOTs para adquirir un lugar diferente en el registro.

En la mayoría de los casos probablemente será bastante sencillo "renovar" su inscripción en el registro de parachain al seguir participando con DOTs.

### Beneficios de ser una parachain

La ventaja de ser una parachain en Polkadot es que su cadena estará protegida con la misma seguridad que toda la red de Polkadot sin necesidad de que usted tenga que mantener su propio consenso. Además, su cadena podrá interactuar con todas las demás parachains de Polkadot a través del sistema de comunicación entre cadenas.

Las herramientas y sistemas de monitorización que se están construyendo para Polkadot o para diferentes parachains serán fácilmente adaptables o reutilizables en el futuro por nuevas parachains. Sin Polkadot, las cadenas soberanas necesitarían tener carteras compatibles que las soportaran. Con Polkadot, gran parte de las herramientas básicas como carteras y exploradores de bloques ya estarán disponibles y configurables para su parachain.

Ser una parachain de Polkadot le permitirá a usted y a su comunidad experimentar con las últimas innovaciones en tecnología de cadenas de bloques. Tanto si está interesado en la gobernanza, la escalabilidad, la privacidad o las VM personalizadas, Polkadot es lo suficientemente general como para soportar sus innovaciones en el lanzamiento y en el futuro.

_Has llegado al final de la sección de parachain, puedes leer sobre los contratos inteligentes o ir directamente a la [conclusión](#conclusion)._

## Entonces, quieres construir un contrato inteligente

La relay chain de Polkadot en sí misma no soportará contratos inteligentes, pero como Polkadot es una red de muchas cadenas de bloques heterogéneas, habrá parachains que sí los soporten.

Parity Technologies ya ha sentado gran parte del trabajo preliminar para una solución lista para usar para las parachains que desean incluir la funcionalidad de contratos inteligentes. El módulo Substrate [contract](https://github.com/paritytech/substrate/tree/master/srml/contract) en el núcleo de SRML soportará contratos inteligentes que se compilan en Wasm.

Para desarrollar un contrato inteligente que compile en Wasm, también se necesita un lenguaje apropiado. Para ello, Parity ha estado trabajando en un lenguaje específico de dominio llamado [pDSL](#pdsl-paritys-domain-specific-language).

Un proyecto que ha anunciado su intención de convertirse en una parachain de Polkadot con soporte para contratos inteligentes es [Edgeware](#edgeware). A medida que el ecosistema madure, existe una alta probabilidad de que más cadenas se presenten como parachains inteligentes habilitados por contrato.

Polkadot también será compatible con plataformas de contratos inteligentes preexistentes como Ethereum y Tezos a través de puentes. Esto significa que incluso el trabajo invertido en el desarrollo de estas plataformas hoy en día puede ser aplicable a la ejecución en Polkadot en el futuro.

### Edgeware

Edgeware es una parachain planificado para Polkadot que permitirá contratos inteligentes. Junto con otras innovaciones interesantes en la gobernanza y la distribución de tokens, probablemente será la primera parachain que se conectará a la red principal de Polkadot con contratos inteligentes habilitados. Puedes estar al día con el proyecto en su página [web](https://edgewa.re).

### pDSL (Lenguaje específico de dominio de Parity)

El [pDSL](https://github.com/Robbepop/pdsl) pretende ser un nuevo lenguaje específico de dominio para la elaboración de contratos inteligentes en Rust que se compilará hasta el código Wasm. Como se indica en el README, todavía está en una fase experimental y le faltan muchas de las características planeadas, pero es posible empezar a escribir contratos inteligentes con él hoy.

Para los desarrolladores interesados, pueden empezar a escribir contratos inteligentes usando pDSL estudiando los [ejemplos](https://github.com/Robbepop/pdsl/tree/master/examples) que ya han sido escritos. Estos pueden ser usados como guías para escribir lógica más compleja que será desplegable en parachains de contratos inteligentes. Sin embargo, dado que el ecosistema todavía es tan reciente, probablemente no sea una buena idea intentar escribir código de producción con él.

## Despliegue de su aplicación como un contrato inteligente

Un contrato inteligente es en esencia un código que existe en una dirección de la cadena y que puede ser ejecutado. Una vez que el código es desarrollado, necesita alguna forma de entrar en la cadena y estar disponible para los usuarios. La implementación de su contrato inteligente en la cadena variará dependiendo de la plataforma específica a la que se dirija, pero en general implicará el envío de una transacción especial que `creará` su contrato inteligente en el libro mayor. Por lo general, esta transacción requerirá una tarifa para cubrir los costes de cálculo de cualquier lógica de constructor y para el almacenamiento que consume.

## Pagando por su contrato inteligente

Diferentes plataformas tendrán diferentes maneras de pagar por el despliegue y mantenimiento de su contrato inteligente.

Algunas de las formas en que las diferentes plataformas pueden implementar el pago de contratos inteligentes:

 - un cargo por transacción asociado con la transacción de implementación
 - un modelo de suscripción en el que se paga para suscribirse a una cadena
 - modelo de token de acceso en el que debe tener suficientes tokens para usar una cadena (cp. EOS)
 - alquiler de almacenamiento
 - prueba gratuita. Algunas cadenas pueden querer atraer a nuevos desarrolladores con promociones

En general, la mayoría de las plataformas de contratos inteligentes implementan la noción de `gas`, que representa el cálculo necesario para ejecutar la lógica de contratos inteligentes a través de la red. El gas se paga normalmente pagando el `gas price` correspondiente que se especifica en la transacción.

Hay un par de cosas que usted querrá tener en cuenta al desarrollar su aplicación para asegurarse de que el coste del gas se mantenga dentro de unos límites razonables y no se vuelva demasiado caro. Estos son el almacenamiento de su contrato y la complejidad de la lógica.

El almacenamiento es costoso en la cadena, ya que aumenta los datos necesarios para que los nodos realicen una sincronización completa. Al desarrollar contratos inteligentes, trate de mantener la cantidad de datos enviados a la cadena lo más bajo posible. Para ello, es posible que desee considerar soluciones de almacenamiento descentralizadas como [IPFS](https://ipfs.io/) o [Storj](https://storj.io/), que a menudo pueden funcionar en paralelo a su contrato inteligente, y le permiten mantener sólo un puntero al almacenamiento como un hash en la cadena.

Asimismo, es aconsejable mantener la complejidad de la lógica en la cadena lo más baja posible para minimizar el importe de las tasas de gas. A menudo esto significa que cualquier cálculo no crítico debe hacerse antes de enviar los datos a la cadena.

### Aún es temprano

Los contratos inteligentes en Polkadot todavía son muy tempranos, lo que explica por qué esta sección está compuesta de indicadores de proyectos en curso e información inespecífica. Manténgase al día con los proyectos anteriores y esté atento a las primeras redes de prueba que se lanzarán. Si te sientes valiente puedes intentar trabajar con herramientas como pDSL pero ten en cuenta que gran parte de la tecnología es anterior a la versión estable, así que probablemente cambiará a medida que continuemos avanzando hacia la futura versión de Polkadot para mainnet.

## Conclusión

Esperamos que esta guía le haya ayudado a tomar la decisión correcta sobre si su nuevo proyecto será un parachain o un contrato inteligente, y le haya mostrado los recursos esenciales que puede utilizar para empezar a desarrollar en Polkadot hoy mismo. A pesar de que las herramientas aún están madurando, hay un beneficio por llegar temprano a la escena: tienes la habilidad de innovar creando algo verdaderamente nuevo.

Si desea compartir sus ideas para una parachain o un contrato inteligente, no dude en hablar con nosotros en el [Polkadot Watercooler](https://riot.im/app/#/room/#polkadot-watercooler:matrix.org) y si tiene preguntas sobre el desarrollo, puede intentarlo en el [Polkadot Beginners Lounge](https://riot.im/app/#/room/#polkadotnoobs:matrix.org). Manténgase al día siguiendo los [canales sociales](https://wiki.polkadot.network/en/latest/community/) apropiados y buena suerte construyendo su visión en realidad en Polkadot!
