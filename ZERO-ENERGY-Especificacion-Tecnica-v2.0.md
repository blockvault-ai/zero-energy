**ZERO ENERGY**

**ESPECIFICACIÓN TÉCNICA DEL PROTOCOLO**

Arquitectura Económica y Tokenomics

Red Base

**Versión 2.0**

# NOTA PRELIMINAR

Este documento es la especificación funcional del protocolo ZERO Energy y la fuente de referencia única para tres audiencias: el equipo que implementa los contratos inteligentes, los auditores que los verifican y la gobernanza que aprueba su evolución.

Tres reglas de lectura:

* Toda afirmación debe poder verificarse contra el código desplegado. Donde el documento describe un límite, ese límite existe como comprobación en el contrato.
* Los **invariantes** no pueden modificarse por ningún mecanismo. Los **parámetros** admiten configuración por gobernanza dentro de los rangos indicados. La Sección 11 los recoge todos en una tabla única.
* Cuando una decisión no está tomada, el documento lo dice. No se sustituye un hueco por una suposición.

**Nomenclatura.** Los activos se identifican como **ZERO DAO (ZDAO)** y **ZERO REC (ZREC)**. Tras la primera mención en cada sección se emplean los tickers. Fórmulas, tablas, diagramas y especificaciones de contratos usan siempre ZDAO y ZREC.

# 1. ARQUITECTURA GENERAL

## 1.1 Qué es ZERO Energy

ZERO Energy es una **infraestructura física descentralizada (DePIN) aplicada al sector energético**, desplegada sobre la red Base.

El protocolo conecta instalaciones reales de generación renovable con una economía digital verificable. La cadena de valor es:

Infraestructura física -> Producción energética -> Medición
 -> Validación (Proof of Energy) -> Registro on-chain
 -> Tokenización -> Incentivos -> Mercado -> Gobernanza

DePIN no es un componente añadido ni una etiqueta comercial: es el marco arquitectónico dentro del cual opera todo lo demás. Cuando resulte más preciso, ZERO Energy puede describirse como **DePIN energética**.

## 1.2 Proof of Energy (PoE)

**Proof of Energy es el modelo de verificación mediante el cual una producción energética física se convierte en una actividad verificable on-chain.** Es la capa que conecta la infraestructura física con la economía digital del protocolo.

Producción física -> Captura de datos -> Validación por Energy Oracles
 -> Verificación de autenticidad, elegibilidad y no duplicidad
 -> Registro verificable on-chain -> Autorización de emisión de ZREC

> **Delimitación obligatoria.** Proof of Energy **no es un mecanismo de consenso de blockchain**. Base mantiene su propio consenso y seguridad. PoE opera en la capa de validación energética del protocolo y habilita acciones on-chain; no sustituye ni modifica el consenso de la red. Esta distinción debe mantenerse en toda comunicación técnica del protocolo.

## 1.3 Los dos activos

ENERGY DePIN
 Infraestructura física distribuida
 |
 Proof of Energy (PoE)
 Medición, validación y prueba de producción
 |
 ZERO REC (ZREC)
 Representación tokenizada de la actividad energética
 |
 ZERO DAO (ZDAO)
 Gobernanza, incentivos y captura de valor

|  |  |  |
| --- | --- | --- |
|  | **ZERO DAO (ZDAO)** | **ZERO REC (ZREC)** |
| Función | Gobernanza e incentivos | Activo energético y utilitario |
| Suministro | Máximo absoluto de 1.000.000.000 | Sin máximo predefinido |
| Decimales | 8 | 3 |
| Origen | Preventa, Méritos, Staking, Estratégicas | Genesis y producción certificada |
| En el staking | Se recibe como recompensa | Es el activo que se deposita |

**Principio estructural.** ZDAO mantiene un límite estricto de emisión; ZREC se expande conforme crece la producción energética certificada. Esta separación permite que la infraestructura energética crezca sin comprometer la escasez del activo de gobernanza. Es el fundamento económico del protocolo y no debe alterarse.

## 1.4 Capas del protocolo

|  |  |  |
| --- | --- | --- |
| **Capa** | **Componente** | **Sección** |
| Física | Instalaciones, medidores, inversores | 4 |
| Validación | Energy Oracles, Proof of Energy | 4 |
| Activos | ZDAO y ZREC | 2 y 3 |
| Incorporación | Preventa, Claim y Genesis | 5 |
| Incentivos | Sistema Oficial de Staking | 6 |
| Interfaz | ZERO DEX (Dashboard) | 7 |
| Gobernanza | DAO, Comité, hitos de descentralización | 8 |

## 1.5 Ciclo operativo completo

PREVENTA / MÉRITOS (off-chain)
 |
 CLAIM + RESOLUCIÓN GENESIS (ventana única de 6 meses)
 |
 +----------------------------+----------------------------+
 | RAMA PREVENTA | RAMA MÉRITOS |
 | elección única: | conversión directa: |
 | a) ZDAO libre | ZREC Genesis |
 | b) ZREC Genesis libre | -> staking automático |
 | | -> 6 meses bloqueo |
 | | -> 2% mensual x 50 meses |
 +----------------------------+----------------------------+
 |
 ZERO DEX (Dashboard) -> staking · mercado · liquidez · gobernanza
 |
 PRODUCCIÓN ENERGÉTICA REAL
 |
 Proof of Energy -> Energy Oracles -> lote -> validación
 |
 EMISIÓN ORDINARIA DE ZREC -> economía energética del protocolo

Cada elemento de este ciclo se desarrolla una sola vez en el documento, en la sección que le corresponde.

# 2. ZERO DAO (ZDAO)

Token ERC-20 de gobernanza e incentivos del protocolo, desplegado sobre Base. **8 decimales**: la unidad mínima es 0,00000001 ZDAO, granularidad necesaria para el cálculo proporcional de recompensas, comisiones y distribuciones.

## 2.1 Magnitudes de suministro

El documento distingue con precisión seis magnitudes que no deben confundirse:

|  |  |
| --- | --- |
| **Magnitud** | **Definición** |
| MAX\_SUPPLY | 1.000.000.000 ZDAO. Techo absoluto, nunca superable |
| CumulativeMinted | Total histórico acuñado. Solo crece, nunca decrece |
| totalSupply | ZDAO existentes ahora (acuñado menos quemado) |
| Suministro circulante | totalSupply menos posiciones bloqueadas |
| Asignaciones off-chain | Derechos registrados en Preventa y Méritos, aún no acuñados |
| ZDAO quemados | Destruidos definitivamente |

**Invariante:** `CumulativeMinted <= MAX\_SUPPLY`, verificado en toda acuñación.

## 2.2 La quema no regenera capacidad de emisión

Es la regla económica central del suministro y debe implementarse de forma explícita.

Los ZDAO quemados se destruyen definitivamente. La quema **reduce `totalSupply` pero no reduce `CumulativeMinted`**, y por tanto **no libera capacidad de acuñación**.

Ejemplo:
 MAX\_SUPPLY .............. 1.000M
 CumulativeMinted ........ 600M
 Quemados ................ 100M
 totalSupply ............. 500M

 Capacidad de emisión restante = 1.000M - 600M = 400M
 (NO 500M: los 100M quemados no vuelven)

Sin esta regla, cada quema abriría margen para acuñar de nuevo y el techo de 1.000M dejaría de ser un límite real: bastaría un ciclo de acuñar y quemar para emitir indefinidamente.

Consecuencias que deben respetarse en toda comunicación del protocolo:

* Una quema **no debe describirse nunca como "nuevo suministro disponible"**.
* Cualquier política extraordinaria futura sobre capacidad de emisión requiere autorización expresa de gobernanza, debe respetar el MAX\_SUPPLY y las reservas inmutables, y **no puede presentarse como consecuencia automática de una quema**.

## 2.3 Distribución del suministro

|  |  |  |  |
| --- | --- | --- | --- |
| **Programa** | **Asignación** | **%** | **Naturaleza** |
| Preventa | 200.000.000 | 20 % | Asignación fundacional (Sección 5) |
| Méritos y Ecosistema | 100.000.000 | 10 % | Cajón único de contribución |
| Recompensas por Staking | 500.000.000 | 50 % | Reserva inmutable (Sección 6) |
| Emisiones Estratégicas | 200.000.000 | 20 % | Requiere aprobación expresa de la DAO |
| Total | 1.000.000.000 | 100 % |  |

**Méritos y Ecosistema es un cajón único.** Comprende equipo fundador, colaboradores, embajadores y campañas de incorporación de usuarios. **No existen cupos separados por categoría.**

**Emisiones Estratégicas.** Destinos previstos, con carácter **indicativo y no vinculante**: liquidez de mercados (150.000.000) y contingencias (50.000.000). Toda emisión contra este programa exige propuesta aprobada por la DAO y el timelock de la Sección 8. No existe ningún mecanismo automático de acuñación contra esta partida.

