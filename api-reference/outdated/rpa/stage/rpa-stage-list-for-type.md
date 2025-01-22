# Get the list of process stages rpa.stage.listForType

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.stage.listForType` will return a list of process stages sorted in order, with final stages at the end.

#|
|| **Name**
`type` | **Description** ||
|| **typeId**^*^ 
[`number`](../../../data-types.md) | Process identifier. ||
|| **start** | Offset for pagination. ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

## Response on success

> 200 OK

The information for each stage will include only basic data, without `tasks`, `robotsCount`, `possibleNextStages`, `permissions`:

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