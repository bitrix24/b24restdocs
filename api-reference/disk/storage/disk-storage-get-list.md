# Get a list of available storages disk.storage.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided
- no detailed example in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getlist` returns a list of available storages.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **filter**
[`unknown`](../../data-types.md) | Optional parameter. Supports filtering by fields specified in [disk.storage.getfields](./disk-storage-get-fields.md) as `USE_IN_FILTER: true`. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% note info %}

See also the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

{% endnote %}

## Example

```js
//search for a group storage with a name containing "Foot"
BX24.callMethod(
    "disk.storage.getlist",
    {
        filter: {
            'ENTITY_TYPE': 'group',
            '%NAME': 'Foot'
        }
    },
    function (result)
    {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}

## Response in case of success

The response is an array of objects, the structure of which is similar to [disk.storage.get](./disk-storage-get.md).