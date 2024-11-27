# Add a New Calendar calendar.section.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.add` adds a new calendar. Here and further, section will be referred to as "calendar".

{% note info %}

Currently, the method adds a new calendar only for the user from whom the method calendar.section.add is executed. This limitation will be lifted in the future.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **name**^*^ | Calendar name. ||
|| **description** | Calendar description. ||
|| **color** | Calendar color. ||
|| **text_color** | Text color in the calendar. ||
|| **export** | List of parameters: 
- ALLOW - allow calendar export; 
- SET - sets the period for which to perform the export. ||
|| **access** | Array of access permissions to the calendar. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.section.add",
        {
            type: 'user',
            ownerId: '2',
            name: 'New Section',
            description: 'Description for section',
            color: '#9cbeee',
            text_color: '#283000',
            export: [{ALLOW: false}],
            access: {
                'D114': 17,
                'G2': 13,
                'U2': 15
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response in case of success

Returns the id of the created calendars.