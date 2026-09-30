# Caso de estudio — Distribuir una extensión fuera de la tienda de Chrome

## El problema

La extensión está pensada para creadoras de contenido, no para personas técnicas. La vía normal, Chrome Web Store, no era posible por el tipo de producto. Las alternativas "obvias" fallan:

- **Instalación forzada por política de Chrome:** Chrome la bloquea en computadoras no gestionadas cuando la extensión no viene de su tienda.
- **Cargar la carpeta en modo desarrollador a mano:** exige varios pasos técnicos, y cualquier error deja a la usuaria sin su herramienta en plena transmisión.
- **Actualizar a mano:** en la práctica, casi nadie lo hace. Una medición real mostró una sola cuenta en la última versión una semana después de publicarla.

## La solución, en una frase

Un **instalador de Windows firmado**, con interfaz propia, que instala la extensión y deja un **actualizador automático** que mantiene cada equipo al día de forma segura.

## Qué hace el instalador

| Capacidad | Qué aporta |
|---|---|
| Asistente visual en español | Una persona no técnica lo completa sin ayuda. |
| Progreso por fases | Ve en qué paso va y qué falta. |
| Resultado verificado | Solo informa éxito si comprobó que la instalación terminó completa. Un archivo bloqueado ya no se "omite" en silencio. |
| Firma digital | Windows lo identifica como software de un editor reconocido, sin avisos de origen desconocido. |
| Guía de instalación integrada | Enlace directo a los pasos finales dentro de Chrome. |

## Qué hace el actualizador

| Capacidad | Qué aporta |
|---|---|
| Revisión periódica | Busca versiones nuevas varias veces al día, sin que la usuaria haga nada. |
| Pregunta antes de actuar | No interrumpe una transmisión sin consentimiento. "Más tarde" significa "en el próximo chequeo", no "nunca". |
| Verificación antes de instalar | Comprueba que el paquete es el publicado y que lo emitió el equipo legítimo. Si algo no coincide, **no instala**. |
| Copia de seguridad y vuelta atrás | Si la actualización falla a medias, restaura la versión anterior. |
| Botón "Actualizar ahora" | Desde el panel o el icono de la extensión, con un clic y con ventana de marca propia. |
| Actualización obligatoria con gracia | Para versiones críticas: plazo de horas, cuenta regresiva visible en la última hora y bloqueo al vencer. |

## Principios de diseño

1. **Falla cerrado.** Ante la duda, no instala. Nunca se prefiere "que funcione" a "que sea seguro".
2. **El actualizador no es una puerta trasera.** Aunque se comprometiera el servidor de descargas, no podría instalar código sin la firma del equipo.
3. **Solo dice lo que verificó.** Coherente con la regla de producto de no inventar datos.
4. **Datos de la usuaria protegidos.** La identidad estable de la extensión evita perder configuración al mover o reinstalar.

## Resultado

- Instalación sin pasos técnicos.
- Versiones nuevas llegan a las usuarias sin depender de que se acuerden de actualizar.
- Cada publicación se verifica de extremo a extremo antes de darla por buena.

> Por confidencialidad no se documentan aquí los mecanismos internos: formatos, hosts, claves ni rutas del sistema.
