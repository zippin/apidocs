---
description: >-
  Aprende cómo configurar distintos aspectos de tu cuenta como orígenes, y
  webhooks.
---

# Configuración

## Orígenes

A la hora de crear un envío es obligatorio indicar el origen del mismo. Los orígenes se dan de alta en la cuenta de cada vendedor.

{% swagger method="get" path="/addresses" baseUrl="/v2" summary="Listar orígenes" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="account_id" type="int" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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


{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/addresses/{id}" baseUrl="/v2" summary="Detalle de un orígen" %}
{% swagger-description %}
Obtiene el detalle de un origen.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del origen
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/addresses" baseUrl="/v2" summary="Crear orígen" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" required="true" %}
Nombre del Origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="document" type="string" required="true" %}
CUIT/RUT o identificacion del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street" type="string" required="true" %}
Calle del domicilio
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street_number" type="string" required="true" %}
Número de puerta del domicilio
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street_extras" type="string" required="true" %}
Datos adicionales de la dirección de origen (piso, depto, local)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="city" type="string" required="true" %}
Localidad (AR) | Comuna (CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="state" type="string" required="true" %}
Provincia (AR) | Region (CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="zipcode" type="string" required="true" %}
Código postal del origen (opcional en CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="phone" type="string" required="true" %}
Telefono del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email" type="string" required="true" %}
E-mail del orígen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours" type="object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours.open" type="string" required="true" %}
Horario de apertura. Ej. "09:00"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours.close" type="string" required="true" %}
Horario de cierre. Ej. "18:00"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options" type="object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.dropoff_only" type="boolean" required="true" %}
Si es 

`true`

, solo se ofrecerán transportes a los que se puedan imponer los envíos en una sucursal (no hay recolecciones para envios con éste origen).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.automatic_status_change" type="boolean" required="true" %}
Si es 

`true`

, el estado del envío cambiará a Listo para Despacho cuando se obtenga la documentación del envío.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.pickup_days" type="array" required="true" %}
Indica qué días se podrá hacer recolecciones al origen. Las opciones van del 0 (domingo) al 6 (sábado).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.use_preparation_time" type="boolean" required="true" %}
Si es 

`true`

, el estado del envío cambiará a Listo para Despacho a las X horas de creado (X se configura en la siguiente opción).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.preparation_time" type="int" required="true" %}
Cantidad de horas desde la creación para que un envío pase automáticamente a Listo para Despacho.
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger method="put" path="/addresses/{id}" baseUrl="/v2" summary="Modificar orígen" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta. Siempre es requerido en todos los requests.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" required="false" %}
Nombre del Origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="document" type="string" required="false" %}
CUIT/RUT o identificacion del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street" type="string" required="false" %}
Calle del domicilio
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street_number" type="string" required="false" %}
Número de puerta del domicilio
{% endswagger-parameter %}

{% swagger-parameter in="body" name="street_extras" type="string" required="false" %}
Datos adicionales de la dirección de origen (piso, depto, local)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="city" type="string" required="false" %}
Localidad (AR) | Comuna (CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="state" type="string" required="false" %}
Provincia (AR) | Region (CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="zipcode" type="string" required="false" %}
Código postal del origen (opcional en CL)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="phone" type="string" required="false" %}
Telefono del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email" type="string" required="false" %}
E-mail del orígen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours" type="object" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours.open" type="string" required="false" %}
Horario de apertura. Ej. "09:00"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hours.close" type="string" required="false" %}
Horario de cierre. Ej. "18:00"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options" type="object" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.dropoff_only" type="boolean" required="false" %}
Si es 

`true`

, solo se ofrecerán transportes a los que se puedan imponer los envíos en una sucursal (no hay recolecciones para envios con éste origen).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.automatic_status_change" type="boolean" required="false" %}
Si es 

`true`

, el estado del envío cambiará a Listo para Despacho cuando se obtenga la documentación del envío.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.pickup_days" type="array" required="false" %}
Indica qué días se podrá hacer recolecciones al origen. Las opciones van del 0 (domingo) al 6 (sábado).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.use_preparation_time" type="boolean" required="false" %}
Si es 

`true`

, el estado del envío cambiará a Listo para Despacho a las X horas de creado (X se configura en la siguiente opción).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="options.preparation_time" type="int" required="false" %}
Cantidad de horas desde la creación para que un envío pase automáticamente a Listo para Despacho.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del origen
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
{% endswagger-response %}
{% endswagger %}

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
* `account`: Se dispara cuando hay una modificacion en los datos o preferencias de una cuenta&#x20;
* `account_balance`: Se dispara cuando hay un cambio en el saldo de la cuenta

{% swagger method="get" path="/accounts/{account_id}/webhooks" baseUrl="/v2" summary="Listar Webhooks" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/accounts/{account_id}/webhooks" baseUrl="/v2" summary="Suscribir a webhook" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="account_id" type="id" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="topic" type="string" required="true" %}
Alguno de los códigos de topic indicados mas arriba.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="url" type="string" required="true" %}
URL de destino del webhook
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="put" path="/accounts/{account_id}/webhooks/{id}" baseUrl="/v2" summary="Actualizar URL" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del webhook
{% endswagger-parameter %}

{% swagger-parameter in="body" name="url" type="string" required="true" %}
Nueva URL de destino
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="delete" path="/accounts/{account_id}/webhooks/{id}" baseUrl="/v2" summary="Desuscribir de un webhook" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del webhook
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
