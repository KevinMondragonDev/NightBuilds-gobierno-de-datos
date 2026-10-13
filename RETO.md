# 🛡️ Reto 6 — Gobierno de datos

**Repositorio:** `sideproject-gobierno-de-datos` · **Nivel:** intermedio · **Entorno:** PostgreSQL, Floci (catálogo de Glue) y Python · **Método y evaluación:** [`README.md`](docs/METODO.md)

## El caso

Un banco recibe un aviso: en tres meses habrá una auditoría de protección de datos personales. La directora de datos te entrega cuatro archivos y una frase: «No sé qué datos personales tenemos, quién los ve, ni si cumplimos. Dime qué está mal, arréglalo y déjalo montado para que no vuelva a pasar».

Gobierno de datos es eso: saber **qué datos hay, de quién son, quién puede usarlos, con qué calidad y durante cuánto tiempo**, y poder demostrarlo.

## Datos

`datos/` (todo sintético: ninguna persona, CURP, RFC, correo o teléfono es real):

| Archivo | Contenido |
|---|---|
| `clientes_pii.csv` | 1,000 clientes con datos personales, consentimiento de marketing, última actividad y si están en una campaña |
| `accesos.csv` | 5,000 consultas a la tabla de clientes en tres meses: quién, con qué rol, qué columnas, cuántas filas y con qué motivo |
| `solicitudes_arco.csv` | 60 solicitudes de acceso, rectificación, cancelación y oposición, con su fecha de respuesta |
| `tickets_soporte.csv` | 800 notas de soporte en texto libre |

Marco de referencia: la [Ley Federal de Protección de Datos Personales en Posesión de los Particulares](https://www.diputados.gob.mx/LeyesBiblio/pdf/LFPDPPP.pdf). No necesitas ser abogado: necesitas leer qué exige sobre consentimiento, finalidad, derechos ARCO y plazos, y traducirlo a comprobaciones.

Los archivos esconden **al menos ocho incumplimientos** de distinto tipo. No se listan aquí: encontrarlos es la primera mitad del reto.

## Lo que debe existir al terminar

- **Un inventario:** cada columna de cada archivo clasificada (identificador directo, dato personal, dato sensible, dato no personal), con su finalidad y su responsable.
- **Un informe de hallazgos:** cada incumplimiento con su evidencia (una consulta que lo demuestra), cuántos registros afecta y su gravedad.
- **Un catálogo consultable** (Glue en Floci o tablas en PostgreSQL) con esas clasificaciones como metadatos, no en una hoja de cálculo.
- **Controles que se aplican solos:** enmascaramiento por rol, acceso por columnas, detección de datos personales en texto libre y una regla de retención.
- **Comprobaciones automáticas** que fallen si reaparece cualquiera de los incumplimientos.
- **Un tablero en D3.js**, servido por una API, que la directora pueda enseñar al auditor: estado de cada control, solicitudes fuera de plazo y accesos anómalos.

## Restricciones

- Cada hallazgo se demuestra con una consulta reproducible, no con una afirmación.
- Ningún dato personal aparece en tus logs, en tus tests ni en el tablero.
- Los controles viven en la base de datos o en el pipeline, no en la buena voluntad de quien consulta.
- Tu repositorio no contiene datos personales reales. Estos son sintéticos y el README lo dice.

## Preguntas que tu diseño debe responder

- ¿Qué columnas, combinadas, identifican a una persona aunque ninguna lo haga por separado?
- ¿Enmascarar, seudonimizar o anonimizar? ¿Qué diferencia hay y cuál usaste para cada caso?
- ¿Quién necesita de verdad el domicilio o el ingreso para hacer su trabajo?
- Un cliente pide que borren sus datos. ¿En cuántos sitios están y cómo lo compruebas?
- ¿Cómo te enteras de un acceso anómalo: al día siguiente o en el momento?

## Defensa: lo que te van a preguntar

- Enséñanos al rol «analista» consultando la tabla de clientes. ¿Qué ve y qué no?
- Mete un correo electrónico en una nota de soporte delante de nosotros. ¿Qué pasa?
- Elige uno de tus hallazgos y defiende por qué es más grave que los demás.
- ¿Qué incumplimiento **no** se puede resolver con tecnología?

## Súbelo de nivel

- Linaje: de qué columnas de origen sale cada columna del tablero.
- Un contrato de datos para `clientes_pii.csv` que rechace una entrega que no lo cumpla.
- Aplica el mismo gobierno al data lake del reto 1 o al warehouse del reto 2.
