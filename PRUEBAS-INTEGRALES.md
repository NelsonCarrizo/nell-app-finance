# NicSys AI V28 — informe de verificación

Fecha: 13 de noviembre de 2026

## Cambios incluidos

- Consultas de Gemini ampliadas para resumen general, billetera, préstamos, tarjetas, compras, transferencias, deudas, haberes, historial, configuración y estadísticas, usando el estado que NicSys carga desde Firestore.
- Las operaciones enviadas por Function Calling siguen quedando como propuesta en la interfaz; esta herramienta no escribe en Firestore. El guardado continúa en el flujo de confirmación de NicSys.
- Tiempo máximo de espera de 30 segundos para llamadas Gemini, con salida de error y recuperación del chat.
- Entrada de voz: reconocimiento del navegador cuando está disponible y grabación con MediaRecorder como alternativa cuando no existe SpeechRecognition. La grabación se envía al mismo chat de Gemini; límite de 20 segundos y rechazo de archivos mayores a 18 MiB.
- Preferencias de voz en Panel Maestro, guardado por UID en `ai_voice_preferences/{uid}` y aplicación a NicSys IA y respuestas de Cámara IA. Volumen se aplica localmente.
- Se eliminó la inferencia del género por el nombre de la voz. Si el motor no declara género, se informa que no puede garantizarse y se elige una voz de español disponible.
- Se conserva Firebase Hosting existente; no se editan reglas ni se crean operaciones financieras de prueba.

## Pruebas ejecutadas

- Sintaxis JavaScript: los 10 bloques inline pasan `node --check`.
- Ocho comprobaciones estáticas de regresión: módulos declarados, datos actuales de estado, ruta de audio, conexión del reconocimiento al procesador, límites de grabación, ausencia de escrituras dentro de la herramienta de preparación, persistencia por usuario y manejo de errores.
- Revisión estática: el preparador de operaciones solo muestra una propuesta; no ejecuta `.add`, `.set` ni `.update`.
- La pantalla de inicio de sesión se abre en el servidor de prueba local.

## Estado que requiere validación en sesión autenticada

No se considera aprobada una integración de extremo a extremo hasta verificar una consulta de Gemini real con Firestore, respuesta hablada, permiso/grabación de micrófono y persistencia de preferencias en una cuenta autenticada. No se hicieron lecturas de finanzas reales en las pruebas estáticas ni escrituras financieras de prueba. La naturalidad y el género disponibles dependen del motor de voz del dispositivo; el motor web nativo no ofrece controles universales para ambos.