**Reserva de Staking.** Los 500.000.000 son un **invariante**. No pueden destinarse a tesorería, equipo, liquidez, Méritos, campañas, emisiones estratégicas ni ninguna finalidad distinta del Sistema Oficial de Staking. Solo el contrato de staking puede acuñarlos, y únicamente cuando exista una recompensa legítimamente devengada. **Los ZDAO no necesarios para recompensas no se acuñan.**

## 2.4 Fungibilidad

Todos los ZDAO emitidos son completamente fungibles. El origen de su emisión no altera la naturaleza del activo ni los derechos que confiere.

De ello se deriva una consecuencia técnica que el protocolo respeta en su diseño: **un ERC-20 no puede distinguir la procedencia de los tokens que componen un saldo.** Por eso ninguna regla del protocolo depende del origen de un ZDAO concreto. El Derecho Genesis, que sí depende del origen, se resuelve fuera del token (Sección 5.3).

## 2.5 Funciones

**Gobernanza.** ZDAO confiere derecho de voto sobre parámetros económicos configurables, propuestas de la comunidad, composición del Comité del Protocolo, Emisiones Estratégicas, destino de los recursos de la Tesorería DAO y actualizaciones del protocolo.

Debe distinguirse con precisión: los titulares **deciden mediante voto** el destino de la Tesorería DAO. No ostentan derecho de cobro sobre ella ni participan en su reparto.

**Incentivo.** ZDAO es el activo con el que el protocolo retribuye la participación en el staking. Solo quien mantiene ZREC depositado recibe estas recompensas. ZDAO no representa producción energética.

**Relación con la actividad del protocolo.** Las comisiones del ZERO DEX se reparten entre Tesorería DAO, proveedores de liquidez, mecanismo de quema y mantenimiento de infraestructura, según los porcentajes de la Sección 7.

## 2.6 Mecanismo de quema

Los ZDAO destinados contractualmente a quema **se acumulan de forma segregada y auditable** hasta alcanzar un umbral mínimo de ejecución.

ZDAO destinados a burn -> acumulación segregada
 -> umbral alcanzado (Burn Execution Threshold)
 -> ejecución -> quema -> evento on-chain verificable

**Burn Execution Threshold inicial: 10.000 ZDAO.** Es un parámetro configurable por gobernanza, no una constante económica.

El mecanismo evita cientos de operaciones de quema de cuantía reducida, reduce el coste operativo y mantiene la trazabilidad on-chain. Ni la DAO ni el Comité intervienen en el momento de ejecución: se dispara al alcanzarse el umbral.

Conforme a la Sección 2.2, los ZDAO quemados por este mecanismo no regeneran capacidad de emisión.

# 3. ZERO REC (ZREC)

Activo energético del protocolo y **único activo depositable en el Sistema Oficial de Staking**. Token ERC-20 sobre Base.

## 3.1 Estándar y decimales

1 ZREC = 1 MWh = 1.000 kWh
0,1 ZREC = 100 kWh
0,01 ZREC = 10 kWh
0,001 ZREC = 1 kWh <- unidad mínima representable

**3 decimales.** La correspondencia con la unidad energética es deliberada: la mínima unidad del token es exactamente 1 kWh, lo que permite representar desde microgeneración residencial hasta parques industriales.

La diferencia de precisión con ZDAO (8 decimales) es intencionada y compatible con EVM. Los contratos deben normalizar ambas escalas mediante aritmética entera segura y reglas explícitas de redondeo, especialmente en el cálculo de recompensas del staking, donde se depositan ZREC y se emiten ZDAO.

ZREC **no tiene suministro máximo**. Crece conforme se certifica nueva producción energética.

## 3.2 Fungibilidad — un único token

ZREC es **un único activo fungible**. No existen tokens distintos para ZREC Genesis de Preventa, ZREC Genesis de Méritos, ZREC de producción energética ni ZREC adquirido en mercado.

Las diferencias de comportamiento se aplican **a la posición o al contrato donde el activo esté depositado, nunca al token**. Un ZREC liberado de una posición de Méritos es exactamente el mismo activo que cualquier otro ZREC libre.

La procedencia puede conservarse como información histórica de trazabilidad, pero no afecta a la fungibilidad.

Este principio permite que el bloqueo de Méritos exista sin fragmentar el activo ni la liquidez del mercado.

## 3.3 Dos orígenes

|  |  |  |  |
| --- | --- | --- | --- |
| **Origen** | **Cuándo** | **Cómo** | **Respaldo energético** |
| ZREC Genesis | Ventana de Claim (6 meses) | Resolución de asignaciones fundacionales | No |
| ZREC Energético | Permanente, tras la ventana | Producción certificada vía PoE | Sí |

### A. ZREC Genesis

Permite arrancar la economía energética y el staking antes de que exista producción certificada suficiente. Se resuelve **durante el Claim**, según el procedimiento de la Sección 5.

**No existe operación de acuñar y quemar.** Cuando se ejerce el Derecho Genesis, los ZDAO correspondientes **no llegan a acuñarse on-chain**. La formulación correcta es:

> El ejercicio del Derecho Genesis extingue el derecho de Claim sobre la cantidad correspondiente de ZDAO y habilita la acuñación equivalente de ZREC Genesis.

1 derecho de Claim sobre 1 ZDAO -> ejercicio del Derecho Genesis
 -> extinción del derecho a recibir ZDAO
 -> acuñación de 1 ZREC Genesis

Finalizada la ventana de seis meses, el mecanismo queda **deshabilitado de forma permanente e irreversible**.

### B. ZREC Energético

Única fuente ordinaria de emisión tras la ventana fundacional. Requiere de forma acumulativa producción real, validación por Energy Oracles y finalización del lote conforme a la Sección 4.

Distribución de cada emisión:

1.000 MWh certificados -> 1.000 ZREC emitidos
 500 ZREC -> Productor Energético
 500 ZREC -> Tesorería DAO

El reparto 50/50 es configurable por gobernanza entre el 30 % y el 70 % para la parte del Productor Energético.

**No se establece un límite inicial de emisión por productor.** La gobernanza podrá introducirlo en el futuro si el desarrollo del protocolo lo requiere.

## 3.4 Contabilidad diferenciada

El protocolo publica de forma permanente y separada:

* ZREC Genesis emitidos.
* ZREC Energéticos emitidos.
* ZREC retirados de circulación, por tipo.
* Porcentaje de la Emisión Genesis compensado (Sección 3.6).

La separación es **exclusivamente contable**: afecta a los agregados publicados, no a las unidades ni a la fungibilidad.

Es necesaria porque durante la fase fundacional la práctica totalidad del ZREC en circulación procederá de la Emisión Genesis y, por tanto, **no representará energía efectivamente producida**. Publicar ambos agregados permite conocer en todo momento qué proporción del activo energético está respaldada por producción real.

## 3.5 Valor económico

ZREC no tiene precio predeterminado ni paridad fija respecto a ZDAO, a ninguna moneda ni a ningún otro activo. **El protocolo no establece, sostiene ni aspira a sostener valor alguno para ZREC.** Su valor lo determina libremente el mercado.

## 3.6 Programa de Compensación Genesis

Acredita de forma progresiva y verificable que la emisión fundacional ha sido igualada por producción renovable real.

Cuando la Tesorería DAO recibe ZREC Energéticos, puede destinarlos al programa:

1. Seleccionar ZREC contabilizados como Energéticos.
2. Retirarlos permanentemente de circulación mediante quema.
3. Emitir el **NFT de Compensación Genesis** correspondiente.
4. Incrementar el contador público de compensación acumulada.

**Alcance y límites.** El programa **no modifica, sustituye ni respalda individualmente** los ZREC Genesis emitidos. No confiere derecho de canje, redención ni reclamación sobre energía. Su función es **contable y demostrativa**: acreditar públicamente que el protocolo ha generado, certificado y retirado una cantidad de energía renovable real equivalente a una fracción de su emisión fundacional.

**Regla de uso único.** Un MWh certificado no puede contabilizarse dos veces. El NFT de Compensación Genesis se emite a nombre de la Tesorería DAO, queda marcado con esa finalidad y **no es transferible ni utilizable como certificado de compensación ambiental por terceros**.

## 3.7 NFT de Certificación Energética

Certificado energético del protocolo. Cada NFT representa energía renovable certificada retirada permanentemente de circulación, e incluye: identificador único, Productor Energético e instalación de origen, fecha, cantidad certificada, referencia de la validación del Oráculo, hash verificable y **finalidad** (compensación de tercero o Compensación Genesis).

El campo de finalidad determina el régimen: los certificados de Compensación Genesis se rigen por la Sección 3.6 y no son transferibles.

