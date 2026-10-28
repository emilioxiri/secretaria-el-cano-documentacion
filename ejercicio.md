# Ejercicio — generación del nuevo año fallero

[← Volver al índice](index.md)

## 1. Propósito

Automatizar el paso de un año fallero al siguiente: prolongar el histórico de
todos los falleros activos, promocionar las categorías de cuota EPIM según la
edad y dejar constancia de que el ejercicio ya se ha generado, todo con un
informe descargable de los cambios aplicados.

## 2. Conceptos

- **Ejercicio (año fallero)**: periodo que va del **20 de marzo al 19 de
  marzo** del año siguiente. Se identifica por los dos años que abarca, por
  ejemplo "2026-2027". Una fecha de junio de 2026 y una de febrero de 2027
  pertenecen ambas al ejercicio 2026-2027.
- **Promoción EPIM**: cambio de categoría de cuota al generar el ejercicio,
  según la edad del fallero a 19 de marzo (fecha de cierre del nuevo
  ejercicio).
- **Generación del ejercicio**: proceso que ejecuta la secretaría una vez al
  año desde la aplicación.

## 3. Funcionalidades

Pantalla **Administración → pestaña Ejercicio**:

- Indicador **Ejercicio actual** (calculado automáticamente por fecha).
- Botón **Nuevo año fallero**: abre el diálogo de confirmación "¿Se va a
  generar un nuevo ejercicio para el año fallero …, desea continuar?" con las
  opciones **Sí, generar ejercicio** y **Cancelar**.
- Al terminar, ventana **Proceso completado** con el número de cambios de
  cuota/comisión y de registros de histórico creados, el aviso de errores si
  los hubo, y el botón **Descargar cambios (Excel)** con el detalle de cada
  cambio (cuota y comisión anterior y nueva por fallero).

## 4. Reglas de negocio

- **RN-01 — Calendario del año fallero.** El ejercicio empieza el 20 de marzo
  y termina el 19 de marzo del año siguiente. La aplicación determina el
  ejercicio actual a partir de la fecha del día.
- **RN-02 — La generación solo puede ejecutarse una vez por ejercicio.** Si el
  ejercicio ya se generó, el proceso se aborta sin efecto alguno y se muestra
  el aviso "El ejercicio … ya se ha generado. El proceso se ha abortado".
- **RN-03 — Prolongación del histórico.** Para cada fallero **activo**, se
  copia su registro de histórico más reciente al **año del ejercicio que se
  genera** (su año de inicio: 2026 para 2026-2027), manteniendo el cargo y la
  comisión y dejando la recompensa vacía (las recompensas se ganan, no se
  arrastran). Si ese año tiene varios registros, se copia el de directiva.
  Los falleros que ya tienen registro para ese año (por ejemplo, dados de alta
  durante el propio ejercicio) se dejan como están, para que nadie quede un
  año por delante del resto.
- **RN-04 — Promoción EPIM.** Con la edad calculada a 19 de marzo del cierre
  del nuevo ejercicio, se aplica un único paso de promoción:
  - **E → P**: todo Exento pasa a Pitufo, sin condición de edad.
  - **P → I**: si el fallero tiene más de 4 años.
  - **I → M**: si tiene más de 15 años; en este caso su comisión también pasa
    de **Infantil a Mayor**, tanto en su ficha como en el nuevo registro de
    histórico.
  - **M**: no cambia.
- **RN-05 — El proceso no se detiene por errores individuales.** Si un fallero
  concreto da error, se anota y el proceso continúa con el resto; al final se
  informa del número de errores.
- **RN-06 — Falleros sin histórico.** Los falleros activos que no tienen
  ningún registro de histórico no reciben la copia (no hay nada que copiar) y
  quedan reflejados en el resultado del proceso.
- **RN-07 — Informe de cambios.** Todo cambio de cuota o comisión queda
  recogido en un Excel descargable para revisión manual (identificador,
  nombre, apellidos, DNI, cuota anterior y nueva, comisión anterior y nueva).

## 5. Casos de uso

### CU-01 — Generar el nuevo año fallero

- **Actor**: secretario/a (normalmente al empezar el ejercicio, tras el 20 de
  marzo).
- **Precondición**: el ejercicio actual no se ha generado todavía.
- **Pasos**: **Administración** → pestaña **Ejercicio** → **Nuevo año
  fallero** → **Sí, generar ejercicio** → esperar a que termine → **Descargar
  cambios (Excel)** → **Cerrar**.
- **Resultado**: cada fallero activo tiene un año más de histórico (RN-03),
  las cuotas EPIM y comisiones promocionan según la edad (RN-04) y el
  ejercicio queda marcado como generado (RN-02).
- **Errores / alternativas**:
  - Ejercicio ya generado → aviso y proceso abortado sin cambios (RN-02).
  - Sin cambios de cuota/comisión → mensaje "El ejercicio … se ha generado.
    No hubo cambios de cuota o comisión".
  - Errores en falleros concretos → aviso "Se produjeron … errores durante el
    proceso" y el resto de falleros procesados con normalidad (RN-05).

### CU-02 — Revisar los cambios aplicados

- **Actor**: secretario/a.
- **Precondición**: la generación ha terminado con cambios.
- **Pasos**: en la ventana **Proceso completado**, pulsar **Descargar cambios
  (Excel)** y revisar la hoja de cambios.
- **Resultado**: constancia documental de todas las promociones de cuota y
  comisión del nuevo ejercicio (RN-07).

## 6. Relación con otros dominios

- [Censo](censo.md) — la promoción EPIM modifica la cuota y la comisión IM de
  la ficha del fallero; la edad de referencia es la misma que valida el
  formulario de alta.
- [Histórico y directiva](historico-directiva.md) — la generación añade un
  registro de histórico por fallero activo, conservando su cargo.
