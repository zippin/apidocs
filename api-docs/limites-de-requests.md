---
description: >-
  Algunos endpoints aplican diferentes políticas de límite de requests (rate
  limiting).
---

# Límites de requests

La API de Zippin define cuatro niveles de límite a la cantidad de requests por minuto que acepta el servidor. Cada uno de ellos tiene un máximo de requests admitidos por minuto, y un tiempo específico en que el consumo es liberado en caso de superar el máximo permitido.

<table><thead><tr><th width="136">Nivel</th><th width="170">Límite</th><th width="119">Liberación</th><th>Impacta en</th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f534">🔴</span> <mark style="color:red;"><strong>Alto</strong></mark></td><td>100 reqs/minuto</td><td>3 minutos</td><td>Creación de envíos</td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f7e0">🟠</span> <mark style="color:orange;"><strong>Medio</strong></mark></td><td>500 reqs/minuto</td><td>2 minutos</td><td>Cotización de envíos</td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f7e1">🟡</span> <mark style="color:yellow;"><strong>Bajo</strong></mark></td><td>1000 reqs/minuto</td><td>1 minuto</td><td>Obtención de envíos y tracking</td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f7e2">🟢</span> <mark style="color:green;"><strong>Nulo</strong></mark></td><td>Ninguno</td><td></td><td>Resto de los endpoints</td></tr></tbody></table>

Actualmente, los límites **son aplicados por IP**.

### Monitorear el consumo de requests

Cuando hagas requests a un endpoint limitado, notarás que aparecen los siguientes headers en la respuesta:

* `X-RateLimit-Limit` indica el límite de requests para el endpoint.
* `X-RateLimit-Remaining` indica cuántos requests disponibles tienes actualmente.

Si consumieras todos los requests permitidos en el minuto, verás que aparecen otros headers adicionales en la respuesta:

* `Retry-After` indica cuantos segundos faltan para que se resetee la disponibilidad de requests.
* `X-RateLimit-Reset` es un timestamp UNIX de cuándo reseteará la disponibilidad de requests.
