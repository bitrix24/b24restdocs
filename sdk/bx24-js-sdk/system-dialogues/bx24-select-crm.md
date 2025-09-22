# Call the CRM Entity Selection Dialog BX24.selectCRM

```js
BX24.selectCRM({entityType: value, multiple: true, value:value}): void;
```

The function `BX24.selectCRM` invokes the system dialog for selecting a CRM entity.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityType**
[`array`](../../../api-reference/data-types.md) | Which types of objects to display in the dialog. Possible values: 
- lead — Leads
- contact — Contacts
- company — Companies
- deal — Deals
- quote — Estimates ||
|| **multiple**
[`boolean`](../../../api-reference/data-types.md) | Whether multiple objects can be selected. Default is `false`. ||
|| **value**
[`array`](../../../api-reference/data-types.md) | Which objects to initially add to the selected in the dialog. Works only if `multiple = true`. ||
|#

What the handler receives:

```json
{
    "lead": [
        {
            "id": "L_1348",
            "type": "lead",
            "place": "lead",
            "title": "Mint Guest #2 - Open Line Bitrix",
            "desc": "Guest",
            "url": "/crm/lead/show/1348/"
        }
    ],
    "contact": [
        {
            "id": "C_2",
            "type": "contact",
            "place": "contact",
            "title": "Vasiliy Pupkin",
            "desc": "",
            "url": "/crm/contact/show/2/",
            "image": "/upload/resize_cache/crm/8b5/25_25_2/MM35_PG13.jpg"
        }
    ],
    "company": [],
    "deal": [],
    "quote": []
}
```

## Code Example

```js
BX24.selectCRM(
    {
        entityType: ['lead', 'contact', 'company', 'deal', 'quote'],
        multiple: true,
        value: {lead:[1348,2,35], contact:[2], company:[4,3], deal:[1,2], quote:[1]}
    }, 
    function(){
        console.log(arguments);
    }
)
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-access.md)