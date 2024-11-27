# View users who read an important message log.blogpost.getusers.important

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no error response is provided
- examples in other languages are missing
- the description needs a link to the user documentation on important messages

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Returns an array of user IDs who have read the specified important message.

#|
|| **Parameter** | **Description** ||
|| **POST_ID** | ID of the news feed message that is an important message. ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'log.blogpost.getusers.important',
        {
            POST_ID: 345
        },
        function(result)
        {
            console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}

## Request:

{% list tabs %}

- URL request

    ```http
    https://my.bitrix24.com/rest/log.blogpost.getusers.important.json?POST_ID=345&auth=xxxxxxx
    ```

{% endlist %}

## Response:

> 200 OK

```json
{"result":["1","2","3"]}
```