# Get the list of badges crm.activity.badge.list

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method retrieves a list of available badges. It will return an array containing all registered badges. Each element of the array contains [badge fields](./index.md#badge-record-fields).

## Method Parameters

No parameters.

## Code Examples

{% include [Example notes](../../../../../../_includes/examples.md) %}

{% list tabs %}
- cURL (Webhook)

- cURL (OAuth)

- JS
    ```js
    BX24.callMethod(
        "crm.activity.badge.list",
        {
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }    
);
    ```
- PHP

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "badges": [
            {
                "code": "missedCall",
                "title": "Call Status",
                "value": "Missed",
                "type": "failure"
            }
        ]
    },
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | The root element of the response, containing an array where each element carries information about a badge ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-add.md)
- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-delete.md)