# 🌐 PROVIWEB Web & Connect

<p align="center">
  <strong>El ecosistema digital donde la música se conecta, aprende, crea y crece.</strong>
</p>

<p align="center">
  Tecnología al servicio de la música. 🎵
</p>

<p align="center">
  <a href="https://proviweb.com/">Sitio web</a> •
  <a href="https://proviweb.com/docs-sso.html">Documentación SSO</a> •
  <a href="https://github.com/PROVIWEB-App/PROVIWEB-Public">Aplicación Android</a>
</p>

---

## 📖 Descripción

**PROVIWEB** es una plataforma digital orientada al ecosistema musical que combina una experiencia web pública, servicios de identidad y autenticación, funcionalidades sociales y una infraestructura de integración para aplicaciones externas.

Este repositorio contiene el **sitio web público y los servicios backend de PROVIWEB Connect**, incluyendo la infraestructura necesaria para permitir que aplicaciones móviles y sitios web de terceros utilicen **"Iniciar sesión con PROVIWEB"** mediante **OAuth 2.0 Authorization Code + PKCE (S256)**.

El proyecto funciona como uno de los componentes centrales del ecosistema PROVIWEB.

---

# 🧩 Ecosistema PROVIWEB

PROVIWEB está compuesto por diferentes componentes que trabajan conjuntamente:

| Proyecto                      | Descripción                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------- |
| 🌐 **PROVIWEB Web & Connect** | Sitio público, documentación, autorización OAuth y backend de identidad.      |
| 📱 **PROVIWEB Android**       | Aplicación móvil principal del ecosistema PROVIWEB.                           |
| 🔐 **PROVIWEB Connect**       | Infraestructura de autenticación y SSO para aplicaciones externas.            |
| 🎵 **Apps del ecosistema**    | Aplicaciones que pueden integrarse mediante el sistema de identidad PROVIWEB. |

### 📱 Aplicación Android

El código público de la aplicación Android se encuentra en:

**PROVIWEB-Public**

https://github.com/PROVIWEB-App/PROVIWEB-Public

Este repositorio contiene el proyecto Android y constituye el cliente móvil principal del ecosistema.

---

# 🌐 Sitio web

La versión pública del sitio está desplegada mediante **Firebase Hosting**.

### Sitio oficial

https://proviweb.com/

La landing presenta:

* Identidad y marca PROVIWEB.
* Funcionalidades principales.
* Ecosistema de aplicaciones.
* Juegos y experiencias.
* Sistema QAV.
* Información del proyecto.
* Capturas y material visual.
* Enlaces de acceso y documentación.

---

# 🔐 PROVIWEB Connect

**PROVIWEB Connect** proporciona una infraestructura de identidad para que otras aplicaciones puedan permitir a sus usuarios autenticarse utilizando su cuenta PROVIWEB.

Conceptualmente funciona como:

> **"Iniciar sesión con PROVIWEB"**

El sistema está basado en:

* OAuth 2.0
* Authorization Code Flow
* PKCE
* SHA-256 (`S256`)
* Firebase Authentication
* Firebase Cloud Functions
* Firebase Hosting

PKCE con `S256` es obligatorio para las integraciones soportadas.

---

# ✨ Funcionalidades principales

## 🌐 Landing pública

Archivo principal:

```text
public/index.html
```

Incluye la presentación pública de PROVIWEB, funcionalidades, identidad visual, juegos, QAV, equipo, capturas y enlaces importantes.

---

## 🧭 Barra superior optimizada

El sitio incorpora una navegación superior:

* Fija.
* Responsive.
* Adaptada a dispositivos móviles.
* Con acceso rápido a las principales secciones.
* Sin superposiciones en diferentes resoluciones.

---

## 🔧 Bloqueo temporal de autenticación web

Determinadas páginas de autenticación web pueden utilizar un mecanismo temporal de mantenimiento.

Cuando está activo, el usuario recibe una interfaz indicando que el servicio se encuentra:

> **En Reparaciones**

y puede ser redirigido hacia la página de descarga de la aplicación.

Este mecanismo permite mantener controlados determinados flujos web mientras se realizan cambios en la infraestructura.

---

# 🔑 Autorización SSO

Archivo:

```text
public/oauth-authorize.html
```

Esta página constituye el punto central de autorización de PROVIWEB Connect.

Realiza, entre otras funciones:

* Validación del `client_id`.
* Validación exacta de `redirect_uri`.
* Validación de scopes.
* Validación de parámetros OAuth.
* Verificación de PKCE.
* Autenticación del usuario.
* Registro de nuevos usuarios cuando corresponde.
* Presentación del consentimiento.
* Emisión del authorization code.
* Redirección segura hacia la aplicación solicitante.

