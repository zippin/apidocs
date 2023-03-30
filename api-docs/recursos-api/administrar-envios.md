# Administrar envíos

## Obtener envíos

### Listado/búsqueda de envíos

{% swagger baseUrl="/v2" method="get" path="/shipments" summary="Buscar envíos" %}
{% swagger-description %}
Obtiene un listado de envíos. Se pueden aplicar filtros sobre distintos campos.
{% endswagger-description %}

{% swagger-parameter in="query" name="account_id" required="false" type="int" %}
Filtrar envíos de una cuenta por ID.

Ejemplo: <mark style="color:red;">`account_id=2`</mark>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="origin_id" type="int" %}
Filtrar envíos de un origen por ID.

Ejemplo: <mark style="color:red;">`origin_id=32`</mark>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="external_id" required="false" type="string" %}
Filtrar envíos por su ID Externo

Ejemplo: <mark style="color:red;">`external_id=DJDSJCMR`</mark>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="order_id" type="string" %}
Filtrar envíos por el ID visible de una venta relacionada de un canal integrada.

\


Ejemplo: 

<mark style="color:red;">

`order_id=200000334445566`

</mark>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="service_type" required="false" type="string" %}
Filtrar envíos por su tipo de servicio.

Ejemplo: <mark style="color:red;">`service_type=standard_delivery`</mark>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="status" required="false" type="string" %}
Filtrar envíos por su estado actual.

Ejemplo: <mark style="color:red;">`status=delivered`</mark>

[Ver estados posibles](../../referencia/estados-de-envio.md)
{% endswagger-parameter %}

{% swagger-response status="200" description="Listado de resultados" %}
```javascript
{
    "data": [
        {
            "id": 103169,
            "external_id": "teste210119",
            "delivery_id": "0099-00222134",
       "created_at": "2019-01-18T04:05:22+0000",
            "account_id": 34,
            "parent_shipment_id": null,
            "service_type": "standard_delivery",
            "logistic_type": "crossdock",
            "status": "new",
            "status_name": "Nuevo",
            "tracking": "http://zippin.local/tracking/103169/34",
            "tracking_external": "http://zippin.local/tracking/teste210119/34/external_id",
            "destination": {
                "name": "eee",
                "city": "Capital Federal",
                "state": "Capital Federal",
                "zipcode": "1425"
            },
            "origin": {
                "id": 25,
                "name": "Origen Demo",
                "city": "Capital Federal",
                "state": "Capital Federal",
                "zipcode": "1005"
            },
            "declared_value": 1425,
            "price": 111.57,
            "price_incl_tax": 135,
            "total_weight": 10000,
            "total_volume": 8000,
            "total_packages": 1
        },
        (...)
    ],
    "links": {
        "first": "http://api.zippin.com.ar/v2/shipments?page=1",
        "last": "http://api.zippin.com.ar/v2/shipments?page=4",
        "prev": null,
        "next": "http://api.zippin.com.ar/v2/shipments?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 4,
        "path": "http://api.zippin.com.ar/v2/shipments",
        "per_page": 15,
        "to": 15,
        "total": 54
    }
}
```
{% endswagger-response %}
{% endswagger %}

### Detalle de un envío

