# Cotizar y Crear Envíos

## Cotizar

La cotización es el proceso mediante el cual se obtienen opciones para hacer un envío.

En Zippin existen múltiples maneras de hacer un envío, combinando diferentes **formas de despacho** (`logistic_type`) y **formas de entrega** (`service_type`).

En los resultados de cotización verás todas las opciones disponibles en tu cuenta para hacer el envío, con los distintos transportes disponibles.

#### Cotizar Por Items

Permite obtener opciones de despacho indicando un conjunto de productos a despachar. La API empaquetará los items en paquetes de acuerdo a la lógica definida en la cuenta, y teniendo en cuenta los contenedores configurados.

{% swagger method="post" path="/shipments/quote" baseUrl="/v2" summary="Cotización de un envío en base a items (recomendado)" %}
{% swagger-description %}
Este request te permitirá obtener cotizaciones de envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="origin_id" type="int" required="true" %}
ID del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" type="object" required="true" %}
Objeto destination
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.city" required="true" type="string" %}
Nombre de la ciudad del domicilio del destinatario.

AR: localidad (ej. Avellaneda)

CL: comuna (ej. Las Condes)

CO: ciudad (ej. Usaquén)

MX: colonia (ej. Polanco)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.state" type="string" required="true" %}
Nombre del Estado del domicilio del destinatario.

AR: provincia (ej. Buenos Aires)

CL: región (ej. RM)

CO: departamento  (ej. Bogotá DC)

MX: estado (ej. Distrito Federal)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.country" type="string" %}
Código ISO 3166-1 alfa-2 del país

Argentina: AR

Chile: CL

Colombia: CO

México: MX
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.zipcode" type="string" required="true" %}
Código postal.&#x20;

No requerido en Chile ni Colombia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items" type="array[item]" required="true" %}
Array de objetos item. Minimo 1 item, maximo 1000 items.

Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.sku" type="string (190)" required="true" %}
Se intentará vincular a un producto cargado en el catalogo de Zippin, en base al código de referencia de los productos.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.description" type="string (190)" %}
Titulo o descripcion del producto
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.weight" type="int" required="true" %}
Peso, en gramos, del item
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.length" required="true" type="int" %}
Largo, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.height" required="true" type="int" %}
Alto, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.width" required="true" type="int" %}
Ancho, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.classification_id" type="int" %}
Identificador de clasificación de producto. Si lo omites o indicas 1 (General) intentaremos clasificarlo automaticamente segun la descripcion.

[Ver Clasificaciones de producto](../../referencia/clasificaciones-de-producto.md)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="type_packaging" %}
Indica la forma de empaquetar los productos. Si se omite se usa el que tenga definido la cuenta por default.\
Valores posible:\
`dynamic`: empaquetado dinámico\
`boxes`: usar cajas configuradas en la cuenta

`none`: no empaquetar (cada item debe resultar en un package separado)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="string (150)" %}
Utilizado en algunas integraciones para identificar el canal de venta.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Una respuesta correcta contendrá este detalle." %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Una respuesta erronea indicará detalladamente el motivo del error." %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

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

{% swagger method="post" path="/shipments/quote" baseUrl="/v2" summary="Cotización de un envío en base a paquetes" %}
{% swagger-description %}
Este request te permitirá obtener cotizaciones de envíos indicando explícitamente los paquete que son parte del despacho.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="origin_id" type="int" required="true" %}
ID del origen
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="string (150)" %}
Utilizado en algunas integraciones para identificar el canal de venta.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" type="object" required="true" %}
Objeto destination
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.city" required="true" type="string" %}
Nombre de la ciudad del domicilio del destinatario.

AR: localidad

CL: comuna

CO: ciudad

MX: colonia
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.state" type="string" required="true" %}
Nombre del Estado del domicilio del destinatario.

AR: provincia

CL: región

CO: departamento

MX: estado
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.country" type="string" %}
Código ISO 3166-1 alfa-2 del país

Argentina: AR

Chile: CL

Colombia: CO

México: MX
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.zipcode" type="string" required="true" %}
Código postal.&#x20;

