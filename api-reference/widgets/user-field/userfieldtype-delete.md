# Delete Registered User Field Type userfieldtype.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

Deletes a user field type registered by the application. The method returns _true_ or an error with a description of the reason.

## Parameters

#|
|| **Parameter** | **Type** | **Description** | **Restrictions** ||
|| **USER_TYPE_ID**^*^
[`String`](../../data-types.md) | String code of the type. | a-z0-9 ||
|#

\* - required parameter

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

Example call:

```js
BX24.callMethod(
    'userfieldtype.delete',
    {
        USER_TYPE_ID: 'test'
    }
);
```

Example request:

```http
POST https://sometestaccount.com/rest/userfieldtype.delete HTTP/1.1

USER_TYPE_ID=test&auth=63t6r4z9cugaciaxocrh2r47zlodp12y

HTTP/1.1 200 OK

{
    "result": true
}
```

{% include [Examples Note](../../../_includes/examples.md) %}