# Cotización y creación de ordenes

La cotización de envíos funciona igual que cualquier otra cotización. La única diferencia es que la autenticación se debe hacer con el access token, en vez de credenciales de la cuenta, y que luego, **en vez de crear un envío, se deberá crear una Orden de Marketplace**.

## Cotización

Revisa la [documentación de la cotización](../api-docs/recursos-api/cotizar-y-crear-envios.md).

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

{% swagger method="post" path="/orders" baseUrl="/v2" summary="Crear orden de marketplace" %}
{% swagger-description %}
Este request te permitirá crear ordenes de marketplace indicando los articulos que son parte del despacho. El sistema los agrupará en paquetes automáticamente, según las reglas definidas en la cuenta.
{% endswagger-description %}

{% swagger-parameter in="body" name="account_id" type="int" required="true" %}
ID de la cuenta del seller
{% endswagger-parameter %}

{% swagger-parameter in="body" name="external_id" type="string (30)" required="true" %}
ID del envío en tus sistemas. Es para tu propia referencia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="channel" type="string" required="true" %}
Identificación del canal. En caso de marketplace, utiliza el prefijo `marketplace:`

Por ejemplo: `marketplace:amazon`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="string (150)" required="true" %}
Utilizado para identificar la integración.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="created_at" required="true" type="timestamp" %}
Fecha y hora de generación de la venta (ISO8601)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="total_amount" type="float" required="true" %}
Monto total de la venta
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paid_amount" type="float" required="true" %}
Monto total de la venta pagado por el comprador
{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping_paid_amount" type="float" required="true" %}
Monto correspondiente al envío pagado por el comprador
{% endswagger-parameter %}

{% swagger-parameter in="body" name="currency" type="string" %}
Código de divisa\
Ej. ARS, CLP, MXN.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping" type="object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping.origin_id" type="int" required="true" %}
ID del origen del envío
{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping.logistic_type" type="string" %}
Código de la forma de despacho del envío.

Ejemplo: `carrier_dropoff`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping.service_type" type="string" %}
Código de la forma de entrega del envío.

Ejemplo: `standard_delivery`

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización utilizar la opcion de envio deseada
{% endswagger-parameter %}

{% swagger-parameter in="body" name="shipping.carrier_id" type="int" %}
ID del transporte que hará la entrega.

Se recomienda explicitarlo basado en los resultados obtenidos en la cotización.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery" type="object" required="true" %}
Objeto con datos de la entrega
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.point_id" type="int" %}
ID de la ubicación de entrega.

Requerido solo cuando la entrega es en un punto de entrega.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.recipient.name" type="string (100)" required="true" %}
Nombre y apellido del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.recipient.document" type="string (50)" required="true" %}
Número de documento (DNI, RUT, o lo que corresponda) del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.recipient.email" type="email (150)" required="true" %}
Dirección de correo electrónico del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.recipient.phone" type="string (50)" required="true" %}
Número de contacto del destinatario
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.street" type="string (100)" required="true" %}
Calle del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.street_number" type="string (10)" required="true" %}
Altura/nro de puerta del domicilio de entrega.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.street_extras" type="string (190)" %}
Referencias adicionales del domicilio como piso, departamento, nro de lote, etc.

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.city" type="string" required="true" %}
Nombre de la ciudad del domicilio del destinatario.

AR: localidad

CL: comuna

CO: ciudad

MX: ciudad

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.state" type="string" required="true" %}
Nombre del Estado del domicilio del destinatario.

AR: provincia

CL: región

CO: departamento

MX: estado

Requerido solo en entregas a domicilio.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.country" type="string" %}
Código ISO 3166-1 alfa-2 del país

Argentina: AR

Chile: CL

Colombia: CO

México: MX
{% endswagger-parameter %}

{% swagger-parameter in="body" name="delivery.address.zipcode" type="string" required="true" %}
Codigo postal de la dirección de destino.

Requerido solo en entregas a domicilio.

Dato no requerido en Chile y Colombia.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items" type="array[Item]" required="true" %}
Array de objetos Item.&#x20;

Cada Item es un articulo a enviar.&#x20;

El máximo de artículos por envío es 1000.

Ten en cuenta que más allá de la cantidad de items, un envío no puede resultar en más de 99 paquetes.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.sku" type="string (190)" required="true" %}
Se intentará vincular a un producto cargado en el catálogo de Zippin.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.name" type="string (190)" required="true" %}
Descripción o nombre del artículo
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.qty" type="int" %}
Cantidad vendida del item
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.unit_price" type="float" %}
Precio unitario del producto
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items.*.currency" type="string" %}
Código de divisa\
Ej. ARS, CLP, MXN.
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

{% swagger-response status="201: Created" description="La orden se crea correctamente." %}
```javascript
{
    // Detalle del orden creado
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

