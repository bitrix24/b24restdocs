# Upload a New Version of the File disk.file.uploadversion

{% if build == 'dev' %}

{% note alert "TO-DO _not uploaded to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of an error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.uploadversion` uploads a new version of a file.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|| **fileContent**
[`unknown`](../../data-types.md) | Similar to `DETAIL_PICTURE` in the example [File Handling](../../bx24-js-sdk/how-to-call-rest-methods/files.md). ||
|#

## Example

```js
BX24.callMethod(
    "disk.file.uploadversion",
    {
        id: 4,
        fileContent: document.getElementById('test_file_input')
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
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

The response has the same structure as in [disk.file.get](./disk-file-get.md).