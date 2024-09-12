# Change Settings for User Field Type userfieldtype.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Change settings for the user field type registered by the application. The method returns _true_ or an error with a description of the reason.

## Parameters

#|
|| **Parameter** | **Type** | **Description** | **Restrictions** ||
|| **USER_TYPE_ID**^*^
[`String`](../../data-types.md) | String code of the type.  | a-z0-9 ||
|| **HANDLER**^*^
[`URL`](../../data-types.md) | Address of the user type handler.  | Must be in the same domain as the main application address. ||
|| **TITLE**^*^
[`String`](../../data-types.md) | Text title of the type. Will be displayed in the administrative interface for user field settings.  | ||
|| **DESCRIPTION**
[`String`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings. | ||
|#

\* - Required parameter.

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

Example of a call

```js
BX24.callMethod(
    'userfieldtype.update',
    {
        USER_TYPE_ID: 'test',
        TITLE: 'Updated test type',
        DESCRIPTION: 'Test user field type for documentation with updated description'
    }
);
```

Example of a request

```http
POST https://sometestaccount.com/rest/userfieldtype.update HTTP/1.1
USER_TYPE_ID=test&TITLE=Updated+test+type&DESCRIPTION=Test+user+field+type+for+documentation+with+updated+description&auth=63t6r4z9cugaciaxocrh2r47zlodp12y
HTTP/1.1 200 OK
{
    "result": true
}
```

{% include [Note on examples](../../../_includes/examples.md) %}