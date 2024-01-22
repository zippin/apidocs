---
description: >-
  Aprende como integrarte a Zippin utilizando oAuth para la conexión a una
  cuenta Zippin.
---

# Autorización con oAuth

## Requisitos

Antes de avanzar con el proceso de autorización, deberás tener una aplicación dada de alta en Zippin. Para obtener tu aplicación, completa el [formulario de alta](https://docs.google.com/forms/d/e/1FAIpQLSdkQDnuTohgHv8chHeX1me-M1QRpVTVADlGtUvWFJwHxIUI7Q/viewform?usp=sf\_link).

Una vez que la demos de alta, te proveeremos de los siguientes datos:

* `client_id`
* `client_secret`

## Procedimiento

### Paso 1: Solicitar código de autorización

Como primer paso, deberás redirigir al usuario a la siguiente URL, para que pueda autorizar el acceso a la aplicación.

{% swagger method="get" path="/oauth/authorize" baseUrl="" summary="Obtención del código de autorización" %}
{% swagger-description %}
Obtener un código de autorización.
{% endswagger-description %}

{% swagger-parameter in="query" name="response_type" required="true" %}
Debe ser "code"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_id" required="true" %}
El client\_id de la app del marketplace
{% endswagger-parameter %}

{% swagger-parameter in="query" name="redirect_uri" required="true" %}
La URL de redirección, debe estar autorizada previamente
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state" required="true" %}
String generado en el momento que luego deberás usar para validar el código. Se recomienda guardarlo en la sesión del usuario.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="scope" required="false" %}
Se deben indicar los permisos requeridos, o utilizar \* para todos los permisos posibles.
{% endswagger-parameter %}
{% endswagger %}

Si el usuario autorizó el acceso, volverá a la URL especificada como redirect\_uri, incluyendo un `code` en el query string.

### Paso 2: Validar la autorización y canjear el código por un token

#### Valida el state

Antes de canjear el código recibido por un token, debería validar que el state recibido en la URL coincida con el que habías enviado en el paso anterior. Si no coincide, deberías anular el flujo.

#### Canjea el código por un token

{% swagger method="post" path="/oauth/token" baseUrl="" summary="Obtención del access token" %}
{% swagger-description %}
Canjear un código de autorización por un access token
{% endswagger-description %}

{% swagger-parameter in="query" name="grant_type" required="true" %}
Debe ser "authorization\_code"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_id" required="true" %}
El client\_id de la app del marketplace
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_secret" %}
El client\_secret de la app del marketplace
{% endswagger-parameter %}

{% swagger-parameter in="query" name="redirect_uri" required="true" %}
La URL de callback, debe estar autorizada previamente
{% endswagger-parameter %}

{% swagger-parameter in="query" name="code" required="true" %}
Envía el código obtenido previamente.
{% endswagger-parameter %}
{% endswagger %}

Como resultado del request anterior obtendrás un json conteniendo un `access_token`, un `refresh_token` y un atributo `expires_in`, que expresa la cantidad de segundos hasta que expire el token recibido.

### Paso 3: Utilizar el token para los requests a la API

Todos los llamados que se hagan a la API de Zippin deben estar autenticados. Para ello se deberá enviar un header Authorization, tipo Bearer, con el access token obtenido en el flujo de autorización.

```bash
curl -X GET \
https://api.zippin.com.xx/v2/shipments \
-H 'Accept: application/json' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {access_token}'

```

### Paso 4: Refrescar el token

Si bien los tokens tienen una larga duración, tienen un vencimiento, lo que implica que será necesario refrescarlos periódicamente para obtener un nuevo token.

{% swagger method="post" path="/oauth/token" baseUrl="" summary="Refrescar access token" %}
{% swagger-description %}
Canjear un código de autorización por un access token
{% endswagger-description %}

{% swagger-parameter in="query" name="grant_type" required="true" %}
Debe ser **`refresh_token`**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_id" required="true" %}
El client\_id de la app del marketplace
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_secret" %}
El client\_secret de la app del marketplace
{% endswagger-parameter %}

{% swagger-parameter in="query" name="refresh_token" required="true" %}
Envía el refresh\_token recibido previamente.
{% endswagger-parameter %}
{% endswagger %}

Como resultado del request anterior volverás a obtener un json conteniendo un nuevo `access_token`, un `refresh_token` y un atributo `expires_in`, que expresa la cantidad de segundos hasta que expire el token recibido.
