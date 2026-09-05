# Contribuir a PROVIWEB Web & Connect

Gracias por tu interés en contribuir.

Este documento describe el proceso recomendado para colaborar en el proyecto,
especialmente en áreas críticas como autenticación, OAuth/PKCE y seguridad.

## Tipos de contribución

- Corrección de bugs.
- Mejoras de UI/UX del sitio.
- Mejoras de documentación técnica.
- Hardening de seguridad en flujos auth/OAuth.
- Integraciones y herramientas para terceros.

## Flujo recomendado

1. Abre un issue para discutir el cambio (si no existe).
2. Haz cambios en una rama de trabajo.
3. Incluye contexto claro en commits.
4. Abre pull request con descripción completa.
5. Espera revisión y responde feedback.

## Convenciones técnicas

1. Mantén cambios pequeños y enfocados.
2. Evita mezclar refactors masivos con fixes funcionales.
3. No introduzcas secretos ni credenciales en código.
4. Mantén consistencia con estilo y estructura existente.
5. Prioriza validaciones explícitas y manejo de errores claro.

## Requisitos para cambios en autenticación/SSO

Si tocas `oauth-authorize.html`, `functions/index.js`, `admin.html` o
`docs-sso.html`, debes preservar:

1. `response_type=code`.
2. PKCE `S256` obligatorio.
3. Validación estricta de `client_id` y `redirect_uri`.
4. Control de expiración y uso único de authorization code.
5. Scopes permitidos (`profile`, `email`, `qav`) y rechazo de no soportados.

## Entorno local

Comandos comunes:

```bash
npm install
npm run build
npm run dev
npm run serve
```

## Validación mínima antes de PR

1. Compilar el proyecto (`npm run build`).
2. Revisar que no se rompa la navegación principal.
3. Probar flujos afectados por el cambio.
4. Confirmar que no se agregaron archivos sensibles.

## Pull Requests

Incluye en tu PR:

1. Problema que resuelve.
2. Enfoque aplicado.
3. Riesgos y compatibilidad.
4. Pasos de prueba.
5. Capturas o evidencia cuando haya cambios visuales.

## Seguridad y divulgación responsable

No abras issues públicos con exploits o datos sensibles.

Para reportes de seguridad, sigue `SECURITY.md`.