El usuario mantiene el control sobre la autorización solicitada por cada aplicación.

---

# 🛠️ Consola OAuth

Archivo:

```text
public/admin.html
```

Proporciona una interfaz administrativa para gestionar clientes OAuth.

Permite trabajar con información como:

```text
client_id
redirect_uri
scopes
```

También permite realizar pruebas rápidas del flujo de autorización.

---

# 📚 Documentación SSO

Archivo:

```text
public/docs-sso.html
```

Documentación orientada a desarrolladores que desean integrar PROVIWEB Connect.

Incluye información sobre:

* Registro de aplicaciones.
* OAuth 2.0.
* PKCE.
* Authorization Code Flow.
* Parámetros de autorización.
* Callback.
* Token exchange.
* Scopes.
* Ejemplos de integración.
* Pruebas desde Android.
* Integración con sitios web.

### Documentación online

https://proviweb.com/docs-sso.html

---

# ⚙️ Backend

Archivo principal:

```text
functions/index.js
```

El backend implementa la infraestructura necesaria para el flujo OAuth.

Entre sus responsabilidades se encuentran:

* Emisión de authorization codes.
* Canje de authorization codes.
* Endpoint `/oauth/token`.
* Generación de Firebase Custom Tokens.
* Validación de clientes.
* Validación estricta de `redirect_uri`.
* Validación de `client_id`.
* Validación de scopes.
* Validación de PKCE.
* Control de expiración.
* Control de uso único de authorization codes.
* Firma de códigos de autorización.

---

# 🌍 Endpoints públicos

## Authorization Endpoint

```text
https://proviweb.com/oauth-authorize.html
```

Este endpoint inicia el proceso de autenticación y autorización.

---

## Token Endpoint

Método:

```text
POST
```

Endpoint:

```text
https://projectname/oauth/token
```

Se utiliza para intercambiar un authorization code por las credenciales necesarias para autenticar al usuario en la aplicación cliente.

---

## Firebase Callable Function

```text
oauthTokenExchange
```

Región:

```text
us-central1
```

Permite realizar el intercambio mediante Firebase SDK cuando la arquitectura de la aplicación lo requiere.

---

# 🔄 Flujo de integración

El flujo general de PROVIWEB Connect es:

```text
┌──────────────────────┐
│   Aplicación cliente │
│   Android / Web      │
└──────────┬───────────┘
           │
           │ 1. Authorization Request
           ▼
┌──────────────────────┐
│ PROVIWEB Connect     │
│ oauth-authorize.html │
└──────────┬───────────┘
           │
           │ 2. Login
           │ 3. Consentimiento
           ▼
┌──────────────────────┐
│ Usuario PROVIWEB     │
└──────────┬───────────┘
           │
           │ 4. Authorization Code
           ▼
┌──────────────────────┐
│ redirect_uri         │
│ de la aplicación     │
└──────────┬───────────┘
           │
           │ 5. POST /oauth/token
           │    + code_verifier
           ▼
┌──────────────────────┐
│ PROVIWEB Backend     │
└──────────┬───────────┘
           │
           │ 6. Firebase Custom Token
           ▼
┌──────────────────────┐
│ Aplicación cliente   │
│ Firebase Auth        │
└──────────────────────┘
```

---

# 🚀 Integración para terceros

Para integrar una aplicación con PROVIWEB Connect:

### 1. Registrar la aplicación

Registrar:

```text
client_id
redirect_uri
```

en la consola OAuth de PROVIWEB.

El `redirect_uri` debe coincidir exactamente con el registrado.

---

### 2. Utilizar Authorization Code

La aplicación debe utilizar:

```text
response_type=code
```

---

### 3. Utilizar PKCE

PKCE es obligatorio.

Método:

```text
code_challenge_method=S256
```

La aplicación debe generar un `code_verifier` seguro y derivar el `code_challenge`.

---

### 4. Solicitar scopes

Los scopes actualmente soportados son:

```text
profile
email
qav
```

---

### 5. Utilizar `state`

Las aplicaciones deben generar y validar un valor `state` para proteger el callback contra ataques CSRF.

---

# 🔗 Ejemplo de autorización

```text
https://projectname/oauth-authorize.html
?client_id=mi.app.tercero
&redirect_uri=https%3A%2F%2Fmi-dominio.com%2Foauth%2Fcallback
&response_type=code
&scope=profile+email+qav
&code_challenge=BASE64URL_SHA256_VERIFIER
&code_challenge_method=S256
&state=RANDOM_CSRF_TOKEN
```

