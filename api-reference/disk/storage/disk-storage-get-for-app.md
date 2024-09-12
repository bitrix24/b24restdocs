# Get Storage Description for Application disk.storage.getforapp

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getforapp` returns the description of the storage that the application can use to store its data (files and folders).

## Parameters

No parameters.

## Example

```js
BX24.callMethod(
    "disk.storage.getforapp",
    {},
    function (result)
    {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on Success

The returned structure is similar to that provided in [disk.storage.get](./disk-storage-get.md).

> 200 OK

```json
"result": {
    "ID": "221990",
    "NAME": "bitrix.restapi",
    "CODE": null,
    "MODULE_ID": "disk",
    "ENTITY_TYPE": "restapp",
    "ENTITY_ID": "1",
    "ROOT_OBJECT_ID": "2"
}
```