# Cotización y creación de ordenes

La cotización de envíos funciona igual que cualquier otra cotización. La única diferencia es que la autenticación se debe hacer con el access token, en vez de credenciales de la cuenta, y que luego, **en vez de crear un envío, se deberá crear una Orden de Marketplace**.

## Cotización

Revisa la [documentación de la cotización](../recursos-api/cotizar-y-crear-envios.md).

{% hint style="warning" %}
Recuerda, no se deberían crear envíos en forma directa, si no Ordenes de Marketplace.
{% endhint %}

## Creación de Ordenes de Marketplace

Una vez que el comprador realice su compra, y haya elegido una opción de entrega, deberías crear una orden en Zippin para que el vendedor pueda verla y luego pueda crearse el envío a partir de ella.

### Requerimientos

Para crear la orden, deberás contar con algunos datos del envío, que surgen de la opción seleccionada por el comprador, entre la opciones disponibles de una cotización.

* Transporte elegido (carrier\_id)
* Forma de despacho (logistic\_type)
* Tipo de servicio de entrega (service\_type)
* Origen del vendedor (origin\_id)
* Punto de entrega (point\_id) - si se eligió una entrega en pickup point.

### Crear la orden

## Crear orden de marketplace

<mark style="color:green;">`POST`</mark> `/v2/orders`

Este request te permitirá crear ordenes de marketplace indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.

#### Request Body

| Name                                                              | Type         | Description                                                                                                                                                                                                                          |
| ----------------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account\_id<mark style="color:red;">\*</mark>                     | int          | ID de la cuenta del seller                                                                                                                                                                                                           |
| shipping.origin\_id<mark style="color:red;">\*</mark>             | int          | ID del origen del envío                                                                                                                                                                                                              |
| shipping.logistic\_type                                           | string       | <p>Código de la forma de despacho del envío.</p><p>Ejemplo: <code>carrier_dropoff</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p>                |
| shipping.service\_type                                            | string       | <p>Código de la forma de entrega del envío.</p><p>Ejemplo: <code>standard_delivery</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p>               |
| shipping.carrier\_id                                              | int          | <p>ID del transporte que hará la entrega.</p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.</p>                                                                                                  |
| source<mark style="color:red;">\*</mark>                          | string (150) | Utilizado para identificar la integración.                                                                                                                                                                                           |
| total\_amount<mark style="color:red;">\*</mark>                   | float        | Monto total de la venta                                                                                                                                                                                                              |
| delivery.recipient.name<mark style="color:red;">\*</mark>         | string (100) | Nombre y apellido del destinatario                                                                                                                                                                                                   |
| delivery.address.street<mark style="color:red;">\*</mark>         | string (100) | <p>Calle del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                 |
| delivery.address.street\_number<mark style="color:red;">\*</mark> | string (10)  | <p>Altura/nro de puerta del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                  |
| delivery.address.street\_extras                                   | string (190) | <p>Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                |
| delivery.point\_id                                                | int          | <p>ID de la ubicación de entrega.</p><p>Requerido solo cuando la entrega es en un punto de entrega.</p>                                                                                                                              |
| delivery.recipient.document<mark style="color:red;">\*</mark>     | string (50)  | Número de documento (DNI, RUT, o lo que corresponda) del destinatario                                                                                                                                                                |
| delivery.recipient.email<mark style="color:red;">\*</mark>        | email (150)  | Dirección de correo electrónico del destinatario                                                                                                                                                                                     |
| delivery.recipient.phone<mark style="color:red;">\*</mark>        | string (50)  | Número de contacto del destinatario                                                                                                                                                                                                  |
| delivery.address.state<mark style="color:red;">\*</mark>          | string       | <p>Nombre del Estado del domicilio del destinatario.</p><p>AR: provincia</p><p>CL: región</p><p>CO: departamento</p><p>MX: estado</p><p>Requerido solo en entregas a domicilio.</p>                                                  |
| delivery.address.city<mark style="color:red;">\*</mark>           | string       | <p>Nombre de la ciudad del domicilio del destinatario.</p><p>AR: localidad</p><p>CL: comuna</p><p>CO: ciudad</p><p>MX: ciudad</p><p>Requerido solo en entregas a domicilio.</p>                                                      |
| delivery.address.zipcode<mark style="color:red;">\*</mark>        | string       | <p>Codigo postal de la dirección de destino.</p><p>Requerido solo en entregas a domicilio.</p><p>Dato no requerido en Chile y Colombia.</p>                                                                                          |
| delivery.address.country                                          | string       | <p>Código ISO 3166-1 alfa-2 del país</p><p>Argentina: AR</p><p>Chile: CL</p><p>Colombia: CO</p><p>México: MX</p>                                                                                                                     |
| delivery<mark style="color:red;">\*</mark>                        | object       | Objeto con datos de la entrega                                                                                                                                                                                                       |
| items<mark style="color:red;">\*</mark>                           | array\[Item] | <p>Array de objetos Item. </p><p>Cada Item es un articulo a enviar. </p><p>El máximo de artículos por envío es 1000.</p><p>Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.</p> |
| items.\*.sku<mark style="color:red;">\*</mark>                    | string (190) | Se intentará vincular a un producto cargado en el catálogo de Zippin.                                                                                                                                                                |
| items.\*.weight<mark style="color:red;">\*</mark>                 | int          | Peso en gramos.                                                                                                                                                                                                                      |
| items.\*.height<mark style="color:red;">\*</mark>                 | int          | Alto del artículo, en centímetros.                                                                                                                                                                                                   |
| items.\*.width<mark style="color:red;">\*</mark>                  | int          | Ancho del artículo, en centímetros.                                                                                                                                                                                                  |
| items.\*.length<mark style="color:red;">\*</mark>                 | int          | Largo del artículo, en centímetros.                                                                                                                                                                                                  |
| items.\*.name<mark style="color:red;">\*</mark>                   | string (190) | Descripción o nombre del artículo                                                                                                                                                                                                    |
| external\_id<mark style="color:red;">\*</mark>                    | string (30)  | ID del envío en tus sistemas. Es para tu propia referencia.                                                                                                                                                                          |
| paid\_amount<mark style="color:red;">\*</mark>                    | float        | Monto total de la venta pagado por el comprador                                                                                                                                                                                      |
| shipping\_paid\_amount<mark style="color:red;">\*</mark>          | float        | Monto correspondiente al envío pagado por el comprador                                                                                                                                                                               |
| currency                                                          | string       | <p>Código de divisa<br>Ej. ARS, CLP, MXN.</p>                                                                                                                                                                                        |
| created\_at<mark style="color:red;">\*</mark>                     | timestamp    | Fecha y hora de generación de la venta (ISO8601)                                                                                                                                                                                     |
| shipping<mark style="color:red;">\*</mark>                        | object       |                                                                                                                                                                                                                                      |
| items.\*.qty                                                      | int          | Cantidad vendida del item                                                                                                                                                                                                            |
| items.\*.unit\_price                                              | float        | Precio unitario del producto                                                                                                                                                                                                         |
| items.\*.currency                                                 | string       | <p>Código de divisa<br>Ej. ARS, CLP, MXN.</p>                                                                                                                                                                                        |
| channel<mark style="color:red;">\*</mark>                         | string       | <p>Identificación del canal. En caso de marketplace, utiliza el prefijo <code>marketplace:</code></p><p>Por ejemplo: <code>marketplace:amazon</code></p>                                                                             |

