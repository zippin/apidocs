# Inventario

## SKUs

## Listar SKUs

<mark style="color:blue;">`GET`</mark> `/v2/inventory/search`

#### Query Parameters

| Name           | Type   | Description                                                                                           |
| -------------- | ------ | ----------------------------------------------------------------------------------------------------- |
| account\_id    | int    | ID de la cuenta                                                                                       |
| sku            | string |                                                                                                       |
| barcode        | string |                                                                                                       |
| classification | int    | ID de clasificación. Ver [Clasificaciones de producto](../referencia/clasificaciones-de-producto.md). |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "data": [
        {
            "id": 9796,
            "account_id": 47,
            "internal_sku": "SOC0009796",
            "sku": "PATAX5RECTA",
            "barcode": "sd",
            "name": "Juego de patas X5 recta",
            "classification_id": 1,
            "attributes": {
                "unit_declared_value": 100,
                "weight": 2000,
                "length": 10,
                "width": 10,
                "height": 10
            },
            "total_qty_available": 97,
            "stocks": [
                {
                    "warehouse": "ba-2",
                    "qty_available": 97,
                    "qty_allocated": 0,
                    "qty_total": 97
                }
            ]
        },
        {
            "id": 9797,
            "account_id": 47,
            "internal_sku": "SOC0009797",
            "sku": "PAMAD",
            "barcode": "sd",
            "name": "Juego de patas X6 conica",
            "classification_id": 1,
            "attributes": {
                "unit_declared_value": 100,
                "weight": 900,
                "length": 10,
                "width": 20,
                "height": 8
            },
            "total_qty_available": 44,
            "stocks": [
                {
                    "warehouse": "ba-2",
                    "qty_available": 44,
                    "qty_allocated": 0,
                    "qty_total": 44
                }
            ]
        }
    ],
    "links": {
        "first": "https://api.zippin.com.ar/v2/inventory/search?account_id=47&page=1",
        "last": "https://api.zippin.com.ar/v2/inventory/search?account_id=47&page=2",
        "prev": null,
        "next": "https://api.zippin.com.ar/v2/inventory/search?account_id=47&page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 2,
        "path": "https://api.zippin.com.ar/v2/inventory/search",
        "per_page": "40",
        "to": 40,
        "total": 75
    }
}
```
{% endtab %}
{% endtabs %}

## Detalle de un SKU

<mark style="color:blue;">`GET`</mark> `/v2/inventory/{sku_id}`

Obtiene un detalle de stock por cada almacén en el que hayan existencias.

#### Path Parameters

| Name                                      | Type | Description |
| ----------------------------------------- | ---- | ----------- |
| sku\_id<mark style="color:red;">\*</mark> | int  | ID del sku  |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "id": 9737,
    "account_id": 47,
    "internal_sku": "SOC0009737",
    "sku": "137006",
    "barcode": "7796442057939",
    "name": "Colchón de espuma 160x200",
    "classification_id": 2,
    "attributes": {
        "unit_declared_value": 2000,
        "weight": 40000,
        "length": 200,
        "width": 160,
        "height": 25
    },
    "total_qty_available": 10,
    "stocks": [
        {
            "warehouse": "ba-2",
            "qty_available": 10,
            "qty_allocated": 0,
            "qty_total": 10
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## Movimientos de inventario

<mark style="color:blue;">`GET`</mark> `/v2/inventory/{sku_id}/movements`

#### Path Parameters

| Name                                      | Type | Description |
| ----------------------------------------- | ---- | ----------- |
| sku\_id<mark style="color:red;">\*</mark> | int  | ID del SKU  |

#### Query Parameters

| Name       | Type       | Description                                                                                 |
| ---------- | ---------- | ------------------------------------------------------------------------------------------- |
| warehouse  | int        | Codigo de almacen. Ej. `ba-2`                                                               |
| date\_from | yyyy-mm-dd | Fecha de inicio para el filtrado de movimientos. Si se omite, el default es 180 días atrás. |
| date\_to   | yyyy-mm-dd | Fecha de fin para el filtrado de movimientos. Por default es hoy.                           |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "data": [
        {
            "id": 882,
            "created_at": "2019-11-27T11:52:19+0000",
            "sku": {
                "id": 9737,
                "sku": "137006",
                "internal_sku": "SOC0009737",
                "account_id": 47
            },
            "warehouse": "ba-2",
            "movement": {
                "qty_changed": 7,
                "type": "IN",
                "reason": "Ingreso por recepcion",
                "comments": null
            }
        },
        {
            "id": 861,
            "created_at": "2019-11-27T11:51:58+0000",
            "sku": {
                "id": 9737,
                "sku": "137006",
                "internal_sku": "SOC0009737",
                "account_id": 47
            },
            "warehouse": "ba-2",
            "movement": {
                "qty_changed": 3,
                "type": "IN",
                "reason": "Ingreso por recepcion",
                "comments": null
            }
        }
    ],
    "links": {
        "first": "https://api.zippin.com.ar/v2/inventory/9737/movements?warehouse=ba-2&page=1",
        "last": "https://api.zippin.com.ar/v2/inventory/9737/movements?warehouse=ba-2&page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://api.zippin.com.ar/v2/inventory/9737/movements",
        "per_page": 20,
        "to": 2,
        "total": 2
    }
}
```
{% endtab %}
{% endtabs %}
