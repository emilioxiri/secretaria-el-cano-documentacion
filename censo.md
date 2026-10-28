# Censo de falleros

[← Volver al índice](index.md)

## 1. Propósito

El censo es el corazón de la secretaría: el registro de todas las personas que
pertenecen o han pertenecido a la falla. Permite dar de alta a nuevos falleros,
mantener sus datos al día, registrar su baja cuando dejan la falla y consultar
en todo momento la composición de la comisión.

## 2. Conceptos

- **Fallero/a**: persona inscrita en el censo, con sus datos personales,
  dirección, contacto, comisión y cuota.
- **Comisión MF**: comisión Masculina o Femenina a la que pertenece el fallero.
- **Comisión IM**: comisión Infantil o Mayor, según la edad.
- **Cuota EPIM**: categoría de cuota ligada a la edad — E (Exento, primer
  ejercicio), P (Pitufo, hasta 4 años), I (Infantil, hasta 15 años),
  M (Mayor, 16+ años).
- **Fecha de alta**: fecha en la que el fallero se apunta a la falla.
- **Fecha de baja**: fecha en la que deja la falla. Un fallero con fecha de
  baja es una "baja"; sin ella, es un fallero "activo".
- **Pulsera**: identificador de la pulsera de acceso, si la tiene.
- **Código JCF**: código del fallero en el censo oficial de la Junta Central
  Fallera, cuando está vinculado (ver [Censo JCF](censo-jcf.md)).

## 3. Funcionalidades

Pantalla **Ver Falleros** (menú lateral):

- Consultar el listado del censo con paginación (tamaño de página configurable
  y salto directo a una página).
- Filtrar por **Nombre**, **Apellidos** y **DNI** (sección "Filtrar Falleros").
- Alternar entre falleros activos y bajas con los botones **Ver bajas** /
  **Ver activos**.
- Consultar el **Resumen del censo activo**: falleros activos, hombres y
  mujeres de comisión mayor, hombres y mujeres de comisión infantil.
- Seleccionar un fallero en la tabla para abrir sus acciones:
  - **Datos completos**: ficha con código JCF, fecha de nacimiento, fecha de
    baja, comisiones, cuota, IBAN y pulsera, más la sección **Familia** (cabeza
    y miembros) con la opción **Asignar / Mover familia**.
  - **Histórico**: años, cargos, comisiones y recompensas del fallero.
  - **Recompensas**: asignar una recompensa a un año de su histórico.
  - **Editar**: abre el **Formulario fallero** en modo edición.

Pantalla **Formulario fallero** (menú lateral):

- **Alta de fallero** con secciones: Datos personales, Dirección, Comisión y
  cuota, Datos bancarios y Otros (fecha de alta, pulsera). La **Fecha baja**
  no se ofrece en el alta: solo tiene sentido sobre un fallero ya existente.
