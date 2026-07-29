# 🤖 n8n AI Onboarding Workflow - MVP Capas Semánticas

Automatización de onboarding hiper-personalizado para un MVP educativo, impulsada por IA Generativa. 
El flujo transforma un registro estándar en una experiencia de bienvenida contextualizada, enriqueciendo datos no estructurados y ejecutando acciones omnicanal.

##  Objetivo
Cuando un Data Engineer o BI Developer se registra para el MVP, la IA (Google Gemini) analiza su rol y su principal frustración con los datos. 
Luego, genera un "Tip de Oro" personalizado, lo envía por email y notifica a la comunidad en Discord, mientras actualiza el CRM en Google Sheets.


## 🛠️ Stack Tecnológico

| Herramienta | Rol en la Automatización |
| :--- | :--- |
| **n8n (Self-hosted)** | Orquestador de flujos (Workflow as Code). |
| **Google Gemini (2.0 Flash)** | Motor de IA para enriquecimiento de datos y generación de texto. |
| **Google Sheets** | Base de datos / CRM mínimo viable. |
| **Gmail** | Canal de comunicación 1 a 1. |
| **Discord (Webhook)** | Canal de comunicación comunitario. |

---

## ⚙️ Requisitos Previos y Credenciales

Para importar y ejecutar este flujo, necesitás configurar las siguientes credenciales en tu instancia de n8n:

1. **Google Sheets API:** OAuth2 con permisos de lectura/escritura.
2. **Gmail API:** OAuth2 con permisos de envío.
3. **Google Gemini API:** API Key obtenida de [Google AI Studio](https://aistudio.google.com/). 
4. **Discord:** URL del Webhook del canal de bienvenida (Configurado en *Integraciones > Webhooks* del servidor). 
	
		
---

## 🚀 Instrucciones de Instalación

1. Clonar este repositorio.
2. Abrir tu instancia local de n8n (ej. `http://localhost:5678`).
3. Crear un nuevo Workflow y navegar a `Menu` -> `Import from File`.
4. Seleccionar el archivo `workflows/workflow_onboarding_mvp.json`.
5. Configurar las credenciales en cada nodo según la sección de *Requisitos Previos*.
6. Asegurarse de que la Google Sheet de destino tenga exactamente las siguientes columnas:
   - `Nombre y Apellido`
   - `Correo electrónico`
   - `¿Cuál es tu rol actual?`
   - `¿Cuál es tu mayor frustración hoy con los datos y la IA?`
   - `Tip_Generado_IA`

---

## 🧠 Prompt de la IA

El prompt utilizado para Google Gemini está versionado en este repositorio para permitir auditoría y mejoras iterativas. A continuación, se detalla la configuración exacta:

**System / User Prompt:**

```text
Actúa como un Director de Producto Educativo experto en Capas Semánticas y Data Engineering. 

Recibiste el registro de un nuevo alumno para nuestro MVP gratuito. 
- Su Nombre es: {{ $json["Nombre y Apellido"] }}
- Su Rol es: {{ $json["¿Cuál es tu rol actual?"] }}
- Su Mayor Frustración es: {{ $json["¿Cuál es tu mayor frustración hoy con los datos y la IA?"] }}

TAREA:
Generá un mensaje de bienvenida en español (tono profesional pero cercano, estilo argentino, usando "vos") que incluya:
1. Un saludo personalizado usando su nombre.
2. Una validación empática de su frustración (1 o 2 líneas máximo).
3. Un "Tip de Oro" de 3 líneas sobre cómo empezar a solucionar ese problema específico usando modelado dimensional o capas semánticas.
4. Un cierre breve invitándolo a revisar su email para los detalles de la primera sesión en vivo.
5. Agregá al final: "Para unirte a la comunidad de Data Engineers y seguir debatiendo, usá este link de acceso al Discord: [INSERTAR_LINK_DISCORD]".

FORMATO DE SALIDA:
Devolvé ÚNICAMENTE el texto del mensaje. 
- PROHIBIDO usar formato JSON. No uses llaves {}, ni comillas al inicio/final, ni claves como "message".
- Usá la etiqueta <br> para los saltos de línea.
- El output debe ser texto plano puro, listo para leer.
```
