# Bank Details Autofill Point in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)

The `CRM_BANK_DETAIL_AUTOCOMPLETE` point connects an application handler to the bank details search in a CRM card. It is required when an application searches for and populates bank company details, for example, by BIC.

The general workflow and common errors are described in the [Autofilling details in the CRM card](./index.md) overview.

## How to register a handler

When registering a handler using the [placement.bind](../../placement-bind.md) method, pass the value `CRM_BANK_DETAIL_AUTOCOMPLETE` in the `PLACEMENT` parameter. Bitrix24 uses this code to determine that the handler belongs to bank details autofill.

Connection parameters are not included in the data that Bitrix24 passes to the handler during a search.

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
[`type`](../../../data-types.md) | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | Embedding point code. Pass the value `CRM_BANK_DETAIL_AUTOCOMPLETE` ||
|| **HANDLER***
[`string`](../../../data-types.md) | Application handler URL ||
|| **TITLE**
[`string`](../../../data-types.md) | Handler name in the search source selection interface ||
|| **OPTIONS[countries]**
[`string`](../../../data-types.md) | Country identifiers separated by commas without spaces. If the parameter is not passed, the handler is available to all countries for which the search field is open.

Country identifiers can be obtained via the [crm.requisite.preset.countries](../../../crm/requisites/presets/crm-requisite-preset-countries.md) ||
|#

Example of registering a handler:

```javascript
BX24.callMethod(
    'placement.bind',
    {
        PLACEMENT: 'CRM_BANK_DETAIL_AUTOCOMPLETE',
        HANDLER: 'https://example.com/bank-detail-autocomplete/',
        TITLE: 'Bank details search',
        OPTIONS: {
            countries: '1'
        }
    },
    function(result)
    {
        if (result.error())
        {
            console.error(result.error());
        }
    }
);
```

## What the handler receives

Bitrix24 sends a POST request with the point data to the handler. The `searchQuery` [`string`](../../../data-types.md) string, which the user entered into the bank details search field, is passed in the `PLACEMENT_OPTIONS` parameter.

Example POST request:

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => ru
    [APP_SID] => 8b3f2c5d9c1a4f6e9d7a2b4c6f8e1a3d
    [AUTH_ID] => 1f0f107e5806d5fe9a98e02021a72e57645f86a
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 1f0f107a80816604b24a8719792ac2a21d629b5
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CRM_BANK_DETAIL_AUTOCOMPLETE
    [PLACEMENT_OPTIONS] => {"searchQuery":"044525225"}
)
```

{% include notitle [description of standard data](../../_includes/widget_data.md) %}

## How to return found options

Pass the found options using the [BX24.placement.call](../../ui-interaction/bx24-placement-call.md) command with the name `crmShowFoundEntities`.

#|
|| **Field**
[`type`](../../../data-types.md) | **Description** ||
|| **data**
[`array`](../../../data-types.md) | List of found options ||
|| **data[].id**
[`string`](../../../data-types.md) | Option identifier on the application side ||
|| **data[].name**
[`string`](../../../data-types.md) | Option name that will be shown to the user ||
|| **data[].phone**
[`string`](../../../data-types.md) | Option phone. Pass the field if the number is found ||
|| **data[].email**
[`string`](../../../data-types.md) | Option E-mail. Pass the field if the address is found ||
|| **data[].web**
[`string`](../../../data-types.md) | Option website. Pass the field if the website is found ||
|#

```javascript
BX24.placement.call(
    'crmShowFoundEntities',
    {
        data: [
            {
                id: 'bank-044525225',
                name: 'Acme Corp',
                phone: '+1 495 000-00-00',
                web: 'https://bank.example.com'
            }
        ]
    }
);
```

## How to create a selected option

If a user selects an option from the application response, Bitrix24 triggers the `onCrmEntityIsNeedToCreate` interface event. Subscribe to it using the [BX24.placement.bindEvent](../../ui-interaction/bx24-placement-bind-event.md) method.

The data of the selected option is passed to the `onCrmEntityIsNeedToCreate` event handler.

#|
|| **Field**
[`type`](../../../data-types.md) | **Description** ||
|| **appSid**
[`string`](../../../data-types.md) | Application session identifier in which the selected option was found ||
|| **data**
[`object`](../../../data-types.md) | Selected option data from the list that the application passed via `crmShowFoundEntities` ||
|#

In the `fields` object, pass the bank detail fields that need to be populated in the CRM card.

```javascript
BX24.placement.bindEvent('onCrmEntityIsNeedToCreate', function (eventData) {
    const selected = eventData.data;
    const selectedTitle = selected.title || selected.name;

    BX24.placement.call(
        'crmShowCreatedEntity',
        {
            entityType: 'bank',
            id: selected.id,
            title: selectedTitle,
            fields: {
                NAME: selectedTitle,
                RQ_BANK_NAME: selectedTitle,
                RQ_BIK: '044525225',
                RQ_BANK_ADDR: 'New York City',
                RQ_COR_ACC_NUM: '30101810400000000225',
                RQ_SWIFT: 'EXAMPLERU'
            }
        }
    );
});
```

Fields of the `crmShowCreatedEntity` command:

#|
|| **Field**
[`type`](../../../data-types.md) | **Description** ||
|| **entityType**
[`string`](../../../data-types.md) | Type of the created option on the application side ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the created option on the application side ||
|| **title**
[`string`](../../../data-types.md) | Name of the created option ||
|| **fields**
[`object`](../../../data-types.md) | Bank details fields that need to be filled into the CRM card ||
|#

Bank detail fields:

#|
|| **Field**
[`type`](../../../data-types.md) | **Description** ||
|| **NAME**
[`string`](../../../data-types.md) | Bank name ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Bank name in bank details ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | Bank BIC ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Bank address ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent account ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./requisite-autocomplete.md)
- [{#T}](../../../crm/requisites/bank-detail/index.md)
