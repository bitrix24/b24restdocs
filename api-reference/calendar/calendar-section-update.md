# Update Calendar calendar.section.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `calendar.section.update` updates the calendar. Here and further, section will be referred to as "calendar".

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **id**^*^ | Identifier of the calendar. ||
|| **name** | Name of the calendar. ||
|| **description** | Description of the calendar. ||
|| **color** | Color of the calendar. ||
|| **text_color** | Text color in the calendar. ||
|| **export** | List of parameters: 
- ALLOW - allow calendar export; 
- SET - sets the period for which to perform the export. ||
|| **access** | Array of access permissions for the calendar. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.section.update",
        {
            id: 325,
            type: 'user',
            ownerId: '2',
            name: 'Changed Section Name',
            description: 'New description for section',
            color: '#9cbeAA',
            text_color: '#283099',
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

Returns the id of modified calendars.