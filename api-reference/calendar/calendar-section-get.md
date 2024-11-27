# Get the list of calendars calendar.section.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.get` returns a list of calendars. Here and further, section will be referred to as "calendar".

#| 
|| **Parameter** | **Description** || 
|| **type**^*^ | Calendar type: 
- user 
- group 
- company_calendar 
- location 
- other types, including custom. || 
|| **ownerId**^*^ | Calendar owner identifier. || 
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

The method can be used to book time in the meeting room calendar through a third-party application. In this case, **type** should be set to `location`, and **ownerId** should be `0`.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.section.get",
        {
            type: 'user',
            ownerId: '1'
        }
    );
    ```

{% endlist %}