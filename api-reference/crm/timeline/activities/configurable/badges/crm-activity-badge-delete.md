# Delete Badge by Code crm.activity.badge.delete

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: `users with administrative access to the crm section`

This method deletes a badge.

## Method Parameters

{% include [Note on Required Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code***  
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall` ||
|#

## Code Examples

{% include [Note on Examples](../../../../../../_includes/examples.md) %}

{% list tabs %}
- cURL (Webhook)

- cURL (OAuth)

- JS
    ```js
    BX24.callMethod(
        "crm.activity.badge.delete",
        {
            code: 'missedCall',
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

HTTP Status: **200**

```json
{
    "result": true,
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
[`boolean`](../../../../data-types.md) | Root element of the response. Contains:
- `true` — on success
- `null` — on failure (an error occurred)
||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `NOT_FOUND` | Badge with the specified code not found ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-add.md)
- [{#T}](./crm-activity-badge-list.md)
- [{#T}](./crm-activity-badge-get.md)