No requerido en Chile ni Colombia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages" type="array[package]" required="true" %}
Array de objetos package. Minimo 1 package, maximo 99.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_1" type="string (60)" required="true" %}
Titulo o descripcion del producto
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_2" type="string (60)" %}
Info adicional del producto
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_3" type="string (60)" %}
Info adicional del producto
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.weight" type="int" required="true" %}
Peso, en gramos, del item
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.length" required="true" type="int" %}
Largo, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.height" required="true" type="int" %}
Alto, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.width" required="true" type="int" %}
Ancho, en centimetros
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.classification_id" type="int" %}
Identificador de clasificación de producto. Si lo omites o indicas 1 (General) intentaremos clasificarlo automaticamente segun la descripcion.

[Ver Clasificaciones de producto](../../referencia/clasificaciones-de-producto.md)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Una respuesta correcta contendrá este detalle." %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Una respuesta erronea indicará detalladamente el motivo del error." %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

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

{% hint style="info" %}
Al crear un envío necesitarás informar algunos datos que surgen de la cotización.&#x20;

**Luego de cotizar, debes almacenar los valores de `logistic_type`, `service_type`, `carrier_id` de la opción de envío deseada, para luego utilizarlos al crear el envío.**

Si se trata de un envio a retirar por sucursal, también debes almacenar el `point_id` de la ubicación donde se vaya a hacer el retiro del envío por el destinatario.
{% endhint %}

#### Crear por items

Permite crear un envío indicando un conjunto de productos a despachar. La API empaquetará los items en paquetes de acuerdo a la lógica definida en la cuenta, y teniendo en cuenta los contenedores configurados.

{% swagger method="post" path="/shipments" baseUrl="/v2" summary="Crear envío (basado en items) - Recomendado" %}
{% swagger-description %}
Este request te permitirá crear envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="origin_id" type="int" required="true" %}
ID del origen del envío
{% endswagger-parameter %}

{% swagger-parameter in="body" name="logistic_type" type="string" %}
Código de la forma de despacho del envío.

Ejemplo: `carrier_dropoff`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="service_type" type="string" %}
Código de la forma de entrega del envío.

Ejemplo: `standard_delivery`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="carrier_id" type="int" %}
ID del transporte que hará la entrega.

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="external_id" type="string (30)" required="true" %}
ID del envío en tus sistemas. Es para tu propia referencia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="string (150)" %}
Utilizado en algunas integraciones para identificar el canal de venta.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado del envío. Monto que se asegurará. Para no utilizar seguro se debe enviar en cero.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" type="object" required="true" %}
Objeto con datos del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.point_id" type="int" %}
ID de la ubicación de entrega.

Requerido solo cuando la entrega es en un punto de entrega.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.name" type="string (100)" required="true" %}
Nombre y apellido del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street" type="string (100)" required="true" %}
Calle del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street_number" type="string (10)" required="true" %}
Altura/nro de puerta del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street_extras" type="string (190)" %}
Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.city" type="string" required="true" %}
Nombre de la ciudad del domicilio del destinatario.

AR: localidad

CL: comuna

CO: ciudad

MX: ciudad

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.state" type="string" required="true" %}
Nombre del Estado del domicilio del destinatario.

AR: provincia

CL: región

CO: departamento

MX: estado

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.country" type="string" %}
Código ISO 3166-1 alfa-2 del país

Argentina: AR

Chile: CL

Colombia: CO

México: MX
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.zipcode" type="string" required="true" %}
Codigo postal de la dirección de destino.

Requerido solo en entregas a domicilio.

Dato no requerido en Chile y Colombia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.document" type="string (50)" required="true" %}
Número de documento (DNI, RUT, o lo que corresponda) del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.email" type="email (150)" required="true" %}
Dirección de correo electrónico del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.phone" type="string (50)" required="true" %}
Número de contacto del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items" type="array[Item]" required="true" %}
Array de objetos Item.&#x20;

Cada Item es un articulo a enviar.&#x20;

El máximo de artículos por envío es 1000.

Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.sku" type="string (190)" %}
Se intentará vincular a un producto cargado en el catálogo de Zippin.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.description" type="string (190)" %}
Descripción o nombre del artículo
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.weight" type="int" required="true" %}
Peso en gramos.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.length" type="int" required="true" %}
Largo del artículo, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.width" type="int" required="true" %}
Ancho del artículo, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.height" type="int" required="true" %}
Alto del artículo, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.classification_id" type="int" %}
ID de clasificación de producto.&#x20;

Si se omite se utilizará la clasificación General (1).

