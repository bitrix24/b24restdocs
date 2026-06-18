# Update User Type crm.type.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method updates an existing SPA by its identifier `id`.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the SPA. Can be obtained using the methods: [`crm.type.list`](./crm-type-list.md), [`crm.type.add`](./crm-type-add.md) ||
|| **fields**
[`object`][1]  | Field values (detailed description provided [below](#parametr-fields)) for updating the SPA ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **title** 
[`string`][1]  | Title of the SPA ||
|| **relations**
[`object`][1]  | An object containing links to other CRM entities. The structure is described by the object [`type.relations`](../../data-types.md#typerelations)  ||
|| **isUseInUserfieldEnabled**
[`boolean`][1] | Is the use of the SPA in the user field enabled ||
|| **linkedUserFields**       
[`object`][1]  | A set of user fields in which this SPA should be displayed. The structure is described by the object [`type.linkedUserFields`](../../data-types.md#typelinkeduserfields) ||
|| **isAutomationEnabled**    
[`boolean`][1] | Are automation rules and triggers enabled ||
|| **isBeginCloseDatesEnabled**
[`boolean`][1] | Are the **Start Date** and **End Date** fields enabled ||
|| **isBizProcEnabled**       
[`boolean`][1] | Is the use of the business process designer enabled ||
|| **isCategoriesEnabled**    
[`boolean`][1] | Are custom Sales Funnels and sales tunnels enabled ||
|| **isClientEnabled**        
[`boolean`][1] | Is the **Client** field enabled. When this option is enabled, the SPA has a preset link to **Contacts** and **Companies** ||
|| **isDocumentsEnabled**     
[`boolean`][1] | Is document printing enabled ||
|| **isLinkWithProductsEnabled**
[`boolean`][1] | Is the link to catalog products enabled ||
|| **isMycompanyEnabled**     
[`boolean`][1] | Is the **Your Company Details** field enabled ||
|| **isObserversEnabled**     
[`boolean`][1] | Is the **Observers** field enabled ||
|| **isRecyclebinEnabled**    
[`boolean`][1] | Is the use of the recycle bin enabled ||
|| **isSetOpenPermissions**   
[`boolean`][1] | Should new funnels be made available to everyone ||
|| **isSourceEnabled**        
[`boolean`][1] | Are the **Source** and **Additional Information about Source** fields enabled ||
|| **isStagesEnabled**        
[`boolean`][1] | Is the use of custom stages and Kanban enabled ||
|| **isExternal**
[`boolean`][1] | Is the SPA external to the CRM (linked to a digital workplace)

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) ||
|| **customSectionId**
[`integer`][1] | Identifier of the digital workplace

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) ||
|| **customSections**
[`array`][1]   | Array of digital workplaces

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) ||
|#

{% note info %}

Changes to the SPA settings fields occur only when the modifiable field values are passed.

For example, if you need to disable document printing functionality for the SPA with `id = 128`, you would pass the following parameters:

```json
{
    "id": 128,
    "fields": {
        "isDocumentsEnabled": "N"
    }
}
```

{% endnote %}

### Relations

- Settings must be passed in full; they are completely overwritten.
- You cannot change the settings of predefined links (`iPredefined: true`). These settings can be omitted in the request.
- If an error occurs while trying to save the passed link settings, it will not be displayed. The settings will simply not be saved.

## Code Examples

1. For the SPA with `id = 20`:
    * Disable the following settings:
        - Automation rules and triggers
        - **Start Date** and **End Date** fields
        - **Client** field
        - **Observers** field
    * Enable the following settings:
        - **Source** and **Additional Information about Source** fields
        - Use of custom stages and Kanban
    * Enable display of the SPA in the **Tasks** field

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":20,"fields":{"isAutomationEnabled":"N","isBeginCloseDatesEnabled":"N","isClientEnabled":"N","isObserversEnabled":"N","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"TASKS_TASK|UF_CRM_TASK":"true"}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":20,"fields":{"isAutomationEnabled":"N","isBeginCloseDatesEnabled":"N","isClientEnabled":"N","isObserversEnabled":"N","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"TASKS_TASK|UF_CRM_TASK":"true"}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.type.update
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        type SmartProcessType = {
          id: number
          title: string
          entityTypeId: number
        }

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type TypeUpdateResult = {
          type: SmartProcessType
        }

        try {
          const response = await $b24.actions.v2.call.make<TypeUpdateResult>({
            method: 'crm.type.update',
            params: {
              id: 20,
              fields: {
                isAutomationEnabled: 'N',
                isBeginCloseDatesEnabled: 'N',
                isClientEnabled: 'N',
                isObserversEnabled: 'N',
                isSourceEnabled: 'Y',
                isStagesEnabled: 'Y',
                isUseInUserfieldEnabled: 'Y',
                linkedUserFields: {
                  'TASKS_TASK|UF_CRM_TASK': 'true',
                },
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(`Updated smart process #${result.type.id} (${result.type.title})`)
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
          async function updateSmartProcessSettings() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.type.update',
                params: {
                  id: 20,
                  fields: {
                    isAutomationEnabled: 'N',
                    isBeginCloseDatesEnabled: 'N',
                    isClientEnabled: 'N',
                    isObserversEnabled: 'N',
                    isSourceEnabled: 'Y',
                    isStagesEnabled: 'Y',
                    isUseInUserfieldEnabled: 'Y',
                    linkedUserFields: {
                      'TASKS_TASK|UF_CRM_TASK': 'true',
                    },
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
              console.info(`Updated smart process #${result.type.id} (${result.type.title})`)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateSmartProcessSettings)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.type.update',
            [
                'id' => 20,
                'fields' => [
                    'isAutomationEnabled' => "N",
                    'isBeginCloseDatesEnabled' => "N",
                    'isClientEnabled' => "N",
                    'isObserversEnabled' => "N",
                    'isSourceEnabled' => "Y",
                    'isStagesEnabled' => "Y",
                    'isUseInUserfieldEnabled' => "Y",
                    'linkedUserFields' => [
                        "TASKS_TASK|UF_CRM_TASK" => "true",
                    ],
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Suppose for the SPA with `id = 20` you need to:
    * Remove all entities linked to the SPA (`relations.parent`)
    * Enable "show in detail form" for `Leads` linked to the SPA

    Initial **relations** in the SPA:

    ```json
    {
        "relations": {
            "parent": [
                {
                    "entityTypeId": 31,
                    "isChildrenListEnabled": "N",
                    "isPredefined": "N"
                }
            ],
            "child": [
                {
                    "entityTypeId": 1,
                    "isChildrenListEnabled": "N",
                    "isPredefined": "N"
                },
                {
                    "entityTypeId": 2,
                    "isChildrenListEnabled": "N",
                    "isPredefined": "N"
                }
            ]
        }
    }
    ```

    **Final request:**

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":20,"fields":{"relations":{"parent":[],"child":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"false"}]}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":20,"fields":{"relations":{"parent":[],"child":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"false"}]}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.type.update
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        type SmartProcessType = {
          id: number
          title: string
          entityTypeId: number
        }

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type TypeUpdateResult = {
          type: SmartProcessType
        }

        try {
          const response = await $b24.actions.v2.call.make<TypeUpdateResult>({
            method: 'crm.type.update',
            params: {
              id: 20,
              fields: {
                relations: {
                  parent: [],
                  child: [
                    {
                      entityTypeId: 1,
                      isChildrenListEnabled: 'true',
                    },
                    {
                      entityTypeId: 2,
                      isChildrenListEnabled: 'false',
                    },
                  ],
                },
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(`Updated smart process #${result.type.id} (${result.type.title})`)
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
          async function updateSmartProcessRelations() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.type.update',
                params: {
                  id: 20,
                  fields: {
                    relations: {
                      parent: [],
                      child: [
                        {
                          entityTypeId: 1,
                          isChildrenListEnabled: 'true',
                        },
                        {
                          entityTypeId: 2,
                          isChildrenListEnabled: 'false',
                        },
                      ],
                    },
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
              console.info(`Updated smart process #${result.type.id} (${result.type.title})`)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateSmartProcessRelations)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.type.update',
            [
                'id' => 20,
                'fields' => [
                    'relations' => [
                        'parent' => [],
                        'child' => [
                            [
                                "entityTypeId" => 1,
                                "isChildrenListEnabled" => "true",
                            ],
                            [
                                "entityTypeId" => 2,
                                "isChildrenListEnabled" => "false",
                            ]
                        ],
                    ],
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
    "result": {
        "type": {
            "id": 20,
            "title": "SPA #3",
            "code": "",
            "createdBy": 1,
            "entityTypeId": 1222,
            "customSectionId": null,
            "isCategoriesEnabled": "Y",
            "isStagesEnabled": "Y",
            "isBeginCloseDatesEnabled": "N",
            "isClientEnabled": "N",
            "isUseInUserfieldEnabled": "Y",
            "isLinkWithProductsEnabled": "N",
            "isMycompanyEnabled": "Y",
            "isDocumentsEnabled": "N",
            "isSourceEnabled": "Y",
            "isObserversEnabled": "N",
            "isRecyclebinEnabled": "N",
            "isAutomationEnabled": "N",
            "isBizProcEnabled": "N",
            "isSetOpenPermissions": "Y",
            "isPaymentsEnabled": "N",
            "isCountersEnabled": "N",
            "createdTime": "2024-07-08T17:24:47+02:00",
            "updatedTime": "2024-07-09T20:55:37+02:00",
            "updatedBy": 1,
            "relations": {
                "parent": [],
                "child": [
                    {
                        "entityTypeId": 1,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 2,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    }
                ]
            },
            "linkedUserFields": {
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "N",
                "TASKS_TASK|UF_CRM_TASK": "Y",
                "TASKS_TASK_TEMPLATE|UF_CRM_TASK": "N"
            },
            "customSections": []
        }
    },
    "time": {
        "start": 1720551426.116454,
        "finish": 1720551426.816224,
        "duration": 0.6997702121734619,
        "processing": 0.20451998710632324,
        "date_start": "2024-07-09T20:57:06+02:00",
        "date_finish": "2024-07-09T20:57:06+02:00",
        "operating": 0.2044668197631836
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains a single key `type` ||
|| **type**
[`type`](../../data-types.md#type) | Information about the updated SPA ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes
#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have administrative rights in CRM ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users | Occurs if the user is not an intranet user ||
|| `400` | `UPDATE_DYNAMIC_TYPE_RESTRICTED` | You cannot change the SPA settings due to your plan's restrictions | SPAs are not available on your plan ||
|| `400` | `0` | Select a workplace where the SPA will be located | When passing `isExternal = 'true'`, but with an empty `customSectionId` ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)

[1]: ../../../data-types.md