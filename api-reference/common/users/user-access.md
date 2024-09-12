# Define the user access rights user.access

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- A more detailed example of obtaining results in JS
- Examples in other languages
- It's unclear which specific rights are being checked; the example does not answer this question. A list of possible types of rights with explanations is needed.
- A more informative example of a successful response is needed — what will it look like if the user has one right but not the other?
- Possible error codes are needed.

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.access` reports whether the current user has at least one of the specified rights.

#|
|| **Parameter** | **Description** ||
|| **ACCESS**
[`integer`](../../data-types.md) or [`array`](../../data-types.md) | Identifier or list of identifiers of the rights to check access for. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}


## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{ "ACCESS": "[U22, U33]", "auth": "d161f25928c3184678924ec127edd29a" }' \
    user.access.json
    ```

- JS

    ```js
    BX24.callMethod('user.access', {ACCESS:['U22','U33']});
    // A clearer example with result handling is needed.

    ```

- PHP

    ```php
    // An example is needed.
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
    "result": false,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+03:00",
        "date_finish": "2024-01-20T18:35:37+03:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
[`boolean`](../../data-types.md)| true or false depending on the presence of the requested rights ||
|| **time**
[`array`](../../data-types.md) | Information about the request execution time ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the request initialization moment ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the request completion moment ||
|| **duration**
[`double`](../../data-types.md) | How long in milliseconds the request took (finish - start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the request initialization moment ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the request completion moment ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of when the REST API resource limit will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds will the REST API resource limit be reset? Read more in the article [operation limits](../../../limits.md) ||
|#

## Error Response

> 200, 50x Error

```json
{
    "error": "TITLE_EMPTY",
    "error_description": "The deal title is required"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **TITLE_EMPTY** | The required field values are not set || 
|| **WRONG_REQUEST** | The parameters of the request were unable to interpret || 
|#