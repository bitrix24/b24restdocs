# Get Public Link for File disk.file.getExternalLink

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.getExternalLink` returns a public link by the file identifier. Public links provide the file for download only if the user has accessed the public link's detail form.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|#

## Example

```js
BX24.callMethod(
    "disk.file.getExternalLink",
    {
        id: 10
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

```json
"result": "https://test.bitrix24.com/~Fjruf2"
```