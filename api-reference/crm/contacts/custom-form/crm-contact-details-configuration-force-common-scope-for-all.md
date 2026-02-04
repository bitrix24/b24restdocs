# Set Common Contact Card for All Users crm.contact.details.configuration.forceCommonScopeForAll

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: Administrator

{% note warning "Method Development Stopped" %}

The method `crm.contact.details.configuration.forceCommonScopeForAll` continues to function, but there is a more relevant alternative [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md).

{% endnote %}

This method allows you to forcibly set a common contact card for all users.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.forceCommonScopeForAll
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.forceCommonScopeForAll
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.contact.details.configuration.forceCommonScopeForAll',
            {}
        );
        
        const result = response.getData().result;
        result.error()
            ? console.error(result.error())
            : console.info(result)
        ;
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.contact.details.configuration.forceCommonScopeForAll',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.contact.details.configuration.forceCommonScopeForAll: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.contact.details.configuration.forceCommonScopeForAll',
        {},
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.details.configuration.forceCommonScopeForAll',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1724671860.18392,
        "finish": 1724671860.843895,
        "duration": 0.6599750518798828,
        "processing": 0.09691596031188965,
        "date_start": "2024-08-26T13:31:00+02:00",
        "date_finish": "2024-08-26T13:31:00+02:00",
        "operating": 0
    }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response.

Returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#


## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**   | **Value** ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-contact-details-configuration-get.md)
- [{#T}](./crm-contact-details-configuration-set.md)
- [{#T}](./crm-contact-details-configuration-reset.md)