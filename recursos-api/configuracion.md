---
description: >-
  Aprende cómo configurar distintos aspectos de tu cuenta como orígenes, y
  webhooks.
---

# Configuración

## Cuentas

Revisa la información de las cuentas a las que tienes acceso.

<mark style="color:blue;">`GET`</mark> `/v2/accounts`

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
	"data": [
		{
			"id": 2,
			"name": "Demo",
			"logo": null,
			"company_name": "Zippin",
			"address": {
				"address": "Calle 24",
				"city": {
					"id": 1,
					"name": "Capital Federal"
				},
				"state": "Capital Federal",
				"zipcode": "1200"
			},
			"national_tax_id": {
				"number": "30-99925367-5",
				"type": "CUIT"
			},
			"state_tax_id": "Buenos Aires",
			"tax_category": "Responsable Inscripto",
			"email": "main-email@domain.com.ar",
			"phone": "-",
			"contacts": {
				"operations": {
					"name": null,
					"email": null,
					"phone": null
				},
				"administrative": {
					"name": "Jose Noguera",
					"email": "jose.noguera@zippin.com.ar",
					"phone": null
				}
			}
		},
		{
			"id": 11876,
			"name": "My Testing APP",
			"logo": null,
			"company_name": "Envios de Prueba SRL",
			"address": {
				"address": "Luna 124",
				"city": {
					"id": 27,
					"name": "Parque Patricios"
				},
				"state": "Capital Federal",
				"zipcode": "1437"
			},
			"national_tax_id": {
				"number": "20-22222222-3",
				"type": "CUIT"
			},
			"state_tax_id": null,
			"tax_category": "Responsable Inscripto",
			"email": "test@zippin.com.ar",
			"phone": "222222",
			"contacts": {
				"operations": {
					"name": null,
					"email": null,
					"phone": null
				},
				"administrative": {
					"name": null,
					"email": null,
					"phone": null
				}
			}
		}
	],
	"links": {
		"first": "http:\/\/api.zippin.local.ar\/v2\/accounts?page=1",
		"last": "http:\/\/api.zippin.local.ar\/v2\/accounts?page=1",
		"prev": null,
		"next": null
	},
	"meta": {
		"current_page": 1,
		"from": 1,
		"last_page": 1,
		"links": [
			{
				"url": null,
				"label": "&laquo; Previous",
				"active": false
			},
			{
				"url": "https:\/\/api.zippin.com.ar\/v2\/accounts?page=1",
				"label": "1",
				"active": true
			},
			{
				"url": null,
				"label": "Next &raquo;",
				"active": false
			}
		],
		"path": "https:\/\/api.zippin.com.ar\/v2\/accounts",
		"per_page": 20,
		"to": 2,
		"total": 2
	}
}
```


{% endtab %}
{% endtabs %}

## Orígenes

A la hora de crear un envío es obligatorio indicar el origen del mismo. Los orígenes se dan de alta en la cuenta de cada vendedor.

### Listar orígenes

<mark style="color:blue;">`GET`</mark> `/v2/addresses`

#### Query Parameters

| Name        | Type | Description     |
| ----------- | ---- | --------------- |
| account\_id | int  | ID de la cuenta |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "data": [
        {
            "id": 450,
            "name": "Direccion editada",
            "document": "20-32216766-6",
            "street": "Av Avellaneda",
            "street_number": "4055",
            "street_extras": "Local 34",
            "city": {
                "id": 1729,
                "name": "San Fernando"
            },
            "state": {
                "id": 2,
                "name": "Buenos Aires"
            },
            "zipcode": "1646",
            "phone": "1561222280",
            "email": "ejemplo@tienda.com.ar",
            "hours": {
                "open": "08:00",
                "close": "16:00"
            },
            "dropoff_only": false,
            "accounts": [
                {
                    "id": 70,
                    "name": "Tienda Local San Fernando",
                    "options": {
                        "automatic_status_change": true,
                        "pickup_days": [
                            "1",
                            "2",
                            "3",
                            "4",
                            "5"
                        ],
                        "preparation_time": null,
                        "use_preparation_time": false
                    }
                }
            ]
        },
        {
            (...)
        }
    ],
    "links": {
        "first": "https://api.zippin.com.ar/v2/addresses?page=1",
        "last": "https://api.zippin.com.ar/v2/addresses?page=6",
        "prev": null,
        "next": "https://api.zippin.com.ar/api/v2/addresses?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://api.zippin.com.ar/v2/addresses",
        "per_page": 20,
        "to": 20,
        "total": 2
    }
}
```


