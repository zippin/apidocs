# URLs y Autenticación

## Obtén tus credenciales de API

Todos los requests deben utilizar autenticación.&#x20;

Existen actualmente dos métodos de autenticarse:

| Método                                                                                               | Cuando usar                                                                   | Ejemplos                                                                                                                            |
| ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [Autenticación básica HTTP con credenciales de cuenta](urls-y-autenticacion.md#autenticacion-basica) | En una integración exclusiva para una cuenta.                                 | <p>Un desarrollo custom.<br>Una tienda propia.</p>                                                                                  |
| <p><a href="autorizacion-con-oauth.md">oAuth2 (access_token)</a><br></p>                             | Cuando la integración será usada por múltiples cuentas Zippin independientes. | <p>Marketplaces.</p><p>Sistemas de ecommerce.<br>Productos SaaS multi cliente como gestores de ordenes, productos, etc.<br>ERPs</p> |

### Autenticación básica

Utiliza la autenticación básica HTTP usando un API token y API secret, generados en la cuenta a la que se quiere acceder.

#### Generación de credenciales para autenticación básica

Para generar tus credenciales debes ingresar a la cuenta de Zippin que estás integrando y acceder a la sección **Configuración > Integraciones > Gestionar credenciales y webhooks**

Allí, podrás crear un nuevo token de cuenta, el cual consiste en un **API Token** y un **API Secret.**

{% hint style="info" %}
Tus requests deberán utilizar [**autenticación básica de HTTP**](https://es.wikipedia.org/wiki/Autenticaci%C3%B3n\_de\_acceso\_b%C3%A1sica)**.**

**Usuario:** API Token \
**Contraseña:** API Secret
{% endhint %}

Si la librería que estás usando no acepta que se le indique un usuario y contraseña de autenticación básica deberás contruir manualmente el header Authorization [como se indica en esta guía](https://diego.com.es/autenticacion-http).

### Autenticación con access\_token

Deberás enviar un Bearer token en cada request. Tendrás un access\_token por cada cuenta de Zippin que te autorice el acceso.

Para ver cómo obtener y refrescar los tokens, revisa la ayuda sobre la [autenticación con Oauth2](autorizacion-con-oauth.md).

## Headers obligatorios

Todas las respuestas de la API que devuelvan datos utilizarán JSON para estructurarlos.

En los requests POST o PUT, donde se debe enviar información, se debe utilizar JSON, con lo cual es necesario.

Para armar el request, es obligatorio incluir el siguiente header (además del de autenticación):

```
Accept: application/json
```

Y si se envían datos a Zippin en el body, deberás incluir el header de Content-Type:

```
Content-Type: application/json
```

## Armado de la URL de los endpoints

Todas las URLs tendrán el siguiente formato:

> https://api.<mark style="color:purple;">`{dominio}`</mark>/<mark style="color:orange;">`{version}`</mark>/<mark style="color:red;">`{endpoint}`</mark>

Donde:

* <mark style="color:purple;">`dominio`</mark>
  * 🇦🇷 Argentina: `zippin.com.ar`&#x20;
  * 🇨🇱 Chile: `zippin.cl`
  * 🇲🇽 México: `zippin.com.mx`
* <mark style="color:orange;">`version`</mark>
  * Actualmente `v2` es la única opción.
* <mark style="color:red;">`endpoint`</mark>
  * Es el que indica la documentación. Por ejemplo: `shipments`

Ejemplo de la URL: **https://api.**<mark style="color:purple;">**zippin.com.ar**</mark>**/**<mark style="color:orange;">**v2**</mark>**/**<mark style="color:red;">**shipments**</mark>&#x20;

{% hint style="warning" %}
Los endpoints de [autenticación oAuth](autorizacion-con-oauth.md) no utilizan el prefijo de versión.
{% endhint %}

## Ejemplo de request CURL

```
curl --request POST \
  --url https://api.zippin.com.ar/v2/shipments/quote \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic YWRtaWXXXXXXXXXXFvIQ==' \
  --header 'Content-Type: application/json' \
  --data '{
	"account_id": "3355",
	"origin_id": "9323",
	"declared_value": 9015,
	"items": [
		{
			"sku": "103964928",
			"weight": 99,
			"height": 10,
			"width": 5,
			"length": 10,
			"description": "Capsulas The Coffee Store comp. Nespresso Mix",
			"classification_id": 1
		},
		{
			"sku": "103964928",
			"weight": 500,
			"height": 5,
			"width": 10,
			"length": 10,
			"description": "Capsulas The Coffee Store comp. Nespresso Mix",
			"classification_id": 1
		}
	],
	"destination": {
		"city": "cordoba",
		"state": "cordoba",
		"zipcode": "5000"
	}
}'
```