[Ver clasificaciones disponibles](../../referencia/clasificaciones-de-producto.md).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="type_packaging" type="string" %}
Indica la forma de empaquetar los productos. Si se omite se usa el que tenga definido la cuenta por default.\
Valores posible:\
`dynamic`: empaquetado dinámico\
`boxes`: usar cajas configuradas en la cuenta

`none`: no empaquetar (cada item debe resultar en un package separado)
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="El envío se crea correctamente." %}
```javascript
{
    // Detalle del envío creado
}
```


{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Un error en los datos impide crear el envío" %}
```javascript
{
    // El detalle indicará detalladamente el motivo del error.
}
```


{% endswagger-response %}
{% endswagger %}

#### Crear por Paquetes

Permite crear un envío indicando un conjunto de paquetes a despachar. Queda de tu lado la agrupación de productos en paquetes, si es que debes enviar más de un producto.

La utilización de múltiples paquetes sirve para cuando en un mismo envío debes despachar múltiples bultos (por ejemplo si estás enviando un colchón y un sommier).

{% swagger method="post" path="/shipments" baseUrl="/v2" summary="Crear envío (basado en packages)" %}
{% swagger-description %}
Este request te permitirá crear envíos indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="origin_id" type="int" required="true" %}
ID del origen del envío
{% endswagger-parameter %}

{% swagger-parameter in="body" name="logistic_type" type="string" %}
Código de la forma de despacho del envío.

Ejemplo: `carrier_dropoff`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="service_type" type="string" %}
Código de la forma de entrega del envío.

Ejemplo: `standard_delivery`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="carrier_id" type="int" %}
ID del transporte que hará la entrega.

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="string (100)" %}
Utilizado en algunas integraciones para identificar el canal de venta.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="external_id" type="string (30)" required="true" %}
ID del envío en tus sistemas. Es para tu propia referencia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado del envío. Monto que se asegurará. Para no utilizar seguro se debe enviar en cero.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" type="object" required="true" %}
Objeto con datos del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.point_id" type="int" %}
ID de la ubicación de entrega.

Requerido solo cuando la entrega es en un punto de entrega.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.name" type="string (100)" required="true" %}
Nombre y apellido del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street" type="string (100)" required="true" %}
Calle del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street_number" type="string (10)" required="true" %}
Altura/nro de puerta del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.street_extras (190)" type="string" %}
Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.city" type="string" required="true" %}
Nombre de la ciudad del domicilio del destinatario.

AR: localidad

CL: comuna

CO: ciudad

MX: ciudad

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.state" type="string" required="true" %}
Nombre del Estado del domicilio del destinatario.

AR: provincia

CL: región

CO: departamento

MX: estado

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.country" type="string" %}
Código ISO 3166-1 alfa-2 del país

Argentina: AR

Chile: CL

Colombia: CO

México: MX
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.zipcode" type="string" required="true" %}
Codigo postal de la dirección de destino.

Requerido solo en entregas a domicilio.

Dato no requerido en Chile y Colombia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.document" type="string (50)" required="true" %}
Número de documento (DNI, RUT, o lo que corresponda) del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.email" type="email (150)" required="true" %}
Dirección de correo electrónico del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination.phone" type="string (50)" required="true" %}
Número de contacto del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages" type="array[package]" required="true" %}
Array de objetos Package. &#x20;

El máximo de paquetes por envío es 99.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_1" type="string (60)" required="true" %}
Descripción del contenido (linea 1)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_2" type="string (60)" %}
Descripción del contenido (linea 2)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.description_3" type="string (60)" %}
Descripción del contenido (linea 3)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.weight" type="int" required="true" %}
Peso en gramos.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.length" type="int" required="true" %}
Largo del paquete, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.width" type="int" required="true" %}
Ancho del paquete, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.height" type="int" required="true" %}
Alto del paquete, en centímetros.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.classification_id" type="int" %}
ID de clasificación de producto del paquete.&#x20;

Si se omite se utilizará la clasificación General (1).

[Ver clasificaciones disponibles](../../referencia/clasificaciones-de-producto.md).
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="El envío se crea correctamente." %}
```javascript
{
    // Detalle del envío creado
}
```


