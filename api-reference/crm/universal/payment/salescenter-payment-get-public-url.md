# Generate a link for a specific payment salescenter.payment.getPublicUrl

> Scope: [`salescenter`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method generates a link for a specific payment. The payment method selected will be passed to this particular payment.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1063}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/salescenter.payment.getPublicUrl
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1063,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/salescenter.payment.getPublicUrl
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'salescenter.payment.getPublicUrl', {
    			id: 1063,
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'salescenter.payment.getPublicUrl',
                [
                    'id' => 1063,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting public URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'salescenter.payment.getPublicUrl', {
            id: 1063,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'salescenter.payment.getPublicUrl',
        [
            'id' => 1063
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
    "result": {
        "payment": {
            "url": "http:\/\/account.home\/pub\/site\/10\/oformleniezakaza\/?orderId=3689\u0026access=f83b5926bd8cb4f0248b4b5a8e48b6fb\u0026paymentId=1063",
            "shortUrl": "http:\/\/account.home\/~tR9sB",
            "qr": "iVBORw0KGgoAAAANSUhEUgAAAXwAAAF8CAYAAADM5wDKAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAHaElEQVR4nO3csW7jVhRF0SjQ\/\/+y0owFN4GVeYou7+y1KhdjiCI5G8\/NuT0ej8dfAPzx\/p6+AAA+4\/71w+12m7wOXnDyx9jJ8536I7D2Tm78Y7v2jDb6\/l454QNECD5AhOADRAg+QITgA0QIPkCE4ANECD5AhOADRAg+QITgA0QIPkCE4ANECD5AxP3nf\/KzjbOuUzbOyU5d88Y56Kl7tXH+Wjde9673ygkfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASLeMo98YuNc8MZZ16lrPnm+tanhjRPHU3Tj9zjhA0QIPkCE4ANECD5AhOADRAg+QITgA0QIPkCE4ANECD5AhOADRAg+QITgA0QIPkDE+Dwy\/JuNk85wZU74ABGCDxAh+AARgg8QIfgAEYIPECH4ABGCDxAh+AARgg8QIfgAEYIPECH4ABGCDxBhHjli4+TvyTWfTCuf\/O7UNcMrnPABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIGJ8Htkk7PVNPaOpSWdT0te38ZqvwAkfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASLeMo+8cU6Wz5ia7d34uTXu1ec54QNECD5AhOADRAg+QITgA0QIPkCE4ANECD5AhOADRAg+QITgA0QIPkCE4ANECD5AxHMe+WQSFv4Ptfncjd9XN3ZxwgeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyAiNvj176padbXTd2rk+\/r+f7ZNj7fKeVuOOEDRAg+QITgA0QIPkCE4ANECD5AhOADRAg+QITgA0QIPkCE4ANECD5AhOADRAg+QMT964ep6V2Tv6\/b+H03cp+vb2OvrsAJHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEinvPItYnjqZnTjdO7G+\/VyTVv\/L4bbZwa3t5JJ3yACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiLg9Nm6U\/mJa+XVT06yLXy9eYNJ5Fyd8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4i4f\/2wceZ040wxr6vNX5+oTVhvvOYT73q+TvgAEYIPECH4ABGCDxAh+AARgg8QIfgAEYIPECH4ABGCDxAh+AARgg8QIfgAEYIPEPGcR944N7pxEnbjfZ5ycq+mppU3TmdvvOaNrnCfnfABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIELwASIEHyBC8AEiBB8gQvABIgQfIOI5jzw13Tk1Fzw1rbxx0rlm47txYuM1X2Fq+L+6wn12wgeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyAiNtjeHO3NnM6ZeN9nrJxsvvExveZ3+OEDxAh+AARgg8QIfgAEYIPECH4ABGCDxAh+AARgg8QIfgAEYIPECH4ABGCDxAh+AAR968fzOd+xsl93jhju3FqeOoZnfzu1PfdaOM7+a5rdsIHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIEHyACMEHiBB8gAjBB4gQfIAIwQeIuD1qe64AUU74ABGCDxDxD3Dwaiq9kYVcAAAAAElFTkSuQmCC"
        }
    },
    "time": {
        "start": 1718372619.163703,
        "finish": 1718372619.567831,
        "duration": 0.4041280746459961,
        "processing": 0.15733885765075684,
        "date_start": "2024-06-14T16:43:39+02:00",
        "date_finish": "2024-06-14T16:43:39+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **url**
[`string`](../../../data-types.md) | Payment link ||
|| **shortUrl**
[`string`](../../../data-types.md) | Short payment link ||
|| **qr**
[`string`](../../../data-types.md) | QR code containing the short payment link ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200640400001,
    "error_description": "payment does not exist"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200640400001` | Payment not found ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-add.md)
- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-get.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-delete.md)