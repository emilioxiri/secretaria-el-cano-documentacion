# Histórico, directiva y recompensas

[← Volver al índice](index.md)

## 1. Propósito

Conservar la trayectoria de cada fallero en la falla — qué años perteneció, con
qué cargo y comisión, y qué recompensas recibió — y gestionar la composición de
la directiva de cada ejercicio.

## 2. Conceptos

- **Registro de histórico**: anotación de un fallero para un año concreto con
  su cargo, su comisión y, si procede, su recompensa.
- **Cargo**: responsabilidad del fallero ese año (Presidente, Secretario,
  Tesorero, delegados…). Quien no tiene cargo de directiva figura como
  **VOCAL**.
- **Directiva**: conjunto de falleros con cargo de directiva (distinto de
  Vocal) en un ejercicio.
- **Recompensa**: distinción oficial (bunyols y distintius) que se anota en el
  histórico del año en que se concede.

## 3. Funcionalidades

Desde **Ver Falleros** (seleccionando un fallero):

- **Histórico**: tabla con Año, Cargo, Comisión, Recompensa y Fecha Baja de
  cada registro del fallero, del año más reciente al más antiguo.
- **Recompensas**: diálogo "Asignar recompensa" — elegir el **Año** (entre los
  años con histórico del fallero) y la **Recompensa** del catálogo, y pulsar
  **Guardar recompensa**.

Pantalla **Administración → pestaña Directiva**:

- Selector **Ejercicio (año de inicio)** con todos los años registrados.
- Indicador **Miembros en la directiva** y tabla Cargo / Fallero, ordenada por
  el rango del cargo (Presidente primero).
- **Asignar cargo**: elegir un fallero activo y un cargo de directiva para el
  ejercicio actual.
- **Editar / quitar cargo**: sobre el ejercicio seleccionado (no solo el
  actual), elegir un miembro y **Cambiar cargo** o **Quitar cargo**.

## 4. Reglas de negocio

- **RN-01 — Un registro por fallero, año y cargo.** No pueden existir dos
  registros idénticos de fallero + año + cargo; el duplicado se rechaza.
- **RN-02 — Alta con histórico.** Al dar de alta un fallero se crea su primer
  registro de histórico como VOCAL del ejercicio actual (ver
  [Censo](censo.md)).
- **RN-03 — Un cargo de directiva por fallero y año.** Al asignar un cargo a
  un fallero que ya tiene cargo de directiva ese año, no se acumulan: el cargo
  anterior se sustituye por el nuevo.
- **RN-04 — El cambio de cargo conserva el resto del registro.** Cambiar el
  cargo de un fallero para un año es una operación única que mantiene la
  comisión y la recompensa que tuviera ese registro.
- **RN-05 — La directiva de cualquier año es corregible.** Desde la pestaña
  Directiva se puede cambiar o quitar un cargo de cualquier ejercicio
  registrado, no solo del actual.
- **RN-06 — Solo cargos del catálogo de directiva.** Al asignar o cambiar un
  cargo se ofrecen únicamente los cargos marcados como de directiva en el
  catálogo (VOCAL no lo es y no aparece en la directiva; ver
  [Catálogos](catalogos.md)).
- **RN-07 — La recompensa se asigna por año.** Al asignar una recompensa a un
  fallero para un año, queda anotada en todos sus registros de ese año y
  sustituye a la que hubiera.
- **RN-08 — Asignación de cargos solo a falleros activos.** El diálogo
  "Asignar cargo a fallero" solo ofrece falleros activos.

## 5. Casos de uso

### CU-01 — Consultar la trayectoria de un fallero

- **Actor**: cualquier usuario de la aplicación.
- **Pasos**: **Ver Falleros** → seleccionar fallero → **Histórico**.
- **Resultado**: tabla con todos sus años, cargos, comisiones y recompensas.
- **Errores / alternativas**: si no tiene registros, se informa "No hay
  registros históricos para este fallero".

### CU-02 — Asignar una recompensa

- **Actor**: secretario/a.
- **Precondición**: el fallero tiene histórico y el catálogo de recompensas no
  está vacío.
- **Pasos**: **Ver Falleros** → seleccionar fallero → **Recompensas** → elegir
  **Año** y **Recompensa** → **Guardar recompensa**.
- **Resultado**: mensaje "Recompensa asignada a … registro(s) del año …"
  (RN-07).
- **Errores / alternativas**: sin histórico o sin catálogo, la pantalla lo
  indica y no permite asignar.

### CU-03 — Componer la directiva del ejercicio

- **Actor**: secretario/a.
- **Precondición**: hay falleros activos y el catálogo de cargos de directiva
  está disponible.
- **Pasos**: **Administración** → pestaña **Directiva** → **Asignar cargo** →
  elegir **Fallero** y **Cargo** → **Guardar cargo**.
- **Resultado**: mensaje "Cargo asignado correctamente a … para el ejercicio
  …"; el fallero aparece en la tabla de la directiva del año actual.
- **Errores / alternativas**: si el fallero ya tenía un cargo ese año, se
  sustituye por el nuevo (RN-03).

### CU-04 — Corregir la directiva de un año

- **Actor**: secretario/a.
- **Precondición**: el ejercicio tiene miembros de directiva registrados.
- **Pasos**: pestaña **Directiva** → seleccionar el **Ejercicio (año de
  inicio)** → **Editar / quitar cargo** → elegir el miembro → acción
  **Cambiar cargo** (con el nuevo cargo) o **Quitar cargo** → **Aplicar
  cambio**.
- **Resultado**: mensaje de confirmación del cambio o retirada; la tabla se
  actualiza.
- **Errores / alternativas**: cambiar a un cargo que el fallero ya tiene ese
  año se rechaza como duplicado (RN-01).

## 6. Relación con otros dominios

- [Censo](censo.md) — el histórico pertenece a un fallero del censo; el alta
  genera el primer registro.
- [Ejercicio](ejercicio.md) — la generación del nuevo año fallero copia el
  último registro de cada fallero activo al año siguiente.
- [Catálogos](catalogos.md) — los cargos y recompensas disponibles provienen de
  los catálogos de referencia.
