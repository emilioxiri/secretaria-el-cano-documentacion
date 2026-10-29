# Mi familia

[← Volver al índice](index.md)

## 1. Propósito

Que los falleros mayores de edad creen y mantengan su propia familia sin pasar
por secretaría, con las mismas reglas de negocio de familias que aplica
Secretaría y sin dar acceso al resto del censo ni a otras familias.

## 2. Conceptos

- **Familia**, **cabeza de familia** y **miembro**: los mismos conceptos que en
  Secretaría (ver
  [Familias](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/familias.md)).
- **Mayor de edad**: 18 años cumplidos a fecha de hoy según la fecha de
  nacimiento registrada, que el propio fallero puede corregir en
  [Mis datos](mis-datos.md).
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
- **Cambiar cabeza**: elegir el nuevo cabeza entre los miembros que pueden
  serlo → **Confirmar cambio de cabeza**.
- **Añadir miembro**: **Nº de fallero** y **Fecha de nacimiento** del fallero
  a añadir → **Añadir miembro**.
- **Quitar miembro**: elegir un miembro distinto del cabeza → **Quitar
  miembro**.
- **🗑 Eliminar familia**: solo activo cuando únicamente queda el cabeza.

**Miembro (no cabeza)**

- Cabeza de familia y tabla de miembros, en solo lectura, con el aviso "Solo
  el cabeza de familia puede modificar la familia.".

## 4. Reglas de negocio

- **RN-01 — Solo mayores de edad.** Solo los falleros activos y mayores de
  edad ven **Mi familia** y pueden hacer cualquier operación de familias. Sin
  fecha de nacimiento registrada no se considera mayor de edad. Como el
  fallero puede corregir su fecha de nacimiento ([Mis datos](mis-datos.md),
  RN-10), el acceso cambia en cuanto la guarda; un menor, sin embargo, no
  puede ponerse una fecha de mayor de edad desde el portal (Mis datos, RN-13).
- **RN-02 — Solo la familia propia.** El fallero solo ve la familia a la que
  pertenece; no ve otras familias ni listados del censo.
- **RN-03 — Solo el cabeza modifica.** Únicamente el cabeza de familia puede
  añadir o quitar miembros, cambiar el cabeza o eliminar la familia. El resto
  de miembros mayores de edad la ven en solo lectura.
- **RN-04 — Crear familia (mismas condiciones que Secretaría).** Para crear
  una familia el fallero debe estar activo, tener asignada la comisión MF y no
  pertenecer ya a ninguna familia. Queda como cabeza y primer miembro.
- **RN-05 — Añadir miembro por nº de fallero y fecha de nacimiento.** Solo se
  añade si existe un fallero activo con ese número, esa fecha de nacimiento y
  sin familia. En cualquier otro caso se muestra el mismo aviso: "No hay ningún
  fallero activo y sin familia con esos datos.", para que el formulario no
  sirva para averiguar datos del censo. Pueden añadirse menores de edad.
- **RN-06 — Bloqueo por intentos fallidos al añadir.** Tras 5 intentos
  fallidos de añadir miembro, el cabeza no puede volver a intentarlo durante 15
  minutos ("Demasiados intentos fallidos al añadir miembros…"). Un intento
  correcto pone el contador a cero. Son los mismos valores configurables que el
  bloqueo del acceso.
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
- **Pasos**: **Cambiar cabeza** → elegir el nuevo cabeza → **Confirmar cambio
  de cabeza**.
- **Resultado**: mensaje "Cabeza de familia cambiado correctamente."; el
  antiguo cabeza pasa a ver la familia en solo lectura (RN-08).

### CU-05 — Eliminar mi familia

- **Actor**: cabeza de familia.
- **Precondición**: solo queda el cabeza.
- **Pasos**: quitar antes al resto de miembros → **🗑 Eliminar familia**.
- **Resultado**: mensaje "Familia eliminada correctamente."; el fallero queda
  sin familia (RN-09).

## 6. Relación con otros dominios

- [Acceso](acceso.md) — el menú **Mi familia** solo aparece a mayores de edad.
- [Familias (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/familias.md)
  — las familias son las mismas; secretaría ve y puede modificar al momento lo
  que se hace desde el portal, y es quien mueve falleros entre familias.
