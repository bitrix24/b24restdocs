# Get Card Parameters crm.contact.details.configuration.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can retrieve their own personal settings and the shared settings
>  - a user with the "Allow to change settings" access permission in CRM can retrieve another user's personal settings

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

Retrieves the contact card configurations: reads the personal card configurations of the specified user or the shared configurations set for all users.

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
[`user`](../../../data-types.md) | User identifier. Required only when requesting another user's personal settings.

If not specified, the current user's ID is used
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Retrieve Personal Configuration of the Card

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

    - BX24.js

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

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.contact.details.configuration.get", b24.Params{
        	"scope":  "P",
        	"userId": 6,
        }, b24.WithIdempotent())
        if err != nil {
        	return fmt.Errorf("crm.contact.details.configuration.get: %w", err)
        }

        var items []struct {
        	Name  string `json:"name"`
        	Title string `json:"title"`
        	Type  string `json:"type"`
        }
        if err := json.Unmarshal(res.Result, &items); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        for _, it := range items {
        	fmt.Println(it.Name, it.Title)
        }
        ```

    {% endlist %}

2. Retrieve Shared Configuration of the Card

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

    - BX24.js

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

    - Python

        Example

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.contact.details.configuration.get(
                scope="C",
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.contact.details.configuration.get", b24.Params{
        	"scope": "C",
        }, b24.WithIdempotent())
        if err != nil {
        	return fmt.Errorf("crm.contact.details.configuration.get: %w", err)
        }

        var items []struct {
        	Name  string `json:"name"`
        	Title string `json:"title"`
        	Type  string `json:"type"`
        }
        if err := json.Unmarshal(res.Result, &items); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        for _, it := range items {
        	fmt.Println(it.Name, it.Title)
        }
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "name": "main",
            "title": "Contact information",
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
            "title": "Additional information",
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
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Section

Describes an individual section containing fields within the `item` card

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

Configuration of an individual field within a section

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

#### Options

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier of the default address type ||
|| **defaultCountry**
[`string`](../../../data-types.md) | 
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | The country code for the default phone number format is a string of two Latin letters.

For example `"RU"` ||
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

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**   | **Value** ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-contact-details-configuration-set.md)
- [{#T}](./crm-contact-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-contact-details-configuration-reset.md)