> **Delimitación.** ZREC certifica **generación renovable verificada por el protocolo**. El NFT certifica la **retirada** dentro del protocolo. Ninguno de los dos equivale automáticamente a una Garantía de Origen oficial, un I-REC ni ningún otro certificado regulado, salvo que exista la integración y acreditación correspondiente. Esta distinción debe mantenerse en toda comunicación.

# 4. PROOF OF ENERGY Y ENERGY ORACLES

Esta sección desarrolla la capa que convierte producción física en emisión autorizada de ZREC: qué se certifica, cómo se obtiene el dato, cómo se valida y cómo se agrupa para su registro on-chain.

## 4.1 Qué certifica el protocolo

**ZERO Energy certifica energía renovable efectivamente generada y validada, con independencia de su destino posterior** —autoconsumo, almacenamiento o vertido a red—, siempre que se garantice la trazabilidad de la producción y se evite la doble contabilización de los atributos energéticos o ambientales asociados.

Un productor que genera 1.000 MWh renovables, autoconsume 300 y vierte 700 a red ha producido 1.000 MWh renovables. Si el protocolo verifica correctamente esos 1.000 MWh en el punto de generación, son potencialmente 1.000 ZREC.

> **El destino de la electricidad no es el problema. El problema es el atributo ambiental.** Si esos 700 MWh exportados ya han generado o transferido garantías de origen, certificados renovables u otro atributo equivalente fuera de ZERO Energy, no pueden presentarse simultáneamente como certificado exclusivo del protocolo sin reglas de interoperabilidad y cancelación. Eso es lo que el protocolo debe resolver, y se aborda en la verificación de exclusividad de la Sección 4.3.

Cualquier regla general que excluyera la energía vertida a red sería técnicamente incorrecta —el protocolo mide generación, no consumo— y penalizaría precisamente a los productores profesionales, cuya actividad principal es inyectar energía a la red.

## 4.2 Objetivos de la capa de validación

* Verificar producción renovable real y garantizar la autenticidad de los datos.
* Prevenir fraude, manipulación y emisiones no autorizadas.
* **Evitar la doble contabilización**: cada unidad de energía se asocia a una única emisión y, al retirarse, a un único certificado con una única finalidad.
* Mantener un registro auditable de todas las validaciones.

El principio de no duplicidad es transversal y vincula al resto del documento.

## 4.3 Modelo de oráculos y descentralización progresiva

**Modelo inicial.** Durante la fase inicial la validación energética se realiza mediante un **oráculo propio autorizado por ZERO Energy**.

Producción -> medición -> Oráculo ZERO -> validación
 -> Proof of Energy -> autorización de emisión de ZREC

> El documento **no presenta la infraestructura inicial como una red de oráculos descentralizada**, porque técnicamente todavía no lo es. La apertura a validadores independientes es un hito verificable de la Sección 8, no una declaración de intenciones.

**Evolución.** La arquitectura está preparada para admitir progresivamente múltiples fuentes independientes: smart meters, APIs de fabricantes, inversores, SCADA, sistemas IoT, plataformas de monitorización, certificadores, validadores externos y operadores de red.

No es obligatorio que todas las instalaciones dispongan del mismo número o tipo de fuentes. La arquitectura permite **reglas de confianza adaptadas a cada tipología de instalación**: una vivienda con autoconsumo y un parque fotovoltaico no requieren el mismo nivel de redundancia.

**Requisitos para validadores externos.** Su incorporación seguirá criterios de seguridad y madurez: independencia respecto del productor validado, capacidad técnica, identidad verificable cuando resulte necesaria, disponibilidad, integridad y trazabilidad de datos, seguridad criptográfica, historial operativo, ausencia de conflictos de interés e integración auditada.

> **La incorporación de un tercero no constituye por sí sola descentralización suficiente.** El objetivo es evolucionar hacia fuentes de validación verdaderamente independientes operando de forma fiable en producción durante un periodo suficiente.

**Registro de unicidad.** El sistema mantiene un registro de identificadores de lote ya procesados. Toda solicitud referida a un identificador previamente registrado se rechaza automáticamente, con independencia del oráculo que la presente.

**Verificación de exclusividad.** Antes de autorizar una emisión debe verificarse que la producción declarada no ha generado ya un certificado energético en otro sistema aplicable a la instalación. El procedimiento y la documentación exigible los aprueba la gobernanza.

**Delimitación entre fuente y oráculo.** La fuente obtiene y normaliza el dato; el oráculo lo verifica y lo firma. Un componente que únicamente lea datos no adquiere condición de oráculo por hacerlo, con independencia de su sofisticación técnica.

## 4.4 Adquisición de datos

**Conector universal.** El protocolo integra un conector capaz de obtener datos de producción de instalaciones de distintos fabricantes **sin requerir hardware adicional en el punto de generación**: el titular selecciona el fabricante de su inversor, autoriza el acceso a sus datos de producción y el conector los normaliza a un formato común.

Este modelo reduce sustancialmente la barrera de entrada para microgeneración residencial, el segmento más numeroso de la infraestructura distribuida.

> **Traslado del modelo de confianza.** La ausencia de un dispositivo en el punto de medida no reduce el riesgo: lo desplaza desde la manipulación del contador hacia la veracidad de la instalación declarada y el control de la cuenta del fabricante. Por eso la verificación no puede descansar en el dato recibido y se apoya en las comprobaciones de 4.5 y 4.6.

## 4.5 Registro y verificación de instalaciones

Antes de la primera emisión, cada instalación queda registrada con: identificador único, titular, ubicación, **potencia nominal**, fabricante y modelo del inversor y fecha de puesta en servicio.

**La potencia nominal no se acepta por declaración del productor.** Se verifica mediante tres comprobaciones combinadas:

|  |  |  |
| --- | --- | --- |
|  | **Comprobación** | **Contenido** |
| A | Documentación | Ficha técnica, certificado o información equivalente de la instalación |
| B | Identificación | Vínculo verificable entre instalación física, titular, medidor, inversor y sistema de monitorización |
| C | Validación empírica | Contraste entre la producción registrada y la capacidad nominal y características físicas |

La validación empírica convierte la potencia nominal en una **barrera de coherencia física**: una instalación registrada como 10 kWp que declara sistemáticamente 500 kWh/h describe una imposibilidad física, y el sistema puede generar alertas, revisión, suspensión provisional o solicitud de información adicional.

Comprobaciones de plausibilidad aplicables a todo dato de producción:

* Coherencia con la potencia nominal registrada.
* Coherencia con la irradiancia correspondiente a ubicación y fecha.
* Coherencia con las condiciones meteorológicas del periodo.
* Coherencia con el histórico de la propia instalación, considerando estacionalidad.
* Coherencia con instalaciones comparables de la misma zona y periodo.

Estas comprobaciones son **deterministas y reproducibles** por cualquier verificador con los mismos datos de entrada.

> **Limitación deliberada.** Cuando la detección de anomalías incorpore modelos de aprendizaje automático, su función queda limitada a **priorizar casos para revisión**, sin capacidad de autorizar por sí misma una emisión. La autorización de una acuñación debe poder justificarse mediante reglas verificables: ningún componente cuyo criterio no sea reproducible puede fundamentar una emisión.

## 4.6 Lotes energéticos

La producción no genera una transacción on-chain por cada lectura. Las lecturas se agregan en lotes.

**Cierre de lote.** Configuración inicial: **cada 24 horas o al alcanzar 1 MWh acumulado**, según la configuración aplicable a la instalación y lo que ocurra primero. Este criterio híbrido permite gestionar tanto una vivienda solar como un parque fotovoltaico con la misma arquitectura. Parámetro modificable por gobernanza.

**Identificador de lote.** Cada lote recibe un identificador único que **es el identificador del registro de unicidad de la Sección 4.3**. Un lote ya procesado no puede volver a procesarse, con independencia del número de intentos. De aquí se deriva la idempotencia del sistema: si una liquidación falla y se reintenta, el registro impide la doble acuñación.

**Provisionalidad: 72 horas.** Cerrado el lote, se abre una ventana durante la cual pueden realizarse reconciliación de datos, detección de anomalías, contraste de fuentes, validaciones adicionales y correcciones técnicamente justificadas.

Lecturas -> agregación -> lote -> 72 h provisional
 -> validación -> FINALIZED -> autorización de mint ZREC

Alcanzado el estado **FINALIZED**, la producción asociada queda identificada de forma inequívoca como procesada y no puede reutilizarse en una segunda emisión. La acuñación es irreversible: la ventana de provisionalidad es el único mecanismo que permite incorporar correcciones. Su duración puede reducirse posteriormente por gobernanza para productores u oráculos de alta confianza.

