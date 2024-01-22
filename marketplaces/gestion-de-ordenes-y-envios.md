# Gestión de ordenes y envíos

Una vez que se cree una orden de marketplace, se creará, luego de unos segundos y en forma automática, un envío en Zippin a partir de ella.

{% swagger method="get" path="/orders/{order_id}" baseUrl="/v2" summary="Detalle de Orden" %}
{% swagger-description %}
Obtiene el detalle de una orden
{% endswagger-description %}

{% swagger-parameter in="path" name="order_id" type="int" %}
Es el id de orden obtenido al crearla.
{% endswagger-parameter %}
{% endswagger %}



{% swagger method="get" path="/orders/{order_id}/shipments" baseUrl="/v2" summary="Listado de envíos de una orden" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}
