# Catálogos de referencia

[← Volver al índice](index.md)

## 1. Propósito

Los catálogos son las listas de valores oficiales que alimentan los
desplegables de la aplicación: los cargos de la falla, las recompensas de la
JCF y el catálogo de fallas censadas. Garantizan que todo el mundo use los
mismos nombres y el mismo orden.

## 2. Conceptos

- **Catálogo de cargos**: lista de los cargos de la falla, con su orden de
  precedencia y la marca de si son cargos de directiva. Incluye desde
  PRESIDENTE hasta los distintos delegados, además de VOCAL (que no es cargo
  de directiva y representa al fallero de base).
- **Catálogo de recompensas**: distinciones oficiales de la JCF en orden de
  rango creciente — cinco *bunyols* (Bunyol de Coure, Bunyol d'Argent, Bunyol
  d'Or, Bunyol d'Or amb Fulles de Llorer, Bunyol d'Or i Brillants amb Fulles
  de Llorer) y tres *distintius* (Distintiu de Coure, Distintiu d'Argent,
  Distintiu d'Or).
- **Catálogo de fallas**: relación de fallas censadas, de consulta.

## 3. Funcionalidades

Los catálogos no tienen pantalla propia de mantenimiento; trabajan por detrás
de otras pantallas:

- El **catálogo de cargos** alimenta los desplegables de cargo en
  **Administración → Directiva** (diálogos "Asignar cargo a fallero" y
  "Editar directiva del ejercicio") y determina el orden en que se muestra la
  tabla de la directiva.
- El **catálogo de recompensas** alimenta el desplegable del diálogo
  **Asignar recompensa** de la ficha del fallero (**Ver Falleros** →
  **Recompensas**).
- El **catálogo de fallas** es de consulta para referenciar fallas censadas.

## 4. Reglas de negocio

- **RN-01 — Valores predefinidos.** Los catálogos de cargos y recompensas se
  cargan automáticamente con sus valores oficiales cuando la aplicación se
  instala en un entorno nuevo, y solo si están vacíos (nunca se duplican ni se
  sobrescriben valores existentes).
- **RN-02 — Orden de precedencia de cargos.** Los cargos tienen un orden
  oficial (Presidente, Vicepresidentes, Secretario, Tesorero, Contador,
  delegados…) que se respeta al mostrar la directiva.
- **RN-03 — VOCAL no es directiva.** El cargo VOCAL existe en el catálogo como
  cargo de base, pero no se ofrece al componer la directiva ni aparece en su
  tabla.
- **RN-04 — Recompensas en orden de rango.** Las recompensas se ofrecen y
  muestran en su orden de rango oficial, del Bunyol de Coure al Distintiu
  d'Or.
- **RN-05 — Fallas de solo consulta.** El catálogo de fallas no se modifica
  desde la aplicación; proviene de la carga inicial de datos.

## 5. Casos de uso

### CU-01 — Elegir un cargo al componer la directiva

- **Actor**: secretario/a.
- **Pasos**: **Administración** → **Directiva** → **Asignar cargo** → el
  desplegable **Cargo** ofrece únicamente los cargos de directiva del
  catálogo, en su orden oficial (RN-02, RN-03).
- **Resultado**: el cargo asignado usa siempre la denominación oficial.
- **Errores / alternativas**: si el catálogo estuviera vacío, la pantalla
  avisa "No hay cargos de directiva disponibles en el catálogo".

### CU-02 — Elegir una recompensa

- **Actor**: secretario/a.
- **Pasos**: **Ver Falleros** → seleccionar fallero → **Recompensas** → el
  desplegable **Recompensa** ofrece el catálogo completo en orden de rango
  (RN-04).
- **Resultado**: la recompensa anotada en el histórico usa la denominación
  oficial de la JCF.
- **Errores / alternativas**: catálogo vacío → aviso "No hay recompensas
  disponibles en el catálogo".

## 6. Relación con otros dominios

- [Histórico y directiva](historico-directiva.md) — cargos y recompensas se
  anotan en el histórico de los falleros.
- [Censo](censo.md) — las recompensas se asignan desde la ficha del fallero.
