# Call the CRM Object Selection Dialog BX24.selectCRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.selectCRM(params: object, callback: callable): void;
```

The `BX24.selectCRM` method displays the standard dialog for selecting leads, contacts, companies, deals, and estimates.

Bitrix24 renders the dialog over the application frame. The application does not need to retrieve the list of CRM items: users see only the items they can access. The dialog requires no scope of its own because it opens the Bitrix24 interface instead of calling the REST API.

The method can be called only from an application embedded in a Bitrix24 frame. Call it inside the [BX24.init](../system-functions/bx24-init.md) handler.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityType**
[`array`](../../../api-reference/data-types.md) | Object types to display in the dialog. Possible values:

- `lead` — leads
- `contact` — contacts
- `company` — companies
- `deal` — deals
- `quote` — estimates

The method excludes unsupported values. If the parameter is omitted or the list is empty after unsupported values are excluded, the method displays leads, contacts, and companies ||
|| **multiple**
[`boolean`](../../../api-reference/data-types.md) | Allows selecting multiple objects. Default is `false` ||
|| **value**
[`object`](../../../api-reference/data-types.md) | Objects to mark as selected when the dialog opens [(detailed description)](#value).

The method ignores identifiers less than one. If `multiple` is `false` and `value` contains multiple items, the first item remains selected ||
|| **callback***
[`callable`](../../../api-reference/data-types.md) | Callback function that receives the selected CRM objects [(detailed description)](#callback) ||
|#

### value Parameter {#value}

The key of the `value` object is a CRM object type, and the value is an array of numeric identifiers. Pass only the types specified in `entityType`.

#|
|| **Name**
`type` | **Description** ||
|| **lead**
[`integer[]`](../../../api-reference/data-types.md) | Lead identifiers ||
|| **contact**
[`integer[]`](../../../api-reference/data-types.md) | Contact identifiers ||
|| **company**
[`integer[]`](../../../api-reference/data-types.md) | Company identifiers ||
|| **deal**
[`integer[]`](../../../api-reference/data-types.md) | Deal identifiers ||
|| **quote**
[`integer[]`](../../../api-reference/data-types.md) | Estimate identifiers ||
|#

## Code Example

Display a multiple-selection dialog, preselect several items, and output the selected objects:

```js
BX24.init(() => {
    BX24.selectCRM(
        {
            entityType: ['lead', 'contact', 'company', 'deal', 'quote'],
            multiple: true,
            value: {
                lead: [1348, 2, 35],
                contact: [2],
                company: [4, 3],
                deal: [1, 2],
                quote: [1]
            }
        },
        (selected) => {
            console.log(selected);
        }
    );
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response Handling {#callback}

The dialog does not return data directly. After the selection is confirmed, `callback` receives an object whose items are grouped by CRM type: `lead`, `contact`, `company`, `deal`, and `quote`.

If the user closes the dialog using the close or cancel button, `callback` is not called.

```json
{
    "lead": {
        "0": {
            "id": "L_1348",
            "type": "lead",
            "place": "lead",
            "title": "Guest #2 - Bitrix Open Channel",
            "desc": "Guest",
            "url": "/crm/lead/show/1348/"
        }
    },
    "contact": {
        "0": {
            "id": "C_2",
            "type": "contact",
            "place": "contact",
            "title": "Klaus Weber",
            "desc": "",
            "url": "/crm/contact/show/2/",
            "image": "/upload/resize_cache/crm/8b5/25_25_2/MM35_PG13.jpg"
        }
    },
    "company": {},
    "deal": {},
    "quote": {}
}
```

### Returned Data

Each object key contains selected items of the corresponding type. Items are stored under numeric keys `0`, `1`, and so on.

#|
|| **Name**
`type` | **Description** ||
|| **lead**
[`object`](../../../api-reference/data-types.md) | Selected leads ||
|| **contact**
[`object`](../../../api-reference/data-types.md) | Selected contacts ||
|| **company**
[`object`](../../../api-reference/data-types.md) | Selected companies ||
|| **deal**
[`object`](../../../api-reference/data-types.md) | Selected deals ||
|| **quote**
[`object`](../../../api-reference/data-types.md) | Selected estimates ||
|#

#### Selected Item Fields

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../api-reference/data-types.md) | Item identifier with a type prefix: `L_` for a lead, `C_` for a contact, `CO_` for a company, `D_` for a deal, and `Q_` for an estimate ||
|| **type**
[`string`](../../../api-reference/data-types.md) | CRM item type: `lead`, `contact`, `company`, `deal`, or `quote` ||
|| **place**
[`string`](../../../api-reference/data-types.md) | Item type in the dialog interface ||
|| **title**
[`string`](../../../api-reference/data-types.md) | Item name ||
|| **desc**
[`string`](../../../api-reference/data-types.md) | Additional item description. Its contents depend on the object type ||
|| **url**
[`string`](../../../api-reference/data-types.md) | Relative path to the CRM item form ||
|| **image**
[`string`](../../../api-reference/data-types.md) | Relative path to the item image. The field may be absent ||
|| **largeImage**
[`string`](../../../api-reference/data-types.md) | Relative path to the large item image. The field may be absent ||
|| **customData**
[`any`](../../../api-reference/data-types.md) | Additional item data. Returned if the selection component supplied the data ||
|| **advancedInfo**
[`any`](../../../api-reference/data-types.md) | Extended item information. Returned if the selection component supplied the data ||
|#

## Error Handling

The dialog returns no error codes. The method ignores unsupported `entityType` values and invalid identifiers in `value`. If the user closes the dialog without confirming the selection, `callback` is not called.

## Continue Exploring

- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-access.md)
