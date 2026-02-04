# Get Parameters of crm.contact.details.configuration.get

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method:
>  - Any user has the right to access their own and shared settings
>  - Only an administrator has the right to access others' settings

{% note warning "Method Development Stopped" %}

The method `crm.contact.details.configuration.get` continues to function, but there is a more relevant alternative [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method retrieves the settings for contact cards: it reads the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. 

Possible values:
- **P** — personal settings
- **C** — shared settings

Default — `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when requesting someone else's personal settings.

If not specified, the current user is taken
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Get personal card configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.get
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
        require_once('crest.php');

        $result = CRest::call(
            'crm.contact.details.configuration.get',
            [
                'scope' => 'P',
                'userId' => 6
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Get shared card configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.get
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
        require_once('crest.php');

        $result = CRest::call(
            'crm.contact.details.configuration.get',
            [
                'scope' => 'C'
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

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
[`section[]`](#section) | The root element of the response.

Contains the configuration of the sections of the detail form of the entity.

Returns `null` if there is no configuration ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### section

Describes an individual section with fields inside the entity card

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Unique name of the section used for identification ||
|| **title**
[`string`](../../../data-types.md) | Title of the section ||
|| **type**
[`string`](../../../data-types.md) | Type of the section ||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the entity card with additional settings ||
|#

#### section_element

Configuration of an individual field within the section

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field identifier ||
|| **optionFlags**
[`boolean`](../../../data-types.md) | Whether to always show the field. 

Possible values:
- `"1"` — yes
- `"0"` — no

||
|| **options**
[`object`](../../../data-types.md) | Additional options for the field.

The structure is described [below](#options) ||
|#


#### options

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier for the default address type ||
|| **defaultCountry**
[`string`](../../../data-types.md) | 
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters.

For example `"DE"` ||
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

HTTP status: **400**

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
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-contact-details-configuration-set.md)
- [{#T}](./crm-contact-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-contact-details-configuration-reset.md)