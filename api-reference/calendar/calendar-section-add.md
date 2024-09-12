# Add a New Calendar calendar.section.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.add` adds a new calendar. Here and further, the section will be referred to as "calendar".

{% note info %}

Currently, the method adds a new calendar only for the user executing the calendar.section.add method. This limitation will be lifted in the future.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **name**^*^ | Name of the calendar. ||
|| **description** | Description of the calendar. ||
|| **color** | Color of the calendar. ||
|| **text_color** | Text color in the calendar. ||
|| **export** | List of parameters: 
- ALLOW - allow calendar export; 
- SET - sets the period for which to perform the export. ||
|| **access** | Array of access permissions for the calendar. ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

## Example

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

{% include [Note on examples](../../_includes/examples.md) %}

## Response on Success

Returns the IDs of the created calendars.