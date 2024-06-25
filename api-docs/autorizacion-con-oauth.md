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

## Obtención del código de autorización

<mark style="color:blue;">`GET`</mark> `/oauth/authorize` (No lleva el prefijo v2 en este endpoint)

Redirige al usuario a la URL para que pueda dar a tu aplicación permiso de acceder a su cuenta. En caso afirmativo, volverá a tu URL de callback con un código de autorización, necesario para canjear luego por un token.

#### Query Parameters

| Name                                             | Type   | Description                                                                                                                                                                               |
| ------------------------------------------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| response\_type<mark style="color:red;">\*</mark> | String | Debe ser `code`                                                                                                                                                                           |
| client\_id<mark style="color:red;">\*</mark>     | String | El `client_id` de la app del marketplace                                                                                                                                                  |
| redirect\_uri<mark style="color:red;">\*</mark>  | String | La URL de redirección, debe estar autorizada previamente                                                                                                                                  |
| scope<mark style="color:red;">\*</mark>          | String | <p>Se deben indicar explicitamente los permisos requeridos (separados por coma).<br><a href="autorizacion-con-oauth.md#permisos-disponibles-para-solicitar-en-scope">Ver permisos</a></p> |
| state<mark style="color:red;">\*</mark>          | String | String generado en el momento que luego deberás usar para validar el código. Se recomienda guardarlo en la sesión del usuario.                                                            |

Ejemplo:

`https://api.zippin.com.ar/oauth/authorize?response_type=code&client_id=9cdnd3-d3d3d-er3gfre43-e2e2e&scope=shipments.quote,shipments.create&state=32323231&redirect_uri=https://misitio.com/zippin/callback`

Si el usuario autorizó el acceso, volverá a la URL especificada como redirect\_uri, incluyendo un `code` en el query string.

### Paso 2: Validar la autorización y canjear el código por un token

#### Valida el state

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
| shipments.create                 | Crear envíos                      |
| shipment\_documentation.download | Descargar documentación del envío |
| deliveries.show                  | Buscar/ver envíos                 |
| deliveries.update                | Actualizar envíos                 |
| **Cuenta y Orígenes**            |                                   |
| accounts.show                    | Ver info de la cuenta             |
| addresses.show                   | Ver orígenes                      |
| addresses.create                 | Crear origen                      |
| addresses.destroy                | Eliminar origenes                 |
| **Fulfillment**                  |                                   |
| stocks.read                      | Ver stocks                        |
