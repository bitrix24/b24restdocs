# Create a new custom type crm.type.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage Smart Process Automation" permission in the CRM access permissions. For SPAs in an automated solution — the administrator of that automated solution or a user with the same permission in it

The `crm.type.add` method creates a new SPA.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`         | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for adding a new SPA ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Default** ||
|| **title***
[`string`](../../../data-types.md)  | Title of the SPA | ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the SPA. Maximum length is 255 characters | ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Identifier of the created SPA. If this field is not provided, it will be generated automatically.

It should be noted that:
1. The parameter is unique
2. The value of `entityTypeId` must be within one of two ranges:
   - an even integer that is greater than or equal to 1030
   - an integer from 128 to 191 inclusive
| ||
|| **relations**
[`object`](../../../data-types.md)  | An object containing links to other CRM objects. The structure is described [below](#relations) | ||
|| **isUseInUserfieldEnabled** 
[`boolean`](../../../data-types.md) | Is the use of the SPA in the user field enabled | `N` ||
|| **linkedUserFields** 
[`object`](../../../data-types.md)  | A set of user fields in which the SPA should be displayed. The structure is described [below](#linkedUserFields) | `{}` ||
|| **isAutomationEnabled**
[`boolean`](../../../data-types.md) | Are Automation rules and triggers enabled | `N` ||
|| **isBeginCloseDatesEnabled**
[`boolean`](../../../data-types.md) | Are the **Start Date** and **End Date** fields enabled | `N` ||
|| **isBizProcEnabled**
[`boolean`](../../../data-types.md) | Is the business process designer enabled | `N` ||
|| **isCategoriesEnabled**
[`boolean`](../../../data-types.md) | Are custom sales funnels and tunnels enabled | `N` ||
|| **isClientEnabled**
[`boolean`](../../../data-types.md) | Is the **Client** field enabled. When this option is enabled, the SPA has a preset binding to **Contacts** and **Companies** | `N` ||
|| **isDocumentsEnabled**
[`boolean`](../../../data-types.md) | Is document printing enabled | `N` ||
|| **isLinkWithProductsEnabled**
[`boolean`](../../../data-types.md) | Is the binding of catalog products enabled | `N` ||
|| **isMycompanyEnabled**
[`boolean`](../../../data-types.md) | Is the **Your Company Details** field enabled | `N` ||
|| **isObserversEnabled** 
[`boolean`](../../../data-types.md) | Is the **Observers** field enabled | `N` ||
|| **isRecyclebinEnabled**
[`boolean`](../../../data-types.md) | Is the use of the recycle bin enabled | `N` ||
|| **isSetOpenPermissions** 
[`boolean`](../../../data-types.md) | Should new funnels be available to everyone | `Y` ||
|| **isSourceEnabled** 
[`boolean`](../../../data-types.md) | Are the **Source** and **Additional Information about Source** fields enabled | `N` ||
|| **isStagesEnabled**
[`boolean`](../../../data-types.md) | Is the use of custom stages and kanban enabled | `N` ||
|| **daysBeforeClose**
[`integer`](../../../data-types.md) | The number of days from creation allotted to an item until completion: from `0` to `365`. To remove the deadline, pass `null` | `null` ||
|| **isRecurringEnabled**
[`boolean`](../../../data-types.md) | Is the **Recurring** field enabled | `N` ||
|| **isMultifieldsEnabled**
[`boolean`](../../../data-types.md) | Are communication fields enabled: phone, email, and others | `N` ||
|| **isPaymentsEnabled**
[`boolean`](../../../data-types.md) | Is accepting payments enabled | `N` ||
|| **isCountersEnabled**
[`boolean`](../../../data-types.md) | Are the Activities view mode and counters enabled | `N` ||
|| **isPersonEnabled**
[`boolean`](../../../data-types.md) | Is person tracking enabled. It can be enabled only for SPAs with a predefined data schema; otherwise, the method returns the `BX_ERROR` error | `N` ||
|| **isExternal**
[`string`](../../../data-types.md) | Is the SPA external to the CRM (linked to an automated solution). To enable it, pass the string `"true"`

This parameter is deprecated. For working with automated solutions, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|| **customSectionId**
[`integer`](../../../data-types.md) | Identifier of the automated solution

This parameter is deprecated. For working with automated solutions, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|| **customSections**
[`array`](../../../data-types.md) | Array of automated solutions

This parameter is deprecated. For working with automated solutions, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|#

### Relations relations {#relations}

#|
|| **Name**
`type` | **Description** ||
|| **parent**
[`relation[]`](#relation) | Parent CRM types to which SPA items are linked ||
|| **child**
[`relation[]`](#relation) | Child CRM types whose items are linked to the SPA ||
|#

#### One relation relation {#relation}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Default** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../index.md) or [custom type](./index.md) of the CRM object | `-` ||
|| **isChildrenListEnabled**
[`string`](../../../data-types.md) | Should the list of child items be shown in the parent item card. It is enabled only by the string `"true"`; any other value, including `Y`, disables it. The response returns `Y` or `N` | `"false"` ||
|#

### Binding to user fields {#linkedUserFields}

The setting is applied only when `isUseInUserfieldEnabled = 'Y'`.

#|
|| **Name**
`type` | **Description** | **Default Value** ||
|| ```CALENDAR_EVENT|UF_CRM_CAL_EVENT```
[`string`](../../../data-types.md) | Calendar event | `"false"` ||
|| ```TASKS_TASK|UF_CRM_TASK```
[`string`](../../../data-types.md) | Tasks | `"false"` ||
|| ```TASKS_TASK_TEMPLATE|UF_CRM_TASK```
[`string`](../../../data-types.md) | Task templates | `"false"` ||
|#

Pass `"true"` to enable and `"false"` to disable; any value other than the string `"true"` disables the binding. The response returns `Y` or `N`.

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Let's create an SPA and configure it:
* enable most of the settings
* link leads, deals, and invoices to the SPA and add each link to the card of its items
* link the SPA to contacts and companies and add the link to contacts to the card
* display the SPA in the "Calendar event" and "Tasks" fields

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"SPA","entityTypeId":2024,"isAutomationEnabled":"Y","isBeginCloseDatesEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y","isDocumentsEnabled":"Y","isLinkWithProductsEnabled":"Y","isMycompanyEnabled":"Y","isObserversEnabled":"Y","isRecyclebinEnabled":"Y","isSetOpenPermissions":"Y","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"CALENDAR_EVENT|UF_CRM_CAL_EVENT":"true","TASKS_TASK|UF_CRM_TASK":"true"},"relations":{"parent":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"true"},{"entityTypeId":31,"isChildrenListEnabled":"true"}],"child":[{"entityTypeId":3,"isChildrenListEnabled":"true"},{"entityTypeId":4}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"SPA","entityTypeId":2024,"isAutomationEnabled":"Y","isBeginCloseDatesEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y","isDocumentsEnabled":"Y","isLinkWithProductsEnabled":"Y","isMycompanyEnabled":"Y","isObserversEnabled":"Y","isRecyclebinEnabled":"Y","isSetOpenPermissions":"Y","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"CALENDAR_EVENT|UF_CRM_CAL_EVENT":"true","TASKS_TASK|UF_CRM_TASK":"true"},"relations":{"parent":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"true"},{"entityTypeId":31,"isChildrenListEnabled":"true"}],"child":[{"entityTypeId":3,"isChildrenListEnabled":"true"},{"entityTypeId":4}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.type.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TypeResult = {
      type: {
        id: number,
        createdBy: number,
        entityTypeId: number,
        isCategoriesEnabled: string,
        isStagesEnabled: string,
        isBeginCloseDatesEnabled: string,
        isClientEnabled: string,
        isUseInUserfieldEnabled: string,
        isLinkWithProductsEnabled: string,
        isMycompanyEnabled: string,
        isDocumentsEnabled: string,
        isSourceEnabled: string,
        isObserversEnabled: string,
        isRecyclebinEnabled: string,
        isAutomationEnabled: string,
        isBizProcEnabled: string,
        isSetOpenPermissions: string,
        isPaymentsEnabled: string,
        isCountersEnabled: string,
        createdTime: ISODate,
        updatedTime: ISODate,
        updatedBy: number,
        title: string,
        relations: {
          parent: Array<{ entityTypeId: number, isChildrenListEnabled: string, isPredefined: string }>,
          child: Array<{ entityTypeId: number, isChildrenListEnabled: string, isPredefined: string }>,
        },
        linkedUserFields: Record<string, string>,
        customSections: unknown[],
        customSectionId: number | null,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<TypeResult>({
        method: 'crm.type.add',
        params: {
          fields: {
            title: "SPA",
            entityTypeId: 2024,
            isAutomationEnabled: "Y",
            isBeginCloseDatesEnabled: "Y",
            isBizProcEnabled: "Y",
            isCategoriesEnabled: "Y",
            isClientEnabled: "Y",
            isDocumentsEnabled: "Y",
            isLinkWithProductsEnabled: "Y",
            isMycompanyEnabled: "Y",
            isObserversEnabled: "Y",
            isRecyclebinEnabled: "Y",
            isSetOpenPermissions: "Y",
            isSourceEnabled: "Y",
            isStagesEnabled: "Y",
            isUseInUserfieldEnabled: "Y",
            linkedUserFields: {
              "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
              "TASKS_TASK|UF_CRM_TASK": "true",
            },
            relations: {
              parent: [
                {
                  entityTypeId: 1,
                  isChildrenListEnabled: "true",
                },
                {
                  entityTypeId: 2,
                  isChildrenListEnabled: "true",
                },
                {
                  entityTypeId: 31,
                  isChildrenListEnabled: "true",
                },
              ],
              child: [
                {
                  entityTypeId: 3,
                  isChildrenListEnabled: "true",
                },
                {
                  entityTypeId: 4,
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
        console.info('Created smart process id:', result.type.id, 'entityTypeId:', result.type.entityTypeId)
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
      async function addCrmType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.type.add',
            params: {
              fields: {
                title: "SPA",
                entityTypeId: 2024,
                isAutomationEnabled: "Y",
                isBeginCloseDatesEnabled: "Y",
                isBizProcEnabled: "Y",
                isCategoriesEnabled: "Y",
                isClientEnabled: "Y",
                isDocumentsEnabled: "Y",
                isLinkWithProductsEnabled: "Y",
                isMycompanyEnabled: "Y",
                isObserversEnabled: "Y",
                isRecyclebinEnabled: "Y",
                isSetOpenPermissions: "Y",
                isSourceEnabled: "Y",
                isStagesEnabled: "Y",
                isUseInUserfieldEnabled: "Y",
                linkedUserFields: {
                  "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
                  "TASKS_TASK|UF_CRM_TASK": "true",
                },
                relations: {
                  parent: [
                    {
                      entityTypeId: 1,
                      isChildrenListEnabled: "true",
                    },
                    {
                      entityTypeId: 2,
                      isChildrenListEnabled: "true",
                    },
                    {
                      entityTypeId: 31,
                      isChildrenListEnabled: "true",
                    },
                  ],
                  child: [
                    {
                      entityTypeId: 3,
                      isChildrenListEnabled: "true",
                    },
                    {
                      entityTypeId: 4,
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
          console.info('Created smart process id:', result.type.id, 'entityTypeId:', result.type.entityTypeId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCrmType)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException
    try:
        bitrix_response = client.crm.type.add(fields={
            "title": "SPA",
            "isCategoriesEnabled": "Y",
            "isStagesEnabled": "Y",
            "isClientEnabled": "Y",
            "isLinkWithProductsEnabled": "Y",
            "isMycompanyEnabled": "Y",
            "isDocumentsEnabled": "Y",
            "isAutomationEnabled": "Y",
            "isBizProcEnabled": "Y",
            "entityTypeId": 2024,
            "isBeginCloseDatesEnabled": "Y",
            "isObserversEnabled": "Y",
            "isRecyclebinEnabled": "Y",
            "isSetOpenPermissions": "Y",
            "isSourceEnabled": "Y",
            "isUseInUserfieldEnabled": "Y",
            "linkedUserFields": {
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
                "TASKS_TASK|UF_CRM_TASK": "true",
            },
            "relations": {
                "parent": [
                    {
                        "entityTypeId": 1,
                        "isChildrenListEnabled": "true",
                    },
                    {
                        "entityTypeId": 2,
                        "isChildrenListEnabled": "true",
                    },
                    {
                        "entityTypeId": 31,
                        "isChildrenListEnabled": "true",
                    },
                ],
                "child": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": "true",
                    },
                    {
                        "entityTypeId": 4,
                    },
                ],
            },
        }).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.type.add',
                [
                    'fields' => [
                        'title'                     => "SPA",
                        'entityTypeId'              => 2024,
                        'isAutomationEnabled'       => "Y",
                        'isBeginCloseDatesEnabled'  => "Y",
                        'isBizProcEnabled'          => "Y",
                        'isCategoriesEnabled'       => "Y",
                        'isClientEnabled'           => "Y",
                        'isDocumentsEnabled'        => "Y",
                        'isLinkWithProductsEnabled' => "Y",
                        'isMycompanyEnabled'        => "Y",
                        'isObserversEnabled'        => "Y",
                        'isRecyclebinEnabled'       => "Y",
                        'isSetOpenPermissions'      => "Y",
                        'isSourceEnabled'           => "Y",
                        'isStagesEnabled'           => "Y",
                        'isUseInUserfieldEnabled'   => "Y",
                        'linkedUserFields'          => [
                            "CALENDAR_EVENT|UF_CRM_CAL_EVENT" => "true",
                            "TASKS_TASK|UF_CRM_TASK"          => "true",
                        ],
                        'relations'                 => [
                            'parent' => [
                                [
                                    'entityTypeId'           => 1,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 2,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 31,
                                    'isChildrenListEnabled'  => "true",
                                ],
                            ],
                            'child'  => [
                                [
                                    'entityTypeId'           => 3,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 4,
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding CRM type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.type.add',
        {
            fields: {
                title: "SPA",
                entityTypeId: 2024,
                isAutomationEnabled: "Y",
                isBeginCloseDatesEnabled: "Y",
                isBizProcEnabled: "Y",
                isCategoriesEnabled: "Y",
                isClientEnabled: "Y",
                isDocumentsEnabled: "Y",
                isLinkWithProductsEnabled: "Y",
                isMycompanyEnabled: "Y",
                isObserversEnabled: "Y",
                isRecyclebinEnabled: "Y",
                isSetOpenPermissions: "Y",
                isSourceEnabled: "Y",
                isStagesEnabled: "Y",
                isUseInUserfieldEnabled: "Y",
                linkedUserFields: {
                    "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
                    "TASKS_TASK|UF_CRM_TASK": "true",
                },
                relations: {
                    parent: [
                        {
                            entityTypeId: 1,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 2,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 31,
                            isChildrenListEnabled: "true",
                            
                        },
                    ],
                    child: [
                        {
                            entityTypeId: 3,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 4,
                        },
                    ],
                },
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());

                return;
            }

            console.info(result.data());
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.type.add',
        [
            'fields' => [
                'title' => "SPA",
                'entityTypeId' => 2024,
                'isAutomationEnabled' => "Y",
                'isBeginCloseDatesEnabled' => "Y",
                'isBizProcEnabled' => "Y",
                'isCategoriesEnabled' => "Y",
                'isClientEnabled' => "Y",
                'isDocumentsEnabled' => "Y",
                'isLinkWithProductsEnabled' => "Y",
                'isMycompanyEnabled' => "Y",
                'isObserversEnabled' => "Y",
                'isRecyclebinEnabled' => "Y",
                'isSetOpenPermissions' => "Y",
                'isSourceEnabled' => "Y",
                'isStagesEnabled' => "Y",
                'isUseInUserfieldEnabled' => "Y",
                'linkedUserFields' => [
                    "CALENDAR_EVENT|UF_CRM_CAL_EVENT" => "true",
                    "TASKS_TASK|UF_CRM_TASK" => "true",
                ],
                'relations' => [
                    'parent' => [
                        [
                            'entityTypeId' => 1,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 2,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 31,
                            'isChildrenListEnabled' => "true",
                        ],
                    ],
                    'child' => [
                        [
                            'entityTypeId' => 3,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 4,
                        ],
                    ],
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.type.add", b24.Params{
    	"fields": b24.Params{
    		"title":                     "SPA",
    		"entityTypeId":              2024,
    		"isAutomationEnabled":       "Y",
    		"isBeginCloseDatesEnabled":  "Y",
    		"isBizProcEnabled":          "Y",
    		"isCategoriesEnabled":       "Y",
    		"isClientEnabled":           "Y",
    		"isDocumentsEnabled":        "Y",
    		"isLinkWithProductsEnabled": "Y",
    		"isMycompanyEnabled":        "Y",
    		"isObserversEnabled":        "Y",
    		"isRecyclebinEnabled":       "Y",
    		"isSetOpenPermissions":      "Y",
    		"isSourceEnabled":           "Y",
    		"isStagesEnabled":           "Y",
    		"isUseInUserfieldEnabled":   "Y",
    		"linkedUserFields": b24.Params{
    			"CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
    			"TASKS_TASK|UF_CRM_TASK":          "true",
    		},
    		"relations": b24.Params{
    			"parent": []b24.Params{
    				{
    					"entityTypeId":          1,
    					"isChildrenListEnabled": "true",
    				},
    				{
    					"entityTypeId":          2,
    					"isChildrenListEnabled": "true",
    				},
    				{
    					"entityTypeId":          31,
    					"isChildrenListEnabled": "true",
    				},
    			},
    			"child": []b24.Params{
    				{
    					"entityTypeId":          3,
    					"isChildrenListEnabled": "true",
    				},
    				{
    					"entityTypeId": 4,
    				},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.type.add: %w", err)
    }

    // The method wraps the response in an object with the "type" key.
    raw, ok := b24.Unwrap(res.Result, "type")
    if !ok {
    	return fmt.Errorf("no type key in the response")
    }

    var item struct {
    	ID                       b24.ID `json:"id"`
    	CreatedBy                int    `json:"createdBy"`
    	EntityTypeID             b24.ID `json:"entityTypeId"`
    	IsCategoriesEnabled      string `json:"isCategoriesEnabled"`
    	IsStagesEnabled          string `json:"isStagesEnabled"`
    	IsBeginCloseDatesEnabled string `json:"isBeginCloseDatesEnabled"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.CreatedBy)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "type": {
            "id": 16,
            "createdBy": 1,
            "entityTypeId": 2024,
            "isCategoriesEnabled": "Y",
            "isStagesEnabled": "Y",
            "isBeginCloseDatesEnabled": "Y",
            "isClientEnabled": "Y",
            "isUseInUserfieldEnabled": "Y",
            "isLinkWithProductsEnabled": "Y",
            "isMycompanyEnabled": "Y",
            "isDocumentsEnabled": "Y",
            "isSourceEnabled": "Y",
            "isObserversEnabled": "Y",
            "isRecyclebinEnabled": "Y",
            "isAutomationEnabled": "Y",
            "isBizProcEnabled": "Y",
            "isSetOpenPermissions": "Y",
            "isPaymentsEnabled": "N",
            "isCountersEnabled": "N",
            "createdTime": "2024-07-05T19:47:33+02:00",
            "updatedTime": "2024-07-05T19:47:33+02:00",
            "updatedBy": 1,
            "title": "SPA",
            "relations": {
                "parent": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "Y"
                    },
                    {
                        "entityTypeId": 4,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "Y"
                    },
                    {
                        "entityTypeId": 1,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 2,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 31,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    }
                ],
                "child": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 4,
                        "isChildrenListEnabled": "N",
                        "isPredefined": "N"
                    }
                ]
            },
            "linkedUserFields": {
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "N",
                "TASKS_TASK|UF_CRM_TASK": "N",
                "TASKS_TASK_TEMPLATE|UF_CRM_TASK": "N"
            },
            "customSections": [],
            "customSectionId": null
        }
    },
    "time": {
        "start": 1720201651.707909,
        "finish": 1720201654.748627,
        "duration": 3.040717840194702,
        "processing": 2.71589994430542,
        "date_start": "2024-07-05T19:47:31+02:00",
        "date_finish": "2024-07-05T19:47:34+02:00",
        "operating": 0
    }
}
```

In the response, `linkedUserFields` may contain values from before the bindings are saved. To retrieve the saved values, call [crm.type.get](./crm-type-get.md).

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains a single key `type` ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`type`](../../data-types.md#type) | Information about the created SPA ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_ARG_VALUE", 
    "error_description": "entityTypeId is out of allowed range"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `INVALID_ARG_VALUE` | entityTypeId is out of allowed range | The passed `entityTypeId` is reserved for a system type based on SPAs ||
|| `400` | `BX_INVALID_VALUE` | EntityTypeId should be more or equal than 128 and less than 192 | An invalid `entityTypeId` is passed ||
|| `400` | `BX_INVALID_VALUE` | Planned SPA item cycle length must be in range 0 to 365 days inclusive | The `daysBeforeClose` value is outside the range from `0` to `365` ||
|| `400` | `ADDRESS_BUSY` | Another address change is still being saved. Please try again | The section address is locked by a concurrent operation while the SPA is being moved to a section or a section is being added ||
|| `400` | `BX_ERROR` | Cannot enable communication fields because the SPA data structure was not updated | Enabling `isMultifieldsEnabled` was rejected because the SPA data schema is not ready. Contact technical support ||
|| `400` | `BX_ERROR` | — | `isPersonEnabled = 'Y'` is passed for an SPA without a predefined data schema ||
|| `400` | `BX_EMPTY_REQUIRED` | The field Name is required | The SPA title is not passed or is empty ||
|| `400` | `INVALID_ARG_VALUE` | customSectionId is invalid | No automated solution with the specified `customSectionId` is found ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have the permissions listed in the "Who can execute the method" block ||
|| `403` | `allowed_only_intranet_user` | This action is available only to Intranet users | Occurs if the user is not an intranet user ||
|| `400` | `100` | `Could not find value for parameter {fields}` | Occurs if the required parameter `fields` is not passed ||
|| `400` | `0` | Select an automated solution to contain the SPA | Occurs when `isExternal = 'true'`, but `customSectionId` is empty ||
|| `400` | `CREATE_DYNAMIC_TYPE_RESTRICTED` | Maximum number of SPA exceeded | Creating an SPA is restricted due to the plan ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)
- [{#T}](./index.md)
