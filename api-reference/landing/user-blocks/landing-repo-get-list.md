# Get the list of custom blocks landing.repo.getList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.getList` retrieves the list of blocks for the current application.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **params**
[`unknown`](../../data-types.md) | Optional array with optional keys:
- select,
- filter,
- order,
- group,

which contain values from the main fields table of the entity. The table is provided below. ||
|#

## Fields

#|
|| **Field** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Record identifier ||
|| **XML_ID**
[`unknown`](../../data-types.md) | Unique record code. ||
|| **APP_CODE**
[`unknown`](../../data-types.md) | Code of the current application. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Activity status (Y / N). ||
|| **NAME**
[`unknown`](../../data-types.md) | Name. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description. ||
|| **SECTIONS**
[`unknown`](../../data-types.md) | Symbolic codes of categories. ||
|| **PREVIEW**
[`unknown`](../../data-types.md) | Preview image. ||
|| **MANIFEST**
[`unknown`](../../data-types.md) | Manifest. ||
|| **CONTENT**
[`unknown`](../../data-types.md) | Content. ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created the record. ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified the record. ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.repo.getList',
    {
        params: {
            select: [
                'ID', 'NAME', 'MANIFEST'
            ],
            filter: {
                '>ID': '1'
            }
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}