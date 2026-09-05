# Contribuir a PROVIWEB Web & Connect

Gracias por tu interés en contribuir a **PROVIWEB Web & Connect**.

PROVIWEB es un ecosistema tecnológico orientado a conectar artistas, usuarios, aplicaciones y servicios mediante una plataforma web, servicios de identidad y mecanismos de integración como **OAuth 2.0 + PKCE**.

Este documento describe el proceso recomendado para contribuir al proyecto, mantener la calidad del código y reducir riesgos, especialmente en áreas críticas como autenticación, autorización, SSO, OAuth, infraestructura y seguridad.

---

## 1. Antes de comenzar

Antes de realizar cambios importantes:

1. Revisa la documentación existente.
2. Comprueba si ya existe un Issue o Pull Request relacionado.
3. Familiarízate con la estructura del proyecto.
4. Identifica si el cambio afecta funcionalidades críticas.
5. Evita comenzar cambios grandes sin discutir previamente su alcance.

Para cambios pequeños, como correcciones de documentación, errores visuales o ajustes menores, puede no ser necesario abrir un Issue previamente.

---

## 2. Tipos de contribución

Las contribuciones pueden incluir:

* Corrección de bugs.
* Mejoras de UI/UX.
* Accesibilidad.
* Optimización de rendimiento.
* Mejoras de compatibilidad.
* Mejoras de documentación.
* Correcciones de errores de configuración.
* Mejoras de seguridad.
* Hardening de autenticación y autorización.
* Mejoras de OAuth/PKCE.
* Integraciones para aplicaciones de terceros.
* Herramientas para desarrolladores.
* Pruebas y validaciones.
* Mejoras de infraestructura y despliegue.
* Correcciones de errores de traducción o contenido.

Las propuestas que introduzcan nuevas funcionalidades importantes deberían describir claramente su propósito, impacto y compatibilidad con la arquitectura existente.

---

## 3. Flujo recomendado de contribución

El flujo general recomendado es:

1. **Abrir o localizar un Issue**

   * Describe el problema o propuesta.
   * Explica por qué es necesario.
   * Indica el comportamiento actual y esperado.

2. **Crear una rama de trabajo**

   Utiliza una rama descriptiva, por ejemplo:

   ```text
   feature/oauth-improvement
   fix/login-redirect
   security/pkce-validation
   docs/update-sso-guide
   ui/improve-navigation
   ```

3. **Realizar los cambios**

   * Mantén el alcance de la rama claramente definido.
   * Evita cambios no relacionados.

4. **Probar localmente**

   * Ejecuta las validaciones correspondientes.
   * Comprueba los flujos afectados.

5. **Crear commits claros**

   * Describe qué cambió.
   * Evita mensajes genéricos como `cambios`, `arreglos` o `update`.

6. **Abrir un Pull Request**

   * Explica el problema.
   * Describe la solución.
   * Incluye pruebas realizadas.
   * Indica riesgos o posibles efectos secundarios.

7. **Revisión**

   * Responde al feedback.
   * Realiza las modificaciones solicitadas.
   * No fuerces cambios directamente sobre ramas protegidas.

---

## 4. Convenciones técnicas

Procura mantener las siguientes reglas:

1. Mantén los cambios pequeños y enfocados.
2. Evita mezclar refactors masivos con correcciones funcionales.
3. No introduzcas secretos, credenciales o información privada.
4. Mantén la estructura y convenciones existentes.
5. Prioriza código legible sobre soluciones innecesariamente complejas.
6. Utiliza validaciones explícitas.
7. Implementa manejo de errores claro.
8. Evita duplicación innecesaria.
9. No elimines validaciones de seguridad para simplificar código.
10. Evita cambios incompatibles sin documentarlos.
11. Mantén actualizada la documentación cuando cambie el comportamiento de una funcionalidad.
12. Evita modificar archivos de configuración sensibles sin justificar el cambio.

---

## 5. Seguridad

La seguridad tiene prioridad sobre la conveniencia.

