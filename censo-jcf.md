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
- **Baja automática**: baja que el volcado registra, con fecha del día, para
  los falleros activos del censo propio que no aparecen en el Censo JCF.
- **Registro ambiguo**: registro de la JCF que podría corresponder a más de un
  fallero del censo; la aplicación no lo toca y lo señala para revisión
  manual.
- **Homónimos**: falleros del censo con exactamente el mismo nombre y
  apellidos (por ejemplo, una abuela y su nieta). Para distinguirlos se usa su
  fecha de nacimiento (ver RN-13).
- **Emparejado por**: columna de la hoja Actualizaciones del informe que
  indica cómo se identificó al fallero: por código JCF, por DNI, por nombre o
  por nombre y fecha de nacimiento (`nombre+f_nacimiento`).

## 3. Funcionalidades

Pantalla **Administración → pestaña Censo JCF**, con tres secciones:

- **Repoblar CensoJCF desde Excel**: seleccionar el fichero oficial de la JCF
  (.xls / .xlsx) y pulsar **Subir y repoblar CensoJCF**; se pide confirmación
  indicando cuántos registros se importarán.
- **Listado CensoJCF**: consulta de la copia local con filtros por apellidos,
  nombre y DNI, y paginación.
- **Comparar JCF con Censo**:
  - **Comparar (dry-run)**: calcula las diferencias y muestra los indicadores
    "Falleros con cambios", "Solo en JCF", "Ambiguos", "Total cambios de
    campo" y "Bajas automáticas".
  - **Descargar informe (.xlsx)**: Excel con hojas Resumen, Actualizaciones,
    Solo en CensoJCF, Solo en Censo y Avisos. El Resumen incluye "Falleros
    que se darán de baja" y la hoja Solo en Censo indica, por fallero, si "Se
    dará de baja".
  - **Volcar a Censo**: aplica los cambios al censo propio y da de baja a los
    falleros que ya no figuran en la JCF, con diálogo de confirmación que
    indica cuántos falleros se actualizarán y cuántos se darán de baja.

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
  mismo nivel, todos se marcan como ambiguos y no se actualiza nada. En el
  nivel de nombre, los homónimos se desempatan por fecha de nacimiento
  (RN-13).
- **RN-08 — Volcado protegido.** El botón **Volcar a Censo** permanece
  deshabilitado hasta haber ejecutado una comparación con cambios pendientes,
  y siempre exige confirmación. Tras un volcado o una repoblación se descarta
  la comparación anterior (ya no es válida) y hay que volver a comparar.
- **RN-09 — Baja automática de quien ya no está en la JCF.** Al volcar, todo
  fallero activo del censo propio que no figura en el Censo JCF se da de baja
  con la fecha del día del volcado. Los falleros que ya estaban de baja no se
  tocan (se conserva su fecha de baja original). Como cualquier baja, el
  fallero no se borra y puede reactivarse vaciando su fecha de baja desde el
  censo.
- **RN-10 — Excepciones a la baja automática.** No se da de baja a nadie si
  el Censo JCF está vacío (evita dar de baja a todo el censo por error), ni a
  los falleros implicados en un registro ambiguo, porque podrían ser la
  persona que la JCF sí tiene censada; quedan en el informe para revisión
  manual.
- **RN-11 — Las altas nuevas de la JCF no se crean.** Los registros que solo
  están en el Censo JCF (normalmente altas nuevas) no se incorporan al censo
  propio, porque la JCF no aporta todos los datos necesarios; la secretaría
  debe darlos de alta a mano. Aparecen en el informe, hoja Solo en CensoJCF.
- **RN-12 — Los DNI provisionales de la JCF no se vuelcan.** La JCF asigna a
  quien no tiene DNI (normalmente menores) un código provisional formado por
  dos letras y su fecha de nacimiento (p. ej. `FN20170504`); los hermanos
  gemelos comparten el mismo. Estos códigos no son DNI: no se usan para
  emparejar registros por DNI ni se copian al censo, que conserva el DNI
  vacío; solo se aprovecha la fecha que contienen para desempatar homónimos
  (RN-13). Si dos
  registros de la JCF aportan el mismo DNI real para falleros distintos, solo
  se vuelca en el primero y el caso aparece como aviso en el informe.
- **RN-13 — Homónimos: desempate por fecha de nacimiento exacta.** Cuando un
  registro de la JCF solo puede emparejarse por nombre y en el censo hay
  varios falleros con ese mismo nombre y apellidos, se elige al que tiene
  **exactamente la misma fecha de nacimiento** que el registro JCF. La fecha
  de la JCF es su fecha de nacimiento o, si no la tiene, la contenida en su
  DNI provisional (p. ej. `FN20150312` → 12/03/2015). Si exactamente un
  fallero coincide, se empareja con él y el informe lo indica en la columna
  "Emparejado por" como `nombre+f_nacimiento`. Si no coincide ninguno (falta
  la fecha en el censo o en la JCF) o coinciden varios (homónimos nacidos el
  mismo día, como gemelos), el registro queda ambiguo y no se actualiza. No se
  admiten coincidencias aproximadas ni solo por año. Si dos registros JCF se
  resuelven así hacia el mismo fallero, ambos quedan ambiguos (RN-07b). El
  aviso de ambigüedad indica: "Revisa que la fecha de nacimiento o el DNI de
  esos falleros estén informados en el Censo. IdCenso posibles: …".

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
  entre CensoJCF y Censo". Si aparecen registros ambiguos por homónimos, se
  completa en el censo la fecha de nacimiento o el DNI de los falleros
  indicados en el aviso (IdCenso posibles) y se vuelve a comparar (RN-13).

### CU-03 — Volcar las diferencias al censo

- **Actor**: secretario/a.
- **Precondición**: se ha ejecutado una comparación y hay falleros con cambios
  o bajas automáticas pendientes.
- **Pasos**: **Volcar a Censo** → revisar el aviso con el número de falleros a
  actualizar y a dar de baja → **Sí, aplicar volcado**.
- **Resultado**: mensaje "Volcado completado: … fallero(s) actualizados y …
  dado(s) de baja en Censo"; las diferencias se recalcularon justo antes de
  aplicarse (RN-06) y los falleros ausentes de la JCF quedan de baja con
  fecha del día (RN-09). Tras el volcado hay que volver a comparar para ver
  el nuevo estado.
- **Errores / alternativas**: los registros ambiguos y sus falleros no se
  tocan (RN-07, RN-10); las altas nuevas de la JCF no se crean (RN-11). Si
  algún fallero se dio de baja por error, se reactiva vaciando su fecha de
  baja en el censo.

## 6. Relación con otros dominios

- [Censo](censo.md) — el volcado actualiza los datos de los falleros del censo
  propio, vincula su código JCF y da de baja a quienes ya no figuran en la
  JCF.
