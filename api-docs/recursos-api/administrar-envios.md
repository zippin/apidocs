# Administrar envíos

## Obtener envíos

{% hint style="warning" %}
Los endpoints de búsqueda y obtención de envíos y tracking utilizan **rate limiting** :yellow\_circle:<mark style="color:yellow;">**Bajo**</mark>\
[Ver más sobre límites de requests](../limites-de-requests.md)
{% endhint %}

### Listado/búsqueda de envíos

## Buscar envíos

<mark style="color:blue;">`GET`</mark> `/v2/shipments`

Obtiene un listado de envíos. Se pueden aplicar filtros sobre distintos campos.

#### Query Parameters

| Name          | Type   | Description                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account\_id   | int    | <p>Filtrar envíos de una cuenta por ID.</p><p>Ejemplo: <mark style="color:red;"><code>account_id=2</code></mark></p>                                                                                   |
| external\_id  | string | <p>Filtrar envíos por su ID Externo</p><p>Ejemplo: <mark style="color:red;"><code>external_id=DJDSJCMR</code></mark></p>                                                                               |
| service\_type | string | <p>Filtrar envíos por su tipo de servicio.</p><p>Ejemplo: <mark style="color:red;"><code>service_type=standard_delivery</code></mark></p>                                                              |
| status        | string | <p>Filtrar envíos por su estado actual.</p><p>Ejemplo: <mark style="color:red;"><code>status=delivered</code></mark></p><p><a href="../../referencia/estados-de-envio.md">Ver estados posibles</a></p> |
| origin\_id    | int    | <p>Filtrar envíos de un origen por ID.</p><p>Ejemplo: <mark style="color:red;"><code>origin_id=32</code></mark></p>                                                                                    |
| order\_id     | string | <p>Filtrar envíos por el ID visible de una venta relacionada de un canal integrada.<br>Ejemplo: <mark style="color:red;"><code>order_id=200000334445566</code></mark></p>                              |

{% tabs %}
{% tab title="200 Listado de resultados" %}
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
{% endtab %}
{% endtabs %}

### Detalle de un envío

## Detalle de un envío

<mark style="color:blue;">`GET`</mark> `/v2/shipments/{shipment_id}`

Obtiene el detalle de un envío determinado

#### Path Parameters

| Name                                           | Type | Description   |
| ---------------------------------------------- | ---- | ------------- |
| shipment\_id<mark style="color:red;">\*</mark> | int  | ID del envío. |

{% tabs %}
{% tab title="200: OK Detalle de un envío" %}
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
{% endtab %}
{% endtabs %}

### Ubicaciones disponibles para despacho

En los envíos con despacho en un punto (cuando el `logistic_type` es `xd_dropoff`, `carrier_dropoff`, o `point_dropoff`), o con recolección unificada (`crossdock`), se puede obtener un listado de ubicaciones cercanas al origen donde se puede ir a despachar el envío.

{% hint style="warning" %}
La ubicaciones ofrecidas sólo muestran aquellas que pueden recibir el envío en función de su peso y dimensiones.
{% endhint %}

## Ubicaciones para despacho de un envío

<mark style="color:blue;">`GET`</mark> `/v2/shipments/{shipment_id}/dropoff_locations`

#### Path Parameters

| Name                                           | Type | Description              |
| ---------------------------------------------- | ---- | ------------------------ |
| shipment\_id<mark style="color:red;">\*</mark> | int  | ID del envío a despachar |

{% tabs %}
{% tab title="200: OK Listado de ubicaciones" %}
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
{% endtab %}
{% endtabs %}

## Edición de envíos

Podrás actualizar algunos aspectos del envío luego de creado.

{% hint style="warning" %}
Actualmente solo es posible editar el atributo de external\_id, siempre y cuando el envío aun no haya sido despachado.&#x20;

Al hacer la modificación, **se reseteará el estado** del envío a Pendiente de Preparación y habrá que **volver a descargar la documentación** de despacho, si ya se hubiera hecho.
{% endhint %}

## Editar un envío

<mark style="color:orange;">`PUT`</mark> `/v2/shipments/{shipment_id}`

#### Path Parameters

| Name                                           | Type   | Description                                                  |
| ---------------------------------------------- | ------ | ------------------------------------------------------------ |
| shipment\_id<mark style="color:red;">\*</mark> | int    | ID del envío                                                 |
| external\_id                                   | string | ID externo del envío. Solo admite letras, números y guiones. |