No deben incorporarse al repositorio:

* Contraseñas.
* API keys.
* Tokens.
* Claves privadas.
* Credenciales de Firebase.
* Service account keys.
* Secretos OAuth.
* Certificados privados.
* Keystores.
* Variables de entorno sensibles.
* Datos personales reales utilizados como prueba.
* Información interna que pueda comprometer infraestructura.

Antes de abrir un Pull Request, revisa especialmente:

```text
.env
.env.*
*.json
*.pem
*.key
*.p12
*.jks
*.keystore
google-services.json
service-account*.json
```

No todos estos archivos son necesariamente secretos, pero deben revisarse cuidadosamente antes de publicarse.

Si accidentalmente se publica un secreto, **no basta con eliminarlo del commit posterior**. Debe notificarse inmediatamente y evaluarse la revocación o rotación de la credencial comprometida.

---

## 6. Cambios relacionados con autenticación y SSO

Los cambios relacionados con autenticación, autorización o SSO requieren especial cuidado.

Esto incluye, entre otros:

```text
public/oauth-authorize.html
functions/index.js
public/admin.html
public/docs-sso.html
```

Cuando se modifique cualquiera de estos componentes, debe preservarse como mínimo:

1. `response_type=code`.
2. PKCE mediante `S256` obligatorio.
3. Validación estricta de `client_id`.
4. Validación estricta de `redirect_uri`.
5. Validación de los parámetros OAuth recibidos.
6. Control de expiración del authorization code.
7. Uso único del authorization code.
8. Validación de coherencia durante el token exchange.
9. Rechazo de scopes no soportados.
10. Validación de `state` por parte del cliente integrador.
11. Protección contra reutilización de códigos.
12. Manejo seguro de errores.
13. No exposición innecesaria de información interna.

Los cambios de seguridad no deben eliminar una protección existente sin proporcionar una alternativa equivalente o superior.

---

## 7. Scopes OAuth

Los scopes actualmente permitidos son:

```text
profile
email
qav
```

Un nuevo scope no debe incorporarse simplemente modificando una lista de valores.

Debe evaluarse:

1. Qué información permite acceder.
2. Qué operaciones permite realizar.
3. Qué aplicaciones podrán solicitarlo.
4. Qué consentimiento debe mostrarse al usuario.
5. Qué validaciones adicionales son necesarias.
6. Qué impacto tiene sobre privacidad y seguridad.
7. Cómo se documentará para terceros.

Los scopes desconocidos o no autorizados deben continuar siendo rechazados.

---

## 8. Cambios en infraestructura y backend

Los cambios sobre:

* Cloud Functions.
* Firebase.
* Hosting.
* Reglas de acceso.
* Autenticación.
* Bases de datos.
* Rewrites.
* Variables de configuración.
* Integraciones externas.

deben tratarse como cambios de alto impacto.

Antes de realizar un Pull Request que afecte infraestructura:

1. Explica qué recurso se modifica.
2. Explica por qué es necesario.
3. Identifica posibles efectos secundarios.
4. Comprueba permisos y reglas de acceso.
5. Prueba los flujos afectados.
6. Evita ampliar permisos sin una justificación clara.
7. Comprueba que los cambios no expongan información privada.

---

## 9. Cambios de interfaz y experiencia de usuario

Los cambios visuales son bienvenidos siempre que mantengan la identidad y funcionalidad del ecosistema.

Para cambios de UI/UX:

1. Mantén la navegación existente.
2. Evita introducir elementos que oculten funcionalidades importantes.
3. Comprueba comportamiento responsive.
4. Comprueba diferentes tamaños de pantalla.
5. Considera accesibilidad.
6. Evita cambios que dificulten autenticación o consentimiento.
7. Incluye capturas o evidencia visual en el Pull Request cuando sea útil.

---

## 10. Documentación

Si una contribución modifica el comportamiento de una funcionalidad pública, también debe actualizarse la documentación correspondiente.