{% endtab %}
{% endtabs %}

### Detalle de un orígen

<mark style="color:blue;">`GET`</mark> `/v2/addresses/{id}`

Obtiene el detalle de un origen.

#### Path Parameters

| Name                                 | Type | Description   |
| ------------------------------------ | ---- | ------------- |
| id<mark style="color:red;">\*</mark> | int  | ID del origen |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "id": 450,
    "name": "Direccion editada",
    "document": "20-3226766-6",
    "street": "Av Avellaneda",
    "street_number": "4055",
    "street_extras": "Local 34",
    "city": {
        "id": 1729,
        "name": "San Fernando"
    },
    "state": {
        "id": 2,
        "name": "Buenos Aires"
    },
    "zipcode": "1646",
    "phone": "1561222280",
    "email": "ejemplo@tienda.com.ar",
    "hours": {
        "open": "08:00",
        "close": "16:00"
    },
    "dropoff_only": false,
    "accounts": [
        {
            "id": 70,
            "name": "Tienda Local San Fernando",
            "options": {
                "automatic_status_change": true,
                "pickup_days": [
                    "1",
                    "2",
                    "3",
                    "4",
                    "5"
                ],
                "preparation_time": null,
                "use_preparation_time": false
            }
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### Crear orígen

<mark style="color:green;">`POST`</mark> `/v2/addresses`

#### Request Body

| Name                                                                | Type    | Description                                                                                                                                            |
| ------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account\_id<mark style="color:red;">\*</mark>                       | int     | ID de la cuenta                                                                                                                                        |
| name<mark style="color:red;">\*</mark>                              | string  | Nombre del Origen                                                                                                                                      |
| document<mark style="color:red;">\*</mark>                          | string  | CUIT/RUT o identificacion del origen                                                                                                                   |
| street<mark style="color:red;">\*</mark>                            | string  | Calle del domicilio                                                                                                                                    |
| street\_number<mark style="color:red;">\*</mark>                    | string  | Número de puerta del domicilio                                                                                                                         |
| street\_extras<mark style="color:red;">\*</mark>                    | string  | Datos adicionales de la dirección de origen (piso, depto, local)                                                                                       |
| city<mark style="color:red;">\*</mark>                              | string  | Localidad (AR) \| Comuna (CL)                                                                                                                          |
| state<mark style="color:red;">\*</mark>                             | string  | Provincia (AR) \| Region (CL)                                                                                                                          |
| zipcode<mark style="color:red;">\*</mark>                           | string  | Código postal del origen (opcional en CL)                                                                                                              |
| phone<mark style="color:red;">\*</mark>                             | string  | Telefono del origen                                                                                                                                    |
| email<mark style="color:red;">\*</mark>                             | string  | E-mail del orígen                                                                                                                                      |
| hours<mark style="color:red;">\*</mark>                             | object  |                                                                                                                                                        |
| hours.open<mark style="color:red;">\*</mark>                        | string  | Horario de apertura. Ej. "09:00"                                                                                                                       |
| hours.close<mark style="color:red;">\*</mark>                       | string  | Horario de cierre. Ej. "18:00"                                                                                                                         |
| options<mark style="color:red;">\*</mark>                           | object  |                                                                                                                                                        |
| options.dropoff\_only<mark style="color:red;">\*</mark>             | boolean | Si es `true`, solo se ofrecerán transportes a los que se puedan imponer los envíos en una sucursal (no hay recolecciones para envios con éste origen). |
| options.automatic\_status\_change<mark style="color:red;">\*</mark> | boolean | Si es `true`, el estado del envío cambiará a Listo para Despacho cuando se obtenga la documentación del envío.                                         |
| options.pickup\_days<mark style="color:red;">\*</mark>              | array   | Indica qué días se podrá hacer recolecciones al origen. Las opciones van del 0 (domingo) al 6 (sábado).                                                |
| options.use\_preparation\_time<mark style="color:red;">\*</mark>    | boolean | Si es `true`, el estado del envío cambiará a Listo para Despacho a las X horas de creado (X se configura en la siguiente opción).                      |
| options.preparation\_time<mark style="color:red;">\*</mark>         | int     | Cantidad de horas desde la creación para que un envío pase automáticamente a Listo para Despacho.                                                      |
| logistic\_types                                                     | array   | Array con los códigos de los logistic\_types aceptados para este origen. Si se modifica, se debe enviar también `default_logistic_type`.               |
| default\_logistic\_type                                             | string  | Código del logistic\_type por defecto. Es obligatorio si se modifican los `logistic_types`.                                                            |

{% tabs %}
{% tab title="201: Created " %}
```javascript
// Ejemplo Request:
{
    "account_id": 7,
    "name": "Nueva direccion",
    "document": "20-31631866-6",
    "street": "Av San Martin",
    "street_number": "2345",
    "street_extras": "Nave 2",
    "city": "San Fernando",
    "state": "Buenos Aires",
    "zipcode": "1646",
    "phone": "119922444",
    "email": "sanfernando@tienda.com.ar",
    "logistic_types":[
                        "crossdock",
                        "carrier_pickup"
                    ],
    "default_logistic_type": "crossdock",
    "hours": {
        "open": "08:00",
        "close": "16:00"
    },
    "options": {
    	"automatic_status_change": false,
    	"pickup_days": [
                    "1",
                    "3",
                    "5"
                ]
    }
}
```
{% endtab %}
{% endtabs %}

### Modificar orígen

<mark style="color:orange;">`PUT`</mark> `/v2/addresses/{id}`

#### Path Parameters

| Name                                 | Type | Description   |
| ------------------------------------ | ---- | ------------- |
| id<mark style="color:red;">\*</mark> | int  | ID del origen |

#### Request Body

| Name                                          | Type    | Description                                                                                                                                            |
| --------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account\_id<mark style="color:red;">\*</mark> | int     | ID de la cuenta. Siempre es requerido en todos los requests.                                                                                           |
| name                                          | string  | Nombre del Origen                                                                                                                                      |
| document                                      | string  | CUIT/RUT o identificacion del origen                                                                                                                   |
| street                                        | string  | Calle del domicilio                                                                                                                                    |
| street\_number                                | string  | Número de puerta del domicilio                                                                                                                         |
| street\_extras                                | string  | Datos adicionales de la dirección de origen (piso, depto, local)                                                                                       |
| city                                          | string  | Localidad (AR) \| Comuna (CL)                                                                                                                          |
| state                                         | string  | Provincia (AR) \| Region (CL)                                                                                                                          |
| zipcode                                       | string  | Código postal del origen (opcional en CL)                                                                                                              |
| phone                                         | string  | Telefono del origen                                                                                                                                    |
| email                                         | string  | E-mail del orígen                                                                                                                                      |
| hours                                         | object  |                                                                                                                                                        |
| hours.open                                    | string  | Horario de apertura. Ej. "09:00"                                                                                                                       |
| hours.close                                   | string  | Horario de cierre. Ej. "18:00"                                                                                                                         |
| options                                       | object  |                                                                                                                                                        |
| options.dropoff\_only                         | boolean | Si es `true`, solo se ofrecerán transportes a los que se puedan imponer los envíos en una sucursal (no hay recolecciones para envios con éste origen). |
| options.automatic\_status\_change             | boolean | Si es `true`, el estado del envío cambiará a Listo para Despacho cuando se obtenga la documentación del envío.                                         |
| options.pickup\_days                          | array   | Indica qué días se podrá hacer recolecciones al origen. Las opciones van del 0 (domingo) al 6 (sábado).                                                |
| options.use\_preparation\_time                | boolean | Si es `true`, el estado del envío cambiará a Listo para Despacho a las X horas de creado (X se configura en la siguiente opción).                      |
| options.preparation\_time                     | int     | Cantidad de horas desde la creación para que un envío pase automáticamente a Listo para Despacho.                                                      |
| logistic\_types                               | array   | Array con los códigos de los logistic\_types aceptados para este origen. Si se modifica, se debe enviar también `default_logistic_type`.               |
| default\_logistic\_type                       | string  | Código del logistic\_type por defecto. Es obligatorio si se modifican los `logistic_types`.                                                            |

{% tabs %}
{% tab title="200: OK " %}
```javascript
// Ejemplo Request:
{
    "account_id": 7,
    "name": "Nueva direccion",
    "hours": {
        "open": "08:00",
        "close": "16:00"
    },
    "options": {
    	"pickup_days": [
                    "2",
                    "4",
                    "6"
                ]
    }
}
```
{% endtab %}
{% endtabs %}

## Webhooks

Puedes crear webhooks ingresando a tu cuenta, o bien por hacerlo por API. Ante determinados eventos dispararemos estos webhooks para que puedas recibir en tu integración una notificación al instante.

{% hint style="info" %}
Cada evento disparado consiste de un **request POST** a la URL que determines, con algunos datos sobre el recurso relacionado.

Es importante que tu servidor **responda con un HTTP 200**, de lo contrario nuestro sistema reintentará el envío de la notificación una vez por hora durante 12 horas.
{% endhint %}

#### Topics

Actualmente, los tópicos a los que te puedes suscribir son:

* `status`: Se dispara cuando hay un cambio de estado en un envío&#x20;
* `shipment`: Se dispara cuando hay cualquier modificación en un envío&#x20;
* `account`: Se dispara cuando hay una modificación en los datos o preferencias de una cuenta&#x20;
* `account_balance`: Se dispara cuando hay un cambio en el saldo de la cuenta
* `stock`: Se dispara cuando hay un cambio en el stock de un SKU

#### Ejemplos del contenido de los Webhooks

{% code title="status" %}
```json
{
   "topic":"status",
   "timestamp":"2024-03-08T18:56:34+00:00",
   "data":{
      "account_id":11600,
      "shipment_id":3850099,
      "external_id":"test-1709820269",
      "status":"Pendiente de preparacion",
      "status_code":"documentation_ready",
      "direction":"forward"
   }
}
```
{% endcode %}

{% code title="shipment" overflow="wrap" %}
```json
{
   "topic":"shipment",
   "timestamp":"2024-03-08T18:56:34+00:00",
   "data":{
      "account_id":11600,
      "shipment_id":3850099,
      "external_id":"test-1709820269"
   }
}
```
{% endcode %}

### Listar Webhooks

<mark style="color:blue;">`GET`</mark> `/v2/accounts/{account_id}/webhooks`

#### Path Parameters

| Name                                          | Type | Description     |
| --------------------------------------------- | ---- | --------------- |
| account\_id<mark style="color:red;">\*</mark> | int  | ID de la cuenta |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "data": [
        {
            "id": 4,
            "account_id": 7,
            "topic": "status",
            "url": "https://website.com/api/zippin/shipment/state/hook?client=13"
        }
    ],
    "links": {
        "first": "https://api.zippin.com.ar/v2/accounts/7/webhooks?page=1",
        "last": "https://api.zippin.com.ar/v2/accounts/7/webhooks?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://api.zippin.com.ar/v2/accounts/7/webhooks",
        "per_page": 20,
        "to": 1,
        "total": 1
    }
}
```
{% endtab %}
{% endtabs %}

### Suscribir a webhook

<mark style="color:green;">`POST`</mark> `/v2/accounts/{account_id}/webhooks`

#### Path Parameters

| Name                                          | Type | Description     |
| --------------------------------------------- | ---- | --------------- |
| account\_id<mark style="color:red;">\*</mark> | id   | ID de la cuenta |

#### Request Body

| Name                                    | Type   | Description                                          |
| --------------------------------------- | ------ | ---------------------------------------------------- |
| topic<mark style="color:red;">\*</mark> | string | Alguno de los códigos de topic indicados mas arriba. |
| url<mark style="color:red;">\*</mark>   | string | URL de destino del webhook                           |

{% tabs %}
{% tab title="201: Created " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

### Actualizar URL

<mark style="color:orange;">`PUT`</mark> `/v2/accounts/{account_id}/webhooks/{id}`

#### Path Parameters

| Name                                          | Type | Description     |
| --------------------------------------------- | ---- | --------------- |
| account\_id<mark style="color:red;">\*</mark> | int  | ID de la cuenta |
| id<mark style="color:red;">\*</mark>          | int  | ID del webhook  |

#### Request Body

| Name                                  | Type   | Description          |
| ------------------------------------- | ------ | -------------------- |
| url<mark style="color:red;">\*</mark> | string | Nueva URL de destino |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

### Desuscribir de un webhook

<mark style="color:red;">`DELETE`</mark> `/v2/accounts/{account_id}/webhooks/{id}`

#### Path Parameters

| Name                                          | Type | Description     |
| --------------------------------------------- | ---- | --------------- |
| account\_id<mark style="color:red;">\*</mark> | int  | ID de la cuenta |
| id<mark style="color:red;">\*</mark>          | int  | ID del webhook  |

{% tabs %}
{% tab title="204: No Content " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}
