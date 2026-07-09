# Familias

[← Volver al índice](index.md)

## 1. Propósito

Agrupar a los falleros por unidades familiares para facilitar la gestión de la
secretaría (comunicaciones, cuotas, organización interna). Cada familia tiene
una cabeza de familia que actúa como referente de la agrupación.

## 2. Conceptos

- **Familia**: agrupación de falleros con una cabeza de familia.
- **Cabeza de familia**: fallero que encabeza la familia. Para poder serlo debe
  estar activo, tener asignada comisión MF (masculina o femenina) y no
  pertenecer ya a otra familia en el momento de crearla.
- **Miembro**: cualquier fallero que pertenece a la familia, incluida la
  cabeza.

## 3. Funcionalidades

Pantalla **Ver Familias** (menú lateral):

- Listado de familias con **Cabeza de familia**, **Nº miembros** e
  **Id familia**.
- **Crear familia**: elegir el fallero que será la cabeza entre los candidatos
  disponibles.
- **Ver / Editar familia** (seleccionando una familia de la tabla): abre el
  detalle con la cabeza y la lista de miembros, y permite:
  - **Cambiar cabeza**: elegir la nueva cabeza entre los miembros actuales.
  - **Añadir miembro**: incorporar un fallero activo que no tenga familia.
  - **Quitar miembro**: retirar a un miembro que no sea la cabeza.
  - **Eliminar familia**: solo cuando no quedan miembros distintos de la
    cabeza.

Desde la ficha del fallero (**Ver Falleros** → **Datos completos**):

- Sección **Familia** con la cabeza y los miembros de la familia del fallero.
- **Asignar / Mover familia**: llevar al fallero a otra familia existente o
  crear una nueva familia con él como cabeza.

## 4. Reglas de negocio

- **RN-01 — Cabeza elegible.** Solo puede elegirse como cabeza de una familia
  nueva un fallero activo, con comisión MF asignada y que no pertenezca ya a
  ninguna familia.
- **RN-02 — Un fallero, una familia.** Un fallero solo puede pertenecer a una
  familia a la vez. Mover a un fallero de una familia a otra es una operación
  única: sale de la anterior y entra en la nueva sin estados intermedios.
- **RN-03 — La nueva cabeza debe ser miembro.** Al cambiar la cabeza de una
  familia, la persona elegida debe ser ya miembro de esa familia; si no lo es,
  la operación se rechaza con el aviso "El nuevo cabeza debe ser miembro de la
  familia".
- **RN-04 — La cabeza no se quita ni se mueve directamente.** No se puede
  quitar a la cabeza de la familia ni moverla a otra familia; primero hay que
  asignar otra cabeza (o eliminar la familia si es el único miembro).
- **RN-05 — Eliminación protegida.** Una familia solo puede eliminarse cuando
  no quedan miembros distintos de la cabeza. Si solo queda la cabeza, al
  eliminar la familia se le desasigna también a ella.
- **RN-06 — Solo falleros sin familia como nuevos miembros.** Al añadir un
  miembro, únicamente se ofrecen falleros activos que no pertenezcan a ninguna
  familia.

## 5. Casos de uso

### CU-01 — Crear una familia

- **Actor**: secretario/a.
- **Precondición**: existe al menos un fallero activo con comisión MF y sin
  familia.
- **Pasos**: **Ver Familias** → **Crear familia** → seleccionar el fallero que
  será la cabeza → **Crear familia**.
- **Resultado**: mensaje "Familia creada correctamente"; la cabeza queda además
  registrada como primer miembro.
- **Errores / alternativas**: si no hay candidatos, la pantalla informa "No hay
  falleros sin familia disponibles para crear una nueva familia".

### CU-02 — Añadir un miembro a una familia

- **Actor**: secretario/a.
- **Precondición**: la familia existe; hay falleros activos sin familia.
- **Pasos**: **Ver Familias** → seleccionar la familia → **Ver / Editar
  familia** → **Añadir miembro** → elegir el fallero → confirmar.
- **Resultado**: mensaje "Miembro añadido correctamente".
- **Errores / alternativas**: si el fallero ya perteneciera a una familia, la
  operación se rechaza (RN-02).

### CU-03 — Mover un fallero de familia

- **Actor**: secretario/a.
- **Precondición**: el fallero no es cabeza de su familia actual.
- **Pasos**: **Ver Falleros** → seleccionar fallero → **Datos completos** →
  **Asignar / Mover familia** → elegir la familia destino (o la opción "Nueva
  familia con este fallero como cabeza") → **Confirmar**.
- **Resultado**: mensaje "Familia asignada correctamente"; el fallero sale de
  su familia anterior y entra en la nueva en una sola operación (RN-02).
- **Errores / alternativas**: si el fallero es cabeza de familia, la pantalla
  lo bloquea con el aviso de que primero debe asignarse otra cabeza (RN-04).

### CU-04 — Cambiar la cabeza de familia

- **Actor**: secretario/a.
- **Precondición**: la familia tiene al menos otro miembro.
- **Pasos**: **Ver / Editar familia** → **Cambiar cabeza** → elegir al nuevo
  cabeza entre los miembros → **Confirmar cambio de cabeza**.
- **Resultado**: mensaje "Cabeza de familia cambiado correctamente".
- **Errores / alternativas**: si la persona elegida no es miembro, se rechaza
  (RN-03).

### CU-05 — Eliminar una familia

- **Actor**: secretario/a.
- **Precondición**: en la familia no quedan miembros distintos de la cabeza.
- **Pasos**: **Ver / Editar familia** → (si hace falta) **Quitar miembro** para
  cada miembro que no sea la cabeza → **Eliminar familia**.
- **Resultado**: mensaje "Familia eliminada correctamente"; la cabeza queda sin
  familia asignada.
- **Errores / alternativas**: mientras queden miembros distintos de la cabeza
  el botón permanece deshabilitado con el aviso correspondiente (RN-05).

## 6. Relación con otros dominios

- [Censo](censo.md) — los miembros de una familia son falleros del censo; la
  ficha del fallero muestra su familia y permite moverlo.
