# Add Page by Template landing.landing.addByTemplate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.addByTemplate` adds a page based on a template (a list of templates that the user sees before creating the page). It returns the `ID` of the created page or an error.

You cannot influence the fields of the created page; for that, you can use [landing.landing.add](./landing-landing-add.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../../data-types.md) | `ID` of the site where the page needs to be created. ||
|| **code**
[`unknown`](../../../data-types.md) | Template identifier for creation. You can get the list of templates using the method [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md). ||
|| **fields**
[`unknown`](../../../data-types.md) | Optional. You can pass an array of fields for the created page. Currently, only the keys TITLE and DESCRIPTION are supported. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.addByTemplate',
        {
            siteId: 870,
            code: 'agency',
            fields: {
                TITLE: 'Page Title',
                DESCRIPTION: 'Page Description'
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

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}