Esto puede incluir:

* README.
* Documentación OAuth.
* Guías de integración.
* Ejemplos de código.
* Parámetros de API.
* Flujos de autenticación.
* Requisitos para terceros.
* Documentación de despliegue.

La documentación debe describir el comportamiento real del sistema y no funcionalidades futuras o no implementadas como si estuvieran disponibles.

---

## 11. Entorno local

Los comandos pueden variar según la configuración del proyecto.

Como referencia:

```bash
npm install
npm run build
npm run dev
npm run serve
```

Si el proyecto requiere herramientas adicionales de Firebase u otros servicios, consulta la documentación correspondiente antes de realizar un despliegue.

**No ejecutes despliegues de producción desde una rama de trabajo sin autorización.**

---

## 12. Validación mínima antes de un Pull Request

Antes de enviar un Pull Request, comprueba como mínimo:

1. El proyecto compila correctamente.
2. No existen errores introducidos por el cambio.
3. La navegación principal continúa funcionando.
4. Los flujos afectados fueron probados.
5. La autenticación continúa funcionando cuando corresponda.
6. Los redirects OAuth continúan funcionando cuando corresponda.
7. No se introdujeron secretos.
8. No se agregaron dependencias innecesarias.
9. La documentación fue actualizada si era necesario.
10. Los cambios están limitados al objetivo del Pull Request.

Para cambios de seguridad o autenticación, se recomienda realizar pruebas adicionales y documentarlas explícitamente.

---

## 13. Pull Requests

Cada Pull Request debería incluir:

### Problema

¿Qué problema se está solucionando?

### Solución

¿Qué cambios se realizaron y por qué?

### Impacto

¿Qué partes del sistema pueden verse afectadas?

### Riesgos

¿Existe algún riesgo conocido?

### Pruebas

¿Qué se probó y cuál fue el resultado?

### Evidencia

Incluye capturas, logs o ejemplos cuando sean útiles.

Ejemplo:

```text
## Problema
El callback OAuth podía producir un comportamiento incorrecto
cuando el redirect_uri no coincidía exactamente.

## Solución
Se agregó validación estricta del redirect_uri registrado
para el client_id correspondiente.

## Pruebas
- Login web: OK
- OAuth Authorization Code: OK
- PKCE S256: OK
- Redirect URI inválido: rechazado
- Scope inválido: rechazado
```

---

## 14. Commits

Los commits deben ser claros y representar cambios coherentes.

Ejemplos recomendados:

```text
fix: validate OAuth redirect URI
feat: add third-party OAuth client registration
docs: update SSO integration guide
security: harden authorization code validation
ui: improve responsive navigation
refactor: simplify OAuth validation
```

Evita commits excesivamente grandes cuando sea posible.

---

## 15. Dependencias

Antes de agregar una nueva dependencia:

1. Comprueba si ya existe una solución dentro del proyecto.
2. Evalúa su mantenimiento.
3. Revisa su licencia.
4. Evalúa posibles riesgos de seguridad.
5. Considera su impacto sobre el tamaño y rendimiento.
6. Justifica su incorporación cuando tenga impacto significativo.

No se deben incorporar dependencias abandonadas, sospechosas o innecesarias.

---

## 16. Código generado o asistido por IA

El uso de herramientas de inteligencia artificial está permitido.

Sin embargo, cualquier código generado o asistido por IA debe ser:

1. Revisado por la persona que realiza la contribución.
2. Comprendido antes de incorporarse al proyecto.
3. Probado adecuadamente.
4. Revisado desde el punto de vista de seguridad.
5. Compatible con la licencia y dependencias del proyecto.

No se debe introducir información privada, credenciales, secretos o datos sensibles en herramientas externas.

La utilización de IA **no transfiere la responsabilidad del código al proyecto ni a la herramienta utilizada**.

---

## 17. Pruebas de seguridad

Las pruebas de seguridad deben realizarse de forma responsable.