**Precisión y residuos.** La conversión de energía acumulada a ZREC se realiza con 3 decimales (mínimo 1 kWh). **La fracción no representable se arrastra al lote siguiente**; en ningún caso se descarta ni se redondea al alza. Con un número elevado de instalaciones de pequeña potencia, un redondeo sistemático en cualquier dirección produce una desviación material acumulada entre energía certificada y ZREC emitidos.

## 4.7 Flujo completo de validación

1. Producción de energía renovable
2. Captura y normalización (conector universal)
3. Comprobaciones de plausibilidad
4. Agregación y cierre de lote con identificador único
5. Ventana de provisionalidad (72 h)
6. Validación por Energy Oracle -> Proof of Energy
7. Comprobación de unicidad y exclusividad
8. Estado FINALIZED
9. Acuñación automática de ZREC y distribución
10. Registro permanente sobre Base

# 5. FASE FUNDACIONAL: PREVENTA, CLAIM Y GENESIS

## 5.1 Preventa

Mecanismo de financiación inicial. Las asignaciones se registran **off-chain**; los ZDAO se acuñan únicamente mediante el Sistema Oficial de Claim tras el lanzamiento. Este desacoplamiento entre financiación y emisión efectiva permite una distribución verificable y sin acuñación anticipada.

**Asignación máxima: 200.000.000 ZDAO** (20 % del MAX\_SUPPLY). Los porcentajes de los tramos se calculan **sobre esos 200.000.000**, no sobre el MAX\_SUPPLY global.

|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **Tramo** | **% de la Preventa** | **Cantidad** | **Precio** | **Inversión mínima** | **Recaudación** |
| 1 | 0 % – 5 % | 10.000.000 | 0,10 USDC | 1.000 USDC | 1.000.000 |
| 2 | 5 % – 15 % | 20.000.000 | 0,25 USDC | 500 USDC | 5.000.000 |
| 3 | 15 % – 40 % | 50.000.000 | 0,50 USDC | 250 USDC | 25.000.000 |
| 4 | 40 % – 70 % | 60.000.000 | 0,65 USDC | 100 USDC | 39.000.000 |
| 5 | 70 % – 100 % | 60.000.000 | 0,75 USDC | 10 USDC | 45.000.000 |
| Total |  | 200.000.000 | 0,575 medio |  | 115.000.000 |

**Los tramos operan en paralelo.** Cada tramo tiene asignación independiente y permanece disponible hasta agotar sus unidades o hasta que se produzca otra condición de cierre establecida por el protocolo. **No es necesario agotar un tramo para habilitar el siguiente.** La inversión mínima es el criterio de acceso a cada tramo.

**Suministro no asignado.** Los ZDAO de Preventa que no lleguen a asignarse no se acuñan y no se transfieren automáticamente a ningún otro programa.

Los recursos obtenidos se destinan al desarrollo de la infraestructura blockchain y energética, implementación y auditoría de contratos, integración de productores, desarrollo del ZERO DEX y crecimiento del ecosistema.

## 5.2 Ventana única de Claim y Genesis

**El Claim y la Emisión Genesis no son fases consecutivas: son una única ventana operativa de seis (6) meses.**

La decisión sobre la incorporación de los activos se toma **directamente durante el Claim**, a partir del registro fundacional off-chain y **antes de que los activos entren en libre circulación**. Esto elimina el modelo anterior de "Claim → recibir ZDAO → conversión posterior".

Durante la ventana:

* Los participantes de Preventa eligen entre ZDAO o ZREC Genesis.
* Los participantes de Méritos resuelven sus asignaciones directamente en ZREC Genesis.
* Se crean las posiciones de staking correspondientes a los ZREC de Méritos.

Finalizada la ventana, finaliza la posibilidad ordinaria de ejercer el Derecho Genesis.

⬜ **Pendiente de decisión del protocolo:** el tratamiento definitivo de las asignaciones que no hayan realizado Claim dentro de la ventana. No se establece en este documento por no existir todavía decisión expresa.

## 5.3 El Derecho Genesis

**Concepto central de la fase fundacional.**

El Derecho Genesis **no es una propiedad del token ERC-20 ZDAO**. Es un derecho asociado exclusivamente a determinadas asignaciones fundacionales registradas off-chain, y se resuelve una única vez durante el Claim.

Esta arquitectura es una consecuencia necesaria de la fungibilidad establecida en la Sección 2.4: un ERC-20 no puede distinguir la procedencia de los tokens de un saldo, de modo que ninguna restricción basada en el origen puede implementarse sobre el token. Se implementa sobre la asignación.

**Principio de consumo único.** La asignación admite dos estados terminales:

PENDIENTE -> CLAIM ZDAO (Derecho Genesis extinguido)
PENDIENTE -> CLAIM ZREC GENESIS (derecho a recibir ZDAO extinguido)

Ejecutada cualquiera de las dos opciones, la asignación queda consumida definitivamente. En consecuencia:

* No puede realizarse un segundo Claim sobre la misma asignación.
* No puede recuperarse posteriormente el Derecho Genesis.
* **La venta o transferencia de ZDAO no transfiere el Derecho Genesis.**
* No puede duplicarse utilizando distintas wallets.
* No puede ejercerse con ZDAO adquiridos en mercados secundarios.

Esto elimina por diseño el vector: \*wallet A reclama ZDAO → los transfiere a wallet B → wallet B intenta ejercer Genesis\*. La operación es imposible porque el derecho quedó consumido en el Claim inicial.

**Esta regla es un principio fundamental de seguridad del protocolo.**

## 5.4 Rama Preventa: elección única

Las asignaciones de Preventa **están libres de Vesting**. Durante el Claim, el participante elige entre dos opciones mutuamente excluyentes.

**Opción A — Claim directo en ZDAO**

Asignación off-chain -> Claim -> elección ZDAO
 -> acuñación y entrega on-chain -> wallet

Los ZDAO recibidos son completamente libres, fungibles y transferibles, sin Vesting ni bloqueo. El Derecho Genesis queda consumido y extinguido: no podrán usarse después para acuñar ZREC Genesis, y ningún adquirente posterior obtendrá derecho alguno.

**Opción B — Ejercicio del Derecho Genesis**

Asignación off-chain -> Claim -> ejercicio del Derecho Genesis
 -> extinción del derecho a recibir ZDAO
 -> acuñación de ZREC Genesis -> wallet

Relación: **1 derecho de Claim sobre 1 ZDAO de Preventa = 1 ZREC Genesis**. Los ZDAO correspondientes no se acuñan, por lo que **esta operación no constituye una quema**.

Los ZREC Genesis así obtenidos son **completamente libres**: sin Vesting, sin bloqueo obligatorio y sin permanencia mínima en staking. Pueden mantenerse, transferirse, venderse, intercambiarse, aportarse como liquidez, utilizarse en el ZERO DEX o depositarse voluntariamente en el staking, con retirada conforme a las reglas generales.

Esta vía da a los participantes de Preventa una entrada directa a la economía energética del protocolo y al staking de ZREC.

## 5.5 Rama Méritos: conversión y staking automático

Las asignaciones del Programa de Méritos se registran off-chain denominadas en ZDAO, pero **dichos ZDAO no se acuñan on-chain**.

Asignación ZDAO Méritos (off-chain) -> Claim
 -> extinción del derecho a recibir ZDAO
 -> acuñación directa de ZREC Genesis -> depósito automático en staking

Relación: **1 ZDAO asignado mediante Méritos = 1 ZREC Genesis**. Tampoco aquí existe quema: los ZDAO nunca se acuñaron.

**No existe contrato de Vesting.** Se elimina cualquier Genesis Vesting Vault, contrato independiente de Vesting o arquitectura equivalente. Las restricciones temporales se implementan **directamente mediante las reglas de retirada de la posición dentro del Sistema Oficial de Staking**.

La formulación correcta no es "ZREC en Vesting → Staking", sino **"ZREC Genesis de Méritos → Staking con calendario especial de retirada"**.

**Calendario de disponibilidad**

|  |  |  |
| --- | --- | --- |
| **Fase** | **Duración** | **Disponibilidad** |
| Bloqueo inicial | 6 meses | 0 % |
| Liberación progresiva | 50 meses | 2 % mensual de la asignación inicial |
| Total | 56 meses | 100 % |

Durante los seis primeros meses no puede retirarse principal, ni venderse, ni transferirse el ZREC depositado, ni utilizarse mecanismo alguno destinado a eludir el bloqueo. La posición participa en el staking desde el primer día.

Ejemplo — asignación inicial de 100.000 ZREC

 Meses 0-6 .... 0 ZREC disponibles
 Mes 7 ........ 2.000 ZREC disponibles
 Mes 8 ........ 4.000 ZREC disponibles
 Mes 31 ....... 50.000 ZREC disponibles
 Mes 56 ....... 100.000 ZREC disponibles

