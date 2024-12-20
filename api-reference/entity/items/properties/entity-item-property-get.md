# Get a list of additional properties of storage elements entity.item.property.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.property.get` retrieves a list of additional properties of storage elements.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY**
[`string`](../../../data-types.md) | String identifier of the required property. ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'entity.item.property.get',
        {
            ENTITY: 'menu_new'
        },
        function(r){
            console.log(r.data());
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.property.get.json?ENTITY=menu_new&auth=340bf57f35ee95e0debf98399632999c
    ```

{% endlist %}

{% include [Notes on examples](../../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{
    "result":
    [
        {
            "PROPERTY":"test",
            "NAME":"Test property",
            "TYPE":"S"
        },
        {
            "PROPERTY":"test1",
            "NAME":"Second test property",
            "TYPE":"N"
        },
        {
            "PROPERTY":"test_file",
            "NAME":"File",
            "TYPE":"F"
        }
    ]
}
```