# Get Parameters of crm.company.details.configuration.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can retrieve their own and shared settings,
>  - a user with the "Allow to modify settings" access permission in CRM can retrieve others' settings.

{% note warning "Method development has been halted" %}

The method `crm.company.details.configuration.get` continues to function, but there is a more relevant alternative [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method `crm.company.details.configuration.get` retrieves the settings of company cards: it reads the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — shared settings

Default — `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier, which can be obtained using the [user.get](../../../user/user-get.md) method.

Required only for administrators when requesting others' personal settings. If not specified, the settings of the current user will be returned.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Retrieve personal card configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.company.details.configuration.get
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'crm.company.details.configuration.get',
                {
                    scope: 'P',
                    userId: 6,
                }
            );
            
            const result = response.getData().result;
            console.log('Data:', result);
            processResult(result);
        }
        catch( error )
        {
            console.error('Error:', error);
        }
        ```

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'crm.company.details.configuration.get',
                    [
                        'scope' => 'P',
                        'userId' => 6
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error: ' . $e->getMessage();
        }
        ```

   - BX24.js

       ```js
        BX24.callMethod(
            'crm.company.details.configuration.get',
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

   - PHP CRest

       ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.company.details.configuration.get',
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

2. Retrieve shared card configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.company.details.configuration.get
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'crm.company.details.configuration.get',
                {
                    scope: 'C',
                }
            );
            
            const result = response.getData().result;
            console.log('Configuration details:', result);
            
            processResult(result);
        }
        catch( error )
        {
            console.error('Error:', error);
        }
        ```

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'crm.company.details.configuration.get',
                    [
                        'scope' => 'C'
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error fetching company details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'crm.company.details.configuration.get',
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

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.company.details.configuration.get',
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

HTTP Status: **200**

```json
{
    "result": [
        {
            "name": "main",
            "title": "About the Company",
            "type": "section",
            "elements": [
                {
                    "name": "TITLE",
                    "optionFlags": "0"
                },
                {
                    "name": "COMPANY_TYPE",
                    "optionFlags": "0"
                },
                {
                    "name": "INDUSTRY",
                    "optionFlags": "0"
                },
                {
                    "name": "REVENUE_WITH_CURRENCY",
                    "optionFlags": "0"
                },
                {
                    "name": "EMAIL",
                    "optionFlags": "0"
                },
                {
                    "name": "CONTACT",
                    "optionFlags": "0",
                    "options": {
                        "defaultCountry": "DE"
                    }
                },
                {
                    "name": "UF_CRM_1687188367",
                    "optionFlags": "1"
                },
                {
                    "name": "UF_CRM_1706178583916",
                    "optionFlags": "1"
                },
                {
                    "name": "COMMENTS",
                    "optionFlags": "1"
                },
                {
                    "name": "REQUISITES",
                    "optionFlags": "1"
                },
                {
                    "name": "PARENT_ID_164",
                    "optionFlags": "0"
                },
                {
                    "name": "LOGO",
                    "optionFlags": "1"
                },
                {
                    "name": "PHONE",
                    "optionFlags": "1",
                    "options": {
                        "defaultCountry": "DE"
                    }
                },
                {
                    "name": "ADDRESS",
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
                    "name": "EMPLOYEES",
                    "optionFlags": "0"
                },
                {
                    "name": "ASSIGNED_BY_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "UTM",
                    "optionFlags": "0"
                },
                {
                    "name": "TRACKING_SOURCE_ID",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "user_pl047oti",
            "title": "New Section",
            "type": "section",
            "elements": [
                {
                    "name": "UF_CRM_1689255858",
                    "optionFlags": "1"
                }
            ]
        }
    ],
    "time": {
        "start": 1769418250,
        "finish": 1769418250.874948,
        "duration": 0.8749480247497559,
        "processing": 0,
        "date_start": "2026-01-26T12:04:10+01:00",
        "date_finish": "2026-01-26T12:04:10+01:00",
        "operating_reset_at": 1769418850,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section) | The root element of the response.

Contains the configuration of the sections of the company detail form.

Returns `null` if there is no configuration ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### section

Describes a specific section with fields inside the element card

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
[`section_element[]`](#section_element) | List of fields displayed in the card with additional settings ||
|#

#### section_element

Configuration of a specific field within the section

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

#### section_element.options {#options}

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier of the default address type. To find out possible address types, use [`crm.enum.addresstype`](../../auxiliary/enum/crm-enum-address-type.md) ||
|| **defaultCountry**
[`string`](../../../data-types.md) |
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters.

For example, `"DE"` ||
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
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | The user does not have the "Allow to modify settings" permission to retrieve others' settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-company-details-configuration-set.md)
- [{#T}](./crm-company-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-company-details-configuration-reset.md)