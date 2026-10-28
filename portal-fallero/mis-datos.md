# Mis datos

[← Volver al índice](index.md)

## 1. Propósito

Que cada fallero mantenga al día sus datos de contacto y su dirección sin pasar
por secretaría, usando el mismo formulario y las mismas validaciones que la
edición de un fallero en Secretaría.

## 2. Conceptos

- **Datos editables**: los datos personales y de dirección que el fallero puede
  cambiar por sí mismo.
- **Datos reservados a secretaría**: los que afectan a la gestión de la falla
  (fecha de nacimiento, comisiones, cuota, cuenta bancaria, fechas de alta y
  baja, pulsera y nº de censo JCF). No se muestran en el portal.

## 3. Funcionalidades

Pantalla **Mis datos** (menú lateral; es la pantalla de entrada):

- Aviso: "Revisa y actualiza tus datos. Para cambiar la fecha de nacimiento,
  la comisión, la cuota o los datos bancarios contacta con secretaría."
- Sección **Datos personales**:
  - **ID Censo**: solo lectura.
  - **Nombre\***, **Apellidos\***, **DNI\***, **Teléfono\*** y **Email**.
  - Debajo del DNI: "Tu DNI es tu contraseña de acceso: si lo cambias, tendrás
    que volver a entrar con el nuevo."
- Sección **Dirección**: **Calle\***, **Población\*** y **Código Postal\***.
- **Guardar cambios**: valida y guarda.
- **Descartar cambios**: vuelve a cargar los datos guardados.

## 4. Reglas de negocio

- **RN-01 — Solo los datos propios.** El fallero solo ve y modifica su propia
  ficha; no tiene acceso a los datos de ningún otro fallero.
- **RN-02 — Campos editables limitados.** Solo se pueden cambiar nombre,
  apellidos, DNI, teléfono, email, calle, población y código postal. El resto
  de la ficha solo lo cambia secretaría y no se muestra en el portal.
- **RN-03 — Mismas validaciones que Secretaría.** Nombre, apellidos, teléfono,
  calle, población y código postal son obligatorios; el email, si se indica,
  debe tener un formato válido; el DNI/NIE debe tener un formato válido y la
  letra de control correcta.
- **RN-04 — DNI siempre obligatorio.** A diferencia de Secretaría (obligatorio
  a partir de 16 años), en el portal el DNI no se puede dejar vacío porque es
  la contraseña: "El DNI es obligatorio: es tu contraseña de acceso al portal."
- **RN-05 — DNI único.** No se puede guardar un DNI que ya tenga otro fallero:
  "Error al actualizar fallero: Ya existe un fallero con el DNI …".
- **RN-06 — Cambiar el DNI cambia la contraseña.** Al guardar un DNI nuevo, la
  sesión se cierra y hay que volver a entrar con el DNI nuevo (ver
  [Acceso](acceso.md), RN-05).
- **RN-07 — Normalización.** Al guardar se quitan los espacios sobrantes, el
  DNI se guarda en mayúsculas y el email en minúsculas.
- **RN-08 — Avisos de error.** Los errores de un campo aparecen bajo el
  formulario en cuanto se modifica ese campo, y todos a la vez al pulsar
  **Guardar cambios**. Con errores no se guarda nada.

## 5. Casos de uso

### CU-01 — Actualizar el teléfono o la dirección

- **Actor**: fallero.
- **Precondición**: ha iniciado sesión.
- **Pasos**: **Mis datos** → modificar los campos → **Guardar cambios**.
- **Resultado**: mensaje "Fallero actualizado correctamente."; secretaría ve
  el cambio al momento.
- **Errores / alternativas**: con un campo obligatorio vacío o un email no
  válido se muestra el error y no se guarda (RN-03).

### CU-02 — Cambiar el DNI

- **Actor**: fallero.
- **Pasos**: **Mis datos** → escribir el DNI nuevo → **Guardar cambios**.
- **Resultado**: la sesión se cierra con el aviso de que los datos de acceso
  han cambiado; se entra de nuevo con el DNI nuevo (RN-06).
- **Errores / alternativas**: DNI vacío (RN-04), mal formado o con letra
  incorrecta (RN-03) o ya usado por otro fallero (RN-05).

### CU-03 — Descartar cambios

- **Actor**: fallero.
- **Pasos**: modificar campos → **Descartar cambios**.
- **Resultado**: el formulario vuelve a mostrar los datos guardados.

## 6. Relación con otros dominios

- [Acceso](acceso.md) — el DNI es la contraseña del portal.
- [Censo (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/censo.md)
  — es la misma ficha del censo; los datos reservados los gestiona secretaría.
