# Get Description of CRM Operating Modes crm.enum.settings.mode

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

Returns a description of the CRM operating modes.

## Method Parameters

The method is called without parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    We will insert the necessary code here, regenerated from your JS example.

- cURL (OAuth)

    We will insert the necessary code here, regenerated from your JS example.

- JS

    ```js
    BX24.callMethod("crm.enum.settings.mode", result => {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    });
    ```

- PHP

    We will insert the necessary code here, regenerated from your JS example.

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

We will fill in the content of this block later. Or we will remove the block if it is unnecessary.

{% endnote %}

## Response Handling

HTTP Status: **200**

```json
{
     "result": [
        {
            "ID": 1,
            "NAME": "Classic CRM",
        },
        {
            "ID": 2,
            "NAME": "Simple CRM",
        }
    ],
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
[`object`](../../../data-types.md) | Returns an array of available operating modes for the CRM: **1: Classic CRM** and **2: Simple CRM**   ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

We will fill in this block later, but if you have recommendations on which methods or documentation pages should be mentioned here, we would appreciate it.