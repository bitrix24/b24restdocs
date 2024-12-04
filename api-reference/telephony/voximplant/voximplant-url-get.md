# Get a set of links for navigating through the telephony pages voximplant.url.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.url.get` returns a set of links for navigating through the telephony pages. The method has no restrictions on [access permissions](https://helpdesk.bitrix24.com/open/18216960/).

There are no input parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.url.get',
        {},
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

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Returned Data

#|
|| **Field** | **Description** ||
|| **detail_statistics** | Detailed statistics page (table). ||
|| **buy_connector** | Page for purchasing a SIP connector. ||
|| **edit_config** | Page for configuring the connected line (SIP number), `#CONFIG_ID#` needs to be replaced with the required configuration identifier. ||
|#