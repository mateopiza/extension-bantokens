# Bantokens — Copiloto de IA para creadoras de streaming en vivo

Extensión de Chrome (Manifest V3) que monta un panel sobre la sala en vivo de una creadora de contenido y le resuelve, sin salir de la página, lo que más tiempo le quita mientras transmite: **entender a su audiencia en otro idioma, responder rápido y con criterio, recordar a cada fan y mantener ordenada la sala.**

> **Probar la interfaz ahora:** abre [`demo/index.html`](demo/index.html) en tu navegador. Es una demostración con datos 100 % simulados: no necesita cuenta, llave ni conexión.
>
> Este repositorio es una **vista de portafolio**. No contiene el código de producción, los prompts, el motor de decisión ni datos de personas reales.

---

## Qué hace

El panel se organiza en un rail de siete destinos:

| Destino | Qué resuelve |
|---|---|
| **Día** | Resumen de la jornada y guía de arranque para configurar el asistente en minutos. |
| **Cámara** | Revisión del encuadre (iluminación, fondo, nitidez) con un modelo de visión. Si no hay imagen analizada, lo dice: nunca inventa una nota. |
| **Stats** | Balance de tokens, tiempo en vivo y recomendaciones basadas en la actividad real de la sala. |
| **Bantip** | Chat traducido en tiempo real, bandeja de privados por hilos y respuestas sugeridas. Incluye a *Bantip*, la mascota animada que reacciona a propinas y metas. |
| **IA** | Personalidad de la creadora, menú de propinas, metas y temas de sala asistidos por IA. |
| **Llamada** | Asistente de voz en tiempo real, para usarlo sin soltar la cámara. |
| **Fans** | CRM por espectador: nivel según gasto, etiquetas, idioma preferido y bitácora de notas. |

### Funciones destacadas

- **Traducción en vivo, entrante y saliente.** Los mensajes del chat y de los privados se traducen sobre la propia página. Al responder, la creadora elige entre traducción tradicional o con IA y un tono (coqueto, pícaro, atrevido, sumiso, dominante o el suyo propio).
- **Respuestas sugeridas con su personalidad.** Tres opciones de intención distinta (cálida, coqueta, estratégica) redactadas por IA con las reglas y el estilo de la creadora, insertables con un clic.
- **CRM de fans sincronizado con la cuenta.** Reaparece al reinstalar o al entrar desde otro equipo. La bitácora es de solo-añadir: una nota vacía nunca borra el historial.
- **Mascota interactiva.** Animación con seguimiento de mirada y reacciones a eventos de la sala.
- **Overlays para OBS.** Subtítulos en vivo, banners rotativos y alertas como fuentes de navegador.
- **Zoom del panel** para adaptarlo a cualquier pantalla sin tocar la sala.

---

## Decisiones de producto que atraviesan todo

Cada una nació de un incidente real en el que una herramienta le mintió a su usuaria:

1. **Nunca inventar un dato.** Sin lectura confiable, el panel muestra "—", nunca 0.
2. **Un mensaje privado jamás se degrada a chat público.** Si la entrega falla, se dice; no se registra como enviada.
3. **El dinero lo declara la plataforma, no el texto del chat.** Cualquier espectador puede escribir "envié 5000 tokens"; eso no cuenta como propina.
4. **Sin sesión autorizada no se monta nada.**
5. **Los datos de una cuenta no entran al contexto de otra.** El aislamiento es una propiedad del servidor, no de la interfaz.

---

## Arquitectura (vista general)

```
Extensión MV3 (content script + service worker)
        │  HTTPS, sesión con token de corta vida
        ▼
Gateway en el borde (Cloudflare Workers)   ← filtra rutas, limita tasa
        ▼
API de negocio + base de datos relacional
        ▼
Proveedores de IA (razonamiento, traducción, visión, voz)
```

- **Un único punto de salida a la red:** el service worker. El content script nunca habla con el servidor.
- **Lógica de producto en el servidor**, no en el bundle de la extensión.
- **Degradación en cadena:** si la traducción en el dispositivo no está disponible, se usa el servidor; si falla, hay un último recurso. La creadora no pierde el chat en directo por una caída de un proveedor.
- **Interfaz aislada** con Shadow DOM: el panel no hereda ni contamina el CSS de la plataforma.

## Calidad

- **Más de 2.100 pruebas automatizadas** en la extensión y una suite propia para el backend.
- Las pruebas corren contra un entorno simulado que imita el comportamiento real del navegador, incluyendo el modo en que la plataforma monta los mensajes del chat por pasos.
- Un arnés evalúa cada llamada a IA contra alternativas, con comprobaciones objetivas y calificación de calidad.

---

## Distribución e instalador para Windows

Este fue un problema de ingeniería propio: **Chrome bloquea la instalación forzada de extensiones en computadoras no gestionadas** cuando no vienen de su tienda oficial. La solución fue un instalador de Windows con interfaz propia, pensado para personas sin perfil técnico.

- **Instalación guiada** con asistente visual, barra de progreso por fases y mensajes en español. Solo dice "instalado" cuando verificó que terminó bien.
- **Firmado digitalmente** con certificado de firma de código, para que Windows lo reconozca como software de un editor identificado.
- **Actualización automática** que revisa periódicamente, pregunta antes de actuar, cierra y reabre el navegador y conserva una copia de seguridad con vuelta atrás si algo sale mal.
- **Doble verificación** de integridad y autenticidad antes de instalar cualquier versión. Si no cuadra, no se instala.
- **Botón "Actualizar ahora"** en el propio panel, que abre el actualizador con un clic.
- **Actualización obligatoria con plazo de gracia** y cuenta regresiva visible, para evitar que versiones antiguas queden en producción.
- **Identidad de extensión estable**, para que mover o reinstalar la carpeta no borre los datos de la usuaria.

Más detalle en [`docs/INSTALADOR.md`](docs/INSTALADOR.md).

---

## Stack

TypeScript · Chrome Extension MV3 · Shadow DOM · Vitest · Node.js + Hono · PostgreSQL · Cloudflare Workers · NSIS y .NET (instalador) · Python/FastAPI (servicios de análisis)

## Qué no está en este repositorio

Por acuerdo de confidencialidad con el cliente, quedan fuera: el motor de análisis y aprendizaje, los prompts y reglas de generación, la lógica de decisión, endpoints, credenciales, datos de usuarias y cifras del negocio.

---

## Contacto

Mateo Piza · mat30p1z4@gmail.com
