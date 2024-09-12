# Delete Lead crm.lead.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.delete` removes a lead and all associated objects, such as links to other entities, lead history, timeline records, etc.

#|
|| **Parameter** | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Integer identifier of the lead. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{ "id": "123" }' \
    https://xxx.bitrix24.com/rest/crm.lead.delete
    ```

- JS

    ```javascript 
    const id = prompt("Enter ID");
    BX24.callMethod(
      'crm.lead.delete',
      { id },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
  
          return;
        }
        
        console.info(result.data());
      }
);
    ```

- PHP

    ```php
    $id = 123;
        
    $result = CRest::call(
        'crm.lead.delete',
        [
            'id' => $id,
        ]
    );
    ```

- HTTPS

    ```http
    https://xxx.bitrix24.com/rest/1/5***/crm.lead.delete.json?id=123
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
    "result": true,
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
`boolean`| Result of the request ||
|| **time**
[`array`](../../data-types.md) | Information about the execution time of the request ||
|| **start**
[`double`](../../data-types.md) | Timestamp of when the request was initiated ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of when the request was completed ||
|| **duration**
[`double`](../../data-types.md) | How long the request took in milliseconds (finish - start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time when the request was initiated ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time when the request was completed ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | How many milliseconds until the limit on REST API resources will be reset? Read more in the article [operation limits](../../../limits.md) ||
|#

## Example Response in Case of Error

> 40x, 50x Error

```json
{
  "error": "",
  "error_description": "Lead #123: insufficient permissions to delete"
}
```