{% tabs %}
{% tab title="200: OK Envío modificado" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": {
                "external_id": "new_external-id"
                }
    }
```
{% endtab %}
{% endtabs %}

## Documentación de despacho

La documentación de despacho está compuesta de una etiqueta por cada paquete del envío, y dependiendo del transporte, una guía de despacho.

{% hint style="danger" %}
Para despachar un envío es obligatorio pegar **cada etiqueta a un paquete distinto**, e **imprimir la guía de despacho**, si estuviera disponible, la cual se debe adherir a la factura de venta o remito legal.
{% endhint %}

## Obtener documentación

<mark style="color:blue;">`GET`</mark> `/v2/shipments/{shipment_id}/documentation?what={what}&format={format}`

Obtiene los archivos de etiquetas o la guia de despacho.

En cualquier caso el contenido del archivo se devuelve encodeado en base64.

#### Path Parameters

| Name                                           | Type | Description  |
| ---------------------------------------------- | ---- | ------------ |
| shipment\_id<mark style="color:red;">\*</mark> | int  | ID del envío |

#### Query Parameters

| Name                                   | Type   | Description                                                                                                                                                                                                                                     |
| -------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| what<mark style="color:red;">\*</mark> | string | <p>Tipo de archivo a obtener:</p><p><mark style="color:red;"><code>document</code></mark> para la guía (solo disponible en PDF) </p><p><mark style="color:red;"><code>label</code></mark> para las etiquetas de cada paquete (en PDF o ZPL)</p> |
| format                                 | string | <p>Formato de las etiquetas:</p><p><mark style="color:red;"><code>pdf</code></mark> (por defecto)</p><p><mark style="color:red;"><code>zpl</code></mark></p>                                                                                    |

{% tabs %}
{% tab title="200: OK Contenido del archivo con encoding base64" %}
```javascript
{
    "format": "zpl",
    "body": "XlhBflRBMDAwfkpTTl5MVDBeTU5XXk1UVF5QT05eUE1OXkxIMCwwXkpNQV5QUjQsNH5TRDE1XkpVU15MUk5eQ0kwXlhaXlhBXk1NVF5QVzgzMV5MTDEyMzleTFMwXkZPRk81ODAsMjBeR0ZBLDIxNjAsMjE2MCwyNywsOjpMMDc4LEwwRkMsSzAxRkUsSzAxRkYsSzAzRkY4LEswM0kZIXF5GRDMgLyAzXkZTXkZPMjAsOTQwXkdCNzcxLDAsOF5GU15GVDE4MCw5OD15CWTIsMywxNDBeRlQxODAsMTE2MF5CQ04sLFksTl5GRD46MDk5OS0wMDEwMTQ3NC0wMzAzXkZTXlBRMSwwLDEsWV5YWg=="
}
```
{% endtab %}
{% endtabs %}

## Tracking

En este endpoint podrás obtener un detalle de los movimientos de un envío.

## Obtener historial de estados

<mark style="color:blue;">`GET`</mark> `/v2/shipments/{shipment_id}/tracking`

#### Path Parameters

| Name                                           | Type | Description  |
| ---------------------------------------------- | ---- | ------------ |
| shipment\_id<mark style="color:red;">\*</mark> | int  | ID del envío |

#### Query Parameters

| Name | Type   | Description                                                                                                                                                                                                                                                                                           |
| ---- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sort | string | <p>Ordenamiento de los movimientos</p><p><mark style="color:red;"><code>oldest</code></mark> ordena desde los movimientos mas antiguos a los mas recientes (por defecto)</p><p><mark style="color:red;"><code>newest</code></mark> ordena desde los movimientos mas recientes a los mas antiguos </p> |

{% tabs %}
{% tab title="200: OK " %}
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
{% endtab %}
{% endtabs %}

## Anular envíos

Este endpoint te permitirá cancelar envíos.

{% hint style="warning" %}
**Solo se puede cancelar envíos que no hayan sido despachados**. \
Cuando se solicite cancelar un envío no despachado su estado pasará a **Anulación Confirmada**.

**Si el envío ya fue despachado, se generará una Solicitud de Rescate**. \
Con esa solicitud se notificará al transporte para que no haga la entrega, aunque no siempre se puede garantizar que se cumpla la solicitud.
{% endhint %}

## Cancelar o solicitar rescate de un envío

<mark style="color:green;">`POST`</mark> `/v2/shipments/{shipment_id}/cancel`

#### Path Parameters

| Name                                           | Type | Description  |
| ---------------------------------------------- | ---- | ------------ |
| shipment\_id<mark style="color:red;">\*</mark> | int  | ID del envío |

{% tabs %}
{% tab title="200: OK Envío Cancelado" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": "canceled"
    }
```
{% endtab %}

