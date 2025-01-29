# Get a List of Process Stages rpa.stage.listForType

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves a list of process stages, sorted in order with final stages at the end.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **typeId***  
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **start**  
[`integer`](../../../data-types.md) | This parameter is used for pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` is the desired page number  ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Response Handling

HTTP Status: **200**

The information about each stage will only include basic data, without `tasks`, `robotsCount`, `possibleNextStages`, `permissions`.

```json
{
    "stages": [
        {
            "id": 1,
            "name": "Launch",
            "code": "",
            "color": "22B9FF",
            "sort": 1000,
            "semantic": null,
            "typeId": 1,
            "isFirst": true,
            "isSuccess": false,
            "isFail": false
        }
    ]
}
```

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-stage-add.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-get.md)
- [{#T}](./rpa-stage-delete.md)