{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Un error en los datos impide crear el envío" %}
```javascript
{
    // El detalle indicará detalladamente el motivo del error.
}
```


{% endswagger-response %}
{% endswagger %}

## Devoluciones

### Cotizar un envío de logística inversa

**El endpoint de cotización de una devolución nos permite obtener los precios y opciones para realizar la misma.**

En cada cotización, se deberá indicar la cuenta, valor declarado del envío y detalle de paquetes e items que se devolverán.

Al hacer el request **hay algunas particularidades** con respecto a la definición de los paquetes e items que los componen que serán devueltos.

* **Particularidades en la definición de paquetes a devolver**\
  En caso de no indicar el array de  **packages** con los paquetes que contendrá la devolución. El sistema interpreta que se devolverán todos los paquetes del envío con todos sus items definidos. Si el array de paquetes esta definido solo se cotizará la devolución de los paquetes que contenga dicho array.
* **Particularidades en la definición de items a devolver**\
  Para cada paquete que se indique en el array de paquetes se deberá indicar que items se devolverán. En caso de no indicar el array de  **items** con los items que contendrán los paquetes de la devolución. El sistema interpreta que se devolverán todos los items del paquete indicado. Si el array de items esta definido dentro de un paquete solo se cotizará la devolución de los items que contenga dicho array.

{% swagger method="post" path="/shipments/{id}/return/quote" baseUrl="/v2" summary="Cotizar devolución" %}
{% swagger-description %}
Este request te permitirá obtener cotizaciones para una devolución de un envío, indicando los ítems específicos que forman parte de la devolución.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages" type="array" required="true" %}
Array de objetos packages.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.id" type="int" required="true" %}
ID del paquete del envio original que se quiere devolver
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.items" type="array" %}
Opcionalmente, se puede indicar qué items de un paquete se quieren incluir en la devolucion. Si se omite, se asume que se quiere devolver el paquete completo
{% endswagger-parameter %}

{% swagger-parameter in="body" name="package.*.items.*.id" type="int" %}
ID de item del paquete
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del envío a cotizar una devolución
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Una respuesta correcta contendrá este detalle." %}
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
{% endswagger-response %}
{% endswagger %}

### Crear envío de logística inversa

**El endpoint de creación de una devolución nos permite crear la devolución, indicando todo el detalle del mismo y su contenido.**

En cada creación, se deberá indicar la cuenta, valor declarado del envío y detalle de paquetes e items que se devolverán. También se indicará el service\_type, logistic\_type que deben haber sido obtenidos previamente de una cotización.

Al hacer el request **hay algunas particularidades** con respecto a la definición de los paquetes e items que los componen que serán devueltos.

* **Particularidades en la definición de paquetes a devolver**\
  En caso de no indicar el array de  **packages** con los paquetes que contendrá la devolución. El sistema interpreta que se devolverán todos los paquetes del envío con todos sus items definidos. Si el array de paquetes esta definido solo se creará la devolución de los paquetes que contenga dicho array.
* **Particularidades en la definición de items a devolver**\
  Para cada paquete que se indique en el array de paquetes se deberá indicar que items se devolverán. En caso de no indicar el array de  **items** con los items que contendrán los paquetes de la devolución. El sistema interpreta que se devolverán todos los items del paquete indicado. Si el array de items esta definido dentro de un paquete solo se creará la devolución de los items que contenga dicho array.

{% swagger method="post" path="/shipments/{id}/return" baseUrl="/v2" summary="Crear devolución" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="declared_value" type="float" required="true" %}
Valor declarado total del contenido. Monto que se utilizará para el seguro. Si no se va a asegurar el contenido, se puede indicar el valor 0.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="service_type" type="string" required="true" %}
Código del tipo de servicio (sale de la cotización)

Ej. `return_origin`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="logistic_type" type="string" required="true" %}
Código del modo de despacho (sale de la cotización)

Ej. `carrier_dropoff`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="carrier_id" type="int" %}
Para indicar con qué transporte crear la devolución (sale de la cotización).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages" type="array" required="true" %}
Array de objetos packages.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.id" type="int" required="true" %}
ID del paquete del envio original que se quiere devolver
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.items" type="array" %}
Opcionalmente, se puede indicar qué items de un paquete se quieren incluir en la devolucion. Si se omite, se asume que se quiere devolver el paquete completo
{% endswagger-parameter %}

{% swagger-parameter in="body" name="package.*.items.*.id" type="int" %}
ID de item del paquete
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="int" required="true" %}
ID del envío a del cual crear una devolución
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Una respuesta correcta contendrá este detalle." %}
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
{% endswagger-response %}
{% endswagger %}

\


\