Cada tramo mensual que alcanza su fecha puede retirarse. **Una vez retirado, el ZREC es completamente libre** y equivalente a cualquier otro. La restricción afecta exclusivamente a la parte de la posición que aún no ha alcanzado su disponibilidad.

## 5.6 Sistema Oficial de Claim

**Consolidación previa.** Antes de abrir el Claim se cierra el registro de asignaciones, se genera un árbol de Merkle con todas ellas y sus condiciones, se publica su raíz on-chain y se publica el conjunto completo para verificación independiente. A partir de ese momento el conjunto es inmutable y cualquier participante puede verificar su asignación sin depender del protocolo.

**Ejecución.** El participante conecta una wallet compatible con Base, obtiene su prueba de inclusión y la presenta al contrato, que verifica la prueba contra la raíz publicada, comprueba que la asignación no ha sido consumida, resuelve la rama que corresponda y marca la asignación como consumida de forma permanente.

Este modelo es preferible a una autorización emitida por un servidor: la raíz publicada fija el conjunto de asignaciones de forma inmutable y auditable **antes de que se acuñe un solo activo**, y ninguna infraestructura externa conserva capacidad de autorizar acuñaciones. Toda modificación de la raíz requiere aprobación de la DAO y el timelock de la Sección 8.

# 6. SISTEMA OFICIAL DE STAKING

Se deposita **ZREC** y se recibe **ZDAO**. Es el único mecanismo mediante el cual pueden acuñarse los 500.000.000 ZDAO de la reserva de recompensas.

## 6.1 Base técnica

La arquitectura toma como **referencia conceptual** el modelo EasyStaking desplegado en xDaiChain, adaptado a la economía de ZERO Energy.

> **La implementación adaptada no conserva la cobertura de auditoría del contrato original.** Toda modificación o nueva implementación debe someterse a pruebas y auditoría de seguridad independiente antes de su despliegue.

Diferencia estructural respecto al modelo de referencia: en EasyStaking el activo depositado y el de recompensa son el mismo token. **En ZERO Energy son distintos**, lo que obliga a definir explícitamente la relación entre ambos y la normalización entre sus escalas de decimales (3 en ZREC, 8 en ZDAO).

## 6.2 Modelo dinámico de recompensas

El sistema **abandona cualquier interpretación de ROI fijo garantizado**. La tasa de recompensa resulta de dos componentes que se suman, cada uno con su propio techo.

### Componente temporal (antigüedad de la posición)

Curva sigmoide sobre el tiempo que la posición lleva depositada:

tasa\_personal = a · (t − b) / raiz((t − b)² + c)

 t = tiempo en staking a = techo del componente
 b = desplazamiento c = parámetro de pendiente
 si (t − b) < 0 -> tasa = 0

La curva crece rápido al principio y se aplana: premia la permanencia sin bloquear a nadie. Con parámetros de referencia, el comportamiento es:

|  |  |
| --- | --- |
| **Tiempo en staking** | **Proporción del techo alcanzada** |
| 1 día | ~3 % |
| 7 días | ~19 % |
| 21 días | ~50 % |
| 30 días | ~63 % |
| 90 días | ~93 % |
| 1 año | ~99 % |

### Componente global (participación en el sistema)

Depende del nivel agregado de participación: cuanto mayor es el volumen depositado respecto a una magnitud de referencia, mayor la tasa, hasta su techo.

tasa\_global = techo\_global · min(1, ZREC\_depositado\_total / referencia)

> **Adaptación obligatoria.** El modelo de referencia calcula esta magnitud sobre el suministro máximo del token depositado. **ZREC no tiene suministro máximo**: crece con la producción energética certificada. Por tanto la referencia **no puede ser el MAX\_SUPPLY** y debe definirse específicamente para la economía de ZREC —por ejemplo, una magnitud de ZREC elegible para staking o un equivalente matemáticamente adecuado. El parámetro concreto queda pendiente de la implementación definitiva.

### Devengo

recompensa = cantidad · (tasa\_personal + tasa\_global) · tiempo / 1 año

**Solo se acuñan los ZDAO efectivamente devengados.** Los ZDAO no necesarios para recompensas no se acuñan. Esta es una desviación deliberada del modelo de referencia, que acuña siempre al máximo y destina el excedente a una dirección de liquidez; ZERO Energy **no adopta ese reparto**, porque consumiría la reserva al ritmo máximo con independencia de la participación real.

## 6.3 Límite de emisión

Dos límites operan de forma acumulativa sobre el resultado de las fórmulas anteriores.

**1. Reserva absoluta — invariante.** 500.000.000 ZDAO. No puede destinarse a tesorería, equipo, liquidez, Méritos, campañas, emisiones estratégicas ni ninguna finalidad distinta del staking.

**2. Curva máxima decreciente.** Techo de emisión por periodo, como referencia de límite superior:

|  |  |
| --- | --- |
| **Periodo** | **Emisión máxima** |
| Año 1 | hasta 250.000.000 ZDAO |
| Año 2 | hasta 125.000.000 ZDAO |
| Año 3 | hasta 62.500.000 ZDAO |
| Año 4 | hasta 31.250.000 ZDAO |
| Siguientes | conforme a la curva decreciente |

> Estos valores son **techos máximos, no obligaciones de emisión**. Si las recompensas devengadas son inferiores al máximo disponible, la diferencia **no se acuña automáticamente**. El documento no debe describir estas cantidades como importes que necesariamente entrarán en circulación.

**Por qué el techo es necesario.** ZREC crece sin límite conforme aumenta la producción certificada. Una tasa porcentual aplicada sobre una base creciente produce una emisión creciente: sin techo, cuanto mayor fuera el éxito energético del protocolo, antes se agotaría la reserva de recompensas. La curva decreciente acota ese riesgo.

⬜ **Pendiente de la implementación definitiva:** el mecanismo de ajuste cuando el devengo agregado supere el techo del periodo. El criterio previsto es reducción a prorrata entre las posiciones activas.

## 6.4 Tipos de posición

El sistema utiliza **posiciones individualizadas**, cada una con su propia cantidad, fecha de entrada, antigüedad, recompensas acumuladas, tipo, estado y disponibilidad de retirada. Cada aportación constituye una posición independiente con su propio cómputo de antigüedad.

|  |  |  |
| --- | --- | --- |
|  | **Staking libre** | **Staking de Méritos** |
| Aplicable a | ZREC Genesis de Preventa, ZREC energético, ZREC de mercado | ZREC Genesis procedente de Méritos |
| Entrada | Voluntaria | Automática durante el Claim |
| Bloqueo | No | 6 meses iniciales |
| Disponibilidad | Reglas generales de retirada | 2 % mensual durante 50 meses |
| Recompensas | Modelo general | Modelo general, idéntico |

**Ambos tipos utilizan el mismo ZREC.** No se crean versiones distintas del token para representar la procedencia (Sección 3.2). La diferencia reside **exclusivamente en las reglas de retirada del principal**.

## 6.5 Recompensas de las posiciones de Méritos

Los ZREC de Méritos **generan recompensas desde el inicio de su posición, incluido el periodo de bloqueo de seis meses**, conforme al mismo modelo económico general. No existe fórmula diferenciada.

Los ZDAO generados como recompensa:

* proceden exclusivamente de la reserva de staking, no del Programa de Méritos;
* **no heredan el bloqueo** del ZREC que los generó;
* son libres una vez distribuidos;
* no tienen Derecho Genesis.

## 6.6 Retirada

Dos modalidades para las posiciones libres y para la parte disponible de las posiciones de Méritos:

|  |  |  |
| --- | --- | --- |
| **Modalidad** | **Procedimiento** | **Coste** |
| Solicitada | Solicitud, periodo de espera y ventana de ejecución | Sin comisión |
| Inmediata | Ejecución directa | Comisión sobre el importe retirado |

Los plazos y la comisión son parámetros configurables por gobernanza dentro de los límites de la Sección 11. La comisión de retirada inmediata se destina al mecanismo previsto por el protocolo.

Las posiciones de Méritos no pueden retirar principal por ninguna de las dos vías mientras no haya alcanzado su disponibilidad conforme al calendario de 5.5.

# 7. ZERO DEX

## 7.1 Naturaleza

**ZERO DEX es el Dashboard e interfaz descentralizada principal del ecosistema**, no únicamente un exchange ni un conjunto de pools de liquidez. Constituye la capa mediante la cual los usuarios interactúan con la infraestructura on-chain del protocolo.

Debe distinguirse expresamente:

ZERO DEX = Dashboard e interfaz del ecosistema
Contratos, pools, routers, protocolos de liquidez = capa tecnológica subyacente

ZERO DEX puede interactuar con infraestructura propia, externa o agregada.

## 7.2 Funcionalidades

