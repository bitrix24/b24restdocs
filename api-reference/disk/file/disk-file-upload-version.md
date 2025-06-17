# Upload a New Version of the File disk.file.uploadversion

{% if build == 'dev' %}

{% note alert "TO-DO _not uploaded to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of an error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.uploadversion` uploads a new version of the file.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|| **fileContent**
[`unknown`](../../data-types.md) | Upload the file in [Base64](../../files/how-to-upload-files.md) format. ||
|#

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

The response has the same structure as in [disk.file.get](./disk-file-get.md).