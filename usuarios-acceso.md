# Usuarios y control de acceso

[← Volver al índice](index.md)

## 1. Propósito

Controlar quién puede entrar en la aplicación de la secretaría. Los usuarios de
la aplicación (no confundir con los falleros) son las personas de la
secretaría o la directiva autorizadas a gestionar los datos.

## 2. Conceptos

- **Usuario**: persona con credenciales de acceso a la aplicación (nombre,
  email y contraseña).
- **Usuario activo / inactivo**: solo los usuarios activos pueden iniciar
  sesión; desactivar un usuario le retira el acceso sin borrar su registro.
- **Inicio de sesión**: pantalla "Secretaria El Cano" con usuario (email) y
  contraseña, previa a cualquier otra pantalla.

## 3. Funcionalidades

Acceso a la aplicación:

- **Iniciar sesión** con email y contraseña (botón **Entrar**).
- **Cerrar Sesión** desde el menú lateral.

Pantalla **Ver Usuarios** (menú lateral):

- Listado de usuarios con **Nombre**, **Email** y estado **Activo** (✅/❌).
- Filtrar por nombre de usuario, email y estado (Todos / Activos / Inactivos).
- **Añadir usuario** (botón ➕): nombre de usuario, email, contraseña y marca
  de activo.
- **Editar** (seleccionando un usuario): cambiar nombre, email y estado
  activo.
- **Contraseña**: cambiar la contraseña del usuario, con confirmación.

## 4. Reglas de negocio

- **RN-01 — Acceso solo con sesión iniciada.** Ninguna pantalla de gestión es
  accesible sin iniciar sesión; con credenciales incorrectas se muestra
  "Usuario o contraseña incorrectos".
- **RN-02 — Solo entran los usuarios activos.** Un usuario desactivado no
  puede iniciar sesión aunque sus credenciales sean correctas.
- **RN-03 — Sin usuarios activos no hay aplicación.** Si no existe ningún
  usuario activo, la aplicación se detiene mostrando "No hay usuarios activos
  en la base de datos. Contacta con el administrador".
- **RN-04 — Email único y válido.** No pueden existir dos usuarios con el
  mismo email, y el email debe tener un formato válido.
- **RN-05 — Contraseña mínima y confirmada.** La contraseña debe tener al
  menos 6 caracteres; al cambiarla se exige escribirla dos veces y ambas deben
  coincidir.
- **RN-06 — Contraseñas siempre cifradas.** Las contraseñas se guardan
  cifradas; nadie — ni siquiera el administrador — puede consultar una
  contraseña en claro.
- **RN-07 — Los usuarios se desactivan, no se borran.** La pantalla de
  usuarios no ofrece borrado: la vía para retirar el acceso es desmarcar
  "Activo" al editar.

## 5. Casos de uso

### CU-01 — Iniciar sesión

- **Actor**: miembro de la secretaría/directiva con usuario.
- **Precondición**: usuario activo con credenciales válidas.
- **Pasos**: abrir la aplicación → introducir usuario y contraseña → **Entrar**.
- **Resultado**: acceso al menú lateral con todas las pantallas.
- **Errores / alternativas**: credenciales incorrectas o usuario inactivo →
  "Usuario o contraseña incorrectos" (RN-01, RN-02).

### CU-02 — Alta de un usuario

- **Actor**: usuario de la aplicación (típicamente el administrador).
- **Pasos**: **Ver Usuarios** → botón ➕ → rellenar nombre de usuario, email y
  contraseña, marcar **Activo** → **Crear usuario**.
- **Resultado**: mensaje "Usuario añadido correctamente".
- **Errores / alternativas**: email repetido o inválido (RN-04), contraseña
  corta (RN-05) → la operación se rechaza con el aviso correspondiente.

### CU-03 — Retirar el acceso a un usuario

- **Actor**: administrador.
- **Precondición**: el usuario existe; queda al menos otro usuario activo.
- **Pasos**: **Ver Usuarios** → seleccionar al usuario → **Editar** →
  desmarcar **Activo** → **Guardar cambios**.
- **Resultado**: el usuario ya no puede iniciar sesión (RN-02); su registro se
  conserva (RN-07).
- **Errores / alternativas**: si se desactivara al último usuario activo, la
  aplicación quedaría inaccesible (RN-03) — evitarlo operativamente.

### CU-04 — Cambiar la contraseña de un usuario

- **Actor**: administrador.
- **Pasos**: **Ver Usuarios** → seleccionar al usuario → **Contraseña** →
  escribir la **Nueva contraseña** y **Confirmar contraseña** → **Cambiar
  contraseña**.
- **Resultado**: mensaje "Contraseña cambiada correctamente"; la nueva
  contraseña queda cifrada (RN-06).
- **Errores / alternativas**: contraseñas que no coinciden ("Las contraseñas
  no coinciden") o de menos de 6 caracteres → se rechaza (RN-05).

## 6. Relación con otros dominios

- Todos los dominios — el control de acceso protege el conjunto de la
  aplicación: [Censo](censo.md), [Familias](familias.md),
  [Cuentas bancarias](cuentas-bancarias.md), [Ejercicio](ejercicio.md),
  [Histórico y directiva](historico-directiva.md) y [Censo JCF](censo-jcf.md).