Se integran progresivamente: intercambio de ZDAO, ZREC y activos compatibles; acceso a pools de liquidez; staking de ZREC y gestión de posiciones; reclamación de recompensas; interacción con la gobernanza; información de producción energética y Proof of Energy; visualización de ZREC y de NFTs de Certificación Energética; información de productores e instalaciones; métricas del protocolo e información de Tesorería; funcionalidades DeFi y futuras aplicaciones construidas sobre ZERO Energy.

**Restricción.** El mercado opera exclusivamente al contado. No se ofrece negociación con apalancamiento, margen ni instrumentos derivados sobre los activos del ecosistema. Incorporar cualquier funcionalidad de esa naturaleza requiere decisión expresa de la DAO y revisión previa de la arquitectura.

## 7.3 Comisiones

Las operaciones generan una comisión sobre el volumen negociado, percibida en el activo de entrada y consolidada periódicamente.

|  |  |  |
| --- | --- | --- |
| **Destino** | **Inicial** | **Rango configurable** |
| Tesorería DAO | 40 % | 20 % – 50 % |
| Proveedores de liquidez | 30 % | 20 % – 50 % |
| Adquisición y quema de ZDAO | 20 % | 0 % – 30 % |
| Desarrollo e infraestructura | 10 % | 0 % – 20 % |

La suma debe ser en todo momento igual al 100 %; el contrato rechaza cualquier configuración que no lo cumpla. La fracción destinada a quema se acumula y ejecuta conforme al Burn Execution Threshold de la Sección 2.6.

# 8. GOBERNANZA Y DESCENTRALIZACIÓN PROGRESIVA

## 8.1 DAO y Comité del Protocolo

La **DAO es la máxima autoridad de gobernanza**. El **Comité del Protocolo** es un órgano operativo con competencias delegadas, **no una autoridad superior a la DAO**.

**Mandato inicial del Comité: 24 meses.** Un año resulta insuficiente para gestionar infraestructura energética; periodos de cuatro o cinco años concentran demasiado poder. Veinticuatro meses permiten trabajar y rendir cuentas.

El Comité está sujeto a renovación, revocación anticipada mediante los mecanismos de gobernanza, sustitución por causa justificada, obligaciones de transparencia, rendición de cuentas, límites de competencias y supervisión por la DAO.

**El Comité no puede** acuñar tokens, modificar parámetros fuera de rango ni disponer de la Tesorería DAO sin aprobación previa.

## 8.2 Permisos administrativos

El protocolo reconoce exclusivamente los siguientes permisos. No existe ningún otro sobre los contratos principales.

|  |  |  |  |
| --- | --- | --- | --- |
| **Permiso** | **Titular** | **Alcance** | **Destino** |
| Acuñación de recompensas | Contrato de Staking | Reserva de 500M | Permanente |
| Acuñación por Claim | Contrato de Claim | Asignaciones de la raíz publicada | Se extingue al cerrar la ventana |
| Acuñación de ZREC Genesis | Contrato de Claim | Derechos ejercidos | Se extingue al cerrar la ventana |
| Acuñación energética de ZREC | Contrato de Emisión Energética | Lotes finalizados | Permanente |
| Operación del oráculo | ZERO Energy | Firma de validaciones | Se abre a terceros (Hito 3) |
| Acuñación estratégica | Gobernanza | Emisiones Estratégicas | Sujeta al invariante de supply |
| Configuración de parámetros | Gobernanza | Rangos de la Sección 11 | Permanente |
| Actualización de contratos | Gobernanza | Contratos principales | Se renuncia (Hito 5) |
| Pausa de emergencia | Comité del Protocolo | Suspensión temporal | Se renuncia (Hito 5) |

**Ningún permiso de acuñación recae sobre una dirección controlada por personas.** Los permisos son titularidad de contratos, cuyo comportamiento es verificable.

## 8.3 Timelock

Toda ejecución derivada del contrato de gobernanza está sujeta a un periodo de espera obligatorio, durante el cual la transacción pendiente es públicamente consultable.

|  |  |
| --- | --- |
| **Operación** | **Plazo mínimo** |
| Parámetros dentro de rango | 48 horas |
| Operaciones de la Tesorería DAO | 72 horas |
| Emisiones Estratégicas | 7 días |
| Modificación del conjunto de oráculos | 7 días |
| Actualización de contratos principales | 14 días |

Su función es garantizar que ninguna decisión que afecte a los activos o a la lógica del protocolo pueda ejecutarse sin que los participantes dispongan de tiempo para conocerla y reaccionar.

## 8.4 Pausa de emergencia

El Comité puede suspender temporalmente las operaciones ante una incidencia de seguridad, con límites estrictos: **no permite acuñar, transferir ni disponer de activo alguno**; duración máxima de 72 horas con levantamiento automático; prórroga sujeta a aprobación de la DAO; justificación pública en 24 horas.

## 8.5 Actualizabilidad

Los contratos principales se despliegan con capacidad de actualización durante las fases iniciales, para permitir corregir defectos detectados en auditoría o producción. Toda actualización requiere aprobación de la DAO, timelock de 14 días, auditoría previa y publicación del código propuesto durante todo el periodo de espera.

**Ninguna actualización puede modificar un invariante de la Sección 11.** Los invariantes se implementan de modo que su verificación no dependa de código actualizable. La capacidad de actualización se extingue en el Hito 5.

## 8.6 Descentralización progresiva por hitos

**La descentralización no se asocia a fechas de calendario.** Un protocolo no se descentraliza porque llegue una fecha determinada. Cada hito se define mediante **condiciones objetivas y verificables**, y determinadas condiciones deben cumplirse simultáneamente, de modo que nadie pueda acelerar artificialmente el proceso para obtener poder o desbloquear una capacidad.

Variables que podrán considerarse conjuntamente: número de productores independientes, energía certificada, diversidad de fuentes de datos, número y distribución de validadores, madurez de los Energy Oracles, TVL, madurez y auditorías de los contratos, actividad y distribución de la participación en gobernanza y seguridad operativa.

**Los criterios concretos permanecen abiertos y se definirán mediante gobernanza** conforme evolucione el protocolo.

Cada hito comporta una **renuncia irreversible** a una capacidad de control: ejecutada, no puede revertirse por ningún mecanismo, incluida la actualización de contratos.

|  |  |  |
| --- | --- | --- |
| **Hito** | **Renuncia** | **Verificación** |
| 1. Cierre de la ventana fundacional | El Claim y el Derecho Genesis quedan deshabilitados | Las funciones revierten de forma incondicional |
| 2. Consolidación de la emisión | Se extinguen los permisos de acuñación de ZDAO salvo staking y estratégicas | Lista pública de direcciones autorizadas |
| 3. Independencia de la validación | ZERO Energy deja de controlar la validación energética | Relación pública de oráculos y su independencia acreditada |
| 4. Madurez del ecosistema | Criterios definidos por gobernanza | Métricas publicadas |
| 5. Inmutabilidad | Se renuncia a actualizar contratos y a la pausa de emergencia | Dirección administradora sin capacidad de acción |
| 6. Gobernanza plena | El Comité pierde toda facultad de iniciativa | Las propuestas solo se originan en el contrato de gobernanza |

> Hasta la ejecución del **Hito 3**, la validación energética depende de infraestructura operada por ZERO Energy. El documento lo declara expresamente en la Sección 4.3 en lugar de presentar como descentralizado un mecanismo que todavía no lo es.

## 8.7 Publicación del estado

El protocolo publica de forma permanente y consultable: el hito alcanzado y la transacción de cada renuncia ejecutada; la relación de direcciones con permiso de acuñación vigente; las transacciones pendientes en el timelock; la composición del Comité y del conjunto de oráculos, con indicación de su operador; `CumulativeMinted`, `totalSupply` y capacidad de emisión restante; el estado de la reserva de staking; el número de instalaciones registradas y la energía certificada acumulada; y el porcentaje de Emisión Genesis compensado.

> El grado de descentralización de un protocolo no se declara: se demuestra mediante la relación de capacidades a las que se ha renunciado de forma verificable.

# 9. ARQUITECTURA DE CONTRATOS

Infraestructura modular sobre Base, con responsabilidades desacopladas.

