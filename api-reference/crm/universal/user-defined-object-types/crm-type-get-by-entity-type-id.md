# Get the user type by entityTypeId crm.type.getByEntityTypeId

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage Smart Process Automation" permission in the CRM access permissions. For SPAs in an automated solution — the administrator of that automated solution or a user with the same permission in it. The method is also available to users with permission to read items of this SPA

The `crm.type.getByEntityTypeId` method retrieves information about an SPA by its type identifier `entityTypeId`.

`entityTypeId` is the CRM object type identifier, while `id` is the identifier of the SPA settings record: if you know `id`, use [`crm.type.get`](./crm-type-get.md). Both identifiers can be retrieved with the [`crm.type.list`](./crm-type-list.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the SPA type ||
|#

## Code Examples

Retrieve information about the SPA with `entityTypeId = 2024`.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2024}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.getByEntityTypeId
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2024,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.type.getByEntityTypeId
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmTypeGetByEntityTypeIdResult = {
      type: {
        id: number
        title: string
        code: string
        createdBy: number
        entityTypeId: number
        customSectionId: number | null
        isCategoriesEnabled: string
        isStagesEnabled: string
        isBeginCloseDatesEnabled: string
        isClientEnabled: string
        isUseInUserfieldEnabled: string
        isLinkWithProductsEnabled: string
        isMycompanyEnabled: string
        isDocumentsEnabled: string
        isSourceEnabled: string
        isObserversEnabled: string
        isRecyclebinEnabled: string
        isAutomationEnabled: string
        isBizProcEnabled: string
        isSetOpenPermissions: string
        isPaymentsEnabled: string
        isCountersEnabled: string
        createdTime: ISODate
        updatedTime: ISODate
        updatedBy: number
        relations: {
          parent: Array<{
            entityTypeId: number
            isChildrenListEnabled: string
            isPredefined: string
          }>
          child: Array<{
            entityTypeId: number
            isChildrenListEnabled: string
            isPredefined: string
          }>
        }
        linkedUserFields: Record<string, string>
        customSections?: unknown[]
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmTypeGetByEntityTypeIdResult>({
        method: 'crm.type.getByEntityTypeId',
        params: {
          entityTypeId: 2024,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.type.id, result.type.title)
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
      async function getTypeByEntityTypeId() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.type.getByEntityTypeId',
            params: {
              entityTypeId: 2024,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.type.id, result.type.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTypeByEntityTypeId)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.type.get_by_entity_type_id(
            entity_type_id=2024,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.type.getByEntityTypeId',
                [
                    'entityTypeId' => 2024,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.type.getByEntityTypeId',
        {
            entityTypeId: 2024,
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
        'crm.type.getByEntityTypeId',
        [
            'entityTypeId' => 2024
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.type.getByEntityTypeId", b24.Params{
    	"entityTypeId": 2024,
    })
    if err != nil {
    	return fmt.Errorf("crm.type.getByEntityTypeId: %w", err)
    }

    // The method wraps the response in an object with the "type" key.
    raw, ok := b24.Unwrap(res.Result, "type")
    if !ok {
    	return fmt.Errorf("no type key in the response")
    }

    var item struct {
    	ID                  b24.ID `json:"id"`
    	Title               string `json:"title"`
    	Code                string `json:"code"`
    	CreatedBy           int    `json:"createdBy"`
    	EntityTypeID        b24.ID `json:"entityTypeId"`
    	IsCategoriesEnabled string `json:"isCategoriesEnabled"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "type": {
            "id": 16,
            "title": "Smart Process",
            "code": "",
            "createdBy": 1,
            "entityTypeId": 2024,
            "customSectionId": null,
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
            "isRecurringEnabled": "N",
            "isRecyclebinEnabled": "Y",
            "isAutomationEnabled": "Y",
            "isBizProcEnabled": "Y",
            "isSetOpenPermissions": "Y",
            "isPaymentsEnabled": "N",
            "isCountersEnabled": "N",
            "isMultifieldsEnabled": "N",
            "isPersonEnabled": "N",
            "createdTime": "2024-07-08T14:46:54+02:00",
            "updatedTime": "2024-07-08T14:46:54+02:00",
            "updatedBy": 1,
            "isInitialized": "Y",
            "daysBeforeClose": null,
            "isStartCatalogEnabled": "N",
            "startCatalogFormId": null,
            "isCreateByFormEnabled": "N",
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
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "Y",
                "TASKS_TASK|UF_CRM_TASK": "Y",
                "TASKS_TASK_TEMPLATE|UF_CRM_TASK": "N"
            },
            "customSections": [],
            "additionalCustomSectionIds": []
        }
    },
    "time": {
        "start": 1720442829.865672,
        "finish": 1720442830.334845,
        "duration": 0.46917295455932617,
        "processing": 0.13246917724609375,
        "date_start": "2024-07-08T14:47:09+02:00",
        "date_finish": "2024-07-08T14:47:10+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object with information about the SPA. Its structure is described [below](#result) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`object`](../../../data-types.md) | Information about the SPA. Object structure: [`type`](../../data-types.md#type); field descriptions: [`crm.type.fields`](./crm-type-fields.md) ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Smart Process Automation was not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `allowed_only_intranet_user` | This action is available only to Intranet users | Occurs if the user is not an intranet user ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have the permissions listed under "Who can execute the method" ||
|| `400` | `100` | `Could not find value for parameter {entityTypeId}` | The required parameter `entityTypeId` was not passed ||
|| `400` | `0` | Smart Process Automation was not found | An SPA with the provided `entityTypeId` was not found ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)
- [{#T}](./index.md)
