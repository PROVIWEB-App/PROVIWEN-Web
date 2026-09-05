# Security Policy

## 1. Objetivo

La seguridad de **PROVIWEB Web & Connect** es una prioridad.

Este proyecto incluye componentes relacionados con autenticación, identidad, SSO, OAuth 2.0, PKCE, Firebase Authentication, Cloud Functions, Firebase Hosting y servicios utilizados por aplicaciones de terceros.

Esta política establece cómo reportar vulnerabilidades de seguridad y cómo deben gestionarse las investigaciones de seguridad relacionadas con el proyecto.

---

## 2. Versiones soportadas

Se prioriza el soporte de:

* La rama activa del repositorio.
* La versión actualmente desplegada en producción.
* Los servicios vigentes de Firebase Hosting.
* Las Cloud Functions actualmente utilizadas por producción.
* Los componentes de autenticación y SSO activos.

Las versiones antiguas o componentes retirados pueden no recibir correcciones de seguridad.

---

## 3. Reportar una vulnerabilidad

Si encuentras una posible vulnerabilidad:

**No publiques información sensible en Issues, Discussions, Pull Requests ni otros canales públicos.**

Utiliza un canal privado de mantenimiento o seguridad de PROVIWEB.

El reporte debería incluir, cuando sea posible:

* Resumen de la vulnerabilidad.
* Componente afectado.
* Impacto potencial.
* Pasos para reproducirla.
* Evidencia técnica mínima necesaria.
* Condiciones necesarias para explotarla.
* Alcance estimado.
* Mitigación sugerida, si existe.
* Información sobre si la vulnerabilidad afecta producción o únicamente el entorno local.

No es necesario proporcionar información sensible de usuarios para demostrar una vulnerabilidad.

---

## 4. Información que NO debe incluirse

Nunca incluyas en un reporte:

* Contraseñas.
* API keys activas.
* Tokens válidos.
* Claves privadas.
* Credenciales de Firebase.
* Service account keys.
* Datos personales innecesarios.
* Información privada de usuarios.
* Credenciales de terceros.
* Dumps completos de bases de datos.
* Secretos de infraestructura.

Si una credencial resulta necesaria para demostrar el problema, utiliza una credencial de prueba específicamente creada para la investigación, siempre que sea posible.

---

## 5. Qué esperar del proceso

El proceso de gestión de vulnerabilidades seguirá, cuando las circunstancias lo permitan, estas etapas:

### 1. Recepción

Se confirma la recepción del reporte.

### 2. Evaluación

Se analiza:

* Validez.
* Severidad.
* Facilidad de explotación.
* Alcance.
* Impacto sobre usuarios.
* Impacto sobre infraestructura.
* Posible exposición de información.

### 3. Contención

Cuando exista riesgo activo, podrán aplicarse medidas temporales para reducir el impacto.

Estas medidas pueden incluir:

* Revocación de credenciales.
* Rotación de secretos.
* Restricción temporal de funcionalidades.
* Modificación de reglas de acceso.
* Bloqueo de clientes afectados.
* Cambios temporales de configuración.

### 4. Corrección

Se desarrolla y prueba una solución.

### 5. Despliegue

La corrección se incorpora al entorno correspondiente después de las validaciones necesarias.

### 6. Cierre

Una vez mitigado el problema, se documentará internamente su resolución y se notificará al reportante cuando corresponda.

Los tiempos de respuesta pueden variar según la severidad, complejidad y disponibilidad de información.

---

## 6. Clasificación de severidad

La severidad puede evaluarse utilizando categorías generales:

### Crítica

Problemas que puedan permitir, por ejemplo:

* Compromiso significativo de cuentas.
* Bypass completo de autenticación o autorización.
* Compromiso de infraestructura crítica.
* Acceso masivo a información sensible.
* Compromiso de credenciales privilegiadas.
* Ejecución remota con impacto significativo.

### Alta

Vulnerabilidades que puedan permitir:

* Acceso no autorizado relevante.
* Compromiso de cuentas bajo determinadas condiciones.
* Escalada significativa de privilegios.
* Manipulación de información sensible.
* Bypass importante de controles de seguridad.

### Media

Problemas con impacto limitado o que requieren condiciones adicionales para ser explotados.

### Baja

Problemas de seguridad con impacto reducido, dificultad elevada de explotación o principalmente relacionados con endurecimiento de configuración.

La clasificación final corresponde al equipo mantenedor y puede cambiar a medida que se obtiene información adicional.

---

## 7. Alcance de seguridad crítico en este repositorio

Las siguientes áreas requieren especial atención:

### OAuth / SSO

* Authorization Code Flow.
* PKCE `S256`.
* Validación de `client_id`.
* Validación estricta de `redirect_uri`.
* Validación de scopes.
* Authorization codes.
* Expiración de códigos.
* Uso único de códigos.
* Token exchange.
* Callbacks.
* Protección contra reutilización de códigos.