No está permitido utilizar sistemas de producción, cuentas de terceros o información real para experimentar sin autorización.

Si durante una prueba se identifica una vulnerabilidad:

1. Detén cualquier actividad que pueda causar daño.
2. No accedas a información innecesaria.
3. No modifiques ni elimines datos.
4. No compartas públicamente la vulnerabilidad.
5. Repórtala mediante el canal privado correspondiente.

Para más información, consulta el `CODE_OF_CONDUCT.md` y la documentación de seguridad del proyecto.

---

## 18. Cambios que requieren especial revisión

Se recomienda una revisión especialmente cuidadosa para cambios que afecten:

* Autenticación.
* Firebase Auth.
* OAuth.
* PKCE.
* SSO.
* Authorization codes.
* Token exchange.
* Redirect URIs.
* Scopes.
* Firebase Database.
* Cloud Functions.
* Reglas de seguridad.
* Hosting.
* Permisos.
* Credenciales.
* Información personal.
* Integraciones externas.

Estos cambios pueden tener consecuencias más allá de la interfaz web.

---

## 19. Cambios que pueden rechazarse

Un Pull Request puede ser rechazado o solicitado para modificación cuando:

* Introduce vulnerabilidades.
* Expone información sensible.
* Rompe funcionalidades existentes.
* Elimina controles de seguridad sin justificación.
* No incluye pruebas suficientes para el riesgo que introduce.
* Añade dependencias innecesarias.
* Modifica funcionalidades críticas sin explicar el impacto.
* Incluye código malicioso o comportamiento oculto.
* No respeta las licencias aplicables.
* Tiene un alcance excesivamente amplio y dificulta su revisión.
* No sigue las reglas del proyecto.

El rechazo de una contribución no constituye una crítica personal.

---

## 20. Responsabilidad del colaborador

Al enviar una contribución, el colaborador declara que, según su conocimiento:

1. Tiene derecho a aportar el código o contenido enviado.
2. No está incorporando información confidencial de terceros.
3. No está incorporando secretos o credenciales.
4. No está introduciendo deliberadamente código malicioso.
5. Ha realizado las pruebas razonables para el cambio.
6. Ha informado de riesgos relevantes que conozca.

---

## 21. Licencias y propiedad intelectual

Las contribuciones deben respetar las licencias aplicables al proyecto y a sus dependencias.

No se debe copiar código, documentación, imágenes, diseños u otros recursos de terceros sin comprobar previamente que su utilización está permitida.

Cuando una licencia requiera atribución, aviso o condiciones específicas, estas deben cumplirse.

---

## 22. Mantenedores

Los mantenedores pueden:

* Solicitar modificaciones.
* Pedir pruebas adicionales.
* Solicitar documentación.
* Rechazar cambios incompatibles con la arquitectura.
* Rechazar cambios que introduzcan riesgos de seguridad.
* Priorizar determinadas contribuciones.
* Cerrar Pull Requests que no cumplan los requisitos del proyecto.

Las decisiones técnicas deben buscar el beneficio a largo plazo del proyecto, su comunidad y sus usuarios.

---

## 23. Principio de colaboración

PROVIWEB busca que las contribuciones sean:

**Pequeñas cuando sea posible.
Claras cuando sea necesario.
Seguras siempre.**

Una buena contribución no es únicamente aquella que funciona.

También debe ser:

* Comprensible.
* Mantenible.
* Segura.
* Documentada cuando corresponda.
* Compatible con el ecosistema.
* Revisable por otros colaboradores.

---

## 24. Gracias por contribuir

Cada corrección, prueba, reporte, mejora de documentación, propuesta técnica y contribución de código ayuda a mejorar PROVIWEB.

Gracias por formar parte del desarrollo de **PROVIWEB Web & Connect** y por contribuir a un ecosistema abierto, seguro y orientado a la colaboración.

## Seguridad y divulgación responsable

No abras issues públicos con exploits o datos sensibles.

Para reportes de seguridad, sigue `SECURITY.md`.
