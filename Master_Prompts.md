Crea una aplicación web completa en un único archivo HTML autocontenido (sin dependencias externas salvo Tailwind CDN) que funcione como un REPOSITORIO MAESTRO DE HERRAMIENTAS DE INGENIERÍA con las siguientes características:

---

## IDENTIDAD
- Título: "Repositorio Maestro v6.0 — Centro de Ejecución"
- Subtítulo: "Ingeniería & Trazabilidad de Activos"
- Diseño oscuro en el header (slate-900), fondo general claro (slate-50)
- Estilo: profesional, compacto, con animaciones sutiles en tarjetas (hover eleva y añade borde azul)

---

## FUNCIONALIDADES PRINCIPALES

### 1. Conexión a carpeta local (FileSystem Access API)
- Botón "CONECTAR CARPETA WINDOWS" que llama a `window.showDirectoryPicker({ mode: 'readwrite' })`
- Al conectar: indicador LED cambia de rojo a verde, se muestra el nombre de la carpeta raíz
- Sin conexión: el botón Guardar permanece deshabilitado

### 2. Formulario de registro con 5 campos:
- **Nombre del Proyecto** (input text) → se convierte en nombre de subcarpeta en mayúsculas con guiones bajos
- **Master Prompt** (textarea oscuro fondo slate-900, texto amarillo) → se guarda en archivo .txt
- **Resumen de la versión** (textarea claro) → va al log de auditoría
- **Código HTML ejecutable** (textarea oscuro, texto cyan) → se guarda en archivo .html
- **Informe de validación** (textarea oscuro, texto verde esmeralda) → se guarda en archivo .md

### 3. Guardado cuádruple en sistema de archivos local
Al pulsar guardar, el sistema debe:
1. Crear/acceder a la subcarpeta del proyecto (nombre en MAYÚSCULAS_CON_GUIONES)
2. Generar timestamp ISO 8601 como `YYYY-MM-DD_HH-mm`
3. Escribir simultáneamente 4 archivos en esa subcarpeta:
   - `app_YYYY-MM-DD_HH-mm.html` → el código HTML
   - `informe_YYYY-MM-DD_HH-mm.md` → el informe de validación
   - `prompt_YYYY-MM-DD_HH-mm.txt` → el master prompt
   - `audit_YYYY-MM-DD_HH-mm.txt` → nombre, fecha y descripción
4. Registrar la herramienta en localStorage bajo la clave `repo_v6_engine`
5. Si ya existe una entrada con el mismo nombre de carpeta, actualizarla (no duplicar)

### 4. Motor de ejecución directa (Blob URLs)
- Cada tarjeta del panel tiene un botón "⚡ EJECUTAR APLICACIÓN"
- Al pulsar: crear un Blob con el código HTML, generar URL temporal con `URL.createObjectURL`, abrir en nueva pestaña (`_blank`), revocar la URL a los 10 segundos

### 5. Panel de activos (tarjetas)
- Grid responsive (1/2/3 columnas según pantalla)
- Cada tarjeta muestra: badge VIVO, fecha de última actualización, nombre del proyecto, descripción (máx. 2 líneas), ruta de la carpeta, botón de ejecución
- Botón "Limpiar Vista Local" que limpia localStorage sin borrar archivos del disco
- Contador de activos registrados en el header

---

## PERSISTENCIA
- localStorage: clave `repo_v6_engine`, array de objetos `{ id, name, folder, lastUpdate, description, code }`
- Los archivos físicos no se tocan al limpiar la vista local

---

## STACK TÉCNICO
- HTML5 + Tailwind CSS (CDN)
- Vanilla JavaScript (sin frameworks)
- FileSystem Access API nativa del navegador
- Blob URLs para ejecución aislada
- localStorage para persistencia del panel

---

## COMPORTAMIENTO UX
- Feedback visual de guardado exitoso: texto verde "✓ REGISTRADO CORRECTAMENTE" aparece y desaparece en 3 segundos
- Validación: nombre y código son obligatorios (alert si faltan)
- Scrollbar personalizado en textareas (color azul, fino)
- Botón guardar: estado disabled estilizado en gris, activo en azul
- Confirmación antes de limpiar el panel local