- **Edición de fallero** (misma pantalla, con el aviso "Estás modificando los
  datos de un fallero existente"): todos los campos editables salvo el
  identificador del censo y el código de Censo JCF, que se muestran solo como
  consulta. Incluye el campo **Fecha baja** para registrar la baja.
- Selección de la cuenta bancaria para la domiciliación desde un desplegable de
  cuentas existentes (opción "Sin cuenta bancaria" disponible). En edición, la
  cuenta que ya tiene asignada el fallero aparece siempre seleccionada, aunque
  sea una cuenta antigua sin IBAN, para que guardar no la desvincule.
- Validación campo a campo con mensajes de error visibles junto al formulario.
- El campo **DNI** se muestra como opcional ("DNI") mientras no haya fecha de
  nacimiento o el fallero sea menor de 16 años, y pasa a obligatorio ("DNI*")
  en cuanto la fecha de nacimiento indica 16 años o más (RN-02). El cambio es
  inmediato al introducir la fecha, sin perder lo ya escrito en el DNI.

## 4. Reglas de negocio

- **RN-01 — Campos obligatorios.** Para guardar un fallero son obligatorios:
  nombre, apellidos, fecha de nacimiento, teléfono, calle, población, código
  postal, comisión MF, comisión IM, cuota EPIM y fecha de alta.
- **RN-02 — DNI obligatorio desde los 16 años y siempre verificado.** El DNI
  es **obligatorio** si el fallero tiene **16 años o más** a la fecha de hoy
  (edad real, no la del 19 de marzo que usa RN-05) y **opcional** si es menor
  de 16 años, porque puede no tenerlo. Sin fecha de nacimiento el DNI se
  considera opcional. Si falta en un fallero de 16 años o más se muestra el
  aviso "El DNI es obligatorio para falleros de 16 años o más." y no se
  guarda. Si se informa, debe tener 8 números y una letra, y la letra de
  control debe ser la correcta; en caso contrario el alta se rechaza. La regla
  se aplica tanto en el alta como en la edición.
- **RN-03 — Fecha de nacimiento verosímil.** No puede ser futura ni suponer
  una edad superior a 120 años.
- **RN-04 — Email con formato válido.** El email es opcional; si se informa,
  debe tener un formato válido.
- **RN-05 — Cuota EPIM acorde a la edad.** La edad se calcula a **19 de marzo**
  (cierre del ejercicio en curso): E si es menor de 1 año, P hasta 4 años,
  I hasta 15 años, M a partir de 16. Si la cuota elegida no corresponde con la
  edad, el formulario avisa indicando la categoría esperada y no permite
  guardar.
- **RN-06 — Alta con histórico automático.** Al dar de alta un fallero se crea
  automáticamente un registro en su histórico como **VOCAL** del ejercicio
  actual (anotado con el **año de inicio** del ejercicio, p. ej. 2026 para
  2026-2027, igual que el resto del histórico), con su comisión IM.
- **RN-07 — La baja es una fecha, nunca un borrado.** Un fallero jamás se
  elimina del censo: la baja se registra rellenando la **Fecha baja** en el
  formulario de edición. Su histórico y sus datos se conservan. La fecha de
  baja no puede ser anterior a la fecha de alta.
- **RN-08 — El listado muestra activos por defecto.** Las bajas solo se ven al
  pulsar **Ver bajas**; las estadísticas del resumen cuentan únicamente
  falleros activos.
- **RN-09 — Identificadores no editables.** El identificador del censo y el
  código de Censo JCF no pueden modificarse desde el formulario.

## 5. Casos de uso

### CU-01 — Alta de un fallero

- **Actor**: secretario/a.
- **Precondición**: sesión iniciada; datos personales del nuevo fallero.
- **Pasos**: menú **Formulario fallero** → rellenar Datos personales,
  Dirección, Comisión y cuota, Datos bancarios (opcional) y Otros → pulsar
  **Añadir Fallero**.
- **Resultado**: mensaje "Fallero añadido correctamente"; el fallero aparece en
  **Ver Falleros** y su histórico ya contiene el año actual como VOCAL (RN-06).
- **Errores / alternativas**: si falta un campo obligatorio o la cuota no
  corresponde con la edad, se muestran los errores campo a campo y no se
  guarda (RN-01, RN-05); si el DNI es incorrecto, o falta y el fallero tiene
  16 años o más, se rechaza (RN-02).

### CU-02 — Edición de datos de un fallero

- **Actor**: secretario/a.
- **Precondición**: el fallero existe en el censo.
- **Pasos**: **Ver Falleros** → seleccionar al fallero en la tabla → **Editar**
  → modificar los campos → **Guardar cambios**.
- **Resultado**: mensaje "Fallero actualizado correctamente".
- **Errores / alternativas**: mismas validaciones que en el alta, incluida la
  obligatoriedad del DNI a partir de 16 años (RN-02): un fallero de 16 años o
  más que no tenga DNI registrado no podrá guardarse hasta que se informe;
  cambiar la fecha de nacimiento también vuelve a evaluar la regla.
  **Cancelar** vuelve al listado sin guardar.

### CU-03 — Baja de un fallero

- **Actor**: secretario/a.
- **Precondición**: el fallero está activo.
- **Pasos**: **Ver Falleros** → seleccionar al fallero → **Editar** → rellenar
  **Fecha baja** → **Guardar cambios**.
- **Resultado**: el fallero deja de aparecer en el listado de activos y pasa a
  verse con **Ver bajas**; sus datos e histórico se conservan (RN-07).
- **Errores / alternativas**: para reactivar a un fallero basta con vaciar su
  fecha de baja desde la misma pantalla. Las bajas también pueden registrarse
  de forma automática al volcar el censo de la JCF (ver
  [Censo JCF](censo-jcf.md)).

### CU-04 — Consulta del censo con filtros

- **Actor**: cualquier usuario de la aplicación.
- **Pasos**: **Ver Falleros** → abrir "Filtrar Falleros" → escribir nombre,
  apellidos o DNI (búsqueda parcial) → navegar por páginas.
- **Resultado**: tabla filtrada con el total de falleros mostrados y el resumen
  estadístico del censo activo.
- **Errores / alternativas**: si no hay coincidencias se informa "No se
  encontraron falleros con los filtros seleccionados".

### CU-05 — Consulta del histórico y asignación de recompensa

- **Actor**: secretario/a.
- **Precondición**: el fallero tiene registros de histórico.
- **Pasos**: **Ver Falleros** → seleccionar fallero → **Histórico** para
  consultar, o **Recompensas** para elegir año y recompensa y pulsar
  **Guardar recompensa**.
- **Resultado**: la recompensa queda anotada en el año elegido (ver
  [Histórico y directiva](historico-directiva.md)).
- **Errores / alternativas**: si el fallero no tiene histórico o el catálogo de
  recompensas está vacío, la pantalla lo indica y no permite asignar.

## 6. Relación con otros dominios

- [Familias](familias.md) — desde la ficha del fallero se consulta y asigna su
  familia; la columna Familia del listado muestra la cabeza.
- [Cuentas bancarias](cuentas-bancarias.md) — la domiciliación de la cuota se
  asigna en el formulario del fallero.
- [Histórico y directiva](historico-directiva.md) — cada alta genera histórico
  y las recompensas se anotan por año.
- [Ejercicio](ejercicio.md) — la generación del nuevo año fallero promociona
  las cuotas EPIM y añade un año al histórico de cada fallero activo.
- [Censo JCF](censo-jcf.md) — los datos del censo pueden actualizarse desde el
  censo oficial de la JCF, y el volcado da de baja a los falleros activos que
  ya no figuran en él.
