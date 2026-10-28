# Cuentas bancarias y domiciliación SEPA

[← Volver al índice](index.md)

## 1. Propósito

Mantener las cuentas bancarias en las que se domicilian las cuotas de los
falleros, junto con la referencia de mandato SEPA de cada una, y saber en todo
momento qué falleros tienen su cuota domiciliada en cada cuenta.

## 2. Conceptos

- **Cuenta bancaria**: registro con el IBAN, unas observaciones libres y su
  referencia de mandato.
- **IBAN**: código internacional de la cuenta bancaria.
- **Referencia de mandato (Mandato Ref.)**: identificador único del mandato de
  domiciliación SEPA asociado a la cuenta. La genera la aplicación y no puede
  modificarse.
- **Titulares / falleros de la cuenta**: falleros activos cuya cuota se
  domicilia en esa cuenta.

## 3. Funcionalidades

Pantalla **Ver Cuentas Bancarias** (menú lateral):

- Listado de cuentas con **IBAN**, **Mandato Ref.**, **Nº falleros** (activos
  domiciliados) y **Observaciones**.
- Filtrar por **IBAN** y por **Mandato Ref.** (sección "Filtrar Cuentas").
- **Añadir cuenta bancaria** (botón ➕): introducir IBAN y observaciones; al
  crearla se informa de la referencia de mandato generada.
- **Editar** (seleccionando una cuenta): modificar IBAN y observaciones; la
  referencia de mandato se muestra como "generado automáticamente" y no es
  editable.
- **Ver falleros**: lista con nombre y apellidos de los falleros activos
  asociados a la cuenta.

Desde el **Formulario fallero**: asignar al fallero una de las cuentas
existentes en la sección Datos bancarios (ver [Censo](censo.md)).

## 4. Reglas de negocio

- **RN-01 — Formato de IBAN.** El IBAN se guarda normalizado: en mayúsculas y
  sin espacios. Debe tener al menos 15 caracteres y empezar por dos letras; si
  no, se rechaza con el aviso de formato no válido. Además se comprueban los
  **dígitos de control** del IBAN (norma ISO 13616, módulo 97): un IBAN mal
  tecleado se rechaza con el aviso "El IBAN no es válido: los dígitos de
  control no cuadran" en lugar de guardarse y fallar después en la remesa.
- **RN-02 — IBAN único.** No pueden existir dos cuentas con el mismo IBAN, ni
  al crear ni al editar. La comparación ignora mayúsculas y espacios, de modo
  que también detecta cuentas antiguas guardadas con el IBAN agrupado
  ("ES91 2100 …"). El filtro por IBAN del listado ignora igualmente los
  espacios.
- **RN-03 — Referencia de mandato automática e inmutable.** La referencia de
  mandato SEPA se genera automáticamente al crear la cuenta (numeración de
  cuatro cifras, con sufijo distintivo si coincidiera con una referencia
  antigua) y **nunca** puede modificarse después.
- **RN-04 — Las cuentas antiguas también tienen mandato.** Las cuentas
  registradas antes de existir la referencia de mandato reciben una
  automáticamente al arrancar la aplicación, de forma que toda cuenta tiene
  siempre su referencia.
- **RN-05 — Recuento de falleros activos.** El número de falleros por cuenta
  cuenta únicamente falleros activos (las bajas no computan).

## 5. Casos de uso

### CU-01 — Alta de una cuenta bancaria

- **Actor**: secretario/a.
- **Pasos**: **Ver Cuentas Bancarias** → botón ➕ → introducir **IBAN** y
  **Observaciones** → **Crear cuenta**.
- **Resultado**: mensaje "Cuenta bancaria añadida correctamente. Referencia de
  mandato generada: …" (RN-03).
- **Errores / alternativas**: IBAN con formato no válido (RN-01) o ya
  existente (RN-02) → la operación se rechaza con el aviso correspondiente.

### CU-02 — Edición de una cuenta

- **Actor**: secretario/a.
- **Precondición**: la cuenta existe.
- **Pasos**: seleccionar la cuenta en la tabla → **Editar** → modificar IBAN u
  observaciones → **Guardar cambios**.
- **Resultado**: mensaje "Cuenta bancaria actualizada correctamente"; la
  referencia de mandato permanece intacta (RN-03).
- **Errores / alternativas**: mismas validaciones de IBAN que en el alta.

### CU-03 — Consultar los falleros de una cuenta

- **Actor**: secretario/a o tesorero/a.
- **Pasos**: seleccionar la cuenta → **Ver falleros**.
- **Resultado**: lista de falleros activos domiciliados en la cuenta.
- **Errores / alternativas**: si no hay ninguno, se informa "No hay falleros
  asignados a esta cuenta".

### CU-04 — Domiciliar la cuota de un fallero

- **Actor**: secretario/a.
- **Precondición**: la cuenta existe.
- **Pasos**: **Formulario fallero** (alta o edición) → sección Datos bancarios
  → elegir la cuenta en el desplegable → guardar.
- **Resultado**: el fallero pasa a contar en el **Nº falleros** de esa cuenta.
- **Errores / alternativas**: puede elegirse "Sin cuenta bancaria" para
  falleros sin domiciliación.

## 6. Relación con otros dominios

- [Censo](censo.md) — cada fallero puede tener asignada una cuenta para la
  domiciliación de su cuota; la asignación se hace en el Formulario fallero.
