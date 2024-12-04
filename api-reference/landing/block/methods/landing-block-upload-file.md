# Upload and Attach an Image to the Block landing.block.uploadfile

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.uploadfile` uploads an image and attaches it to the specified block. On success, it returns a pair: the direct path to the uploaded file and the id of the saved file. From this point, the image will only be deleted when the entire block, the page containing the block, is deleted, or through the call of the method [landing.landing.removeEntities](../../page/methods/landing-landing-remove-entities.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **block**
[`unknown`](../../../data-types.md) | `ID` of the block | ||
|| **picture**
[`unknown`](../../../data-types.md) | Options:
1. Path to the image hosted at a web address.
2. `document.getElementById('file')` when working through the JS API.
3. Array name + content
```json
{
    "0": "name.jpg",
    "1": "base64 file content"
}
``` 
| ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.uploadfile',
        {
            block: 12294,
            picture: 'https://site.com/******.jpg'
    //        picture: document.getElementById('file')
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}