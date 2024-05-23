# Cotizar y Crear Envíos

## Cotizar

{% hint style="warning" %}
Los endpoints de cotización utilizan **rate limiting** :orange\_circle:<mark style="color:orange;">**Medio**</mark>\
[Ver más sobre límites de requests](../limites-de-requests.md)
{% endhint %}

La cotización es el proceso mediante el cual se obtienen opciones para hacer un envío.

En Zippin existen múltiples maneras de hacer un envío, combinando diferentes **formas de despacho** (`logistic_type`) y **formas de entrega** (`service_type`).

En los resultados de cotización verás todas las opciones disponibles en tu cuenta para hacer el envío, con los distintos transportes disponibles.

{% hint style="info" %}
Recomendamos completar el atributo `source` con algo que identifique a tu integración, para que luego los clientes puedan definir reglas personalizadas de cotización utilizando el [Motor de Reglas](https://ayuda.zippin.app/automatizaciones), utilizando el atributo source como criterio de filtrado.
{% endhint %}

#### Cotizar Por Items

Permite obtener opciones de despacho indicando un conjunto de productos a despachar. La API empaquetará los items en paquetes de acuerdo a la lógica definida en la cuenta, y teniendo en cuenta los contenedores configurados.

## Cotización de un envío en base a items (recomendado)

<mark style="color:green;">`POST`</mark> `/v2/shipments/quote`

Este request te permitirá obtener cotizaciones de envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.

#### Request Body

| Name                                                  | Type         | Description                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>         | int          | ID de la cuenta                                                                                                                                                                                                                                                                                                                           |
| origin\_id<mark style="color:red;">\*</mark>          | int          | ID del origen                                                                                                                                                                                                                                                                                                                             |
| declared\_value<mark style="color:red;">\*</mark>     | float        | Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.                                                                                                                                                                                             |
| destination<mark style="color:red;">\*</mark>         | object       | Objeto destination                                                                                                                                                                                                                                                                                                                        |
| destination.city<mark style="color:red;">\*</mark>    | string       | <p>Nombre de la ciudad del domicilio del destinatario.</p><p>AR: localidad (ej. Avellaneda)</p><p>CL: comuna (ej. Las Condes)</p><p>CO: ciudad (ej. Usaquén)</p><p>MX: colonia (ej. Polanco)</p>                                                                                                                                          |
| destination.state<mark style="color:red;">\*</mark>   | string       | <p>Nombre del Estado del domicilio del destinatario.</p><p>AR: provincia (ej. Buenos Aires)</p><p>CL: región (ej. RM)</p><p>CO: departamento  (ej. Bogotá DC)</p><p>MX: estado (ej. Distrito Federal)</p>                                                                                                                                 |
| destination.zipcode<mark style="color:red;">\*</mark> | string       | <p>Código postal. </p><p>No requerido en Chile ni Colombia.</p>                                                                                                                                                                                                                                                                           |
| items<mark style="color:red;">\*</mark>               | array\[item] | <p>Array de objetos item. Minimo 1 item, maximo 1000 items.</p><p>Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.</p>                                                                                                                                                               |
| items.\*.sku<mark style="color:red;">\*</mark>        | string (190) | Se intentará vincular a un producto cargado en el catalogo de Zippin, en base al código de referencia de los productos.                                                                                                                                                                                                                   |
| items.\*.description                                  | string (190) | Titulo o descripcion del producto                                                                                                                                                                                                                                                                                                         |
| items.\*.weight<mark style="color:red;">\*</mark>     | int          | Peso, en gramos, del item                                                                                                                                                                                                                                                                                                                 |
| items.\*.length<mark style="color:red;">\*</mark>     | int          | Largo, en centimetros                                                                                                                                                                                                                                                                                                                     |
| items.\*.height<mark style="color:red;">\*</mark>     | int          | Alto, en centimetros                                                                                                                                                                                                                                                                                                                      |
| items.\*.width<mark style="color:red;">\*</mark>      | int          | Ancho, en centimetros                                                                                                                                                                                                                                                                                                                     |
| items.\*.classification\_id                           | int          | <p>Identificador de clasificación de producto. Si lo omites o indicas 1 (General) intentaremos clasificarlo automaticamente segun la descripcion.</p><p><a href="../../referencia/clasificaciones-de-producto.md">Ver Clasificaciones de producto</a></p>                                                                                 |
| destination.country                                   | string       | <p>Código ISO 3166-1 alfa-2 del país</p><p>Argentina: AR</p><p>Chile: CL</p><p>Colombia: CO</p><p>México: MX</p>                                                                                                                                                                                                                          |
| type\_packaging                                       | String       | <p>Indica la forma de empaquetar los productos. Si se omite se usa el que tenga definido la cuenta por default.<br>Valores posible:<br><code>dynamic</code>: empaquetado dinámico<br><code>boxes</code>: usar cajas configuradas en la cuenta</p><p><code>none</code>: no empaquetar (cada item debe resultar en un package separado)</p> |
| source                                                | string (150) | Utilizado en algunas integraciones para identificar el canal de venta.                                                                                                                                                                                                                                                                    |

{% tabs %}
{% tab title="200: OK Una respuesta correcta contendrá este detalle." %}
```javascript
{
  "sorted_by": "price",
  "destination": {
    "id": 1735,
    "city": "San Isidro",
    "state": "Buenos Aires",
    "zipcode": "1642"
  },
  "packages": [
  {
  "internal_id": 148121,
  "sku_id": null,
  "weight": 1100,
  "height": 12,
  "width": 20,
  "length": 17,
  "volume": 4080,
  "description_1": "2 productos",
  "description_2": "Mueble para armar",
  "description_3": null,
  "classification": {
      "id": 1,
      "name": "General"
  },
  "items": [
      {
      "sku": null,
      "description": "Mueble Parte 1",
      "must_keep_vertical": 0,
      "weight": 500,
      "width": 8,
      "length": 8,
      "height": 8,
      "pos_x": 0,
      "pos_y": 0,
      "pos_z": 0
      },
      {
      "sku": null,
      "description": "Mueble Parte 2",
      "must_keep_vertical": 0,
      "weight": 500,
      "width": 8,
      "length": 8,
      "height": 8,
      "pos_x": 8,
      "pos_y": 0,
      "pos_z": 0
      }
  ],
  "container": {
      "id": 1,
      "description": "Caja mediana 1",
      "outer_width": 20,
      "outer_height": 12,
      "outer_length": 17,
      "inner_width": 19,
      "inner_length": 16,
      "inner_height": 11,
      "max_weight": 5000
  }
  }
]
  "results": {
    "standard_delivery": {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    }
  },
  "all_results": [
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    },
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 220,
        "name": "Malargue",
        "rating": 0.94,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/malargue\/gzHM0KZOWMtDbd0svUiRJpiExH9uIQsAa3n8auke.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 5,
        "max": 10
      },
      "amounts": {
        "price_shipment": 725.7,
        "price_insurance": 8.18,
        "price": 733.88,
        "price_incl_tax": 888
      },
      "rate": {
        "source": "tariff",
        "id": 13050,
        "tariff_id": 113
      },
      "tags": []
    }
  ]
}
```
{% endtab %}

{% tab title="400: Bad Request Una respuesta erronea indicará detalladamente el motivo del error." %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

<details>

<summary>Ejemplo Body Request </summary>

```json
{
	"account_id": "2",
	"origin_id": "25",
	"declared_value": 9015,
	"items": [
		{
			"sku": "103964928",
			"weight": 1000,
			"height": 10,
			"width": 5,
			"length": 10,
			"description": "Capsulas The Coffee Store comp. Nespresso Mix",
			"classification_id": 1
		},
		{
			"sku": "103964928",
			"weight": 1000,
			"height": 5,
			"width": 10,
			"length": 10,
			"description": "Capsulas The Coffee Store comp. Nespresso Mix",
			"classification_id": 1
		}
	],
	"destination": {
		"city": "Palermo",
		"state": "Ciudad de Buenos Aires",
		"zipcode": "1425"
	}
}
```

</details>

#### Cotizar por Paquetes

Permite obtener opciones de despacho indicando un conjunto de paquetes a despachar. Queda de tu lado la agrupación de productos en paquetes, si es que debes enviar más de un producto.

La utilización de múltiples paquetes sirve para cuando en un mismo envío debes despachar múltiples bultos (por ejemplo si estás enviando un colchón y un sommier).

## Cotización de un envío en base a paquetes

<mark style="color:green;">`POST`</mark> `/v2/shipments/quote`

Este request te permitirá obtener cotizaciones de envíos indicando explícitamente los paquete que son parte del despacho.

#### Request Body

| Name                                                         | Type            | Description                                                                                                                                                                                                                                               |
| ------------------------------------------------------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>                | int             | ID de la cuenta                                                                                                                                                                                                                                           |
| origin\_id<mark style="color:red;">\*</mark>                 | int             | ID del origen                                                                                                                                                                                                                                             |
| declared\_value<mark style="color:red;">\*</mark>            | float           | Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.                                                                                                             |
| destination<mark style="color:red;">\*</mark>                | object          | Objeto destination                                                                                                                                                                                                                                        |
| destination.city<mark style="color:red;">\*</mark>           | string          | <p>Nombre de la ciudad del domicilio del destinatario.</p><p>AR: localidad</p><p>CL: comuna</p><p>CO: ciudad</p><p>MX: colonia</p>                                                                                                                        |
| destination.state<mark style="color:red;">\*</mark>          | string          | <p>Nombre del Estado del domicilio del destinatario.</p><p>AR: provincia</p><p>CL: región</p><p>CO: departamento</p><p>MX: estado</p>                                                                                                                     |
| destination.zipcode<mark style="color:red;">\*</mark>        | string          | <p>Código postal. </p><p>No requerido en Chile ni Colombia.</p>                                                                                                                                                                                           |
| packages<mark style="color:red;">\*</mark>                   | array\[package] | Array de objetos package. Minimo 1 package, maximo 99.                                                                                                                                                                                                    |
| packages.\*.description\_1<mark style="color:red;">\*</mark> | string (60)     | Titulo o descripcion del producto                                                                                                                                                                                                                         |
| packages.\*.weight<mark style="color:red;">\*</mark>         | int             | Peso, en gramos, del item                                                                                                                                                                                                                                 |
| packages.\*.length<mark style="color:red;">\*</mark>         | int             | Largo, en centimetros                                                                                                                                                                                                                                     |
| packages.\*.height<mark style="color:red;">\*</mark>         | int             | Alto, en centimetros                                                                                                                                                                                                                                      |
| packages.\*.width<mark style="color:red;">\*</mark>          | int             | Ancho, en centimetros                                                                                                                                                                                                                                     |
| packages.\*.classification\_id                               | int             | <p>Identificador de clasificación de producto. Si lo omites o indicas 1 (General) intentaremos clasificarlo automaticamente segun la descripcion.</p><p><a href="../../referencia/clasificaciones-de-producto.md">Ver Clasificaciones de producto</a></p> |
| packages.\*.description\_2                                   | string (60)     | Info adicional del producto                                                                                                                                                                                                                               |
| packages.\*.description\_3                                   | string (60)     | Info adicional del producto                                                                                                                                                                                                                               |
| destination.country                                          | string          | <p>Código ISO 3166-1 alfa-2 del país</p><p>Argentina: AR</p><p>Chile: CL</p><p>Colombia: CO</p><p>México: MX</p>                                                                                                                                          |
| source                                                       | string (150)    | Utilizado en algunas integraciones para identificar el canal de venta.                                                                                                                                                                                    |

{% tabs %}
{% tab title="200: OK Una respuesta correcta contendrá este detalle." %}
```javascript
{
  "sorted_by": "price",
  "destination": {
    "id": 1735,
    "city": "San Isidro",
    "state": "Buenos Aires",
    "zipcode": "1642"
  },
  "packages": [
  {
  "internal_id": 148121,
  "sku_id": null,
  "weight": 1100,
  "height": 12,
  "width": 20,
  "length": 17,
  "volume": 4080,
  "description_1": "2 productos",
  "description_2": "Mueble para armar",
  "description_3": null,
  "classification": {
      "id": 1,
      "name": "General"
  },
  "items": [
      {
      "sku": null,
      "description": "Mueble Parte 1",
      "must_keep_vertical": 0,
      "weight": 500,
      "width": 8,
      "length": 8,
      "height": 8,
      "pos_x": 0,
      "pos_y": 0,
      "pos_z": 0
      },
      {
      "sku": null,
      "description": "Mueble Parte 2",
      "must_keep_vertical": 0,
      "weight": 500,
      "width": 8,
      "length": 8,
      "height": 8,
      "pos_x": 8,
      "pos_y": 0,
      "pos_z": 0
      }
  ],
  "container": {
      "id": 1,
      "description": "Caja mediana 1",
      "outer_width": 20,
      "outer_height": 12,
      "outer_length": 17,
      "inner_width": 19,
      "inner_length": 16,
      "inner_height": 11,
      "max_weight": 5000
  }
  }
]
  "results": {
    "standard_delivery": {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    }
  },
  "all_results": [
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    },
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "crossdock",
      "carrier": {
        "id": 220,
        "name": "Malargue",
        "rating": 0.94,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/malargue\/gzHM0KZOWMtDbd0svUiRJpiExH9uIQsAa3n8auke.png"
      },
      "service_type": {
        "id": 1,
        "code": "standard_delivery",
        "name": "Entrega a domicilio",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 5,
        "max": 10
      },
      "amounts": {
        "price_shipment": 725.7,
        "price_insurance": 8.18,
        "price": 733.88,
        "price_incl_tax": 888
      },
      "rate": {
        "source": "tariff",
        "id": 13050,
        "tariff_id": 113
      },
      "tags": []
    }
  ]
}
```
{% endtab %}

{% tab title="400: Bad Request Una respuesta erronea indicará detalladamente el motivo del error." %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

<details>

<summary>Ejemplo Body Request </summary>

```json
{
	"account_id": "2",
	"origin_id": "25",
	"declared_value": 9015,
	"packages": [
		{
			"weight": 30000,
			"height": 35,
			"width": 140,
			"length": 190,
			"description_1": "Colchon 140x190 Cannon",
			"classification_id": 2
		},
		{
			"weight": 10000,
			"height": 30,
			"width": 140,
			"length": 190,
			"description_1": "Sommier 160x190 Expert",
			"classification_id": 2
		},
		{
			"weight": 1000,
			"height": 10,
			"width": 10,
			"length": 10,
			"description_1": "Patas de Sommier",
			"classification_id": 2
		}
	],
	"destination": {
		"city": "Palermo",
		"state": "Ciudad de Buenos Aires",
		"zipcode": "1425"
	}
}
```

</details>

### Respuesta de cotización

La respuesta de la cotización incluirá los siguientes elementos:

<details>

<summary>destination</summary>

Describe la localidad/comuna/ciudad de destino que fue identificada según los datos suministrados en el request.

</details>

<details>

<summary>packages</summary>

Sirve como referencia para entender cómo se construyeron los paquetes que conforman el envío.&#x20;

Si al cotizar indicaste paquetes, reflejará la misma información del request.&#x20;

En cambio, si indicaste items, aquí te mostrará cómo han sido agrupados esos items en paquetes.

</details>

<details>

<summary>results/all_results</summary>

Aquí estarán las distintas opciones para poder realizar un envío.&#x20;

En el objeto `results` tendrás un solo resultado ganador por cada `service_type` (forma de entrega).

En el objeto `all_results` tendrás todos los resultados disponibles.

</details>

#### Atributos de un result

<table><thead><tr><th width="164">Atributo</th><th>Descripción</th></tr></thead><tbody><tr><td>service_type</td><td>Tipo de servicio: la forma de entrega del envío.<br>El atributo <code>code</code> deberá ser usado al crear el envío (ej. standard_delivery)</td></tr><tr><td>logistic_type</td><td>Modo de despacho: cómo se va a despachar el envío</td></tr><tr><td>carrier</td><td>El transporte que hace la entrega. El atributo <code>id</code> deberá ser usado para crear el envío.</td></tr><tr><td>delivery_time</td><td>Indica el tiempo de entrega.<br><code>estimated_delivery</code> indica la fecha máxima de entrega<br><code>estimation_expires_at</code> indica cuando vence la estimación<br>times: indica distintos tiempos del proceso de entrega, en formato ISO8601 de duración.</td></tr><tr><td>amounts</td><td><p>Indica aspectos del precio del envío.<br><code>price</code> es el precio sin IVA que debe pagar el comprador<br><code>price_incl_tax</code> es el precio con IVA que debe pagar el comprador<br><code>seller_price</code> es el precio sin IVA que paga el vendedor<br><code>seller_price_incl_tax</code> es el precio con IVA que paga el vendedor<br><code>price_shipment</code> refleja la porción del precio del envío que es pura del envío<br><code>price_insurance</code> refleja la porción del precio del envío que corresponde al seguro y depende del valor declarado.<br></p><p><code>price</code> y <code>seller_price</code> por lo general son lo mismo, salvo en algunos casos:</p><ul><li>Cuando el resultado es de Flota Propia o Contrato Propio, el <code>price</code> refleja el precio de la tarifa y <code>seller_price</code> lo que cobra Zippin.</li><li>Cuando haya una regla que modifiquen el precio del envio, esa modificación se ve reflejada en <code>price</code>, mientras que <code>seller_price</code> mantiene el valor original.</li></ul></td></tr><tr><td>pickup_points</td><td>Es un array con puntos habilitados para la entrega del envío, cuando el tipo de servicio es <code>pickup_point</code>.<br>De cada punto es importante obtener el <code>point_id</code>, que deberá ser enviado al crear el envío para indicar la sucursal de entrega.</td></tr></tbody></table>

<details>

<summary>Ejemplo</summary>

```json
{
	"sorted_by": "price",
	"destination": {
		"id": 8013
		"city": "Formosa",
		"state": "Formosa",
		"country": "Argentina",
		"zipcode": "3600",
		"geolocation": {
			"lat": -26.1828223,
			"lng": -58.1733931
		}
	},
	"declared_value": 864.7,
	"packages": [
		{
			"descriptions": null,
			"weight": 38000,
			"height": 20,
			"width": 50,
			"length": 30,
			"volume": 30000,
			"sku_id": null,
			"classification_id": 1,
			"items": [],
			"container": null
		}
	],
	"results": {
		"standard_delivery": {
			"selectable": true,
			"impediments": null,
			"logistic_type": "carrier_dropoff",
			"carrier": {
				"id": 1,
				"name": "Andreani",
				"rating": 1,
				"logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/andreani-personalizado\/sIIX6bZ0AHNjq6a4mN41P4wqnDyEa88iFQio1lkW.png"
			},
			"service_type": {
				"id": 1,
				"code": "standard_delivery",
				"name": "Entrega a domicilio",
				"is_urgent": 0
			},
			"delivery_time": {
				"warning": "Dear developer, min and max attributes will be deprecated soon. Use provided estimation date or times",
				"min": 5,
				"max": 7,
				"estimated_delivery": "2023-02-23T23:00:00+00:00",
				"estimation_expires_at": "2023-02-14T19:00:00+00:00",
				"times": {
					"preparation": "P1DT10H",
					"crossdocking": "PT0S",
					"carrier": {
						"min": "P3D",
						"max": "P5D",
						"unadjusted": null
					},
					"total": {
						"min": "P4DT10H",
						"max": "P6DT10H"
					}
				}
			},
			"amounts": {
				"price_shipment": 2830.01,
				"price_insurance": 12.97,
				"price": 2842.98,
				"price_incl_tax": 3440,
				"seller_price": 2842.98,
				"seller_price_incl_tax": 3440
			},
			"rate": {
				"source": "tariff",
				"id": null,
				"tariff_id": 2112
			},
			"tags": []
		},
		"pickup_point": {
			"selectable": true,
			"impediments": null,
			"logistic_type": "carrier_dropoff",
			"carrier": {
				"id": 1,
				"name": "Andreani",
				"rating": 1,
				"logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/andreani-personalizado\/sIIX6bZ0AHNjq6a4mN41P4wqnDyEa88iFQio1lkW.png"
			},
			"service_type": {
				"id": 9,
				"code": "pickup_point",
				"name": "Entrega en punto de entrega",
				"is_urgent": 0
			},
			"delivery_time": {
				"warning": "Dear developer, min and max attributes will be deprecated soon. Use provided estimation date or times",
				"min": 5,
				"max": 7,
				"estimated_delivery": "2023-02-23T23:00:00+00:00",
				"estimation_expires_at": "2023-02-14T19:00:00+00:00",
				"times": {
					"preparation": "P1DT10H",
					"crossdocking": "PT0S",
					"carrier": {
						"min": "P3D",
						"max": "P5D",
						"unadjusted": null
					},
					"total": {
						"min": "P4DT10H",
						"max": "P6DT10H"
					}
				}
			},
			"amounts": {
				"price_shipment": 2149.01,
				"price_insurance": 12.97,
				"price": 2161.98,
				"price_incl_tax": 2616,
				"seller_price": 2161.98,
				"seller_price_incl_tax": 2616
			},
			"rate": {
				"source": "tariff",
				"id": null,
				"tariff_id": 2113
			},
			"tags": [
				"cheapest"
			],
			"pickup_points": [
				{
					"point_id": 14677,
					"description": "Andreani - Formosa (Centro)",
					"open_hours": null,
					"phone": "0810-122-1111",
					"location": {
						"street": "Saavedra",
						"street_number": "423",
						"street_extras": null,
						"city": "Formosa",
						"state": "Formosa",
						"geolocation": {
							"lat": -26.18125,
							"lng": -58.16975,
							"distance": 400
						}
					}
				}
			]
		}
	},
	"all_results": [
		{
			"selectable": true,
			"impediments": null,
			"logistic_type": "carrier_dropoff",
			"carrier": {
				"id": 1,
				"name": "Andreani",
				"rating": 1,
				"logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/andreani-personalizado\/sIIX6bZ0AHNjq6a4mN41P4wqnDyEa88iFQio1lkW.png"
			},
			"service_type": {
				"id": 9,
				"code": "pickup_point",
				"name": "Entrega en punto de entrega",
				"is_urgent": 0
			},
			"delivery_time": {
				"warning": "Dear developer, min and max attributes will be deprecated soon. Use provided estimation date or times",
				"min": 5,
				"max": 7,
				"estimated_delivery": "2023-02-23T23:00:00+00:00",
				"estimation_expires_at": "2023-02-14T19:00:00+00:00",
				"times": {
					"preparation": "P1DT10H",
					"crossdocking": "PT0S",
					"carrier": {
						"min": "P3D",
						"max": "P5D",
						"unadjusted": null
					},
					"total": {
						"min": "P4DT10H",
						"max": "P6DT10H"
					}
				}
			},
			"amounts": {
				"price_shipment": 2149.01,
				"price_insurance": 12.97,
				"price": 2161.98,
				"price_incl_tax": 2616,
				"seller_price": 2161.98,
				"seller_price_incl_tax": 2616
			},
			"rate": {
				"source": "tariff",
				"id": null,
				"tariff_id": 2113
			},
			"tags": [
				"cheapest"
			],
			"pickup_points": [
				{
					"point_id": 14677,
					"description": "Andreani - Formosa (Centro)",
					"open_hours": null,
					"phone": "0810-122-1111",
					"location": {
						"street": "Saavedra",
						"street_number": "423",
						"street_extras": null,
						"city": "Formosa",
						"state": "Formosa",
						"geolocation": {
							"lat": -26.18125,
							"lng": -58.16975,
							"distance": 400
						}
					}
				}
			]
		}
		
	]
}
```

</details>



## Crear envíos

{% hint style="warning" %}
Los endpoints de creación de envíos utilizan **rate limiting** :red\_circle:<mark style="color:red;">**Alto**</mark>\
[Ver más sobre límites de requests](../limites-de-requests.md)
{% endhint %}

{% hint style="warning" %}
Al crear un envío necesitarás informar algunos datos que surgen de la cotización.&#x20;

**Luego de cotizar, debes almacenar los valores de `logistic_type`, `service_type`, `carrier_id` de la opción de envío deseada, para luego utilizarlos al crear el envío.**

Si se trata de un envio a retirar por sucursal, también debes almacenar el `point_id` de la ubicación donde se vaya a hacer el retiro del envío por el destinatario.
{% endhint %}

{% hint style="info" %}
Recomendamos completar el atributo `source` con algo que identifique a tu integración, para que luego los clientes puedan definir reglas personalizadas de cotización utilizando el [Motor de Reglas](https://ayuda.zippin.app/automatizaciones), utilizando el atributo source como criterio de filtrado.
{% endhint %}

#### Crear por items

Permite crear un envío indicando un conjunto de productos a despachar. La API empaquetará los items en paquetes de acuerdo a la lógica definida en la cuenta, y teniendo en cuenta los contenedores configurados.

## Crear envío (basado en items) - Recomendado

<mark style="color:green;">`POST`</mark> `/v2/shipments`

Este request te permitirá crear envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.

#### Request Body

| Name                                                         | Type         | Description                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>                | int          | ID de la cuenta                                                                                                                                                                                                                                                                                                                           |
| origin\_id<mark style="color:red;">\*</mark>                 | int          | ID del origen del envío                                                                                                                                                                                                                                                                                                                   |
| logistic\_type                                               | string       | <p>Código de la forma de despacho del envío.</p><p>Ejemplo: <code>carrier_dropoff</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p>                                                                                                                     |
| service\_type                                                | string       | <p>Código de la forma de entrega del envío.</p><p>Ejemplo: <code>standard_delivery</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p>                                                                                                                    |
| carrier\_id                                                  | int          | <p>ID del transporte que hará la entrega.</p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.</p>                                                                                                                                                                                                       |
| source                                                       | string (150) | Utilizado en algunas integraciones para identificar el canal de venta.                                                                                                                                                                                                                                                                    |
| declared\_value<mark style="color:red;">\*</mark>            | float        | Valor declarado del envío. Monto que se asegurará. Para no utilizar seguro se debe enviar en cero.                                                                                                                                                                                                                                        |
| destination.name<mark style="color:red;">\*</mark>           | string (100) | Nombre y apellido del destinatario                                                                                                                                                                                                                                                                                                        |
| destination.street<mark style="color:red;">\*</mark>         | string (100) | <p>Calle del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                                                                                                                      |
| destination.street\_number<mark style="color:red;">\*</mark> | string (10)  | <p>Altura/nro de puerta del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                                                                                                       |
| destination.street\_extras                                   | string (190) | <p>Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                                                                     |
| destination.point\_id                                        | int          | <p>ID de la ubicación de entrega.</p><p>Requerido solo cuando la entrega es en un punto de entrega.</p>                                                                                                                                                                                                                                   |
| destination.document<mark style="color:red;">\*</mark>       | string (50)  | Número de documento (DNI, RUT, o lo que corresponda) del destinatario                                                                                                                                                                                                                                                                     |
| destination.email<mark style="color:red;">\*</mark>          | email (150)  | Dirección de correo electrónico del destinatario                                                                                                                                                                                                                                                                                          |
| destination.phone<mark style="color:red;">\*</mark>          | string (50)  | Número de contacto del destinatario                                                                                                                                                                                                                                                                                                       |
| destination.state<mark style="color:red;">\*</mark>          | string       | <p>Nombre del Estado del domicilio del destinatario.</p><p>AR: provincia</p><p>CL: región</p><p>CO: departamento</p><p>MX: estado</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                       |
| destination.city<mark style="color:red;">\*</mark>           | string       | <p>Nombre de la ciudad del domicilio del destinatario.</p><p>AR: localidad</p><p>CL: comuna</p><p>CO: ciudad</p><p>MX: ciudad</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                                           |
| destination.zipcode<mark style="color:red;">\*</mark>        | string       | <p>Codigo postal de la dirección de destino.</p><p>Requerido solo en entregas a domicilio.</p><p>Dato no requerido en Chile y Colombia.</p>                                                                                                                                                                                               |
| destination.country                                          | string       | <p>Código ISO 3166-1 alfa-2 del país</p><p>Argentina: AR</p><p>Chile: CL</p><p>Colombia: CO</p><p>México: MX</p>                                                                                                                                                                                                                          |
| destination<mark style="color:red;">\*</mark>                | object       | Objeto con datos del destinatario                                                                                                                                                                                                                                                                                                         |
| items<mark style="color:red;">\*</mark>                      | array\[Item] | <p>Array de objetos Item. </p><p>Cada Item es un articulo a enviar. </p><p>El máximo de artículos por envío es 1000.</p><p>Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.</p>                                                                                                      |
| items.\*.sku                                                 | string (190) | Se intentará vincular a un producto cargado en el catálogo de Zippin.                                                                                                                                                                                                                                                                     |
| items.\*.weight<mark style="color:red;">\*</mark>            | int          | Peso en gramos.                                                                                                                                                                                                                                                                                                                           |
| items.\*.classification\_id                                  | int          | <p>ID de clasificación de producto. </p><p>Si se omite se utilizará la clasificación General (1).</p><p><a href="../../referencia/clasificaciones-de-producto.md">Ver clasificaciones disponibles</a>.</p>                                                                                                                                |
| items.\*.height<mark style="color:red;">\*</mark>            | int          | Alto del artículo, en centímetros.                                                                                                                                                                                                                                                                                                        |
| items.\*.width<mark style="color:red;">\*</mark>             | int          | Ancho del artículo, en centímetros.                                                                                                                                                                                                                                                                                                       |
| items.\*.length<mark style="color:red;">\*</mark>            | int          | Largo del artículo, en centímetros.                                                                                                                                                                                                                                                                                                       |
| items.\*.description                                         | string (190) | Descripción o nombre del artículo                                                                                                                                                                                                                                                                                                         |
| external\_id<mark style="color:red;">\*</mark>               | string (30)  | ID del envío en tus sistemas. Es para tu propia referencia.                                                                                                                                                                                                                                                                               |
| type\_packaging                                              | string       | <p>Indica la forma de empaquetar los productos. Si se omite se usa el que tenga definido la cuenta por default.<br>Valores posible:<br><code>dynamic</code>: empaquetado dinámico<br><code>boxes</code>: usar cajas configuradas en la cuenta</p><p><code>none</code>: no empaquetar (cada item debe resultar en un package separado)</p> |

{% tabs %}
{% tab title="201: Created El envío se crea correctamente." %}
```javascript
{
    // Detalle del envío creado
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

#### Crear por Paquetes

Permite crear un envío indicando un conjunto de paquetes a despachar. Queda de tu lado la agrupación de productos en paquetes, si es que debes enviar más de un producto.

La utilización de múltiples paquetes sirve para cuando en un mismo envío debes despachar múltiples bultos (por ejemplo si estás enviando un colchón y un sommier).

## Crear envío (basado en packages)

<mark style="color:green;">`POST`</mark> `/v2/shipments`

Este request te permitirá crear envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.

#### Request Body

| Name                                                         | Type            | Description                                                                                                                                                                                                            |
| ------------------------------------------------------------ | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>                | int             | ID de la cuenta                                                                                                                                                                                                        |
| origin\_id<mark style="color:red;">\*</mark>                 | int             | ID del origen del envío                                                                                                                                                                                                |
| logistic\_type                                               | string          | <p>Código de la forma de despacho del envío.</p><p>Ejemplo: <code>carrier_dropoff</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p>  |
| service\_type                                                | string          | <p>Código de la forma de entrega del envío.</p><p>Ejemplo: <code>standard_delivery</code></p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada</p> |
| carrier\_id                                                  | int             | <p>ID del transporte que hará la entrega.</p><p>Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.</p>                                                                                    |
| source                                                       | string (100)    | Utilizado en algunas integraciones para identificar el canal de venta.                                                                                                                                                 |
| declared\_value<mark style="color:red;">\*</mark>            | float           | Valor declarado del envío. Monto que se asegurará. Para no utilizar seguro se debe enviar en cero.                                                                                                                     |
| destination.name<mark style="color:red;">\*</mark>           | string (100)    | Nombre y apellido del destinatario                                                                                                                                                                                     |
| destination.street<mark style="color:red;">\*</mark>         | string (100)    | <p>Calle del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                                   |
| destination.street\_number<mark style="color:red;">\*</mark> | string (10)     | <p>Altura/nro de puerta del domicilio de entrega.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                                                    |
| destination.street\_extras (190)                             | string          | <p>Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.</p><p>Requerido solo en entregas a domicilio.</p>                                                                                  |
| destination.point\_id                                        | int             | <p>ID de la ubicación de entrega.</p><p>Requerido solo cuando la entrega es en un punto de entrega.</p>                                                                                                                |
| destination.document<mark style="color:red;">\*</mark>       | string (50)     | Número de documento (DNI, RUT, o lo que corresponda) del destinatario                                                                                                                                                  |
| destination.email<mark style="color:red;">\*</mark>          | email (150)     | Dirección de correo electrónico del destinatario                                                                                                                                                                       |
| destination.phone<mark style="color:red;">\*</mark>          | string (50)     | Número de contacto del destinatario                                                                                                                                                                                    |
| destination.state<mark style="color:red;">\*</mark>          | string          | <p>Nombre del Estado del domicilio del destinatario.</p><p>AR: provincia</p><p>CL: región</p><p>CO: departamento</p><p>MX: estado</p><p>Requerido solo en entregas a domicilio.</p>                                    |
| destination.city<mark style="color:red;">\*</mark>           | string          | <p>Nombre de la ciudad del domicilio del destinatario.</p><p>AR: localidad</p><p>CL: comuna</p><p>CO: ciudad</p><p>MX: ciudad</p><p>Requerido solo en entregas a domicilio.</p>                                        |
| destination.zipcode<mark style="color:red;">\*</mark>        | string          | <p>Codigo postal de la dirección de destino.</p><p>Requerido solo en entregas a domicilio.</p><p>Dato no requerido en Chile y Colombia.</p>                                                                            |
| destination.country                                          | string          | <p>Código ISO 3166-1 alfa-2 del país</p><p>Argentina: AR</p><p>Chile: CL</p><p>Colombia: CO</p><p>México: MX</p>                                                                                                       |
| destination<mark style="color:red;">\*</mark>                | object          | Objeto con datos del destinatario                                                                                                                                                                                      |
| packages<mark style="color:red;">\*</mark>                   | array\[package] | <p>Array de objetos Package.  </p><p>El máximo de paquetes por envío es 99.</p>                                                                                                                                        |
| packages.\*.weight<mark style="color:red;">\*</mark>         | int             | Peso en gramos.                                                                                                                                                                                                        |
| packages.\*.classification\_id                               | int             | <p>ID de clasificación de producto del paquete. </p><p>Si se omite se utilizará la clasificación General (1).</p><p><a href="../../referencia/clasificaciones-de-producto.md">Ver clasificaciones disponibles</a>.</p> |
| packages.\*.height<mark style="color:red;">\*</mark>         | int             | Alto del paquete, en centímetros.                                                                                                                                                                                      |
| packages.\*.width<mark style="color:red;">\*</mark>          | int             | Ancho del paquete, en centímetros.                                                                                                                                                                                     |
| packages.\*.length<mark style="color:red;">\*</mark>         | int             | Largo del paquete, en centímetros.                                                                                                                                                                                     |
| packages.\*.description\_1<mark style="color:red;">\*</mark> | string (60)     | Descripción del contenido (linea 1)                                                                                                                                                                                    |
| packages.\*.description\_3                                   | string (60)     | Descripción del contenido (linea 3)                                                                                                                                                                                    |
| packages.\*.description\_2                                   | string (60)     | Descripción del contenido (linea 2)                                                                                                                                                                                    |
| external\_id<mark style="color:red;">\*</mark>               | string (30)     | ID del envío en tus sistemas. Es para tu propia referencia.                                                                                                                                                            |

{% tabs %}
{% tab title="201: Created El envío se crea correctamente." %}
```javascript
{
    // Detalle del envío creado
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

## Devoluciones

### Cotizar un envío de logística inversa

**El endpoint de cotización de una devolución nos permite obtener los precios y opciones para realizar la misma.**

En cada cotización, se deberá indicar la cuenta, valor declarado del envío y detalle de paquetes e items que se devolverán.

Al hacer el request **hay algunas particularidades** con respecto a la definición de los paquetes e items que los componen que serán devueltos.

* **Particularidades en la definición de paquetes a devolver**\
  En caso de no indicar el array de  **packages** con los paquetes que contendrá la devolución. El sistema interpreta que se devolverán todos los paquetes del envío con todos sus items definidos. Si el array de paquetes esta definido solo se cotizará la devolución de los paquetes que contenga dicho array.
* **Particularidades en la definición de items a devolver**\
  Para cada paquete que se indique en el array de paquetes se deberá indicar que items se devolverán. En caso de no indicar el array de  **items** con los items que contendrán los paquetes de la devolución. El sistema interpreta que se devolverán todos los items del paquete indicado. Si el array de items esta definido dentro de un paquete solo se cotizará la devolución de los items que contenga dicho array.

## Cotizar devolución

<mark style="color:green;">`POST`</mark> `/v2/shipments/{id}/return/quote`

Este request te permitirá obtener cotizaciones para una devolución de un envío, indicando los ítems específicos que forman parte de la devolución.

#### Path Parameters

| Name                                 | Type | Description                           |
| ------------------------------------ | ---- | ------------------------------------- |
| id<mark style="color:red;">\*</mark> | int  | ID del envío a cotizar una devolución |

#### Request Body

| Name                                              | Type  | Description                                                                                                                                                   |
| ------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>     | int   | ID de la cuenta                                                                                                                                               |
| declared\_value<mark style="color:red;">\*</mark> | float | Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.                 |
| packages<mark style="color:red;">\*</mark>        | array | Array de objetos packages.                                                                                                                                    |
| packages.\*.id<mark style="color:red;">\*</mark>  | int   | ID del paquete del envio original que se quiere devolver                                                                                                      |
| packages.\*.items                                 | array | Opcionalmente, se puede indicar qué items de un paquete se quieren incluir en la devolucion. Si se omite, se asume que se quiere devolver el paquete completo |
| package.\*.items.\*.id                            | int   | ID de item del paquete                                                                                                                                        |

{% tabs %}
{% tab title="200: OK Una respuesta correcta contendrá este detalle." %}
```javascript
// Request
{
	"account_id": "2",
	"declared_value": 629,
	"packages": [
		{
			"id": 76524,
		},
		{
			"id": 76525,
      "items":[
      {
        "id": 986,
      },
      {
        "id": 987,
      }
		}
	],
}

// Response
{
  "sorted_by": "price",
  "destination": {
    "id": 1735,
    "city": "San Isidro",
    "state": "Buenos Aires",
    "zipcode": "1642"
  },
  "packages": [
    {
      "id": 76524
      "weight": 10000,
      "height": 10,
      "width": 50,
      "length": 30,
      "volume": 15000,
      "sku_id": null,
      "classification_id": 1,
      "items": [
        {
          "id": 984
          "weight": 500,
          "height": 8,
          "width": 8,
          "length": 8,
          "sku": "ITEM1"
        }
      ],
      "container": null
    },
    {
      "id": 76524
      "weight": 10000,
      "height": 10,
      "width": 50,
      "length": 30,
      "volume": 15000,
      "sku_id": null,
      "classification_id": 1,
      "items": [
        {
          "id": 986
          "weight": 500,
          "height": 8,
          "width": 8,
          "length": 8,
          "sku": "ITEM1"
        },
        {
          "id": 987
          "weight": 500,
          "height": 8,
          "width": 8,
          "length": 8,
          "sku": "ITEM2"
        }
      ],
      "container": {
        "id": 2,
        "description": "Caja Mediana 2",
        "inner_width": 14,
        "inner_length": 11,
        "inner_height": 8,
        "outer_width": 15,
        "outer_length": 12,
        "outer_height": 9,
        "empty_weight": 100,
        "max_weight": 3000
      }
    }
  ],
  "results": {
    "standard_delivery": {
      "selectable": true,
      "impediments": null,
      "logistic_type": "carrier_dropoff",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 10,
        "code": "return_origin",
        "name": "Devolución a origen",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    }
  },
  "all_results": [
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "carrier_dropoff",
      "carrier": {
        "id": 205,
        "name": "Leset",
        "rating": 0.95,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/leset\/9Bw6vmnLH2ZUvKTdAcQtqJRkvkZdJTqcPlR9nwYU.png"
      },
      "service_type": {
        "id": 10,
        "code": "return_origin",
        "name": "Devolución a origen",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 3,
        "max": 7
      },
      "amounts": {
        "price_shipment": 315.79,
        "price_insurance": 8.18,
        "price": 323.97,
        "price_incl_tax": 392
      },
      "rate": {
        "source": "tariff",
        "id": 12957,
        "tariff_id": 71
      },
      "tags": [
        "cheapest",
        "fastest"
      ]
    },
    {
      "selectable": true,
      "impediments": null,
      "logistic_type": "carrier_pickup",
      "carrier": {
        "id": 220,
        "name": "Malargue",
        "rating": 0.94,
        "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/malargue\/gzHM0KZOWMtDbd0svUiRJpiExH9uIQsAa3n8auke.png"
      },
      "service_type": {
        "id": 10,
        "code": "return_origin",
        "name": "Devolución a origen",
        "is_urgent": 0
      },
      "delivery_time": {
        "min": 5,
        "max": 10
      },
      "amounts": {
        "price_shipment": 725.7,
        "price_insurance": 8.18,
        "price": 733.88,
        "price_incl_tax": 888
      },
      "rate": {
        "source": "tariff",
        "id": 13050,
        "tariff_id": 113
      },
      "tags": []
    }
  ]
}
```
{% endtab %}
{% endtabs %}

### Crear envío de logística inversa

**El endpoint de creación de una devolución nos permite crear la devolución, indicando todo el detalle del mismo y su contenido.**

En cada creación, se deberá indicar la cuenta, valor declarado del envío y detalle de paquetes e items que se devolverán. También se indicará el service\_type, logistic\_type que deben haber sido obtenidos previamente de una cotización.

Al hacer el request **hay algunas particularidades** con respecto a la definición de los paquetes e items que los componen que serán devueltos.

* **Particularidades en la definición de paquetes a devolver**\
  En caso de no indicar el array de  **packages** con los paquetes que contendrá la devolución. El sistema interpreta que se devolverán todos los paquetes del envío con todos sus items definidos. Si el array de paquetes esta definido solo se creará la devolución de los paquetes que contenga dicho array.
* **Particularidades en la definición de items a devolver**\
  Para cada paquete que se indique en el array de paquetes se deberá indicar que items se devolverán. En caso de no indicar el array de  **items** con los items que contendrán los paquetes de la devolución. El sistema interpreta que se devolverán todos los items del paquete indicado. Si el array de items esta definido dentro de un paquete solo se creará la devolución de los items que contenga dicho array.

## Crear devolución

<mark style="color:green;">`POST`</mark> `/v2/shipments/{id}/return`

#### Path Parameters

| Name                                 | Type | Description                                  |
| ------------------------------------ | ---- | -------------------------------------------- |
| id<mark style="color:red;">\*</mark> | int  | ID del envío a del cual crear una devolución |

#### Request Body

| Name                                              | Type   | Description                                                                                                                                                   |
| ------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account\_id<mark style="color:red;">\*</mark>     | int    | ID de la cuenta                                                                                                                                               |
| declared\_value<mark style="color:red;">\*</mark> | float  | Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.                 |
| packages<mark style="color:red;">\*</mark>        | array  | Array de objetos packages.                                                                                                                                    |
| packages.\*.id<mark style="color:red;">\*</mark>  | int    | ID del paquete del envio original que se quiere devolver                                                                                                      |
| packages.\*.items                                 | array  | Opcionalmente, se puede indicar qué items de un paquete se quieren incluir en la devolucion. Si se omite, se asume que se quiere devolver el paquete completo |
| package.\*.items.\*.id                            | int    | ID de item del paquete                                                                                                                                        |
| carrier\_id                                       | int    | Para indicar con qué transporte crear la devolución (sale de la cotización).                                                                                  |
| logistic\_type<mark style="color:red;">\*</mark>  | string | <p>Código del modo de despacho (sale de la cotización)</p><p>Ej. <code>carrier_dropoff</code></p>                                                             |
| service\_type<mark style="color:red;">\*</mark>   | string | <p>Código del tipo de servicio (sale de la cotización)</p><p>Ej. <code>return_origin</code></p>                                                               |

{% tabs %}
{% tab title="201: Created Una respuesta correcta contendrá este detalle." %}
```javascript
// Request
{
	"account_id": "2",
	"declared_value": 9015,
  "service_type": "return_origin",
  "logistic_type": "carrier_dropoff",
	"packages": [
		{
			"id": 76524,
		},
		{
			"id": 76525,
      "items":[
      {
        "id": 986,
      },
      {
        "id": 987,
      }
		}
	],
}

// Response
{
  "id": 151060,
  "external_id": "testcosts",
  "delivery_id": "0999-00151060",
  "carrier_tracking_id": null,
  "created_at": "2020-06-09T15:19:07+0000",
  "account_id": 2,
  "parent_shipment_id": null,
  "logistic_type": "carrier_dropoff",
  "service_type": "return_origin",
  "carrier": {
    "id": 8,
    "name": "La Sevillanita",
    "logo": "https:\/\/zippin-ar.s3.amazonaws.com\/carriers\/la-sevillanita\/Kt4qo3TsENegNCCnrW70OxQhBqbo7uyfemNRT5bH.png"
  },
  "status": "new",
  "status_name": "Procesando",
  "tracking": "http:\/\/app.zippin.com.ar\/track\/151060",
  "tracking_external": "http:\/\/app.zippin.com.ar\/track\/2\/testcosts",
  "destination": {
    "name": "TEST",
    "document": "3333",
    "street": "TEST",
    "street_number": "222",
    "street_extras": "TEST",
    "city": "Salta",
    "state": "Salta",
    "zipcode": "4400",
    "phone": "00000",
    "email": "TEST@test.com"
  },
  "origin": {
    "id": 25,
    "name": "Origen Demo",
    "document": "22222222",
    "street": "Av Luis Maria Campos",
    "street_number": "877",
    "street_extras": "PRUEBA",
    "city": "Capital Federal",
    "state": "Capital Federal",
    "zipcode": "1005",
    "phone": "22222222",
    "email": "demo@zippin.com.ar"
  },
  "declared_value": 9015,
  "price": 869.42,
  "price_incl_tax": 1052,
  "total_weight": 1100,
  "total_volume": 4080,
  "packages": [
  {
    "internal_id": 76524,
    "sku_id": null,
    "weight": 1100,
    "height": 12,
    "width": 20,
    "length": 17,
    "volume": 4080,
    "description_1": "1 producto",
    "description_2": "Test 1",
    "description_3": null,
    "classification": {
      "id": 1,
      "name": "General"
    },
    "items": [
      {
        "sku": null,
        "description": "985",
        "must_keep_vertical": 0,
        "weight": 500,
        "width": 8,
        "length": 8,
        "height": 8,
        "pos_x": 0,
        "pos_y": 0,
        "pos_z": 0
      }
    ],
    "container": null,
    {
      "internal_id": 76525,
      "sku_id": null,
      "weight": 1100,
      "height": 12,
      "width": 20,
      "length": 17,
      "volume": 4080,
      "description_1": "2 productos",
      "description_2": "Test 1",
      "description_3": null,
      "classification": {
        "id": 1,
        "name": "General"
      },
      "items": [
        {
          "sku": null,
          "description": "986",
          "must_keep_vertical": 0,
          "weight": 500,
          "width": 8,
          "length": 8,
          "height": 8,
          "pos_x": 0,
          "pos_y": 0,
          "pos_z": 0
        },
        {
          "sku": null,
          "description": "987",
          "must_keep_vertical": 0,
          "weight": 500,
          "width": 8,
          "length": 8,
          "height": 8,
          "pos_x": 8,
          "pos_y": 0,
          "pos_z": 0
        }
      ],
      "container": {
        "id": 1,
        "description": "Test 1",
        "outer_width": 20,
        "outer_height": 12,
        "outer_length": 17,
        "inner_width": 19,
        "inner_length": 16,
        "inner_height": 11,
        "max_weight": 5000
      }
    }
  ],
  "tags": []
}
```
{% endtab %}
{% endtabs %}

\


\