### Endpoints

* `/oauth/token`
* Endpoint de autorización.
* Callable Functions relacionadas con OAuth.
* Rewrites de Firebase Hosting.

### Autenticación

* Firebase Authentication.
* Inicio de sesión.
* Registro.
* Custom tokens.
* Sesiones.
* Autorización de usuarios.

### Administración

* Consola de clientes OAuth.
* Registro y modificación de clientes.
* Configuración de `client_id`.
* Redirect URIs.
* Scopes autorizados.
* Funciones administrativas.

### Infraestructura

* Firebase Hosting.
* Cloud Functions.
* Firebase Database.
* Reglas de seguridad.
* Configuración de despliegue.
* Secretos y credenciales.

---

## 8. Vulnerabilidades OAuth especialmente relevantes

Se consideran especialmente importantes los problemas que puedan provocar:

* Bypass de PKCE.
* Aceptación de métodos PKCE distintos de `S256` cuando no estén permitidos.
* Manipulación de `redirect_uri`.
* Confusión entre clientes OAuth.
* Reutilización de authorization codes.
* Uso de códigos expirados.
* Intercambio de un código perteneciente a otro cliente.
* Elevación de scopes.
* Bypass del consentimiento.
* Emisión indebida de custom tokens.
* Acceso a cuentas de otros usuarios.
* Filtración de tokens.
* Manipulación de parámetros de autorización.
* Ataques de sesión o autenticación.

Los reportes relacionados con estas áreas deben tratarse con especial prioridad.

---

## 9. Buenas prácticas obligatorias para colaboradores

Los colaboradores deben:

1. Nunca subir API keys, tokens o secretos al repositorio.
2. No introducir bypasses de autenticación.
3. No deshabilitar controles de seguridad para facilitar pruebas o desarrollo.
4. No relajar validaciones OAuth sin revisión explícita.
5. Mantener PKCE `S256` cuando corresponda.
6. Mantener la validación estricta de `client_id`.
7. Mantener la validación estricta de `redirect_uri`.
8. Mantener controles de expiración y uso único de authorization codes.
9. No ampliar scopes sin evaluar su impacto.
10. Mantener manejo de errores explícito.
11. Evitar revelar información sensible mediante mensajes de error.
12. Revisar nuevas dependencias.
13. Revisar cambios de configuración de Firebase.
14. Revisar reglas de acceso antes de desplegar.
15. Utilizar datos de prueba cuando sea posible.
16. No utilizar credenciales reales en ejemplos o documentación.

---

## 10. Secretos y credenciales comprometidas

Si un secreto se publica accidentalmente:

**Eliminar el archivo del repositorio no debe considerarse una solución suficiente.**

El colaborador debe informar inmediatamente al equipo mantenedor.

Dependiendo del secreto afectado, podrán ser necesarias medidas como:

* Revocación.
* Rotación.
* Regeneración.
* Invalidación de tokens.
* Cambio de credenciales.
* Revisión de logs.
* Revisión de accesos.
* Evaluación del historial del repositorio.

Cuando corresponda, también se evaluará si el secreto estuvo disponible públicamente durante un período suficiente para considerarlo comprometido.

---

## 11. Pruebas de seguridad

Las pruebas de seguridad deben realizarse de forma responsable.

Se permite investigar problemas en:

* Entornos locales.
* Entornos de prueba autorizados.
* Cuentas creadas específicamente para pruebas.

No está permitido realizar pruebas destructivas o intrusivas contra producción sin autorización explícita.

En particular, no deben realizarse sin autorización:

* Denegación de servicio.
* Destrucción de datos.
* Modificación masiva de información.
* Extracción masiva de datos.
* Ataques contra cuentas de terceros.
* Ingeniería social contra usuarios o colaboradores.
* Acceso persistente a sistemas.
* Instalación de puertas traseras.
* Alteración deliberada de infraestructura.

---

## 12. Minimización durante una prueba

Al demostrar una vulnerabilidad:

1. Utiliza la mínima cantidad de información necesaria.
2. Accede únicamente a los recursos necesarios para confirmar el problema.
3. No descargues información innecesaria.
4. No modifiques datos que no sean necesarios para la prueba.
5. No elimines información.
6. No mantengas acceso después de confirmar la vulnerabilidad.
7. Detén la prueba si existe riesgo de afectar a usuarios reales.

El objetivo debe ser **demostrar el problema, no maximizar el impacto**.

---

## 13. Safe Harbor

PROVIWEB considera que las investigaciones de seguridad realizadas **de buena fe y de manera responsable** forman parte de la mejora del proyecto.

Cuando una persona:

* Actúe de buena fe.
* Respete esta política.
* Evite acceder a información innecesaria.
* No cause daños deliberados.
* No utilice una vulnerabilidad para obtener beneficios personales.
* No realice ataques destructivos.
* Informe el problema de manera responsable.

PROVIWEB procurará no interpretar dicha actividad como un comportamiento malicioso únicamente por el hecho de haber identificado o demostrado una vulnerabilidad.

Sin embargo, esta política **no constituye una autorización general para acceder a sistemas, cuentas, información o infraestructura de terceros**.

El Safe Harbor no cubre:

* Robo de información.
* Exfiltración masiva de datos.
* Denegación deliberada de servicio.
* Destrucción de información.
* Persistencia no autorizada.
* Uso fraudulento de cuentas.
* Extorsión.
* Amenazas.
* Ingeniería social.
* Ataques contra terceros.
* Uso de vulnerabilidades para obtener beneficios ilegítimos.

Cuando una investigación pueda afectar significativamente a producción o a usuarios reales, debe detenerse tan pronto como se haya obtenido evidencia suficiente y reportarse el problema.

---

## 14. Divulgación responsable

Solicitamos que las vulnerabilidades se mantengan privadas mientras exista una posibilidad razonable de que su divulgación pueda facilitar ataques.

No publiques:

* Exploits funcionales.
* Credenciales.
* Tokens.
* Datos privados.
* Detalles operativos que permitan comprometer producción.
* Información que facilite ataques contra usuarios.

La divulgación pública de una vulnerabilidad podrá evaluarse después de que exista una mitigación razonable y se haya coordinado, cuando corresponda, con las partes afectadas.

---

## 15. Vulnerabilidades de terceros

Si una vulnerabilidad afecta principalmente a una dependencia, proveedor o servicio externo, el problema podrá ser comunicado al responsable correspondiente.

Esto puede incluir:

* Firebase.
* Google Cloud.
* Bibliotecas utilizadas por el proyecto.
* Proveedores de infraestructura.
* Servicios externos utilizados por PROVIWEB.

En estos casos se evitará publicar información sensible antes de que exista una mitigación razonable.

---

## 16. Reportes que no constituyen necesariamente vulnerabilidades

No todos los errores representan una vulnerabilidad de seguridad.

Por ejemplo:

* Bugs visuales.
* Errores de UX.
* Problemas de documentación.
* Errores funcionales sin impacto de seguridad.
* Solicitudes de nuevas funcionalidades.

Estos problemas pueden reportarse mediante los canales habituales del proyecto.

Si existe duda sobre si un problema tiene implicaciones de seguridad, es preferible tratarlo inicialmente como un posible incidente de seguridad y utilizar un canal privado.

---

## 17. Protección del reportante

Los reportes realizados de buena fe serán tratados con seriedad.

PROVIWEB no busca penalizar a una persona simplemente por descubrir y reportar responsablemente una vulnerabilidad.

No obstante, el Safe Harbor no protege actividades que excedan significativamente una investigación responsable o que impliquen daño deliberado, fraude, abuso o acceso ilegítimo a información de terceros.

---

## 18. Actualizaciones de seguridad

Las correcciones de seguridad pueden implicar:

* Cambios de código.
* Actualizaciones de dependencias.
* Cambios en reglas Firebase.
* Rotación de credenciales.
* Cambios en Cloud Functions.
* Modificación de configuraciones.
* Actualización de documentación.
* Restricción temporal de funcionalidades.
* Cambios en flujos OAuth.

Cuando sea necesario, los cambios podrán desplegarse de manera prioritaria para reducir el riesgo antes de completar mejoras secundarias.

---

## 19. Reconocimiento

Cuando corresponda, PROVIWEB podrá reconocer públicamente a quienes hayan contribuido responsablemente a identificar vulnerabilidades, siempre que:

* La persona lo autorice.
* No exista un riesgo de seguridad o privacidad.
* La divulgación sea apropiada para el caso.

No se publicarán datos personales sin autorización.

---

## 20. Contacto de seguridad

Los reportes de seguridad deben enviarse mediante un canal privado de mantenimiento o seguridad administrado por PROVIWEB.

**No utilices Issues públicos para reportar vulnerabilidades que puedan comprometer cuentas, información, credenciales, infraestructura o servicios de producción.**

---

## Principio de seguridad

> **La seguridad se construye protegiendo a los usuarios, reduciendo el impacto y corrigiendo los problemas de forma responsable.**

PROVIWEB agradece a investigadores, colaboradores y usuarios que ayuden a identificar y solucionar problemas de seguridad de manera responsable.

No se tomarán acciones contra investigadores que:

1. Actúen de buena fe.
2. Eviten afectar disponibilidad o datos de usuarios.
3. Reporten de forma responsable y confidencial.