|  |  |
| --- | --- |
| **Contrato** | **Responsabilidades principales** |
| ZERO DAO (ZDAO) | ERC-20, 8 decimales. Control de CumulativeMinted y MAX\_SUPPLY como invariante. Acuñación restringida a direcciones autorizadas. Quema sin regeneración de capacidad |
| ZERO REC (ZREC) | ERC-20, 3 decimales, activo único fungible. Acuñación desde Claim y desde Emisión Energética. Contabilidad diferenciada Genesis / Energético |
| Claim | Verificación de pruebas de Merkle contra la raíz publicada. Resolución del Derecho Genesis con consumo único. Entrega de ZDAO o acuñación de ZREC Genesis. Creación automática de posiciones de Méritos |
| Emisión Energética | Verificación de la validación del oráculo. Registro de unicidad por identificador de lote. Estado del lote y ventana de provisionalidad. Acuñación y distribución |
| Staking | Posiciones individualizadas. Modelo dinámico de recompensas. Reglas de retirada diferenciadas por tipo de posición. Único titular del permiso sobre la reserva de 500M |
| Certificación Energética | Quema de ZREC destinados a compensación. Emisión del NFT con su finalidad. Intransferibilidad de los certificados de Compensación Genesis |
| Gobernanza | Propuestas y votaciones. Parámetros dentro de rango. Nombramiento y revocación del Comité. Ejecución sujeta a timelock |
| Tesorería | Custodia multifirma. Recepción de la fracción de ZREC energético. Ejecución exclusiva de operaciones aprobadas |
| Energy Oracles | Registro de oráculos autorizados. Autorizaciones firmadas con marca temporal y caducidad. Registro auditable de validaciones |

**Capa de adquisición de datos.** Infraestructura fuera de la cadena (Sección 4.4). No forma parte del conjunto de contratos y **no ostenta permiso alguno sobre ellos**. Su sustitución o indisponibilidad no afecta a la integridad de los activos ya emitidos.

La arquitectura debe permitir incorporar nuevos contratos, servicios e integraciones sin reemplazar los contratos principales.

# 10. SEGURIDAD

## 10.1 Auditoría

Todos los contratos deben someterse a auditoría independiente antes de producción, verificando expresamente los invariantes de la Sección 11. Toda modificación relevante requiere auditoría adicional previa al despliegue. La adaptación del modelo de staking (Sección 6.1) requiere auditoría propia: no hereda la del contrato de referencia.

## 10.2 Vectores considerados

|  |  |  |
| --- | --- | --- |
| **Vector** | **Medida** | **Sección** |
| Ejercicio repetido del Derecho Genesis tras vender ZDAO | El derecho reside en la asignación off-chain, con consumo único | 5.3 |
| Duplicación del derecho usando varias wallets | Estado terminal por asignación | 5.3 |
| Regeneración de capacidad de emisión mediante quema | CumulativeMinted no decrece | 2.2 |
| Elusión del calendario de Méritos | El bloqueo vive en las reglas de retirada de la posición | 5.5 y 6.4 |
| Doble acuñación por reintento de un lote | Identificador de lote en registro de unicidad | 4.6 |
| Reutilización de producción ya procesada | Estado FINALIZED irreversible | 4.6 |
| Doble contabilización frente a certificados externos | Verificación de exclusividad previa | 4.3 |
| Doble uso de un certificado | Finalidad consignada e intransferibilidad | 3.6 |
| Declaración inflada de producción | Comprobaciones de plausibilidad | 4.5 |
| Instalación inexistente o sobredimensionada | Triple verificación de potencia nominal | 4.5 |
| Autorización discrecional de acuñación | Árbol de Merkle publicado antes del Claim | 5.6 |
| Desviación acumulada por redondeo | Arrastre obligatorio de residuos | 4.6 |
| Agotamiento acelerado de la reserva de staking | Curva máxima decreciente como techo | 6.3 |

## 10.3 Tesorería y seguridad operativa

La Tesorería DAO opera bajo esquema multifirma; ningún miembro del Comité puede disponer individualmente de los fondos; toda operación cumple las reglas de autorización y el timelock; todas quedan registradas on-chain.

Medidas operativas: separación de responsabilidades, control de permisos, registro permanente de operaciones, validación criptográfica de acciones críticas, protección frente a ataques de repetición y dobles ejecuciones, y monitorización continua.

## 10.4 Mejora continua

La gobernanza puede incorporar nuevas auditorías, actualizaciones de infraestructura, mejoras en la gestión de permisos, nuevos mecanismos criptográficos y procedimientos de respuesta ante incidentes.

**El protocolo no introduce actualmente requisitos de colateral para los oráculos.** Un depósito en garantía no demuestra que un dato energético sea verdadero: solo introduce una penalización económica si alguien engaña. Para ZERO Energy resulta más determinante la cadena de verificación técnica —instalación, medidor, medición, periodo, validación, unicidad— descrita en la Sección 4. La gobernanza podrá incorporar en el futuro mecanismos de garantía económica o penalización para determinados validadores, sin convertirlos ahora en requisito del protocolo.

# 11. INVARIANTES Y PARÁMETROS

Esta sección sustituye a cualquier enumeración de principios dispersa por el documento. Recoge en un único lugar qué no puede cambiar y qué sí, con su rango.

## 11.1 Invariantes

No pueden modificarse por ningún mecanismo de gobernanza ni por actualización de contratos. Se implementan de modo que su verificación no dependa de código actualizable.

|  |  |  |
| --- | --- | --- |
|  | **Invariante** | **Sección** |
| I-1 | `CumulativeMinted` de ZDAO nunca supera 1.000.000.000 | 2.1 |
| I-2 | La quema no reduce `CumulativeMinted` ni regenera capacidad de emisión | 2.2 |
| I-3 | 500.000.000 ZDAO quedan reservados en exclusiva al Sistema Oficial de Staking y solo el contrato de staking puede acuñarlos | 2.3 y 6.3 |
| I-4 | El Derecho Genesis se consume una única vez y no reside en el token | 5.3 |
| I-5 | Cerrada la ventana fundacional, el Derecho Genesis queda deshabilitado de forma irreversible | 5.2 |
| I-6 | ZREC es un único activo fungible; las restricciones viven en las posiciones, nunca en el token | 3.2 |
| I-7 | Ninguna unidad de energía puede sustentar más de una emisión ni más de un certificado | 4.2 |
| I-8 | Un lote en estado FINALIZED no puede reutilizarse | 4.6 |
| I-9 | Ninguna acuñación puede autorizarse por un criterio no reproducible | 4.5 |
| I-10 | La DAO es la máxima autoridad; el Comité solo ejerce competencias delegadas | 8.1 |

## 11.2 Parámetros configurables

Modificables por gobernanza dentro de los rangos indicados y sujetos al timelock de la Sección 8.3.

|  |  |  |  |
| --- | --- | --- | --- |
| **Parámetro** | **Valor inicial** | **Rango** | **Sección** |
| Reparto de la Emisión Energética | 50 % productor | 30 % – 70 % | 3.3 |
| Cierre de lote — tiempo | 24 horas | configurable | 4.6 |
| Cierre de lote — energía | 1 MWh | configurable | 4.6 |
| Ventana de provisionalidad | 72 horas | configurable a la baja | 4.6 |
| Burn Execution Threshold | 10.000 ZDAO | configurable | 2.6 |
| Comisión del ZERO DEX | sobre volumen | 0,05 % – 1,00 % | 7.3 |
| Reparto de comisiones | 40/30/20/10 | ver tabla 7.3 | 7.3 |
| Mandato del Comité | 24 meses | renovable | 8.1 |
| Parámetros de la curva de recompensas | pendiente de implementación | con límites de seguridad | 6.2 |
| Plazos y comisión de retirada del staking | pendiente de implementación | con límites de seguridad | 6.6 |

## 11.3 Decisiones pendientes

Se declaran expresamente en lugar de sustituirse por suposiciones.

|  |  |  |
| --- | --- | --- |
| **Cuestión** | **Bloquea** | **Sección** |
| Tratamiento de las asignaciones no reclamadas al cerrar la ventana | Cierre del Hito 1 | 5.2 |
| Magnitud de referencia del componente global del staking | Implementación del staking | 6.2 |
| Mecanismo de ajuste cuando el devengo supere el techo del periodo | Implementación del staking | 6.3 |
| Valores definitivos de plazos y comisión de retirada | Implementación del staking | 6.6 |
| Criterios objetivos de los Hitos 4, 5 y 6 | Descentralización | 8.6 |
| Requisitos definitivos de un oráculo externo | Hito 3 | 4.3 |

# 12. DISPOSICIONES GENERALES

## 12.1 Finalidad e interpretación

Este documento define la arquitectura económica, técnica y funcional del protocolo y constituye la especificación de referencia para su desarrollo y evolución.

Durante el desarrollo pueden realizarse adaptaciones técnicas siempre que no alteren los invariantes de la Sección 11.1, no comprometan la separación funcional entre ZDAO y ZREC, no reduzcan las garantías de seguridad y sean compatibles con las decisiones aprobadas por la gobernanza.

**Ninguna adaptación técnica puede afectar a un invariante.** Toda adaptación que modifique el comportamiento descrito requiere aprobación expresa de la gobernanza y la correspondiente actualización de esta especificación.

## 12.2 Evolución

