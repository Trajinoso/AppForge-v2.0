# INFORME DE VALIDACIÓN TÉCNICA: SISTEMA DE REPOSITORIO (v6.0)

**ID_INFORME:** INF-20260403-06 | **VERSIÓN:** 6.0 | **FECHA:** 2026-04-03
**ESTADO:** ✅ PRODUCCIÓN / VALIDADO

---

## 1. Especificaciones de Diseño

El sistema ha sido evolucionado para actuar como un **Dashboard de Ejecución Directa**. Se han cumplido los siguientes requerimientos de ingeniería:

- **Persistencia Física:** Integración con la API de acceso al sistema de archivos para creación de subcarpetas locales de forma automática.
- **Trazabilidad Total:** Generación de 4 activos por versión (Código, Informe, Prompt y Auditoría).
- **Acceso Directo:** Implementación de un motor de ejecución basado en Blob URLs para lanzar aplicaciones desde la interfaz sin salir del navegador.
- **Seguridad:** Aislamiento de procesos mediante apertura en pestañas independientes (`_blank`), garantizando que fallos en las aplicaciones secundarias no afecten al repositorio principal.

---

## 2. Matriz de Correspondencia (Solicitud vs. Respuesta)

| Requerimiento Usuario | Solución Técnica Implementada | Estado |
|---|---|---|
| Carpeta Raíz Variable | Integración con FileSystem Access API (`showDirectoryPicker`). | ✅ OK |
| No Sobrescribir | Estampado de tiempo (ISO Timestamp) en nombres de archivos. | ✅ OK |
| Campo para Prompts | Área de texto dedicada y guardado en archivo `.txt` independiente. | ✅ OK |
| Acceso Directo | Almacenamiento de código en `localStorage` y ejecución vía `URL.createObjectURL`. | ✅ OK |

---

## 3. Lógica de Guardado Incremental

La aplicación sigue el siguiente algoritmo de seguridad de datos:

1. Identifica o crea la subcarpeta del proyecto (ej: `CALCULO_ESTRUCTURAS`).
2. Genera un sello de tiempo único (ISO 8601).
3. Guarda simultáneamente en la carpeta física:
   - `app_YYYY-MM-DD_HH-mm.html`
   - `informe_YYYY-MM-DD_HH-mm.md`
   - `prompt_YYYY-MM-DD_HH-mm.txt`
   - `audit_log_YYYY-MM-DD_HH-mm.txt`

> **NOTA:** Esta versión permite que el ingeniero tenga un panel de control vivo donde puede probar sus herramientas al instante mientras mantiene el respaldo físico íntegro en su ordenador.
