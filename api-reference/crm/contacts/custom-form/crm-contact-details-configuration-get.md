# Get Parameters of crm.contact.details.configuration.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: access rights when executing the method depend on the provided data:
>   - Any user has the right to access their own and shared settings.
>   - A user can access another user's settings only if they are an administrator.

The method `crm.contact.details.configuration.get` retrieves the settings of contact cards. The method reads the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. 

Allowed values:
- **P** - personal settings
- **C** - shared settings

Default - `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. If not specified, the current user is used. Required only when requesting another user's personal settings. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Retrieving Personal Configuration

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.details.configuration.get',
        {
            scope: "P",
            userId: 6,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    todo
    ```

{% endlist %}

### Retrieving Shared Configuration

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.details.configuration.get',
        {
            scope: "C",
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    todo
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
  "result": [
    {
      "name": "main",
      "title": "About the Contact",
      "type": "section",
      "elements": [
        {
          "name": "LAST_NAME",
          "optionFlags": "0"
        },
        {
          "name": "PHOTO",
          "optionFlags": "0"
        },
        {
          "name": "NAME",
          "optionFlags": "1"
        },
        {
          "name": "SECOND_NAME",
          "optionFlags": "1"
        },
        {
          "name": "BIRTHDATE",
          "optionFlags": "1"
        },
        {
          "name": "PHONE",
          "optionFlags": "1",
          "options": {
            "defaultCountry": "AU"
          }
        },
        {
          "name": "EMAIL",
          "optionFlags": "1"
        }
      ]
    },
    {
      "name": "additional",
      "title": "Additional",
      "type": "section",
      "elements": [
        {
          "name": "TYPE_ID",
          "optionFlags": "0"
        },
        {
          "name": "SOURCE_ID",
          "optionFlags": "0"
        },
        {
          "name": "OPENED",
          "optionFlags": "0"
        },
        {
          "name": "EXPORT",
          "optionFlags": "0"
        },
        {
          "name": "ASSIGNED_BY_ID",
          "optionFlags": "0"
        }
      ]
    }
  ],
  "time": {
    "start": 1724677217.639681,
    "finish": 1724677217.986853,
    "duration": 0.3471717834472656,
    "processing": 0.01840806007385254,
    "date_start": "2024-08-26T15:00:17+02:00",
    "date_finish": "2024-08-26T15:00:17+02:00",
    "operating": 0
  }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section) | The root element of the response. Contains the configuration of the sections of the detail form of the entity. Returns `null` if there is no configuration. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### section

Describes a specific section with fields within the entity's card.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Unique name of the section used for identification. ||
|| **title**
[`string`](../../../data-types.md) | Title of the section. ||
|| **type**
[`string`](../../../data-types.md) | Type of the section. ||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the entity's card with additional settings. ||
|#

#### section_element

Configuration of a specific field within the section.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field identifier. ||
|| **optionFlags**
[`boolean`](../../../data-types.md) | Whether to always show the field. 

Possible values:
- `"1"` — yes
- `"0"` — no

||
|| **options**
[`object`](../../../data-types.md) | Additional options for the field. The structure is described [below](#options). ||
|#

#### options

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier of the default address type. ||
|| **defaultCountry**
[`string`](../../../data-types.md) | 
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters. For example, `"RU"`. ||
|| **isPayButtonVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Whether the payment acceptance button is visible.

Possible values:
- `'true'` — visible
- `'false'` — hidden

||
|| **isPaymentDocumentsVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Whether the "Payment and Delivery" block is visible.

Possible values:
- `'true'` — visible
- `'false'` — hidden

||
|#

## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**   | **Value** ||
|| `-`     | Access denied. | The user does not have administrative rights. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

TODO