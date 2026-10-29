# Mis datos

[← Volver al índice](index.md)

## 1. Propósito

Que cada fallero mantenga al día sus datos de contacto, su fecha de nacimiento
y su dirección sin pasar por secretaría, usando el mismo formulario y las mismas validaciones que la
edición de un fallero en Secretaría.

## 2. Conceptos

- **Datos editables**: los datos personales, la fecha de nacimiento y la
  dirección, que el fallero puede cambiar por sí mismo.
- **Datos de solo lectura**: el DNI (es la contraseña del portal) y la cuenta
  bancaria. Se muestran, pero solo los cambia secretaría.
- **Datos reservados a secretaría**: comisiones, cuota, fechas de alta y baja,
  pulsera y nº de censo JCF. No se muestran en el portal.

## 3. Funcionalidades

Pantalla **Mis datos** (menú lateral; es la pantalla de entrada):

- Aviso: "Revisa y actualiza tus datos. El DNI, la comisión, la cuota y los
  datos bancarios solo los puede cambiar secretaría."
- Sección **Datos personales**:
  - **ID Censo**: solo lectura.
  - **Nombre\***, **Apellidos\***, **Fecha de nacimiento\***, **Teléfono\*** y
    **Email**.
  - **DNI**: solo lectura, con la nota "Tu DNI es tu contraseña de acceso.
    Para cambiarlo contacta con secretaría."
- Sección **Dirección**: **Calle\***, **Población\*** y **Código Postal\***.
- Sección **Datos bancarios**: **Cuenta bancaria (IBAN)** en solo lectura,
  con parte del número oculto (por ejemplo "ES91 \*\*\*\* \*\*\*\* \*\*\*\*
  \*\*\*\* 1332"), o "— Sin cuenta bancaria —" si no tiene ninguna asignada.
- **Guardar cambios**: valida y guarda.
- **Descartar cambios**: vuelve a cargar los datos guardados.

## 4. Reglas de negocio

- **RN-01 — Solo los datos propios.** El fallero solo ve y modifica su propia
  ficha; no tiene acceso a los datos de ningún otro fallero.
- **RN-02 — Campos editables limitados.** Solo se pueden cambiar nombre,
  apellidos, fecha de nacimiento, teléfono, email, calle, población y código
  postal. El DNI y la cuenta bancaria se muestran en solo lectura (RN-09,
  RN-11); el resto de la ficha solo lo cambia secretaría y no se muestra.
- **RN-03 — Mismas validaciones que Secretaría.** Nombre, apellidos, fecha de
  nacimiento, teléfono, calle, población y código postal son obligatorios; la
  fecha de nacimiento no puede ser futura ni de hace más de 120 años; el email,
  si se indica, debe tener un formato válido.
- **RN-04 — (Retirada) DNI siempre obligatorio.** Retirada al pasar el DNI a
  solo lectura (RN-09): el fallero ya no puede vaciarlo.
- **RN-05 — (Retirada) DNI único.** Retirada al pasar el DNI a solo lectura
  (RN-09); la unicidad la sigue controlando Secretaría.
- **RN-06 — (Retirada) Cambiar el DNI cambia la contraseña.** Retirada al
  pasar el DNI a solo lectura (RN-09).
- **RN-07 — Normalización.** Al guardar se quitan los espacios sobrantes y el
  email se guarda en minúsculas.
- **RN-08 — Avisos de error.** Los errores de un campo aparecen bajo el
  formulario en cuanto se modifica ese campo, y todos a la vez al pulsar
  **Guardar cambios**. Con errores no se guarda nada.
- **RN-09 — DNI en solo lectura.** El DNI es la contraseña del portal y solo
  lo cambia secretaría. Si secretaría lo cambia mientras el fallero está
  dentro, su sesión se cierra (ver [Acceso](acceso.md), RN-05).
- **RN-10 — Fecha de nacimiento editable.** El fallero puede corregir su fecha
  de nacimiento. El cambio tiene efecto inmediato:
  - Decide si es mayor de edad y, por tanto, si ve **Mi familia** (ver
    [Mi familia](mi-familia.md), RN-01).
  - Es la fecha con la que se le identifica al añadirle a una familia.
  - Secretaría calcula con ella la categoría de cuota (EPIM). Por eso el
    portal no admite una fecha que cambie la cuota asignada (RN-12).
- **RN-11 — Cuenta bancaria en solo lectura.** Se muestra la cuenta en la que
  se domicilia la cuota, con solo los 4 primeros y los 4 últimos caracteres
  visibles. Para cambiarla hay que contactar con secretaría.
- **RN-12 — La fecha de nacimiento no cambia la cuota.** Una fecha de
  nacimiento nueva solo se guarda si mantiene la categoría de cuota EPIM que
  el fallero tiene asignada (edad a 19 de marzo: Exento, Pitufo hasta 4 años,
  Infantil hasta 15, Mayor desde 16). Así, un fallero de cuota Mayor puede
  corregir su fecha libremente mientras siga teniendo 16 años o más, y un
  infantil no puede pasar a otra categoría. Si la fecha cambia la categoría,
  no se guarda y se muestra: "Con esa fecha de nacimiento tu cuota pasaría de
  «…» a «…». Para corregirla contacta con secretaría.". No se comprueba si la
  fecha no cambia (para no impedir guardar otros datos) ni si el fallero
  todavía no tiene cuota asignada.

## 5. Casos de uso

### CU-01 — Actualizar el teléfono o la dirección

- **Actor**: fallero.
- **Precondición**: ha iniciado sesión.
- **Pasos**: **Mis datos** → modificar los campos → **Guardar cambios**.
- **Resultado**: mensaje "Fallero actualizado correctamente."; secretaría ve
  el cambio al momento.
- **Errores / alternativas**: con un campo obligatorio vacío o un email no
  válido se muestra el error y no se guarda (RN-03).

### CU-02 — (Retirado) Cambiar el DNI

El DNI ya no se cambia desde el portal (RN-09): lo cambia secretaría.

### CU-03 — Descartar cambios

- **Actor**: fallero.
- **Pasos**: modificar campos → **Descartar cambios**.
- **Resultado**: el formulario vuelve a mostrar los datos guardados.

### CU-04 — Corregir la fecha de nacimiento

- **Actor**: fallero.
- **Pasos**: **Mis datos** → **Fecha de nacimiento\*** → elegir la fecha
  correcta → **Guardar cambios**.
- **Resultado**: mensaje "Fallero actualizado correctamente."; si con la nueva
  fecha pasa a ser (o deja de ser) mayor de edad, el menú muestra (u oculta)
  **Mi familia** (RN-10).
- **Errores / alternativas**: fecha vacía, futura o de hace más de 120 años
  (RN-03); fecha que cambia la categoría de cuota (RN-12): hay que pedir la
  corrección a secretaría.

## 6. Relación con otros dominios

- [Acceso](acceso.md) — el DNI es la contraseña del portal.
- [Mi familia](mi-familia.md) — la fecha de nacimiento decide si el fallero es
  mayor de edad.
- [Censo (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/censo.md)
  — es la misma ficha del censo; los datos reservados los gestiona secretaría.
- [Cuentas bancarias (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/cuentas-bancarias.md)
  — la cuenta que se muestra es la que secretaría tiene asignada al fallero.