{% tab title="200: OK Solicitud de rescate generada" %}
```javascript
{
        "shipment_id": 123456,
        "success": true,
        "result": "rescue_requested"
    }
```
{% endtab %}

{% tab title="401: Unauthorized Cancelación no es posible" %}
```javascript
{
        "shipment_id": 123456,
        "success": false,
        "result": null
    }
```
{% endtab %}
{% endtabs %}



## Actualizar estados de envíos de Flota Propia

<mark style="color:green;">`POST`</mark> `/v2/shipments/{shipment_id}/tracking`

Si deseas actualizar los estados de tus envíos de flota propia, podrás usar este endpoint. Ten en cuenta que solo podrás definir algunos estados.

#### Estados permitidos

<table><thead><tr><th width="167">Estado</th><th width="184">Código</th><th>Subestados</th></tr></thead><tbody><tr><td>Listo para Despacho</td><td><code>ready_to_ship</code> </td><td></td></tr><tr><td>Anulacion Confirmada </td><td><code>cancelled</code> </td><td></td></tr><tr><td>Despachado de Origen</td><td><code>shipped</code> </td><td></td></tr><tr><td>En Transito a Transporte</td><td><code>in_transit_to_carrier</code> </td><td></td></tr><tr><td>Recibido Transporte</td><td><code>received_by_carrier</code> </td><td></td></tr><tr><td>En Camino</td><td><p><code>in_transit</code></p><p></p></td><td></td></tr><tr><td>Entregado</td><td><code>delivered</code> </td><td></td></tr><tr><td>No Entregado</td><td><code>not_delivered</code> </td><td><p>Se debe indicar alguno de estos subestados:</p><table><thead><tr><th>Subestado</th><th>Codigo</th></tr></thead><tbody><tr><td>Cliente no esta en destino</td><td><code>client_not_in_address</code></td></tr><tr><td>Cliente no quiere Recibir</td><td><code>client_rejected_shipment</code></td></tr><tr><td>Daño Parcial</td><td><code>partial_damage</code></td></tr><tr><td>Daño Total</td><td><code>total_damage</code></td></tr><tr><td>Direccion Incorrecta</td><td><code>bad_address</code></td></tr><tr><td>Error del Transporte</td><td><code>carrier_error</code></td></tr><tr><td>Extravio Parcial</td><td><code>partial_loss</code></td></tr><tr><td>Extravio Total</td><td><code>total_loss</code></td></tr><tr><td>Falta documentacion</td><td><code>missing_documentation</code></td></tr><tr><td>Imposibilidad de Acceso</td><td><code>inaccessible</code></td></tr><tr><td>Intentos de Entrega Agotados</td><td><code>exhausted_delivery_attempts</code></td></tr><tr><td>Zona Peligrosa</td><td><code>danger_zone</code></td></tr><tr><td>Otros</td><td><code>others</code></td></tr></tbody></table></td></tr></tbody></table>

#### Path Parameters

| Name                                            | Type | Description  |
| ----------------------------------------------- | ---- | ------------ |
| `shipment_id`<mark style="color:red;">\*</mark> | int  | ID del envío |

**Body**

| Name                                       | Type   | Description                                 |
| ------------------------------------------ | ------ | ------------------------------------------- |
| `status`<mark style="color:red;">\*</mark> | string | Código del estado                           |
| `substatus`                                | string | Código del subestado (solo si es necesario) |
| `comment`                                  | string | Comentario opcional. Máximo 150 caracteres. |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  // Detalle del envío recién actualizado
}
```
{% endtab %}

{% tab title="400" %}
```json
{
	"status": "error",
	"message": "Validation Error",
	"errors": [
		"The substatus field is required."
	]
}
```
{% endtab %}
{% endtabs %}
