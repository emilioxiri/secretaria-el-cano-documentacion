# Mi familia

[← Volver al índice](index.md)

## 1. Propósito

Que los falleros mayores de edad creen y mantengan su propia familia sin pasar
por secretaría, con las mismas reglas de negocio de familias que aplica
Secretaría y sin dar acceso al resto del censo ni a otras familias.

Además, que el cabeza de familia pueda revisar y actualizar los datos de los
menores de su familia. Los niños pequeños no suelen tener DNI y, por tanto, no
pueden entrar al portal (ver [Acceso](acceso.md), RN-01): sin esto, sus datos
solo podría mantenerlos secretaría.

## 2. Conceptos

- **Familia**, **cabeza de familia** y **miembro**: los mismos conceptos que en
  Secretaría (ver
  [Familias](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/familias.md)).
- **Mayor de edad**: 18 años cumplidos a fecha de hoy según la fecha de
  nacimiento registrada, que el propio fallero puede corregir en
  [Mis datos](mis-datos.md).
- **Menor de la familia**: miembro de la familia, activo, con fecha de
  nacimiento registrada y que no ha cumplido 18 años a fecha de hoy. Un
  miembro sin fecha de nacimiento registrada no cuenta como menor.
- **Identificación de un miembro**: para añadir a alguien a la familia se
  indican su nº de fallero y su fecha de nacimiento; el portal nunca muestra
  listados del censo.

## 3. Funcionalidades

Pantalla **Mi familia** (menú lateral, solo mayores de edad). Lo que se ve
depende de la situación del fallero:

**Sin familia**

- Aviso "No perteneces a ninguna familia.".
- **➕ Crear familia**, si cumple los requisitos para ser cabeza. Si no, aviso
  "Para crear una familia necesitas estar activo y tener asignada la comisión
  MF. Contacta con secretaría.".

**Cabeza de familia**

- **Cabeza de familia** y tabla de **Miembros** (IdCenso, Nombre, Apellidos).
- **Cambiar cabeza de familia**: elegir el nuevo cabeza entre los miembros
  que pueden serlo → **Confirmar cambio de cabeza**.
- **Añadir miembro**: **Nº de fallero** y **Fecha de nacimiento** del fallero
  a añadir → **Añadir miembro**.
- **Quitar miembro**: elegir un miembro distinto del cabeza → **Quitar
  miembro**.
- **Datos de los menores**: elegir el menor en **Menor** y se muestra su
  ficha con el mismo formulario de [Mis datos](mis-datos.md) (**Guardar
  cambios** / **Descartar cambios**). Si la familia no tiene menores, aviso
  "No hay miembros menores de edad en tu familia.".
- **🗑 Eliminar familia**: solo activo cuando únicamente queda el cabeza.

**Miembro (no cabeza)**

- Cabeza de familia y tabla de miembros, en solo lectura, con el aviso "Solo
  el cabeza de familia puede modificar la familia.".

## 4. Reglas de negocio

- **RN-01 — Solo mayores de edad.** Solo los falleros activos y mayores de
  edad ven **Mi familia** y pueden hacer cualquier operación de familias. Sin
  fecha de nacimiento registrada no se considera mayor de edad. Aunque el
  fallero puede corregir su fecha de nacimiento ([Mis datos](mis-datos.md),
  RN-10), desde el portal no puede pasar de menor a mayor de edad ni al revés
  (Mis datos, RN-13 y RN-14): ese cambio lo hace secretaría.
- **RN-02 — Solo la familia propia.** El fallero solo ve la familia a la que
  pertenece; no ve otras familias ni listados del censo.
- **RN-03 — Solo el cabeza modifica.** Únicamente el cabeza de familia puede
  añadir o quitar miembros, cambiar el cabeza, eliminar la familia y
  gestionar los datos de los menores (RN-14). El resto de miembros mayores de
  edad la ven en solo lectura.
- **RN-04 — Crear familia (mismas condiciones que Secretaría).** Para crear
  una familia el fallero debe estar activo, tener asignada la comisión MF y no
  pertenecer ya a ninguna familia. Queda como cabeza y primer miembro.
