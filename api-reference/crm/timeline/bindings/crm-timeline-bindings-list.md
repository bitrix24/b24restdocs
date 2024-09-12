# Get the list of bindings for a record in the timeline crm.timeline.bindings.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves the list of bindings for a record in the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering selected records.

The `OWNER_ID` field must be used; other fields are not necessary. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.list",
        {
            filter: {
                "OWNER_ID": 999,
            },
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
                if (result.more()) 
                {
                    result.next();
                }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.bindings.list',
        [
            'filter' => [
                'OWNER_ID' => 999,
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
    "result": [
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "deal"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "92",
            "ENTITY_TYPE": "company"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "205",
            "ENTITY_TYPE": "lead"
        }
    ],
    "total": 3,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+03:00",
        "date_finish": "2024-05-03T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response containing an array of objects with [information](crm-timeline-bindings-bind.md#parametr-fields) about the found bindings ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "OWNER_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | OWNER_ID is not defined or invalid | The required parameter `OWNER_ID` was not provided or the provided `OWNER_ID` is invalid. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-bindings-bind.md)
- [{#T}](./crm-timeline-bindings-unbind.md)
- [{#T}](./crm-timeline-bindings-fields.md)