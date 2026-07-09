# Secretaría El Cano — Documentación funcional

Esta documentación describe, en lenguaje de negocio, qué hace la aplicación
**Secretaría El Cano** y qué reglas aplica. Está pensada para la directiva y la
secretaría de la falla: no contiene detalles técnicos, sino funcionalidades,
reglas de negocio y casos de uso tal y como se ven en pantalla.

## ¿Qué es la aplicación?

Secretaría El Cano es la herramienta de gestión de la secretaría de la falla.
Centraliza el censo de falleros, las familias, las cuentas bancarias para la
domiciliación de cuotas, el histórico de cada fallero (cargos, comisiones y
recompensas), la generación del nuevo año fallero y la sincronización con el
censo oficial de la Junta Central Fallera (JCF).

## Navegación de la aplicación

Tras iniciar sesión, el menú lateral ofrece estas opciones:

| Opción de menú | Qué contiene |
|---|---|
| **Ver Falleros** | Listado del censo con filtros, estadísticas, ficha de datos completos, histórico, recompensas y acceso a la edición |
| **Formulario fallero** | Alta de un nuevo fallero o edición de uno existente |
| **Ver Familias** | Listado de familias, creación y gestión de miembros y cabeza de familia |
| **Ver Usuarios** | Gestión de los usuarios que pueden entrar en la aplicación |
| **Ver Cuentas Bancarias** | Cuentas para la domiciliación de cuotas y sus mandatos SEPA |
| **Administración** | Tres pestañas: **Ejercicio** (generar el nuevo año fallero), **Directiva** (cargos por ejercicio) y **Censo JCF** (importación y sincronización con la JCF) |

## Mapa de dominios

| Dominio | Documento |
|---|---|
| Censo de falleros | [censo.md](censo.md) |
| Familias | [familias.md](familias.md) |
| Cuentas bancarias y mandatos SEPA | [cuentas-bancarias.md](cuentas-bancarias.md) |
| Ejercicio (nuevo año fallero) | [ejercicio.md](ejercicio.md) |
| Histórico, directiva y recompensas | [historico-directiva.md](historico-directiva.md) |
| Censo JCF (importación y sincronización) | [censo-jcf.md](censo-jcf.md) |
| Usuarios y control de acceso | [usuarios-acceso.md](usuarios-acceso.md) |
| Catálogos de referencia | [catalogos.md](catalogos.md) |

## Mapa de conceptos de negocio
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

<pre class="mermaid">
graph TD
    FALLERO["Fallero<br/>(miembro del censo)"]
    FAMILIA["Familia<br/>(agrupación con cabeza de familia)"]
    CUENTA["Cuenta bancaria<br/>(IBAN + mandato SEPA)"]
    HISTORICO["Histórico<br/>(año, cargo, comisión, recompensa)"]
    EJERCICIO["Ejercicio<br/>(año fallero: 20 mar - 19 mar)"]
    JCF["Censo JCF<br/>(censo oficial de la Junta Central Fallera)"]
    USUARIO["Usuario<br/>(acceso a la aplicación)"]
    CATALOGOS["Catálogos<br/>(cargos, recompensas, fallas)"]

    FALLERO -->|"pertenece como máximo a una"| FAMILIA
    FAMILIA -->|"tiene una cabeza de familia"| FALLERO
    FALLERO -->|"domicilia su cuota en"| CUENTA
    FALLERO -->|"acumula registros de"| HISTORICO
    EJERCICIO -->|"al generarse añade un año al"| HISTORICO
    EJERCICIO -->|"promociona la categoría EPIM del"| FALLERO
    JCF -->|"actualiza los datos del"| FALLERO
    CATALOGOS -->|"aportan cargos y recompensas al"| HISTORICO
    USUARIO -->|"gestiona la aplicación"| FALLERO
</pre>

## Glosario

| Término | Significado |
|---|---|
| **Falla** | Asociación festiva valenciana. La aplicación gestiona la secretaría de una falla concreta. |
| **Fallero / fallera** | Persona inscrita en el censo de la falla. Puede pertenecer a la comisión masculina o femenina (comisión MF) y a la comisión infantil o mayor (comisión IM). |
| **JCF** | Junta Central Fallera, organismo oficial que mantiene el censo de falleros de todas las fallas. La aplicación puede importar su censo y compararlo con el propio. |
| **EPIM** | Categorías de cuota según la edad del fallero a 19 de marzo: **E** (Exento, menos de 1 año), **P** (Pitufo, hasta 4 años), **I** (Infantil, hasta 15 años) y **M** (Mayor, 16 años o más). |
| **Ejercicio (año fallero)** | Periodo anual de la falla: va del **20 de marzo al 19 de marzo** del año siguiente (por ejemplo, "2026-2027"). |
| **Mandato SEPA** | Autorización de domiciliación bancaria. Cada cuenta bancaria tiene una referencia de mandato única, generada automáticamente e inmutable. |
| **Directiva** | Conjunto de falleros con cargo de responsabilidad en un ejercicio (Presidente, Secretario, Tesorero, delegados, etc.). Quien no tiene cargo de directiva figura como Vocal. |
| **Recompensa** | Distinción oficial que la JCF concede por años de pertenencia (bunyols y distintius, en orden de rango creciente). Se anota en el histórico del fallero. |
| **Cabeza de familia** | Fallero que encabeza una familia y actúa como referente de la agrupación. Para serlo debe estar activo, tener asignada comisión MF (masculina o femenina) y no pertenecer ya a otra familia. |
| **Baja** | Fecha en la que un fallero deja la falla. El fallero nunca se elimina: queda en el censo como baja y su histórico se conserva. |
