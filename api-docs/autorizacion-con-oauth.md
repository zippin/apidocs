---
description: >-
  Aprende como integrarte a Zippin utilizando oAuth para la conexión a una
  cuenta Zippin.
---

# Autorización con oAuth

## Cuándo utilizar la autenticación por oAuth

Recomendamos el uso de este método de autenticación cuando tu integración sirva para conectar múltiples cuentas de Zippin de diferentes clientes.

Si la integración que estás haciendo es para tu propia cuenta Zippin, quizas sea mejor que utilices credenciales simples, como indica [ésta página](urls-y-autenticacion.md).

## Requisitos

Antes de avanzar con el proceso de autorización, deberás tener una aplicación dada de alta en Zippin. Para obtener tu aplicación, completa el [formulario de alta](https://docs.google.com/forms/d/e/1FAIpQLSdkQDnuTohgHv8chHeX1me-M1QRpVTVADlGtUvWFJwHxIUI7Q/viewform?usp=sf\_link).

Deberás informarnos la o las URLs que quieras dar de alta para el callback del proceso de autorización

Una vez que demos de alta la aplicación, te proveeremos de los siguientes datos:

* `client_id`
* `client_secret`

## Procedimiento

### Paso 1: Solicitar código de autorización

Como primer paso, deberás redirigir al usuario a la siguiente URL, para que pueda autorizar el acceso a la aplicación.

## Obtención del código de autorización

Redirige al usuario a la URL <mark style="color:blue;">https://api.zippin.</mark>_<mark style="color:blue;">pais</mark>_<mark style="color:blue;">/oauth/authorize?response\_type=code\&state=XX\&client\_id=ID\&scope=...\&redirect\_uri=URL</mark> (ver a continuación cómo armarla) para que pueda dar a tu aplicación permiso de acceder a su cuenta.&#x20;

En caso que el usuario autorice a tu aplicación a acceder a la cuenta, lo redirigiremos a tu URL de callback con un código de autorización, necesario para canjear luego por un token.

#### Parámetros de la URL de autorización

| Name                                             | Type   | Description                                                                                                                                                                                                                |
| ------------------------------------------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| response\_type<mark style="color:red;">\*</mark> | String | Debe ser `code`                                                                                                                                                                                                            |
| client\_id<mark style="color:red;">\*</mark>     | String | El `client_id` de la app del marketplace                                                                                                                                                                                   |
| redirect\_uri<mark style="color:red;">\*</mark>  | String | La URL de redirección, debe estar habilitada previamente en la aplicación al darla de alta.                                                                                                                                |
| scope<mark style="color:red;">\*</mark>          | String | <p>Se deben indicar explícitamente los permisos requeridos (<strong>separados por espacios</strong>).<br><a href="autorizacion-con-oauth.md#permisos-disponibles-para-solicitar-en-scope">Ver permisos disponibles</a></p> |
| state<mark style="color:red;">\*</mark>          | String | String generado en el momento que luego podrás usar para validar el código. Se recomienda guardarlo en la sesión del usuario.                                                                                              |

Ejemplo:

`https://api.zippin.com.ar/oauth/authorize?response_type=code&client_id=9c5nd3-dxd3d-er3gfre43-e2e2e&scope=shipments.quote shipments.create&state=32323231&redirect_uri=https://misitio.com/zippin/callback`

Si el usuario autorizó el acceso, volverá a la URL especificada como redirect\_uri, incluyendo un `code` en el query string, por ejemplo https://misitio.com/zippin/callback?code=def502000854851...\&state=32323231

### Paso 2: Validar la autorización y canjear el código por un token

#### Valida el state

Ese paso no es obligatorio, pero es aconsejable para dar mas seguridad al proceso.&#x20;

Antes de canjear el código recibido por un token, deberías validar que el state recibido en la URL coincida con el que habías enviado en el paso anterior. Si no coincide, deberías anular el flujo.

#### Canjea el código por un token

## Obtención del access token

<mark style="color:green;">`POST`</mark> `/oauth/token` (No lleva el prefijo v2 en este endpoint)

Canjear un código de autorización por un access token

#### Query Parameters

| Name                                             | Type   | Description                                           |
| ------------------------------------------------ | ------ | ----------------------------------------------------- |
| grant\_type<mark style="color:red;">\*</mark>    | String | Debe ser `authorization_code`                         |
| client\_id<mark style="color:red;">\*</mark>     | String | El `client_id` de la app                              |
| redirect\_uri<mark style="color:red;">\*</mark>  | String | La URL de callback, debe estar autorizada previamente |
| code<mark style="color:red;">\*</mark>           | String | Envía el código obtenido previamente.                 |
| client\_secret<mark style="color:red;">\*</mark> | String | El `client_secret` de la app                          |

Como resultado del request anterior obtendrás un json conteniendo un `access_token`, un `refresh_token` y un atributo `expires_in`, que expresa la cantidad de segundos hasta que expire el token recibido.

#### Request Ejemplo

```bash
curl --request POST \
  --url https://api.zippin.xx/oauth/token \
  --header 'Content-Type: application/json' \
  --data '{
	"grant_type": "authorization_code",
	"code": "def502000854851...",
	"client_id": "9c8f-f139-4cfe-9be1-6e799",
	"client_secret": "Qzxs7u9WN3GsE24BwNJrWx8VVcvhjZ",
	"redirect_uri": "https://misitio.com/zippin/callback"
}'

```

#### Respuesta Ejemplo

```json
{
	"token_type": "Bearer",
	"expires_in": 31536000,
	"access_token": "eyJ0eXAiOiJ2329scsd9bGciOiJSUzI1NiJ9.eyJhdWQiOiI5YzVkMzQwskdm399OTAxNWUzOTkiLCJqdGkiOiJjNTliOWE5NG",
	"refresh_token": "def502005b186b4a8762ee98bd64aff4aab2a2ee6c8917777d5d5e5733e36fcc2989eae43f38d12c6b19b0f0ac97aa218"
}
```

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

Si bien los tokens tienen una larga duración, **tienen un vencimiento** (un año, inicialmente), lo que implica que será necesario refrescarlos periódicamente para obtener un nuevo token.

## Refrescar access token

<mark style="color:green;">`POST`</mark> `/oauth/token` (No lleva el prefijo v2 en este endpoint)

Canjear un código de autorización por un access token

#### Query Parameters

| Name                                             | Type   | Description                                    |
| ------------------------------------------------ | ------ | ---------------------------------------------- |
| grant\_type<mark style="color:red;">\*</mark>    | String | Debe ser **`refresh_token`**                   |
| client\_id<mark style="color:red;">\*</mark>     | String | El `client_id` de la app                       |
| refresh\_token<mark style="color:red;">\*</mark> | String | Envía el `refresh_token` recibido previamente. |
| client\_secret                                   | String | El `client_secret` de la app                   |

Como resultado del request anterior volverás a obtener un json conteniendo un nuevo `access_token`, un `refresh_token` y un atributo `expires_in`, que expresa la cantidad de segundos hasta que expire el token recibido.

## Permisos disponibles para solicitar en `scope`

| Permiso                          | Descripción                       |
| -------------------------------- | --------------------------------- |
| **Envíos**                       |                                   |
| shipments.quote                  | Cotizar envíos                    |
| shipments.create                 | Crear/Actualizar envíos           |
| shipment\_documentation.download | Descargar documentación del envío |
| shipments.show                   | Buscar/ver envíos                 |
| shipments.update                 | Actualizar estados de envíos      |
| **Ordenes**                      |                                   |
| orders.view                      | Ver Ordenes                       |
| orders.create                    | Crear ordenes                     |
| orders.update                    | Actualizar Ordenes                |
| **Cuenta y Orígenes**            |                                   |
| accounts.show                    | Ver info de la cuenta             |
| addresses.show                   | Ver orígenes                      |
| addresses.create                 | Crear origen                      |
| addresses.destroy                | Eliminar origenes                 |
| **Fulfillment**                  |                                   |
| stocks.read                      | Ver stocks                        |
