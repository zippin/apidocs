# Ubicaciones

## Puntos de despacho

Utiliza los siguientes recursos para obtener los puntos de despacho disponibles cerca de una ubicación determinada, por medio de “city” y “state” o “lat” (latitud) y “lng” (longitud).

{% hint style="info" %}
Ten en cuenta que no todos los puntos admiten todos los envíos. Según el punto puede que solo se acepten envíos de un transporte determinado, y que el tamaño o peso de los paquetes esté restringido por cuestiones de espacio u operacionales.
{% endhint %}

{% swagger method="get" path="/locations" baseUrl="/v2" summary="Listado de puntos de despacho" %}
{% swagger-description %}
En ésta búsqueda simple se puede indicar una ubicación, distancia, y restricciones. Al indicar la ubicación se debe hacer con city-state o bien latitud-longitud (no ambas).
{% endswagger-description %}

{% swagger-parameter in="query" name="city" type="string" required="true" %}
_Requerido si está state_

 

**Nombre de la ciudad**

 

\


AR: localidad (ej. Avellaneda

\


CL: comuna (ej. Las Condes)

\


CO: ciudad (ej. Usaquén)

\


MX: colonia (ej. Polanco)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state" type="string" required="true" %}
_Requerido si está city_

\




**Nombre del Estado**

 del domicilio del destinatario. 

\


AR: provincia (ej. Buenos Aires)

\


CL: región (ej. RM)

\


CO: departamento (ej. Bogotá DC)

\


MX: estado (ej. Distrito Federal)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="lat" required="true" type="decimal" %}
_Requerido si está lng_

\




**Latitud**

 de la región o zona a buscar
{% endswagger-parameter %}

{% swagger-parameter in="query" name="lng" required="true" type="decimal" %}
_Requerido si está lng_

\




**Longitud**

 de la región o zona a buscar
{% endswagger-parameter %}

{% swagger-parameter in="query" name="radius" type="int" %}
Radio de la zona de búsqueda para filtrado (en metros)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="carrier_id" type="int" %}
id del transporte a filtrar
{% endswagger-parameter %}

{% swagger-parameter in="query" type="int" name="total_weight" %}
Peso total en gramos que acepte los carrier para el filtrado
{% endswagger-parameter %}

{% swagger-parameter in="query" name="total_volume" type="int" %}
Volumen total en centímetros cúbicos que acepte el carrier para el filtrado
{% endswagger-parameter %}

{% swagger-parameter in="query" name="package_count" type="int" %}
Cantidad de paquetes del envío para el filtrado
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Listado de ubicaciones" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/locations/search" baseUrl="/v2" summary="Búsqueda de puntos de despacho" %}
{% swagger-description %}
En ésta búsqueda avanzada se puede indicar una ubicación, distancia, y restricciones, con mayor nivel de detalle. Al indicar la ubicación se debe hacer con city-state o bien latitud-longitud (no ambas).
{% endswagger-description %}

{% swagger-parameter in="body" name="lat" required="true" type="decimal" %}
_Requerido si está lng_

\




**Latitud**

 de la región o zona a buscar
{% endswagger-parameter %}

{% swagger-parameter in="body" name="lng" required="true" type="decimal" %}
_Requerido si está lng_

\




**Longitud**

 de la región o zona a buscar
{% endswagger-parameter %}

{% swagger-parameter in="body" name="city" type="string" required="true" %}
_Requerido si está state_

 

**Nombre de la ciudad**

 

\


AR: localidad (ej. Avellaneda

\


CL: comuna (ej. Las Condes)

\


CO: ciudad (ej. Usaquén)

\


MX: colonia (ej. Polanco)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="state" type="string" required="true" %}
_Requerido si está city_

\




**Nombre del Estado**

 del domicilio del destinatario. 

\


AR: provincia (ej. Buenos Aires)

\


CL: región (ej. RM)

\


CO: departamento (ej. Bogotá DC)

\


MX: estado (ej. Distrito Federal)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="radius" type="int" %}
Radio de la zona de búsqueda para filtrado (en metros)
{% endswagger-parameter %}

{% swagger-parameter in="query" type="int" name="total_weight" %}
Peso total en gramos que acepte los carrier para el filtrado
{% endswagger-parameter %}

{% swagger-parameter in="body" name="carrier_id" type="int" %}
id del transporte a filtrar
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages" type="array" required="true" %}
Array de packages.&#x20;

Mínimo un package
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.weight" type="int" required="true" %}
Peso del paquete, en gramos
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.height" type="int" %}
Alto del paquete, en cm.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.length" type="int" %}
Largo del paquete, en cm.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="packages.*.width" type="int" %}
Ancho del paquete, en cm.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Listado de ubicaciones" %}
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
{% endswagger-response %}
{% endswagger %}