---

# 🔄 Ejemplo de Token Exchange

```bash
curl -X POST "https://projectname/oauth/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code" \
  -d "client_id=mi.app.tercero" \
  -d "code=AUTH_CODE" \
  -d "code_verifier=ORIGINAL_PKCE_VERIFIER"
```

Respuesta esperada:

```json
{
  "success": true,
  "custom_token": "....",
  "token_type": "Bearer",
  "scope": "profile email qav",
  "user": {
    "uid": "...",
    "email": "..."
  }
}
```

---

# 📱 Redirect URIs oficiales

El ecosistema PROVIWEB contempla actualmente clientes oficiales con callbacks específicos.

### PROVIWEB

```text
com.project.a
proviweb://oauth/callback/app
```

### PROVIWEB Admin

```text
com.project.b
projectname-b://oauth/callback
```

### PROVIWEB Nunti

```text
com.project.c
projectname-c://oauth/callback
```

### PROVIWEB Pulso

```text
com.project.d
projectname-d://oauth/callback
```

Los `redirect_uri` deben mantenerse exactamente iguales a los valores registrados para cada cliente.

---

# 🛡️ Seguridad

PROVIWEB Connect incorpora diferentes mecanismos de protección para el flujo OAuth.

### PKCE

Se exige:

```text
Authorization Code + PKCE S256
```

---

### Validación del cliente

Se valida estrictamente:

```text
client_id
redirect_uri
```

antes de continuar con el flujo.

---

### Authorization Code

Los códigos de autorización:

* Tienen tiempo de vida limitado.
* Son de un solo uso.
* Se encuentran vinculados al cliente.
* Se encuentran vinculados al `redirect_uri`.
* Se encuentran protegidos mediante firma.

---

### Token Exchange

Durante el intercambio se valida la coherencia entre:

```text
client_id
authorization code
redirect_uri
code_verifier
```

---

### Scopes

Los scopes no reconocidos o no permitidos son rechazados.

Scopes soportados:

```text
profile
email
qav
```

---

# 📂 Estructura del proyecto

Una estructura simplificada del proyecto es:

```text
PROVIWEN-Web/
│
├── public/
│   ├── index.html
│   ├── oauth-authorize.html
│   ├── admin.html
│   ├── docs-sso.html
│   └── ...
│
├── functions/
│   └── index.js
│
├── docs/
│   └── OAUTH_PKCE_BACKEND.md
│
├── firebase.json
├── .gitignore
└── README.md
```

---

# 🔥 Tecnologías

El proyecto utiliza principalmente:

* HTML5
* CSS3
* JavaScript
* Firebase Hosting
* Firebase Authentication
* Firebase Cloud Functions
* Firebase Realtime Database
* OAuth 2.0
* PKCE / S256

---

# 📱 Proyecto Android

La aplicación Android principal de PROVIWEB cuenta con un repositorio público independiente.

### Repositorio

https://github.com/PROVIWEB-App/PROVIWEB-Public

En él se encuentra el código fuente público de la aplicación Android y sus componentes asociados.

**PROVIWEB Web & Connect** y **PROVIWEB Android** forman parte del mismo ecosistema, pero se mantienen en repositorios independientes para facilitar su desarrollo, mantenimiento y distribución.

---

# 🧭 Roadmap

PROVIWEB Connect está diseñado como una base para ampliar progresivamente el ecosistema de identidad.

Entre las posibilidades futuras se encuentran:

* Incorporación de nuevos clientes OAuth.
* Integración de más aplicaciones externas.
* Ampliación de scopes.
* Herramientas adicionales para desarrolladores.
* Mejoras de administración de clientes.
* Mejoras de observabilidad y auditoría.
* Expansión del concepto **"Login with PROVIWEB"**.

---

# 📖 Documentación técnica adicional

Para información específica sobre la implementación del backend OAuth/PKCE:

```text
docs/OAUTH_PKCE_BACKEND.md
```

La documentación pública para integradores está disponible en:

https://proviweb-d8764-c592e.web.app/docs-sso.html

---

# 🎵 PROVIWEB

**Tecnología al servicio de la música.**

PROVIWEB busca construir un ecosistema digital donde artistas, músicos, aplicaciones y servicios puedan conectarse dentro de una infraestructura común.

🌐 Web
📱 Mobile
🔐 Identity
🎵 Música
🚀 Ecosistema

---

## 📄 Licencia

Consulta los archivos de licencia incluidos en este repositorio para conocer los términos aplicables al uso y distribución del proyecto.
