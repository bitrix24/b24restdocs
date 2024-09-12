# Get a list of registered custom field types userfieldtype.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieving a list of custom field types registered by the application. This is a list method. It returns a paginated list of field types.

## Parameters

There are no input parameters.

## Examples

Example call:

```js
BX24.callMethod(
    'userfieldtype.list',
    {},
    function(result)
    {
        console.log(result.data());
    }
);
```

Example request:

```http
POST https://sometestaccount.com/rest/userfieldtype.list HTTP/1.1

auth=63t6r4z9cugaciaxocrh2r47zlodp12y

HTTP/1.1 200 OK

{
    "result": [
        {
            "DESCRIPTION": "Test userfield type for documentation",
            "HANDLER": "https://www.myapplication.com/handler/",
            "TITLE": "Test type",
            "USER_TYPE_ID": "test"
        }
    ],
    "total": 1
}
```

{% include [Footnote on examples](../../../_includes/examples.md) %}