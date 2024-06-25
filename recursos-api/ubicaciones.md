# Ubicaciones

## Puntos de despacho

Utiliza los siguientes recursos para obtener los puntos de despacho disponibles cerca de una ubicación determinada, por medio de “city” y “state” o “lat” (latitud) y “lng” (longitud).

{% hint style="info" %}
Ten en cuenta que no todos los puntos admiten todos los envíos. Según el punto puede que solo se acepten envíos de un transporte determinado, y que el tamaño o peso de los paquetes esté restringido por cuestiones de espacio u operacionales.
{% endhint %}

## Listado de puntos de despacho

<mark style="color:blue;">`GET`</mark> `/v2/locations`

En ésta búsqueda simple se puede indicar una ubicación, distancia, y restricciones. Al indicar la ubicación se debe hacer con city-state o bien latitud-longitud (no ambas).

#### Query Parameters

| Name                                    | Type    | Description                                                                                                                                                                                                                                       |
| --------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| city<mark style="color:red;">\*</mark>  | string  | <p><em>Requerido si está state</em> <strong>Nombre de la ciudad</strong> <br>AR: localidad (ej. Avellaneda<br>CL: comuna (ej. Las Condes)<br>CO: ciudad (ej. Usaquén)<br>MX: colonia (ej. Polanco)</p>                                            |
| state<mark style="color:red;">\*</mark> | string  | <p><em>Requerido si está city</em><br><strong>Nombre del Estado</strong> del domicilio del destinatario. <br>AR: provincia (ej. Buenos Aires)<br>CL: región (ej. RM)<br>CO: departamento (ej. Bogotá DC)<br>MX: estado (ej. Distrito Federal)</p> |
| lat<mark style="color:red;">\*</mark>   | decimal | <p><em>Requerido si está lng</em><br><strong>Latitud</strong> de la región o zona a buscar</p>                                                                                                                                                    |
| lng<mark style="color:red;">\*</mark>   | decimal | <p><em>Requerido si está lng</em><br><strong>Longitud</strong> de la región o zona a buscar</p>                                                                                                                                                   |
| radius                                  | int     | Radio de la zona de búsqueda para filtrado (en metros)                                                                                                                                                                                            |
| carrier\_id                             | int     | id del transporte a filtrar                                                                                                                                                                                                                       |
| total\_weight                           | int     | Peso total en gramos que acepte los carrier para el filtrado                                                                                                                                                                                      |
| total\_volume                           | int     | Volumen total en centímetros cúbicos que acepte el carrier para el filtrado                                                                                                                                                                       |
| package\_count                          | int     | Cantidad de paquetes del envío para el filtrado                                                                                                                                                                                                   |

{% tabs %}
{% tab title="200: OK Listado de ubicaciones" %}
<pre class="language-json"><code class="lang-json">GET /v2/locations?lat=-34.4728044&#x26;lng=-58.513802&#x26;radius=20000

<strong>[
</strong>	{
		"name": "Agente Oficial - Canga Mariano",
		"carrier": {
			"id": 208,
			"name": "OCA"
		},
		"default_logistic_type": "carrier_dropoff",
		"address": {
			"street": "Centenario",
			"street_number": "189",
			"street_extras": null,
			"city": "San Isidro",
			"state": "Buenos Aires",
			"geolocation": {
				"lat": -34.472654,
				"lng": -58.514024
			}
		},
		"restrictions": {
			"max_shipment_volume": null,
			"max_shipment_weight": 25000,
			"max_package_volume": null,
			"max_package_weight": 25000,
			"max_package_dimension_sum": 250,
			"max_package_dimension": 150,
			"max_package_count": 1
		}
	},
	{
		"name": "Correo Argentino - S Isidro Labrador Turismo",
		"carrier": {
			"id": 233,
			"name": "Correo Argentino"
		},
		"default_logistic_type": "carrier_dropoff",
		"address": {
			"street": "Av Centenario",
			"street_number": "189",
			"street_extras": null,
			"city": "San Isidro",
			"state": "Buenos Aires",
			"geolocation": {
				"lat": -34.4726539,
				"lng": -58.5140243
			}
		},
		"restrictions": {
			"max_shipment_volume": null,
			"max_shipment_weight": 25000,
			"max_package_volume": null,
			"max_package_weight": 25000,
			"max_package_dimension_sum": 250,
			"max_package_dimension": 150,
			"max_package_count": 1
		}
	}
]
</code></pre>
{% endtab %}
{% endtabs %}

## Búsqueda de puntos de despacho

<mark style="color:green;">`POST`</mark> `/v2/locations/search`

En ésta búsqueda avanzada se puede indicar una ubicación, distancia, y restricciones, con mayor nivel de detalle. Al indicar la ubicación se debe hacer con city-state o bien latitud-longitud (no ambas).

#### Request Body

| Name                                                 | Type    | Description                                                                                                                                                                                                                                       |
| ---------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| city<mark style="color:red;">\*</mark>               | string  | <p><em>Requerido si está state</em> <strong>Nombre de la ciudad</strong> <br>AR: localidad (ej. Avellaneda<br>CL: comuna (ej. Las Condes)<br>CO: ciudad (ej. Usaquén)<br>MX: colonia (ej. Polanco)</p>                                            |
| state<mark style="color:red;">\*</mark>              | string  | <p><em>Requerido si está city</em><br><strong>Nombre del Estado</strong> del domicilio del destinatario. <br>AR: provincia (ej. Buenos Aires)<br>CL: región (ej. RM)<br>CO: departamento (ej. Bogotá DC)<br>MX: estado (ej. Distrito Federal)</p> |
| lat<mark style="color:red;">\*</mark>                | decimal | <p><em>Requerido si está lng</em><br><strong>Latitud</strong> de la región o zona a buscar</p>                                                                                                                                                    |
| lng<mark style="color:red;">\*</mark>                | decimal | <p><em>Requerido si está lng</em><br><strong>Longitud</strong> de la región o zona a buscar</p>                                                                                                                                                   |
| radius                                               | int     | Radio de la zona de búsqueda para filtrado (en metros)                                                                                                                                                                                            |
| carrier\_id                                          | int     | ID del transporte a filtrar                                                                                                                                                                                                                       |
| packages<mark style="color:red;">\*</mark>           | array   | <p>Array de packages. </p><p>Mínimo un package</p>                                                                                                                                                                                                |
| packages.\*.width<mark style="color:red;">\*</mark>  | int     | Ancho del paquete, en cm.                                                                                                                                                                                                                         |
| packages.\*.weight<mark style="color:red;">\*</mark> | int     | Peso del paquete, en gramos                                                                                                                                                                                                                       |
| packages.\*.length<mark style="color:red;">\*</mark> | int     | Largo del paquete, en cm.                                                                                                                                                                                                                         |
| packages.\*.height<mark style="color:red;">\*</mark> | int     | Alto del paquete, en cm.                                                                                                                                                                                                                          |

{% tabs %}
{% tab title="200: OK Listado de ubicaciones" %}
<pre class="language-json"><code class="lang-json">curl --request POST \
  --url http://api.zippin.com.ar/v2/locations/search \
  --header 'Content-Type: application/json' \
  --data '{
	"lat": -34.466666666667,
	"lng": -58.516666666667,
	"radius": 25000,
	"packages": [
		{
			"weight": 50000,
			"height": 20,
			"length": 20,
			"width": 20
		}
	]
}'

<strong>[
</strong>	{
		"name": "Agente Oficial - Canga Mariano",
		"carrier": {
			"id": 208,
			"name": "OCA"
		},
		"default_logistic_type": "carrier_dropoff",
		"address": {
			"street": "Centenario",
			"street_number": "189",
			"street_extras": null,
			"city": "San Isidro",
			"state": "Buenos Aires",
			"geolocation": {
				"lat": -34.472654,
				"lng": -58.514024
			}
		},
		"restrictions": {
			"max_shipment_volume": null,
			"max_shipment_weight": 25000,
			"max_package_volume": null,
			"max_package_weight": 25000,
			"max_package_dimension_sum": 250,
			"max_package_dimension": 150,
			"max_package_count": 1
		}
	},
	{
		"name": "Correo Argentino - S Isidro Labrador Turismo",
		"carrier": {
			"id": 233,
			"name": "Correo Argentino"
		},
		"default_logistic_type": "carrier_dropoff",
		"address": {
			"street": "Av Centenario",
			"street_number": "189",
			"street_extras": null,
			"city": "San Isidro",
			"state": "Buenos Aires",
			"geolocation": {
				"lat": -34.4726539,
				"lng": -58.5140243
			}
		},
		"restrictions": {
			"max_shipment_volume": null,
			"max_shipment_weight": 25000,
			"max_package_volume": null,
			"max_package_weight": 25000,
			"max_package_dimension_sum": 250,
			"max_package_dimension": 150,
			"max_package_count": 1
		}
	}
]
</code></pre>
{% endtab %}
{% endtabs %}
