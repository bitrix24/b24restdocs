# Get Parameters of CRM Item Detail Configuration

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access rights during method execution depend on the provided data:
>   - Any user has the right to retrieve their own and shared settings
>   - A user can access another user's settings only if they are an administrator

The method `crm.item.details.configuration.get` returns the settings of the detail form for a specific CRM object. It can work with both personal settings of the specified user and shared settings defined for all users.

{% include [Extras Notice](./_includes/extras_notice.md) %}

## Method Parameters

{% include [Required Parameters Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**                                                                                                                    ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../index.md) or [custom type](../user-defined-object-types/index.md) of CRM objects ||
|| **userId**
[`user`][1] | Identifier of the user whose configuration you want to retrieve.

If this parameter is not provided, the `userId` of the user calling this method will be used.

Required only when requesting personal settings
||
|| **scope**
[`string`][1] | Scope of the settings. Acceptable values:
- `'P'` — personal settings
- `'C'` — shared settings

By default, the value is `'P'`

||
|| **extras**
[`object`][1] | Additional parameters. Possible values and their structure are described [below](#extras) ||
|#

### extras

The `extras` parameter depends on the CRM object.

#|
|| **CRM Object** | **Name** | **Description** ||
|| **SPA** | `categoryId` | Identifier of the SPA funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for this SPA will be used ||
|| **Deal** | `dealCategoryId` | Identifier of the deal funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for deals will be used ||
|| **Lead** | `leadCustomerType` | Type of leads. 

Possible values:
- `1` — simple leads
- `2` — repeat leads
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

1. Retrieve the general configuration of item details for deals in the funnel with `id = 9`, for the user with `id = 1`

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.get
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type SectionElement = {
          name: string
          optionFlags: string
          options?: Record<string, string>
        }

        type Section = {
          name: string
          title: string
          type: string
          elements: SectionElement[]
        }

        try {
          const response = await $b24.actions.v2.call.make<Section[] | null>({
            method: 'crm.item.details.configuration.get',
            params: {
              entityTypeId: 2,
              userId: 1,
              scope: 'C',
              extras: {
                dealCategoryId: 9,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
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
          async function getDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.details.configuration.get',
                params: {
                  entityTypeId: 2,
                  userId: 1,
                  scope: 'C',
                  extras: {
                    dealCategoryId: 9,
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getDetailsConfiguration)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.details.configuration.get',
            [
                'entityTypeId' => 2,
                'userId' => 1,
                'scope' => "C",
                'extras' => [
                    'dealCategoryId' => 9,
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Retrieve the personal configuration of item details for the SPA with `entityTypeId = 1032` in the funnel with `id = 5`

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":1032,"extras":{"categoryId":5}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":1032,"extras":{"categoryId":5},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.get
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type SectionElement = {
          name: string
          optionFlags: string
          options?: Record<string, string>
        }

        type Section = {
          name: string
          title: string
          type: string
          elements: SectionElement[]
        }

        try {
          const response = await $b24.actions.v2.call.make<Section[] | null>({
            method: 'crm.item.details.configuration.get',
            params: {
              entityTypeId: 1032,
              extras: {
                categoryId: 5,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
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
          async function getDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.details.configuration.get',
                params: {
                  entityTypeId: 1032,
                  extras: {
                    categoryId: 5,
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getDetailsConfiguration)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.details.configuration.get',
            [
                'entityTypeId' => 1032,
                'extras' => [
                    'categoryId' => 5,
                ]
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
            "title": "About the Deal",
            "type": "section",
            "elements": [
                {
                    "name": "TITLE",
                    "optionFlags": "0"
                },
                {
                    "name": "STAGE_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "OPPORTUNITY_WITH_CURRENCY",
                    "optionFlags": "0"
                },
                {
                    "name": "CLOSEDATE",
                    "optionFlags": "0"
                },
                {
                    "name": "CLIENT",
                    "optionFlags": "1",
                    "options": {
                        "defaultCountry": "DE"
                    }
                },
                {
                    "name": "UF_CRM_1686898039656",
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
                    "name": "SOURCE_DESCRIPTION",
                    "optionFlags": "0"
                },
                {
                    "name": "BEGINDATE",
                    "optionFlags": "0"
                },
                {
                    "name": "OPENED",
                    "optionFlags": "0"
                },
                {
                    "name": "ASSIGNED_BY_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "OBSERVER",
                    "optionFlags": "0"
                },
                {
                    "name": "COMMENTS",
                    "optionFlags": "0"
                },
                {
                    "name": "UTM",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "products",
            "title": "Products",
            "type": "section",
            "elements": [
                {
                    "name": "PRODUCT_ROW_SUMMARY",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "recurring",
            "title": "Recurring Deal",
            "type": "section",
            "elements": [
                {
                    "name": "RECURRING",
                    "optionFlags": "0"
                }
            ]
        }
    ],
    "time": {
        "start": 1720624891.017344,
        "finish": 1720624891.405621,
        "duration": 0.3882770538330078,
        "processing": 0.02097320556640625,
        "date_start": "2024-07-10T17:21:31+02:00",
        "date_finish": "2024-07-10T17:21:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section)\|`null` | Root element of the response. Contains the configuration of the sections of the detail form. Returns `null` if there is no configuration ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

#### section

Describes an individual section with fields within the item detail form

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`][1] | Unique name of the section used for identification ||
|| **title**
[`string`][1] | Title of the section ||
|| **type**
[`string`][1] | Type of the section ||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the entity detail form with additional settings ||
|#

#### section_element

Configuration of an individual field within the section

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`][1] | Identifier of the field ||
|| **optionFlags**
[`string`][1] | Values:
- `"1"` — always show
- `"0"` — not always show ||
|| **options**
[`object`][1] | Additional options for the field ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'entityTypeId' is not defined"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty Value | Parameter 'entityTypeId' is not defined | Required parameter `entityTypeId` is missing ||
|| Empty Value | The entity type '`entityTypeName`' is not supported in the current context. | The method does not support this entity type || 
|| Empty Value | Access denied. | The user does not have administrative rights ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-details-configuration-set.md)
- [{#T}](./crm-item-details-configuration-reset.md)
- [{#T}](./crm-item-details-configuration-forceCommonScopeForAll.md)

[1]: ../../../data-types.md