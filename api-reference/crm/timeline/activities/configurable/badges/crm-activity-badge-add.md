# Add Badge crm.activity.badge.add

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: `users with administrative access to the crm section`

This method adds a new badge for a configurable deal.

## Method Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code***  
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall` ||
|| **title***  
[`string`\|`array`](../../../../../data-types.md) | Title of the badge. Can be either a string or an array of strings for different languages ||
|| **value***  
[`string`\|`array`](../../../../../data-types.md) | Value of the badge. Can be either a string or an array of strings for different languages ||
|| **type***  
[`string`](../../../../../data-types.md) | [Type of the badge](./index.md#tip-bejdzha) ||
|#

## Code Examples

{% include [Note on examples](../../../../../../_includes/examples.md) %}

{% list tabs %}
- cURL (Webhook)

- cURL (OAuth)

- JS
    ```js
    BX24.callMethod(
        "crm.activity.badge.add",
        {
            code: 'missedCall',
            title: 'Call Status',
            value: 'Missed',
            type: 'failure'
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
        "badge": {
            "code": "missedCall",
            "title": "Call Status",
            "value": "Missed",
            "type": "failure"
        }
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
[`object`](../../../../data-types.md) | Root element of the response containing information about the added badge in case of success. In case of failure, it will return `null` ||
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
|| `REQUIRED_ARG_MISSING` | Required fields are not filled ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-list.md)
- [{#T}](./crm-activity-badge-delete.md)