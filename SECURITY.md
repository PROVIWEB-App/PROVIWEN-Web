# Security Policy

## Versiones soportadas

Se prioriza soporte sobre la rama activa del repositorio y despliegues vigentes
en Firebase Hosting y Cloud Functions.

## Reportar una vulnerabilidad

Si encuentras una vulnerabilidad de seguridad:

1. No publiques detalles sensibles en issues públicos.
2. Envía un reporte privado al equipo mantenedor del proyecto.
3. Incluye:
   - Resumen del riesgo.
   - Impacto potencial.
   - Pasos de reproducción.
   - Evidencia técnica mínima.
   - Mitigación sugerida (si aplica).

## Qué esperar del proceso

1. Confirmación de recepción.
2. Evaluación de severidad y alcance.
3. Plan de corrección.
4. Notificación de cierre.

El tiempo de respuesta puede variar según criticidad y complejidad.

## Alcance de seguridad crítico en este repositorio

- OAuth Authorization Code + PKCE (S256).
- Validación de `client_id`, `redirect_uri` y scopes.
- Integridad y uso único de authorization codes.
- Token exchange (`/oauth/token` y callable).
- Acceso a consola administrativa.
- Manejo de credenciales, secretos y datos sensibles.

## Buenas prácticas obligatorias para colaboradores

1. Nunca subir API keys, tokens o secretos al repositorio.
2. No introducir bypasses de autenticación.
3. No relajar validaciones OAuth sin revisión explícita.
4. Mantener manejo de errores explícito, sin ocultar fallas.
5. Revisar dependencias y cambios de configuración de Firebase.

## Safe Harbor

No se tomarán acciones contra investigadores que:

1. Actúen de buena fe.
2. Eviten afectar disponibilidad o datos de usuarios.
3. Reporten de forma responsable y confidencial.