- **RN-05 — Añadir miembro por nº de fallero y fecha de nacimiento.** Solo se
  añade si existe un fallero activo con ese número, esa fecha de nacimiento y
  sin familia. En cualquier otro caso se muestra el mismo aviso: "No hay ningún
  fallero activo y sin familia con esos datos.", para que el formulario no
  sirva para averiguar datos del censo. Pueden añadirse menores de edad.
- **RN-06 — Bloqueo por intentos fallidos al añadir.** Tras 5 intentos
  fallidos de añadir miembro en 15 minutos, el cabeza no puede volver a
  intentarlo durante 15 minutos ("Demasiados intentos fallidos al añadir
  miembros…"). Los fallos de hace más de 15 minutos ya no cuentan. Un intento
  correcto **no** pone el contador a cero: si lo hiciera, se podrían alternar
  intentos con el alta (y la baja) de un miembro conocido sin llegar nunca al
  bloqueo. Tras añadir un miembro, el formulario se vacía. Son los mismos
  valores configurables que el bloqueo del acceso.
- **RN-07 — El cabeza no se quita.** No se puede quitar al cabeza de la
  familia; primero hay que cambiar el cabeza o eliminar la familia si es el
  único miembro.
- **RN-08 — Nuevo cabeza: miembro, activo y mayor de edad.** Al cambiar el
  cabeza, el elegido debe ser miembro de la familia (igual que en Secretaría)
  y, además, estar activo y ser mayor de edad, para que la familia siga
  pudiendo gestionarse desde el portal. Si no hay ningún candidato se informa
  "No hay otros miembros mayores de edad que puedan ser cabeza de familia.".
  Tras el cambio, el antiguo cabeza ve la familia en solo lectura.
- **RN-09 — Eliminación protegida.** La familia solo puede eliminarse cuando
  no quedan miembros distintos del cabeza; al eliminarla, el cabeza queda
  también sin familia.
- **RN-10 — Sin mover de familia.** Desde el portal no se mueve a nadie de una
  familia a otra. Un fallero que ya pertenece a otra familia no puede añadirse
  (RN-05); el cambio lo hace secretaría.
- **RN-11 — Homónimos distinguibles.** En los desplegables de miembros, las
  personas con el mismo nombre se distinguen por su número de fallero (#nº).

- **RN-12 — Nombres tal cual.** Los nombres de los miembros (que cada
  fallero escribe en [Mis datos](mis-datos.md)) se muestran siempre como
  texto: un nombre con símbolos como `[`, `*` o `_` no se convierte en un
  enlace ni cambia el formato de la pantalla.
- **RN-13 — Errores de la base de datos.** Si una operación falla por un
  error de la base de datos se muestra "No se ha podido completar la
  operación. Inténtalo de nuevo más tarde."; el detalle técnico no se muestra.
- **RN-14 — El cabeza gestiona los datos de los menores.** Solo el cabeza de
  familia puede ver y modificar los datos de los menores de su familia (ver
  Conceptos). Puede cambiar exactamente los mismos campos que el fallero en
  [Mis datos](mis-datos.md) (nombre, apellidos, fecha de nacimiento,
  teléfono, email, calle, población y código postal), con las mismas
  validaciones, aplicadas a la ficha del menor. El DNI y la cuenta bancaria
  (con parte del número oculto) se ven en solo lectura. Las reglas de la
  fecha de nacimiento son las del menor: no puede cambiar su cuota (Mis
  datos, RN-12) ni hacerle mayor (Mis datos, RN-13), así que el menor sigue
  siéndolo tras el cambio; si la fecha es correcta, la cambia secretaría. Se
  aplica a todos los menores, también a los que tienen DNI y pueden entrar
  por sí mismos; si los dos modifican la ficha a la vez, se guarda campo a
  campo como en Mis datos, RN-15.
- **RN-15 — Los datos de los adultos no se muestran.** De los miembros
  mayores de edad (por ejemplo, la pareja o un hijo de 25 años) solo se ven
  el nº de fallero, el nombre y los apellidos de la tabla de miembros. Cada
  adulto mantiene sus datos desde su propio acceso.
- **RN-16 — El permiso se pierde solo.** El permiso sobre los datos de un
  menor se comprueba de nuevo en cada acción: desaparece en cuanto el menor
  cumple 18 años, deja la familia, se le da de baja o el cabeza deja de serlo
  (por cambio de cabeza o eliminación de la familia). Si ocurre mientras el
  cabeza tiene el formulario abierto, al guardar no se guarda nada y se
  muestra un aviso de error, por ejemplo "Error al actualizar fallero: Solo
  puedes ver y modificar los datos de los miembros activos y menores de edad
  de tu familia.".
- **RN-17 — Registro de cambios.** Cada cambio en los datos de un menor
  queda registrado en el registro interno del portal indicando qué fallero lo
  hizo y sobre qué ficha, para poder averiguarlo si hay una reclamación.

## 5. Casos de uso

### CU-01 — Crear mi familia

- **Actor**: fallero mayor de edad.
- **Precondición**: activo, con comisión MF y sin familia.
- **Pasos**: **Mi familia** → **➕ Crear familia**.
- **Resultado**: mensaje "Familia creada correctamente."; el fallero aparece
  como cabeza y primer miembro.
- **Errores / alternativas**: sin comisión MF se muestra el aviso de que debe
  contactar con secretaría (RN-04).

### CU-02 — Añadir a un hijo a la familia

- **Actor**: cabeza de familia.
- **Precondición**: el hijo está dado de alta en el censo, activo y sin
  familia.
- **Pasos**: **Mi familia** → **Añadir miembro** → escribir su nº de fallero y
  su fecha de nacimiento → **Añadir miembro**.
- **Resultado**: mensaje "Miembro añadido correctamente.".
- **Errores / alternativas**: si los datos no coinciden, el fallero está de
  baja o ya pertenece a otra familia, aviso genérico (RN-05); tras 5 fallos,
  bloqueo temporal (RN-06).

### CU-03 — Quitar un miembro

- **Actor**: cabeza de familia.
- **Pasos**: **Quitar miembro** → elegir el miembro → **Quitar miembro**.
- **Resultado**: mensaje "Miembro eliminado correctamente.".

### CU-04 — Ceder la cabeza de familia

- **Actor**: cabeza de familia.
- **Precondición**: hay otro miembro activo y mayor de edad.
- **Pasos**: **Cambiar cabeza de familia** → elegir el nuevo cabeza →
  **Confirmar cambio de cabeza**.
- **Resultado**: mensaje "Cabeza de familia cambiado correctamente."; el
  antiguo cabeza pasa a ver la familia en solo lectura (RN-08).

### CU-05 — Eliminar mi familia

- **Actor**: cabeza de familia.
- **Precondición**: solo queda el cabeza.
- **Pasos**: quitar antes al resto de miembros → **🗑 Eliminar familia**.
- **Resultado**: mensaje "Familia eliminada correctamente."; el fallero queda
  sin familia (RN-09).

### CU-06 — Actualizar los datos de un hijo

- **Actor**: cabeza de familia.
- **Precondición**: el hijo es miembro de su familia, está activo, tiene
  fecha de nacimiento registrada y es menor de edad.
- **Pasos**: **Mi familia** → **Datos de los menores** → elegir al hijo en
  **Menor** → modificar los campos → **Guardar cambios**.
- **Resultado**: mensaje "Fallero actualizado correctamente."; secretaría ve
  el cambio al momento.
- **Errores / alternativas**: las mismas validaciones que en Mis datos
  (RN-14); si el hijo ha dejado de ser un menor de la familia mientras tanto,
  no se guarda (RN-16).

## 6. Relación con otros dominios

- [Acceso](acceso.md) — el menú **Mi familia** solo aparece a mayores de edad.
- [Mis datos](mis-datos.md) — el cabeza edita los datos de los menores con
  el mismo formulario y las mismas reglas.
- [Familias (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/familias.md)
  — las familias son las mismas; secretaría ve y puede modificar al momento lo
  que se hace desde el portal, y es quien mueve falleros entre familias.
