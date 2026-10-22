# Censo JCF — importación y sincronización

[← Volver al índice](index.md)

## 1. Propósito

Mantener el censo de la falla alineado con el censo oficial de la Junta
Central Fallera (JCF): importar el fichero oficial, comparar ambos censos sin
tocar nada y, cuando la secretaría lo decida, volcar las diferencias al censo
propio con plena trazabilidad.

## 2. Conceptos

- **Censo JCF**: copia local del censo oficial de la JCF, importada desde el
  fichero Excel que distribuye la Junta.
- **Repoblación**: importación que vacía la copia local y la reemplaza por el
  contenido del fichero.
- **Comparación (dry-run / simulación)**: cálculo de diferencias entre el
  Censo JCF y el censo propio **sin aplicar ningún cambio**.
- **Volcado**: aplicación de las diferencias detectadas sobre el censo propio.
- **Registro ambiguo**: registro de la JCF que podría corresponder a más de un
  fallero del censo; la aplicación no lo toca y lo señala para revisión
  manual.

## 3. Funcionalidades

Pantalla **Administración → pestaña Censo JCF**, con tres secciones:

- **Repoblar CensoJCF desde Excel**: seleccionar el fichero oficial de la JCF
  (.xls / .xlsx) y pulsar **Subir y repoblar CensoJCF**; se pide confirmación
  indicando cuántos registros se importarán.
- **Listado CensoJCF**: consulta de la copia local con filtros por apellidos,
  nombre y DNI, y paginación.
- **Comparar JCF con Censo**:
  - **Comparar (dry-run)**: calcula las diferencias y muestra los indicadores
    "Falleros con cambios", "Solo en JCF", "Ambiguos" y "Total cambios de
    campo".
  - **Descargar informe (.xlsx)**: Excel con hojas Resumen, Actualizaciones,
    Solo en CensoJCF, Solo en Censo y Avisos.
  - **Volcar a Censo**: aplica los cambios al censo propio, con diálogo de
    confirmación que indica cuántos falleros se actualizarán.

## 4. Reglas de negocio

- **RN-01 — La repoblación reemplaza todo.** Importar el fichero vacía la
  copia local del Censo JCF y la sustituye íntegramente por el contenido del
  Excel; se pide confirmación expresa antes de hacerlo.
- **RN-02 — Duplicados bloquean la importación.** Si el fichero contiene DNIs
  o códigos JCF (COD.JCF) repetidos, la importación completa se rechaza
  indicando cuáles son, **antes** de vaciar la copia local, que queda intacta.
- **RN-03 — El fichero debe ser el oficial.** Si las cabeceras del Excel no
  son las esperadas, el fichero se rechaza mostrando las cabeceras recibidas.
- **RN-04 — Fechas de nacimiento incompletas.** Los registros cuya fecha de
  nacimiento solo indica el año se importan sin fecha de nacimiento y se
  informa de cuántos son. Se aceptan fechas completas en formato DD-MM-AAAA,
  DD/MM/AAAA o como celda de fecha de Excel. Los códigos postales numéricos
  recuperan el cero inicial (3001 → 03001).
- **RN-05 — La comparación no modifica nada.** El dry-run es una simulación:
  calcula y muestra las diferencias y permite exportarlas a Excel, sin tocar
  el censo.
- **RN-06 — El volcado recalcula siempre las diferencias.** Justo antes de
  aplicar los cambios se recalcula la comparación, para no aplicar nunca datos
  desfasados.
- **RN-07 — Solo cambian los campos con novedad.** El volcado actualiza
  únicamente los campos en los que la JCF aporta un valor distinto del que
  consta en el censo; los registros ambiguos no se modifican (quedan en el
  informe para revisión manual).
- **RN-07b — Un registro JCF por fallero.** Cada fallero del censo se empareja
  como mucho con un registro JCF. El emparejamiento se hace por niveles (código
  JCF, luego DNI, luego nombre) y un fallero emparejado en un nivel no vuelve a
  emparejarse en otro. Si varios registros JCF apuntan al mismo fallero en el
  mismo nivel, todos se marcan como ambiguos y no se actualiza nada.
- **RN-08 — Volcado protegido.** El botón **Volcar a Censo** permanece
  deshabilitado hasta haber ejecutado una comparación con cambios pendientes,
  y siempre exige confirmación. Tras un volcado o una repoblación se descarta
  la comparación anterior (ya no es válida) y hay que volver a comparar.

## 5. Casos de uso

### CU-01 — Importar el censo oficial de la JCF

- **Actor**: secretario/a.
- **Precondición**: se dispone del fichero Excel oficial de la JCF.
- **Pasos**: **Administración** → pestaña **Censo JCF** → seleccionar el
  fichero → **Subir y repoblar CensoJCF** → confirmar en el diálogo.
- **Resultado**: mensaje "CensoJCF repoblado: … registros importados", con
  aviso de los registros con fecha de nacimiento de solo año si los hay
  (RN-04).
- **Errores / alternativas**: DNIs duplicados (RN-02) o cabeceras incorrectas
  (RN-03) → la importación se rechaza y la copia local no cambia.

### CU-02 — Comparar el censo propio con el de la JCF

- **Actor**: secretario/a.
- **Precondición**: la copia local del Censo JCF tiene datos.
- **Pasos**: sección **Comparar JCF con Censo** → **Comparar (dry-run)** →
  revisar los indicadores → **Descargar informe (.xlsx)**.
- **Resultado**: visión completa de las diferencias sin haber modificado nada
  (RN-05); el Excel documenta cada cambio propuesto, los registros solo
  presentes en un censo, los ambiguos y los avisos.
- **Errores / alternativas**: sin diferencias → "No se detectaron diferencias
  entre CensoJCF y Censo".

### CU-03 — Volcar las diferencias al censo

- **Actor**: secretario/a.
- **Precondición**: se ha ejecutado una comparación y hay falleros con cambios.
- **Pasos**: **Volcar a Censo** → revisar el aviso con el número de falleros a
  actualizar → **Sí, aplicar volcado**.
- **Resultado**: mensaje "Volcado completado: … fallero(s) actualizados en
  Censo"; las diferencias se recalcularon justo antes de aplicarse (RN-06).
- **Errores / alternativas**: si tras recalcular no queda nada pendiente, se
  informa "No hay cambios pendientes que aplicar"; los registros ambiguos no
  se tocan (RN-07).

## 6. Relación con otros dominios

- [Censo](censo.md) — el volcado actualiza los datos de los falleros del censo
  propio y vincula su código JCF.
