# Portal Fallero El Cano — Documentación funcional

Esta documentación describe, en lenguaje de negocio, qué hace el **Portal
Fallero El Cano** y qué reglas aplica. Está pensada para la directiva y la
secretaría de la falla: no contiene detalles técnicos, sino funcionalidades,
reglas de negocio y casos de uso tal y como se ven en pantalla.

## ¿Qué es el portal?

El Portal Fallero es la aplicación de autoservicio de los falleros. Cada
fallero entra con su número de fallero y su DNI y puede:

- Revisar y actualizar sus datos personales y su dirección.
- Si es mayor de edad, crear y gestionar su familia.

El portal trabaja sobre **los mismos datos que la aplicación de Secretaría**:
cualquier cambio hecho desde el portal se ve al momento en Secretaría, y al
revés. Las reglas de negocio de las familias son las mismas que en Secretaría,
con las restricciones propias de un portal en el que cada fallero solo ve lo
suyo.

## Navegación del portal

Tras iniciar sesión, el menú lateral ofrece estas opciones:

| Opción de menú | Quién la ve | Qué contiene |
|---|---|---|
| **Mis datos** | Todos los falleros | Formulario con sus datos personales y su dirección, en modo edición. Es la pantalla de entrada. |
| **Mi familia** | Solo falleros mayores de edad | Su familia: miembros, cabeza de familia y, si es el cabeza, la gestión de la familia. |

## Mapa de documentos

| Tema | Documento |
|---|---|
| Acceso al portal (usuario, contraseña, bloqueo, sesión) | [acceso.md](acceso.md) |
| Mis datos | [mis-datos.md](mis-datos.md) |
| Mi familia | [mi-familia.md](mi-familia.md) |

## Diferencias con Secretaría

| Aspecto | Secretaría | Portal Fallero |
|---|---|---|
| Quién entra | Usuarios de la secretaría (email y contraseña) | Cada fallero activo con DNI (nº de fallero y DNI) |
| Datos del fallero | Todos los falleros, todos los campos | Solo los suyos y solo datos personales y dirección |
| DNI | Obligatorio a partir de 16 años | Siempre obligatorio: es la contraseña |
| Familias | Todas las familias | Solo la suya y solo si es mayor de edad |
| Añadir miembros | Desplegable con los falleros sin familia | Nº de fallero y fecha de nacimiento del miembro |
| Mover de familia | Sí | No (lo hace Secretaría) |
| Nuevo cabeza de familia | Cualquier miembro | Un miembro activo y mayor de edad |

## Glosario

| Término | Significado |
|---|---|
| **Nº de fallero** | Número del fallero en el censo de la falla (el "ID Censo" de Secretaría). Es el usuario del portal. |
| **DNI** | DNI o NIE del fallero tal como figura en el censo. Es la contraseña del portal. |
| **Mayor de edad** | Fallero con 18 años cumplidos a fecha de hoy según la fecha de nacimiento registrada por secretaría. Sin fecha de nacimiento registrada no se considera mayor de edad. |
| **Fallero activo** | Fallero sin fecha de baja. |
| **Cabeza de familia** | Fallero que encabeza una familia y el único que puede modificarla desde el portal. |
| **Comisión MF** | Comisión masculina o femenina asignada por secretaría. Es necesaria para poder crear una familia. |
