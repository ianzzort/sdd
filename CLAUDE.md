# CLAUDE.md

Plantilla del archivo de agente que va en la raíz del repositorio. Copiarla,
borrar lo que no aplique y llenar los `<...>`. Las reglas de aquí son generales:
sirven en cualquier proyecto. Lo específico del proyecto (dominios, cálculos,
contratos externos) va en `sdd_architecture/context/Documentation.md`, no aquí.

## Modo de trabajo

- Ejecutar siempre el modo `ponytail` (skill `ponytail:ponytail`, nivel `full`)
  en cada sesión: la solución más simple que funcione, reusar lo que ya existe
  antes de crear, sin abstracciones ni scaffolding especulativo. Si ya está
  activo, no reinvocarlo.

## Arquitectura SDD

- En cada cambio de este proyecto, entrar primero a
  `sdd_architecture/Instruction.md` (el cerebro del SDD) y seguir su flujo. Es
  la puerta de entrada: el detalle completo (contexto, skills y actualización
  del grafo) vive ahí, no en este archivo.

## Estilo de código

- No comentar el código, ni en inglés ni en español. El código debe ser
  autoexplicativo.
- Los formularios complejos deben actuar como orquestadores. Extraer secciones
  visuales, validaciones y utilidades a archivos dedicados por dominio; no
  concentrar todo el flujo en un solo componente.
- Mover una sección a `shared` solo cuando exista reutilización real entre dos o
  más flujos. Las particularidades de cada flujo viven en su propia carpeta,
  para evitar componentes compartidos llenos de condiciones.
- Los nombres de archivo siempre en inglés (kebab-case para módulos y
  utilidades, PascalCase para componentes React). El contenido del archivo puede
  ir en español (docs, mensajes de UI).
- Todo selector desplegable debe tener filtro por texto. Nunca usar el `Select`
  plano para listas seleccionables. Cuál usar depende de la **naturaleza del
  dato**, no del largo de la lista: valores estáticos, datos dinámicos de la
  base, o búsqueda server-side. Declarar en `Documentation.md` qué componente
  cubre cada caso en este proyecto.

## Fechas y horas

- Definir una sola zona horaria para el proyecto y declararla en
  `Documentation.md`. Toda fecha y hora se maneja en esa zona.
- Los campos de solo fecha (vencimiento, producción, etc.) se guardan como
  medianoche UTC y representan un día calendario. Mostrarlos por sus componentes
  UTC con el helper del proyecto; nunca con `new Date(x).toLocaleDateString()`
  directo, porque la conversión a hora local los retrocede un día.

## Commits

- Usar Conventional Commits (feat, fix, chore, refactor, etc.).
- Escribir los mensajes de commit en inglés.
- No incluir atribución a Claude ni menciones de que el cambio fue generado con
  IA (nada de `Co-Authored-By: Claude`, `Generated with Claude Code`, etc.).
- No incluir enlaces a chats ni sesiones.
- Ser concisos: no repetir la misma idea en el título y el cuerpo, ni enumerar
  cada detalle del diff.
- No usar emojis, ni en el título ni en el cuerpo.

## Tickets

- No usar emojis en los tickets, ni en el título ni en la descripción ni en los
  comentarios.
- No mencionar que el ticket o el cambio fue generado con IA, ni incluir enlaces
  a chats o sesiones.
- Ser concisos: describir el problema y el alcance, sin enumerar cada detalle
  del diff.

## Base de datos

- Para consultar o verificar datos, conectarse **siempre con el cliente del ORM
  del proyecto**, inline (por ejemplo `DATABASE_URL=... node -e '...'`), nunca
  creando archivos de script ni con clientes SQL sueltos. No dejar archivos de
  consulta en el repo.
- Las migraciones se generan siempre con la herramienta del ORM a partir del
  schema, nunca se escriben de cero. **El `.sql` tampoco se edita después de
  generado**: si quedó mal, se corrige el schema y se genera otra migración.
- **Los renombres se hacen expandiendo y contrayendo, en dos migraciones
  generadas.** La herramienta no infiere renombres: emite `DROP COLUMN` +
  `ADD COLUMN`, que borra los datos. El renombre va así: (1) agregar la columna
  nueva como **opcional**, conviviendo con la vieja, en una migración generada;
  (2) llenar los datos con el cliente del ORM inline, sin dejar scripts en el
  repo; (3) verificar que no queden filas sin migrar, y parar a reportar si
  alguna no se puede mapear; (4) recién entonces hacerla obligatoria y borrar la
  vieja, en una segunda migración generada. Revisar siempre el bloque de
  advertencias que imprime la herramienta: si dice que se perderán los datos de
  una columna que debe conservarse, la migración está mal aunque el schema esté
  bien.
- Una migración que ya se aplicó a una base compartida no se toca: cualquier
  corrección va en una migración nueva.
- Nunca resetear la base para salir de un problema de migraciones: borra toda la
  base de desarrollo, no solo el dominio que se está tocando.
- El nombre de la migración siempre en inglés (kebab-case).
- Todo lo que quede escrito en el schema o en el SQL va en inglés: nombres de
  modelo y de enum, nombres de campo, mapeos de tabla y columna, y valores de
  enum. Excepción: los modelos que calcan a propósito el contrato de un sistema
  externo se dejan en el idioma de ese contrato, y la razón se anota en
  `Documentation.md`. El contenido no-schema (mensajes de UI) sí puede ir en
  español.

## Seeds

- Los catálogos que deben existir siempre en producción se registran en la
  carpeta de seeds y se cablean en el bloque de seeds que corre en cada
  arranque, para que lleguen a producción en cada deploy sin depender del seed
  completo.
- Los seeds de catálogo deben ser idempotentes e **insert-only**: insertar solo
  lo que falta, sin reescribir filas ya presentes ni modificadas.
- El seed completo (el de base nueva) no es lugar para estos catálogos: solo
  corre una vez y suele resetear el admin.
- No crear scripts de seed sueltos con su propio comando en `package.json`:
  usar el mecanismo de seeds del proyecto.

## Agentes y costo

- Bash primero. Un `diff -rq`, un `grep` o un conteo de líneas resuelven la
  mayor parte de una exploración a costo casi cero. Recién después, agentes.
- Los agentes se usan puntualmente, para el caso ambiguo, de a uno o pocos, un
  dominio a la vez.
- **Nunca** hacer fan-outs grandes de agentes para clasificar o inventariar:
  queman el límite y el triage por bash da el ~80% gratis.

## Secretos

- Ningún secreto en el código ni en el repositorio. Todo secreto se lee del
  entorno y se declara en `.env.example` con el valor vacío.
- Nunca imprimir un secreto en un log, en una URL o en un mensaje de error.
