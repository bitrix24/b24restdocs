# Update Payment System sale.paysystem.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method updates the payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **ID*** 
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system ||
|| **FIELDS** 
[`object`](../data-types.md) | Object containing new field values (detailed description provided [below](#parametr-fields)) ||
|#

### FIELDS Parameter

#| 
|| **Name**
`type` | **Description** ||
|| **NAME** 
[`string`](../data-types.md) | Name of the payment system ||
|| **DESCRIPTION** 
[`string`](../data-types.md) | Description of the payment system ||
|| **PERSON_TYPE_ID** 
[`sale_person_type.id`](../sale/data-types.md) | Identifier of the payer type ||
|| **BX_REST_HANDLER** 
[`sale_paysystem_handler.CODE`](../sale/data-types.md#sale_paysystem_handler) | Code of the REST handler specified when adding the handler using the [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) method ||
|| **ACTIVE** 
[`string`](../data-types.md) | Indicator of the payment system's activity. Possible values:
- `Y` — yes
- `N` — no
||
|| **LOGOTYPE** 
[`string`](../data-types.md) | Logo of the payment system (image in Base64 format) ||
|| **NEW_WINDOW** 
[`string`](../data-types.md) | Flag for the "Open in new window" setting. Possible values:
- `Y` — yes
- `N` — no
||
|| **XML_ID** 
[`string`](../data-types.md) | External identifier of the payment system ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"FIELDS":{"NAME":"New Payment System Name","DESCRIPTION":"New Payment System Description","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","NEW_WINDOW":"N","LOGOTYPE":"/* base64 image */"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"FIELDS":{"NAME":"New Payment System Name","DESCRIPTION":"New Payment System Description","PERSON_TYPE_ID":1,"BX_REST_HANDLER":"resthandlercode","ACTIVE":"Y","NEW_WINDOW":"N","LOGOTYPE":"/* base64 image */"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sale.paysystem.update',
            {
                "ID": 12,
                "FIELDS": {
                    "NAME": "New Payment System Name",
                    "DESCRIPTION": "New Payment System Description",
                    "PERSON_TYPE_ID": 1,
                    "BX_REST_HANDLER": 'resthandlercode',
                    "ACTIVE": 'Y',
                    'NEW_WINDOW': 'N',
                    'LOGOTYPE': '/* base64 image */',
                }
            }
        );
        
        const result = response.getData().result;
        console.info(result);
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
                'sale.paysystem.update',
                [
                    'ID'          => 12,
                    'FIELDS'      => [
                        'NAME'           => 'New Payment System Name',
                        'DESCRIPTION'    => 'New Payment System Description',
                        'PERSON_TYPE_ID' => 1,
                        'BX_REST_HANDLER' => 'resthandlercode',
                        'ACTIVE'         => 'Y',
                        'NEW_WINDOW'     => 'N',
                        'LOGOTYPE'       => '/* base64 image */',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating payment system: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sale.paysystem.update',
    {
        "ID": 12,
        "FIELDS": {
            "NAME": "New Payment System Name",
            "DESCRIPTION": "New Payment System Description",
            "PERSON_TYPE_ID": 1,
            "BX_REST_HANDLER": 'resthandlercode',
            "ACTIVE": 'Y',
            'NEW_WINDOW': 'N',
            'LOGOTYPE': '/* base64 image */',
        }
    }, 
        function(result) 
        { 
            if(result.error()) 
            { 
                console.error(result.error()); 
            } 
            else 
            { 
                console.info(result.data()); 
            } 
        } 
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.update',
        [
            'ID' => 12,
            'FIELDS' => [
                'NAME' => 'New Payment System Name',
                'DESCRIPTION' => 'New Payment System Description',
                'PERSON_TYPE_ID' => 1,
                'BX_REST_HANDLER' => 'resthandlercode',
                'ACTIVE' => 'Y',
                'NEW_WINDOW' => 'N',
                'LOGOTYPE' => '/* base64 image */',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../data-types.md) | Result of the payment system update ||
|| **time** 
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_NOT_FOUND",
    "error_description": "Handler not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Access denied. The application is trying to modify a payment system added by another application, or insufficient rights to modify the payment system | 403 ||
|| `ERROR_HANDLER_NOT_FOUND` | The handler specified in the `BX_REST_HANDLER` parameter was not found | 400 ||
|| `ERROR_PERSON_TYPE_NOT_FOUND` | The payer type specified in the `PERSON_TYPE_ID` parameter was not found | 400 ||
|| `ERROR_PAY_SYSTEM_NOT_FOUND` | The payment system with the specified `ID` was not found | 400 ||
|| `ERROR_CHECK_FAILURE` | The `ID` parameter is not specified | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)