# Check Current User Permissions sonet_group.feature.access

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no error response is provided
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

Checks if the current user has permission to perform an operation in the social network group by calling the function `CSocNetFeaturesPerms::CurrentUserCanPerformOperation()`.

## Request:

```http
https://mydomain.bitrix24.com/rest/sonet_group.feature.access.json?auth=52423d4a5f19f5f964f9b4e96a925cfa&GROUP_ID=1&FEATURE=blog&OPERATION=write_post
```

## Response:

>200 OK

```json
{"result":true}
```

## Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the social network group. ||
|| **FEATURE** | Symbolic code of the functionality. ||
|| **OPERATION** | Symbolic code of the operation. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

Returns **true** if the user has permission to perform the operation, **false** if not, and an error in case of incorrect parameters.

{% note info "Note" %}

Operation and functionality codes can be found in the description of the method `CanPerformOperation`.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    // Getting the list of the current user's groups

    BX24.callMethod('sonet_group.feature.access', {
        'GROUP_ID': 1,
        'FEATURE': 'blog',
        'OPERATION': 'write_post'
    });
    ```

{% endlist %}


{% include [Footnote about examples](../../_includes/examples.md) %}