# Get Parameters For crm.company.details.configuration.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can retrieve their own and shared settings,
>  - a user with the "Allow to modify settings" access permission in CRM can retrieve others' settings.

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method `crm.company.details.configuration.get` retrieves the settings for company cards: it reads the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — shared settings

Default is `P`
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

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of each section returned in result[] (result is null when no configuration exists)
        type CrmCompanyDetailsSection = {
          name: string
          title: string
          type: string
          elements: CrmCompanyDetailsSectionElement[]
        }

        type CrmCompanyDetailsSectionElement = {
          name: string
          optionFlags?: string
          options?: {
            defaultCountry?: string
            defaultAddressType?: number
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<CrmCompanyDetailsSection[] | null>({
            method: 'crm.company.details.configuration.get',
            params: {
              scope: 'P',
              userId: 6,
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Card sections:', result?.length ?? 0, result)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function getCompanyDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.company.details.configuration.get',
                params: {
                  scope: 'P',
                  userId: 6,
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Card sections:', result?.length ?? 0, result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getCompanyDetailsConfiguration)
        </script>
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

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of each section returned in result[] (result is null when no configuration exists)
        type CrmCompanyDetailsSection = {
          name: string
          title: string
          type: string
          elements: CrmCompanyDetailsSectionElement[]
        }

        type CrmCompanyDetailsSectionElement = {
          name: string
          optionFlags?: string
          options?: {
            defaultCountry?: string
            defaultAddressType?: number
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<CrmCompanyDetailsSection[] | null>({
            method: 'crm.company.details.configuration.get',
            params: {
              scope: 'C',
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Card sections:', result?.length ?? 0, result)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function getCompanyDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.company.details.configuration.get',
                params: {
                  scope: 'C',
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Card sections:', result?.length ?? 0, result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getCompanyDetailsConfiguration)
        </script>
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

    - Python

        Example

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.company.details.configuration.get(
                scope="C",
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API Error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK Error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
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

HTTP status: **200**

```json
{
    "result": [
        {
            "name": "main",
            "title": "About Us",
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
                        "defaultCountry": "RU"
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
                        "defaultCountry": "RU"
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
            "title": "Additional Information",
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
        "date_start": "2026-01-26T12:04:10+03:00",
        "date_finish": "2026-01-26T12:04:10+03:00",
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
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Section

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

#### Section_Element

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

For example `"RU"` ||
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
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | The user does not have the "Allow to modify settings" permission to retrieve others' settings ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-company-details-configuration-set.md)
- [{#T}](./crm-company-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-company-details-configuration-reset.md)