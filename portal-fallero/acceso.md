# Acceso al portal

[← Volver al índice](index.md)

## 1. Propósito

Permitir que cada fallero entre en el portal con datos que ya conoce (su número
de fallero y su DNI), sin que la secretaría tenga que dar de alta usuarios ni
contraseñas, y asegurar que solo entra quien sigue siendo fallero activo.

## 2. Conceptos

- **Usuario**: el número de fallero (el "ID Censo" de Secretaría).
- **Contraseña**: el DNI o NIE del fallero tal como figura en el censo.
- **Sesión**: periodo en el que el fallero está dentro del portal. Dura
  mientras la pestaña del navegador esté abierta.
- **Bloqueo**: tras varios intentos fallidos seguidos con el mismo número de
  fallero, el acceso con ese número queda bloqueado un tiempo.

## 3. Funcionalidades

Pantalla de inicio de sesión **Portal Fallero El Cano**:

- **Iniciar sesión** con **Usuario (nº de fallero)** y **Contraseña (DNI)**
  (botón **Entrar**).

Menú lateral, una vez dentro:

- Saludo "Bienvenido" con el nombre y apellidos del fallero.
- **Cerrar Sesión**.
- Menú de navegación: **Mis datos** y, solo para mayores de edad,
  **Mi familia**.

## 4. Reglas de negocio

- **RN-01 — Solo falleros activos con DNI.** Solo pueden entrar los falleros
  sin fecha de baja que tienen DNI registrado en el censo. Un fallero sin DNI
  debe pedir a secretaría que lo registre.
- **RN-02 — El DNI se compara sin distinguir mayúsculas ni espacios.**
  "12345678z" y "12345678Z" son la misma contraseña.
- **RN-03 — Error genérico.** Si el número de fallero no existe, el DNI no
  coincide, el fallero está de baja o no tiene DNI, se muestra siempre el mismo
  aviso: "Login fallido. Verifica tus credenciales.". Así no se puede averiguar
  qué números de fallero existen.
- **RN-04 — Bloqueo por intentos fallidos.** Tras 5 intentos fallidos seguidos
  con el mismo número de fallero, ese número queda bloqueado 15 minutos, aunque
  después se introduzca el DNI correcto. Se muestra "Demasiados intentos
  fallidos. Vuelve a intentarlo dentro de 15 minutos.". Un acceso correcto
  pone el contador a cero. Los dos valores (5 y 15) son configurables en la
  instalación. El contador se pierde si el portal se reinicia.
- **RN-05 — Sesión revocada.** Si mientras el fallero está dentro secretaría le
  da de baja o le cambia el DNI (o lo cambia él mismo desde **Mis datos**), en
  su siguiente acción la sesión se cierra con el aviso "Tus datos de acceso han
  cambiado o ya no estás activo. Vuelve a iniciar sesión.".
- **RN-06 — Sesión no persistente.** No hay opción de "recordarme": recargar la
  página o cerrar la pestaña cierra la sesión.
- **RN-07 — Menú según la edad.** **Mis datos** aparece siempre. **Mi familia**
  solo aparece para falleros mayores de edad (18 años cumplidos a fecha de hoy
  según la fecha de nacimiento registrada por secretaría).

## 5. Casos de uso

### CU-01 — Entrar en el portal

- **Actor**: fallero.
- **Precondición**: el fallero está activo y tiene DNI registrado.
- **Pasos**: introducir el nº de fallero en **Usuario (nº de fallero)** y el
  DNI en **Contraseña (DNI)** → **Entrar**.
- **Resultado**: se abre **Mis datos** con sus datos cargados.
- **Errores / alternativas**: con datos incorrectos se muestra "Login fallido.
  Verifica tus credenciales." (RN-03).

### CU-02 — Acceso bloqueado

- **Actor**: fallero.
- **Precondición**: se han hecho 5 intentos fallidos seguidos con ese nº de
  fallero.
- **Pasos**: intentar entrar de nuevo.
- **Resultado**: aviso de bloqueo; hay que esperar 15 minutos (RN-04).

### CU-03 — Cerrar sesión

- **Actor**: fallero.
- **Pasos**: **Cerrar Sesión** en el menú lateral.
- **Resultado**: vuelve la pantalla de inicio de sesión.

## 6. Relación con otros dominios

- [Mis datos](mis-datos.md) — cambiar el DNI cambia la contraseña del portal.
- [Mi familia](mi-familia.md) — solo accesible para mayores de edad.
- [Censo (Secretaría)](https://github.com/emilioxiri/secretaria-el-cano-documentacion/blob/main/censo.md)
  — el nº de fallero, el DNI, la fecha de nacimiento y la baja vienen del censo
  que gestiona secretaría.
