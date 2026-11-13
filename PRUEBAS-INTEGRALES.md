# NicSys AI V28 — informe de verificación

Fecha: 13 de noviembre de 2026

## Cambios incluidos

- Gemini puede consultar resumen general, billetera, préstamos, tarjetas, compras, transferencias, deudas, haberes, historial, configuración y estadísticas a partir del estado cargado por NicSys desde Firestore.
- Las llamadas de Function Calling preparan propuestas, pero no escriben en Firestore. El guardado sigue detrás de la confirmación de NicSys.
- Las llamadas de Gemini tienen un máximo de espera de 30 segundos y reinician el chat si vence el tiempo o falla la conexión.
- Micrófono: usa el reconocimiento de voz del navegador cuando existe y MediaRecorder con audio enviado al mismo chat Gemini como alternativa. Límite de 20 segundos y 14 MiB; el aviso de privacidad informa que el audio se envía a Google.
- Panel Maestro ofrece preferencia de voz, acento, estilo de prosodia, velocidad, volumen, prueba y guardado por UID en `ai_voice_preferences/{uid}`. NicSys IA y Cámara IA comparten la síntesis del navegador.
- Se eliminó la inferencia de género por el nombre de la voz. Si el motor no declara el género, la interfaz explica esa limitación.
- No se añadió un proveedor externo de pago para síntesis de voz. El audio se procesa con el backend Gemini que ya usa la app, con sus cuotas y condiciones de facturación.

## Verificaciones realizadas

- Los 10 bloques JavaScript inline pasan `node --check`.
- Ocho comprobaciones estáticas de regresión pasan: cobertura de módulos, uso del estado cargado, conexión de audio, conexión del reconocimiento a la conversación, límites de grabación, preparación sin escrituras, preferencias por usuario y manejo de errores.
- La pantalla de inicio de sesión se abre localmente y el Hosting público sirve el código actualizado.
- El despliegue se ejecutó con Firebase Hosting solamente en `nell-app-finance`; no se desplegaron reglas, índices ni datos.
- No se crearon ni modificaron movimientos financieros de prueba.

## Verificación pendiente

La sesión de la aplicación mostró el inicio de sesión con error de credenciales. Sin una sesión autenticada no pude validar una respuesta Gemini real con Firestore, reproducción de voz, permiso/grabación real del micrófono ni lectura y guardado de preferencias por cuenta. Por eso no se marca la integración como aprobada de extremo a extremo. La naturalidad y la disponibilidad de voces dependen del motor instalado en el dispositivo.