La incorporación de nuevas funcionalidades, mecanismos económicos, integraciones energéticas, mercados o servicios se realiza mediante propuestas aprobadas por la gobernanza. La arquitectura modular permite dicha evolución sin comprometer la compatibilidad con la infraestructura existente.

## 12.3 Transparencia

Toda la actividad del protocolo se desarrolla conforme a los principios de transparencia, trazabilidad, seguridad, descentralización, sostenibilidad, escalabilidad y auditabilidad. Las operaciones ejecutadas mediante los contratos deben poder verificarse públicamente sobre Base.

## 12.4 Visión

ZERO Energy construye una infraestructura descentralizada que conecta financiación, producción energética renovable, tokenización de activos, incentivos económicos, gobernanza y certificación ambiental dentro de un único ecosistema, sobre la red Base.

# ANEXO A — REFERENCIA TÉCNICA

Implementación de referencia de los dos mecanismos con mayor impacto económico. **No es código de producción**: falta el control de accesos, la integración completa y las verificaciones de seguridad, y debe someterse a auditoría independiente.

**A.1 Guardián del suministro de ZDAO** (Secciones 2.1 y 2.2)

uint256 constant MAX\_SUPPLY = 1\_000\_000\_000e8; // ZDAO usa 8 decimales
uint256 constant STAKING\_RESERVE = 500\_000\_000e8;

uint256 public cumulativeMinted; // solo crece: la quema NO lo reduce
uint256 public stakingMinted; // acunado por el contrato de staking

/// @notice Capacidad de emision restante bajo el techo absoluto.
function remainingMintCapacity() public view returns (uint256) {
 return MAX\_SUPPLY - cumulativeMinted;
}

/// @notice Reserva de staking pendiente de acunar.
function pendingStakingReserve() public view returns (uint256) {
 return STAKING\_RESERVE - stakingMinted;
}

/// @dev Invariantes I-1 e I-3. Se aplica en TODA acunacion.
function \_mint(address to, uint256 amount) internal override {
 cumulativeMinted += amount; // nunca decrece
 require(cumulativeMinted <= MAX\_SUPPLY, "max supply exceeded");
 super.\_mint(to, amount);
}

/// @dev La quema destruye tokens pero NO devuelve capacidad de emision.
function burn(uint256 amount) external {
 \_burn(msg.sender, amount); // totalSupply baja, cumulativeMinted NO
}

/// @dev Solo el contrato de staking acuna contra la reserva.
function mintStakingReward(address to, uint256 amount) external onlyStaking {
 stakingMinted += amount;
 require(stakingMinted <= STAKING\_RESERVE, "staking reserve exceeded");
 \_mint(to, amount);
}

Obsérvese que `burn()` no toca `cumulativeMinted`: esa única omisión es lo que convierte el techo en un límite real.

**A.2 Devengo de recompensas del staking** (Sección 6.2)

struct Position {
 uint256 amount; // ZREC depositado (3 decimales)
 uint256 startDate; // inicio del computo de antiguedad
 uint256 released; // solo posiciones de Meritos
 PositionType kind; // FREE | MERITS\_LOCKED
}

/// @notice Curva sigmoide del componente temporal.
/// y = a·(t-b) / sqrt((t-b)^2 + c)
function personalRate(uint256 elapsed) public view returns (uint256) {
 int256 k = int256(elapsed) - sigmoidB;
 if (k < 0) return 0;
 uint256 uk = uint256(k);
 return sigmoidA \* uk / sqrt(uk \* uk + sigmoidC);
}

/// @notice Componente global. La referencia NO es el max supply de ZREC:
/// ZREC no tiene techo. Ver decision pendiente en 6.2.
function globalRate() public view returns (uint256) {
 if (totalStaked >= stakingReference) return maxGlobalRate;
 return maxGlobalRate \* totalStaked / stakingReference;
}

/// @notice Devengo. Solo se acuna lo efectivamente devengado:
/// el excedente hasta el techo NO se emite.
function accrued(Position storage p) internal view returns (uint256) {
 uint256 elapsed = block.timestamp - p.startDate;
 uint256 rate = personalRate(elapsed) + globalRate();
 uint256 raw = p.amount \* rate \* elapsed / (YEAR \* SCALE);
 return \_applyPeriodCap(raw); // techo decreciente de la Seccion 6.3
}

Notas de implementación:

1. Los invariantes se verifican con comprobaciones que revierten la transacción, no mediante configuración administrativa.
2. `cumulativeMinted` y `stakingMinted` solo se incrementan. Ninguna operación de quema los modifica.
3. Cada aportación crea una posición independiente con su propio cómputo de antigüedad.
4. La normalización entre los 3 decimales de ZREC y los 8 de ZDAO debe resolverse con aritmética entera y reglas explícitas de redondeo.
5. `stakingReference` y `\_applyPeriodCap` corresponden a las decisiones pendientes de la Sección 11.3.

# ANEXO B — REGISTRO DE CAMBIOS

Cambios de la versión 2.0 respecto a la 1.2, para revisión individual por la gobernanza.

**Cambios de arquitectura**

|  |  |  |
| --- | --- | --- |
|  | **Cambio** | **Sección** |
| 1 | Claim y Genesis se unifican en una ventana única de 6 meses. Desaparece el modelo Claim → ZDAO → conversión posterior | 5.2 |
| 2 | Se elimina la operación de acuñar y quemar. Los ZDAO del Derecho Genesis no llegan a existir on-chain | 3.3 y 5.4 |
| 3 | El Derecho Genesis reside en la asignación off-chain, no en el token, y se consume una única vez | 5.3 |
| 4 | Se elimina el contrato de Vesting. El bloqueo de Méritos vive en las reglas de retirada de la posición de staking | 5.5 |
| 5 | Méritos pasa a resolverse directamente en ZREC Genesis con staking automático: 6 meses de bloqueo y 2 % mensual durante 50 meses | 5.5 |
| 6 | Nuevo modelo de staking dinámico (componente temporal + componente global) tomando EasyStaking como referencia conceptual | 6 |
| 7 | ZERO DEX pasa a definirse como Dashboard e interfaz principal del ecosistema | 7 |
| 8 | Se incorporan DePIN y Proof of Energy como marco arquitectónico, con delimitación expresa frente al consenso de Base | 1.1 y 1.2 |
| 9 | Nomenclatura oficial con tickers ZDAO y ZREC | Nota preliminar |

**Cambios económicos**

|  |  |  |
| --- | --- | --- |
|  | **Cambio** | **Sección** |
| 10 | El techo vuelve a operar sobre `CumulativeMinted`. La quema NO regenera capacidad de emisión. Revierte la formulación sobre circulante de la v1.2 | 2.1 y 2.2 |
| 11 | Se sustituye el halving por consumo por la curva anual decreciente como techo del modelo dinámico | 6.3 |
| 12 | Emisiones Estratégicas se fija en 200M (liquidez 150M + contingencias 50M, indicativo) | 2.3 |
| 13 | Tabla de Preventa completa: cinco tramos con inversión mínima, disponibles en paralelo | 5.1 |
| 14 | Decimales oficiales: ZDAO 8, ZREC 3, con unidad mínima de 1 kWh | 2 y 3.1 |
| 15 | Burn Execution Threshold inicial de 10.000 ZDAO, gobernable | 2.6 |

**Cambios en la capa energética**

|  |  |  |
| --- | --- | --- |
|  | **Cambio** | **Sección** |
| 16 | Se elimina la restricción sobre energía vertida a red. Se certifica generación, con independencia del destino | 4.1 |
| 17 | Se elimina el requisito de colateral para los oráculos | 10.4 |
| 18 | Se omite el límite inicial de emisión por productor | 3.3 |
| 19 | Cierre híbrido de lotes: 24 horas o 1 MWh | 4.6 |
| 20 | Ventana de provisionalidad de 72 horas y estado FINALIZED | 4.6 |
| 21 | Triple verificación de la potencia nominal de las instalaciones | 4.5 |

**Cambios en gobernanza**

|  |  |  |
| --- | --- | --- |
|  | **Cambio** | **Sección** |
| 22 | Mandato del Comité del Protocolo: 24 meses | 8.1 |
| 23 | Los hitos de descentralización se definen por métricas objetivas, no por fechas, y permanecen abiertos | 8.6 |

**Cambios de estructura y extensión**

El documento pasa de 13 a 12 secciones y reduce sustancialmente su extensión sin perder contenido normativo. Se eliminan las secciones que repetían material ya expuesto: el flujo completo del ecosistema (ahora un único diagrama en 1.5), la enumeración de principios fundamentales (sustituida por la tabla de invariantes de 11.1) y los apartados de objetivos de cada componente. El Sistema de Claim se integra en la fase fundacional y la capa de adquisición de datos en la sección de Proof of Energy. Cada concepto se define una sola vez y el resto del documento lo referencia.

**FIN DEL DOCUMENTO**