{% tabs %}
{% tab title="201: Created La orden se crea correctamente." %}
```javascript
{
	"id": 22736208,
	"account_id": 2,
	"channel": {
		"type": "marketplace",
		"marketplace": "marketplace_code",
		"created_at": "2024-03-01T16:42:02+00:00",
		"paid_at": "2024-03-01T16:42:02+00:00",
		"data": null
	},
	"completed_at": null,
	"shipment_requested_at": null,
	"saved_origin_id": 343693,
	"logistic_type": null,
	"carrier_id": null,
	"service_type_id": null,
	"destination": {
		"id": 25235248,
		"saved_address_id": null,
		"name": "Marco Niccolini",
		"street": "Marconi",
		"street_number": "3055",
		"street_extras": "Piso 3",
		"zipcode": "1643",
		"phone": "1168198894",
		"email": "juan@hotmail.com",
		"document": "11111111",
		"city": {
			"id": 260,
			"name": "Beccar"
		},
		"state": {
			"id": 2,
			"name": "Buenos Aires"
		},
		"country": {
			"id": 1,
			"name": "Argentina"
		},
		"location_latitude": "-34.4791165",
		"location_longitude": "-58.5701005",
		"location_type": "ROOFTOP",
		"location_metadata": null,
		"is_accurate": true,
		"created_at": "2024-03-01T16:42:03.000000Z",
		"updated_at": "2024-03-01T16:42:03.000000Z",
		"confirmed_at": null
	},
	"items": [
		{
			"id": 25226018,
			"qty": 2,
			"unit_price": 0,
			"currency": "ARS",
			"product_id": null,
			"data": null
		}
	],
	"total_paid_amount": 12345.67,
	"total_shipping_paid_amount": 300,
	"currency": null,
	"tags": [],
	"created_at": "2024-03-01T16:42:03+00:00",
	"updated_at": "2024-03-01T16:42:04+00:00"
}
```


{% endtab %}

{% tab title="400: Bad Request Un error en los datos impide crear el envío" %}
```javascript
{
    // El detalle indicará detalladamente el motivo del error.
}
```


{% endtab %}
{% endtabs %}

