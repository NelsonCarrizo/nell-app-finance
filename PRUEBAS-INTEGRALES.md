# NicSys AI V28 — informe de verificación

Fecha: 13 de noviembre de 2026

## Cambios incluidos

- Gemini puede consultar resumen general, billetera, préstamos, tarjetas, compras, transferencias, deudas, haberes, historial, configuración y estadísticas a partir del estado cargado por NicSys.
- Las llamadas de Function Calling preparan propuestas, pero no escriben en Firestore. El guardado sigue detrás de la confirmación de NicSys.
- Las llamadas de Gemini tienen un máximo de espera de 30 segundos y reinician el chat si vence el tiempo o falla la conexión. Si Gemini falla, la interfaz lo informa y ofrece interpretar el comando localmente.
- Micrófono: usa reconocimiento de voz del navegador cuando existe y MediaRecorder con audio enviado al mismo chat Gemini como alternativa. Límite de 20 segundos y 14 MiB; el aviso informa que el audio se envía a Google.
- Panel Maestro ofrece acento, estilo, velocidad, volumen, prueba y preferencias guardadas localmente en este dispositivo, separadas por UID. No se añadió una colección nueva en Firestore ni hacen falta cambios de reglas.
- NicSys IA tiene una entrada rotulada «IA» en la barra principal, además del botón flotante existente. El asistente muestra el GIF proporcionado con un aro de estado para escucha, procesamiento y respuesta; las instrucciones y ejemplos indican cómo preguntar o dar órdenes.
- No se infiere género por el nombre de la voz. La preferencia solo se aplica si el motor del navegador informa el género.
- No se añadió un proveedor externo de pago para síntesis de voz. La síntesis usa el navegador/dispositivo; la recepción de audio usa Firebase AI Logic ya integrado.

## Verificaciones realizadas

- Los 9 bloques JavaScript inline pasan `node --check`.
- Comprobaciones estáticas confirmaron cobertura de módulos, uso del estado cargado, flujo de audio, límites de grabación, preparación de operaciones sin escrituras, preferencias locales por usuario, y manejo explícito de errores.
- El sitio público sirve la versión desplegada y la sesión autenticada de la app abre.
- En la versión pública comprobé que aparece «IA» en la barra principal, que abre el asistente, que el GIF animado se carga y que el campo de texto y los ejemplos están disponibles.
- Firebase Hosting se desplegó únicamente en `nell-app-finance`; no se desplegaron reglas, índices ni datos.
- No se crearon ni modificaron movimientos financieros de prueba.
- Se envió una consulta inocua de conectividad («Respondé únicamente: conexión de prueba»). Firebase AI Monitoring registró una solicitud con latencia p95 de 4,26 minutos; la consola del navegador informó primero error 500 por alta demanda y luego vencimiento del límite de 30 segundos. La app no produjo una respuesta Gemini en esa prueba.

## Limitaciones pendientes

- Gemini no queda aprobado como funcional de extremo a extremo: el servicio respondió con saturación en la prueba y la respuesta agotó el límite de espera. El código maneja ese fallo y comunica que usa interpretación local, pero no se verificó una respuesta generada por Gemini.
- El navegador interno de esta sesión no expone SpeechRecognition, MediaRecorder, acceso al micrófono ni síntesis de voz. No se pudo probar una nota de voz real, la reproducción hablada ni el panel de preferencias en un navegador compatible.
- Las preferencias quedan guardadas en el dispositivo actual y separadas por cuenta; no se sincronizan entre dispositivos.
- La calidad, disponibilidad y género declarados de las voces dependen del motor instalado en el navegador/dispositivo.