{% swagger method="get" path="/shipments/{shipment_id}" baseUrl="/v2" summary="Detalle de un envío" %}
{% swagger-description %}
Obtiene el detalle de un envío determinado
{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Detalle de un envío" %}
```javascript
{
    "id": 101981,
    "external_id": "414311",
    "delivery_id": "0999-00101915",
    "created_at": "2019-01-18T04:05:22+0000",
    "account_id": 7,
    "parent_shipment_id": 101793,
    "service_type": "standard_delivery",
    "logistic_type": "crossdock",
    "status": "delivered",
    "status_name": "Entregado",
    "tracking": "http://zippin.local/tracking/101981/7",
    "tracking_external": "http://zippin.local/tracking/414311/7/external_id",
    "destination": {
        "name": "Pablo",
        "document": "34353917",
        "street": "Ruta 7 km",
        "street_number": "259",
        "street_extras": null,
        "city": "Junin",
        "state": "Buenos Aires",
        "zipcode": "6000",
        "phone": "0236154503837",
        "email": "pgiaeytknzq@mail.mercadolibre.com"
    },
    "origin": {
        "id": 32,
        "name": "Zippin - Tortuguitas",
        "document": "30-70835592-1",
        "street": "Brasil",
        "street_number": "2990",
        "street_extras": "LESET",
        "city": "Tortuguitas",
        "state": "Buenos Aires",
        "zipcode": "1667",
        "phone": "000000",
        "email": "operaciones@zippin.com.ar"
    },
    "declared_value": 27140,
    "price": 1950.41,
    "price_incl_tax": 2360,
    "total_weight": 56000,
    "total_volume": 97020,
    "packages": [
        {
            "internal_id": 67188,
            "sku_id": null,
            "weight": 28000,
            "height": 33,
            "width": 30,
            "length": 49,
            "volume": 48510,
            "description_1": "1910811535",
            "description_2": "Aire Acondicionado Inverter 5000fc",
            "description_3": "",
            "classification": {
                "id": 1,
                "name": "General"
            }
        },
        {
            "internal_id": 67189,
            "sku_id": null,
            "weight": 28000,
            "height": 33,
            "width": 30,
            "length": 49,
            "volume": 48510,
            "description_1": "1910811535",
            "description_2": "Aire Acondicionado Inverter 5000fc",
            "description_3": "",
            "classification": {
                "id": 1,
                "name": "General"
            }
        }
    ]
}
```
{% endswagger-response %}
{% endswagger %}

### Ubicaciones disponibles para despacho

En los envíos con despacho en un punto (cuando el `logistic_type` es `xd_dropoff`, `carrier_dropoff`, o `point_dropoff`), o con recolección unificada (`crossdock`), se puede obtener un listado de ubicaciones cercanas al origen donde se puede ir a despachar el envío.

{% hint style="warning" %}
La ubicaciones ofrecidas sólo muestran aquellas que pueden recibir el envío en función de su peso y dimensiones.
{% endhint %}

{% swagger method="get" path="/shipments/{shipment_id}/dropoff_locations" baseUrl="/v2" summary="Ubicaciones para despacho de un envío" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío a despachar
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Listado de ubicaciones" %}
```javascript
{
    "data": [
        {
            "carrier": {
                "id": 208,
                "name": "OCA"
            },
            "name": "Sucursal OCA - QUILMES",
            "address": {
                "street": "Lavalle",
                "street_number": "663",
                "street_extras": "  ",
                "city": "Quilmes",
                "state": "Buenos Aires",
                "zipcode": "1878",
                "lat": "-34.7231545",
                "lng": "-58.2566685",
                "phone": "4224-7481",
                "open_hours": "LUN A VIE 8:30 A 18 HS."
            }
        },
        {
            "carrier": {
                "id": 208,
                "name": "OCA"
            },
            "name": "Agente Oficial - RACZ ROBERTO RUBEN",
            "address": {
                "street": "Aristovulo Del Valle",
                "street_number": "4295",
                "street_extras": "  ",
                "city": "Claypole",
                "state": "Buenos Aires",
                "zipcode": "1849",
                "lat": "-34.80522",
                "lng": "-58.338937",
                "phone": "4219-3163",
                "open_hours": "A Confirmar"
            }
        }
    ]
}
```
{% endswagger-response %}
{% endswagger %}

## Edición de envíos

Podrás actualizar algunos aspectos del envío luego de creado.

{% hint style="warning" %}
Actualmente solo es posible editar el atributo de external\_id, siempre y cuando el envío aun no haya sido despachado.&#x20;

Al hacer la modificación, **se reseteará el estado** del envío a Pendiente de Preparación y habrá que **volver a descargar la documentación** de despacho, si ya se hubiera hecho.
{% endhint %}

{% swagger method="put" path="/shipments/{shipment_id}" baseUrl="/v2" summary="Editar un envío" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío
{% endswagger-parameter %}

{% swagger-parameter in="path" name="external_id" type="string" %}
ID externo del envío. Solo admite letras, números y guiones.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Envío modificado" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": {
                "external_id": "new_external-id"
                }
    }
```
{% endswagger-response %}
{% endswagger %}

## Documentación de despacho

La documentación de despacho está compuesta de una etiqueta por cada paquete del envío, y dependiendo del transporte, una guía de despacho.

{% hint style="danger" %}
Para despachar un envío es obligatorio pegar **cada etiqueta a un paquete distinto**, e **imprimir la guía de despacho**, si estuviera disponible, la cual se debe adherir a la factura de venta o remito legal.
{% endhint %}

{% swagger method="get" path="/shipments/{shipment_id}/documentation?what={what}&format={format}" baseUrl="/v2" summary="Obtener documentación" %}
{% swagger-description %}
Obtiene los archivos de etiquetas o la guia de despacho.

En cualquier caso el contenido del archivo se devuelve encodeado en base64.
{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío
{% endswagger-parameter %}

{% swagger-parameter in="query" name="what" type="string" required="true" %}
Tipo de archivo a obtener:

<mark style="color:red;">`document`</mark> para la guía (solo disponible en PDF)&#x20;

<mark style="color:red;">`label`</mark> para las etiquetas de cada paquete (en PDF o ZPL)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="format" type="string" %}
Formato de las etiquetas:

<mark style="color:red;">`pdf`</mark> (por defecto)

<mark style="color:red;">`zpl`</mark>
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Contenido del archivo con encoding base64" %}
```javascript
{
    "format": "zpl",
    "body": "XlhBflRBMDAwfkpTTl5MVDBeTU5XXk1UVF5QT05eUE1OXkxIMCwwXkpNQV5QUjQsNH5TRDE1XkpVU15MUk5eQ0kwXlhaXlhBXk1NVF5QVzgzMV5MTDEyMzleTFMwXkZPRk81ODAsMjBeR0ZBLDIxNjAsMjE2MCwyNywsOjpMMDc4LEwwRkMsSzAxRkUsSzAxRkYsSzAzRkY4LEswM0kZIXF5GRDMgLyAzXkZTXkZPMjAsOTQwXkdCNzcxLDAsOF5GU15GVDE4MCw5OD15CWTIsMywxNDBeRlQxODAsMTE2MF5CQ04sLFksTl5GRD46MDk5OS0wMDEwMTQ3NC0wMzAzXkZTXlBRMSwwLDEsWV5YWg=="
}
```
{% endswagger-response %}
{% endswagger %}

## Tracking

En este endpoint podrás obtener un detalle de los movimientos de un envío.

{% swagger method="get" path="/shipments/{shipment_id}/tracking" baseUrl="/v2" summary="Obtener historial de estados" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort" type="string" %}
Ordenamiento de los movimientos

<mark style="color:red;">`oldest`</mark> ordena desde los movimientos mas antiguos a los mas recientes (por defecto)

<mark style="color:red;">`newest`</mark> ordena desde los movimientos mas recientes a los mas antiguos&#x20;
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
[
    {
        "created_at": "2019-04-12T00:10:29+00:00",
        "status": {
            "code": "delivered",
            "name": "Entregado",
            "visible_name": "Entregado",
            "substatus": null
        }
    },
    {
        "created_at": "2019-04-11T19:14:48+00:00",
        "status": {
            "code": "in_transit",
            "name": "En Camino",
            "visible_name": "Envio en distribucion",
            "substatus": null
        }
    },
    {
        "created_at": "2019-04-11T19:14:48+00:00",
        "status": {
            "code": "shipped",
            "name": "Despachado de Origen",
            "visible_name": "El vendedor ha despachado el pedido",
            "substatus": null
        }
    },
    {
        "created_at": "2019-04-11T14:31:02+00:00",
        "status": {
            "code": "documentation_ready",
            "name": "Remito Generado",
            "visible_name": "El vendedor esta preparando el pedido",
            "substatus": null
        }
    },
    {
        "created_at": "2019-04-11T14:31:00+00:00",
        "status": {
            "code": "new",
            "name": "Nuevo",
            "visible_name": "Nuevo",
            "substatus": null
        }
    }
]
```
{% endswagger-response %}
{% endswagger %}

## Anular envíos

Este endpoint te permitirá cancelar envíos.

{% hint style="warning" %}
**Solo se puede cancelar envíos que no hayan sido despachados**. \
Cuando se solicite cancelar un envío no despachado su estado pasará a **Anulación Confirmada**.

**Si el envío ya fue despachado, se generará una Solicitud de Rescate**. \
Con esa solicitud se notificará al transporte para que no haga la entrega, aunque no siempre se puede garantizar que se cumpla la solicitud.
{% endhint %}

{% swagger method="post" path="/shipments/{shipment_id}/cancel" baseUrl="/v2" summary="Cancelar o solicitar rescate de un envío" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="shipment_id" type="int" required="true" %}
ID del envío
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Envío Cancelado" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": "canceled"
    }
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Solicitud de rescate generada" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": "rescue_requested"
    }
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Cancelación no es posible" %}
```javascript
{
        "shipment_id": 123456,
        "success": false,
        "result": null
    }
```
{% endswagger-response %}
{% endswagger %}
