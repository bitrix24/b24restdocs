# Get an SPA Type v 3.0 crm.smartprocess.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator, an administrator of this SPA, a user with permission to read its items, or a user with the "Manage Smart Process Automation" permission

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.smartprocess.get` method returns the settings of an SPA type by its `entityTypeId`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | SPA type ID, for example, `1038`. The value is greater than `0`.

Type IDs are returned by the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method ||
|#

The method always returns all fields of the type, including the `relations` links, the `linkedUserFields` bindings to custom fields, and the `customSections` digital workplaces. The method ignores the `select` parameter without returning an error. To retrieve only some of the fields, use the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method with a filter by `entityTypeId` and the `select` parameter.

The method name is written in lowercase: `crm.smartprocess.get`. For the name `crm.smartProcess.get`, the method returns the ["Method Not Found"](#method-not-found) error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.get`

{% endnote %}

The examples retrieve the settings of the SPA type with `entityTypeId = 1038`. Replace it with the ID of your type from the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) response.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SmartProcessGetResult = {
      item: {
        entityTypeId: number
        title: string
        isCategoriesEnabled: boolean
        isStagesEnabled: boolean
        [key: string]: unknown
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SmartProcessGetResult>(
        'crm.smartprocess.get',
        {
          entityTypeId: 1038,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const item = response.getData()!.result.item
        console.info('Smart process type:', item.entityTypeId, item.title)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getSmartProcessType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.get',
            {
              entityTypeId: 1038,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const item = response.getData().result.item
          console.info('Smart process type:', item.entityTypeId, item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSmartProcessType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={"entityTypeId": 1038},
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["entityTypeId"], item["title"])
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess.get',
                [
                    'entityTypeId' => 1038,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess.get',
        {
            entityTypeId: 1038,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess.get',
        [
            'entityTypeId' => 1038,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess.get", b24.Params{
    	"entityTypeId": 1038,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item key")
    }

    var item struct {
    	EntityTypeID        int    `json:"entityTypeId"`
    	Title               string `json:"title"`
    	IsCategoriesEnabled bool   `json:"isCategoriesEnabled"`
    	IsStagesEnabled     bool   `json:"isStagesEnabled"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.EntityTypeID, item.Title)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "entityTypeId": 1038,
            "title": "Supplier Requests",
            "code": "",
            "createdBy": 1,
            "updatedBy": 1,
            "createdTime": "2025-07-01T09:26:42+01:00",
            "updatedTime": "2026-09-29T12:33:45+01:00",
            "customSectionId": null,
            "daysBeforeClose": null,
            "startCatalogFormId": null,
            "isCategoriesEnabled": true,
            "isStagesEnabled": true,
            "isBeginCloseDatesEnabled": true,
            "isClientEnabled": true,
            "isUseInUserfieldEnabled": true,
            "isLinkWithProductsEnabled": true,
            "isMycompanyEnabled": true,
            "isDocumentsEnabled": true,
            "isSourceEnabled": true,
            "isObserversEnabled": true,
            "isRecurringEnabled": false,
            "isRecyclebinEnabled": false,
            "isAutomationEnabled": false,
            "isBizProcEnabled": false,
            "isPaymentsEnabled": false,
            "isCountersEnabled": true,
            "isMultifieldsEnabled": false,
            "isStartCatalogEnabled": false,
            "isCreateByFormEnabled": false,
            "isPersonEnabled": false,
            "isSetOpenPermissions": false,
            "additionalCustomSectionIds": [],
            "description": null,
            "relations": {
                "parent": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": true,
                        "isPredefined": true
                    },
                    {
                        "entityTypeId": 4,
                        "isChildrenListEnabled": true,
                        "isPredefined": true
                    },
                    {
                        "entityTypeId": 1,
                        "isChildrenListEnabled": false,
                        "isPredefined": false
                    },
                    {
                        "entityTypeId": 14,
                        "isChildrenListEnabled": false,
                        "isPredefined": false
                    },
                    {
                        "entityTypeId": 31,
                        "isChildrenListEnabled": false,
                        "isPredefined": false
                    },
                    {
                        "entityTypeId": 177,
                        "isChildrenListEnabled": false,
                        "isPredefined": false
                    }
                ],
                "child": [
                    {
                        "entityTypeId": 39,
                        "isChildrenListEnabled": true,
                        "isPredefined": true
                    }
                ]
            },
            "linkedUserFields": {
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": true,
                "TASKS_TASK|UF_CRM_TASK": true,
                "TASKS_TASK_TEMPLATE|UF_CRM_TASK": true
            },
            "customSections": [
                {
                    "id": 1,
                    "title": "Procurement",
                    "isSelected": false,
                    "isAdditional": false
                }
            ]
        }
    },
    "time": {
        "start": 1795430922,
        "finish": 1795430922.129126,
        "duration": 0.12912607192993164,
        "processing": 0,
        "date_start": "2026-11-23T11:48:42+01:00",
        "date_finish": "2026-11-23T11:48:42+01:00",
        "operating_reset_at": 1795431480,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.item**
[`object`](#item) | SPA type settings. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### SPA Type Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Type ID. It is part of the item method names, for example, `crm.smartprocess1038.add` ||
|| **title**
[`string`](../../../data-types.md) | Type name ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the type. If the code is not set, an empty string ||
|| **description**
[`string`](../../../data-types.md) | Type description, up to 1000 characters. If there is no description, `null` ||
|| **createdBy**
[`integer`](../../../data-types.md) | ID of the user who created the type ||
|| **updatedBy**
[`integer`](../../../data-types.md) | ID of the user who last updated the type ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time the type was created ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time the type was last updated ||
|| **customSectionId**
[`integer`](../../../data-types.md) | ID of the digital workplace the type is placed in. If the type is placed in CRM, `null` ||
|| **additionalCustomSectionIds**
[`integer[]`](../../../data-types.md) | IDs of the digital workplaces the type is additionally added to. The list includes only the workplaces the user can see ||
|| **daysBeforeClose**
[`integer`](../../../data-types.md) | Number of days from creation allotted to an item until completion: from `0` to `365`. If no deadline is set, `null` ||
|| **startCatalogFormId**
[`integer`](../../../data-types.md) | ID of the CRM form used to start the process from the "Start workflow" page. If no form is selected, `null` ||
|| **isCategoriesEnabled**
[`boolean`](../../../data-types.md) | Whether custom pipelines are enabled ||
|| **isStagesEnabled**
[`boolean`](../../../data-types.md) | Whether custom stages and kanban are enabled ||
|| **isBeginCloseDatesEnabled**
[`boolean`](../../../data-types.md) | Whether the "Start date" and "End date" fields are enabled ||
|| **isClientEnabled**
[`boolean`](../../../data-types.md) | Whether the "Customer" field is enabled. When the field is enabled, the type has system links to contacts and companies ||
|| **isUseInUserfieldEnabled**
[`boolean`](../../../data-types.md) | Whether the type can be used in a custom field. The fields it is shown in are set by the `linkedUserFields` object ||
|| **isLinkWithProductsEnabled**
[`boolean`](../../../data-types.md) | Whether binding to catalog products is enabled ||
|| **isMycompanyEnabled**
[`boolean`](../../../data-types.md) | Whether the "Your company details" field is enabled ||
|| **isDocumentsEnabled**
[`boolean`](../../../data-types.md) | Whether document printing is enabled ||
|| **isSourceEnabled**
[`boolean`](../../../data-types.md) | Whether the "Source" and "Source information" fields are enabled ||
|| **isObserversEnabled**
[`boolean`](../../../data-types.md) | Whether the "Observers" field is enabled ||
|| **isRecurringEnabled**
[`boolean`](../../../data-types.md) | Whether recurring items are enabled ||
|| **isRecyclebinEnabled**
[`boolean`](../../../data-types.md) | Whether deleted items are moved to the Recycle Bin ||
|| **isAutomationEnabled**
[`boolean`](../../../data-types.md) | Whether automation rules and triggers are enabled ||
|| **isBizProcEnabled**
[`boolean`](../../../data-types.md) | Whether the workflow designer is enabled ||
|| **isPaymentsEnabled**
[`boolean`](../../../data-types.md) | Whether payment acceptance is enabled ||
|| **isCountersEnabled**
[`boolean`](../../../data-types.md) | Whether the Activities view mode and counters are enabled ||
|| **isMultifieldsEnabled**
[`boolean`](../../../data-types.md) | Whether communication fields are enabled: phone, email, website, messenger ||
|| **isStartCatalogEnabled**
[`boolean`](../../../data-types.md) | Whether the type is shown on the "Start workflow" page ||
|| **isCreateByFormEnabled**
[`boolean`](../../../data-types.md) | Whether items can be created from a CRM form ||
|| **isPersonEnabled**
[`boolean`](../../../data-types.md) | Whether person tracking is enabled ||
|| **isSetOpenPermissions**
[`boolean`](../../../data-types.md) | Whether new pipelines become available to everyone ||
|| **relations**
[`object`](#relations) | Links of the type to other CRM objects. [Object description](#relations) ||
|| **linkedUserFields**
[`object`](#linked-user-fields) | Custom fields of other modules in which an item of the type can be selected. [Object description](#linked-user-fields) ||
|| **customSections**
[`object[]`](#custom-sections) | Digital workplaces the user can see, with a flag showing whether the type is placed in them. [Object description](#custom-sections) ||
|#

#### The relations Object {#relations}

#|
|| **Name**
`type` | **Description** ||
|| **parent**
[`object[]`](#relation) | Parent CRM object types: an SPA item can be linked to their items ||
|| **child**
[`object[]`](#relation) | Child CRM object types: their items can be linked to an SPA item ||
|#

#### Link Object {#relation}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | ID of the linked CRM object type, for example, `3` for a contact. System type IDs are returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md#entity-types) method ||
|| **isChildrenListEnabled**
[`boolean`](../../../data-types.md) | Whether the list of child items is shown in the parent item form ||
|| **isPredefined**
[`boolean`](../../../data-types.md) | Whether the link is a system one. For example, when the "Customer" field is enabled, the links to contacts and companies are system links ||
|#

The response includes only links to types the user can read.

#### The linkedUserFields Object {#linked-user-fields}

The key is the custom field code, and the value is the `true` or `false` flag: whether an SPA item can be selected in this field.

#|
|| **Name**
`type` | **Description** ||
|| ```CALENDAR_EVENT|UF_CRM_CAL_EVENT```
[`boolean`](../../../data-types.md) | CRM binding in a calendar event ||
|| ```TASKS_TASK|UF_CRM_TASK```
[`boolean`](../../../data-types.md) | CRM binding in a task ||
|| ```TASKS_TASK_TEMPLATE|UF_CRM_TASK```
[`boolean`](../../../data-types.md) | CRM binding in a task template ||
|#

#### Digital Workplace Object {#custom-sections}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Digital workplace ID ||
|| **title**
[`string`](../../../data-types.md) | Digital workplace name ||
|| **isSelected**
[`boolean`](../../../data-types.md) | Whether the type is placed in this workplace. Matches the `customSectionId` field ||
|| **isAdditional**
[`boolean`](../../../data-types.md) | Whether the type is additionally added to this workplace. Matches the `additionalCustomSectionIds` list ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 0 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Type Not Found {#not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `0` was not found | There is no type with this `entityTypeId`, it is a system type, for example, `2` for a deal or `31` for an invoice, or the user has no access to the type. Check the ID with the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method. The error message always contains `0`, not the passed ID ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `entityTypeId` | Required field `entityTypeId` is missing | Pass `entityTypeId`. The method does not accept the `id` parameter ||
|| `entityTypeId` | Field value must be 1 or greater | Pass a positive type ID ||
|| `entityTypeId` | This request requires data type `int` in field `entityTypeId` | Pass the ID as a number, for example, `1038` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.get` was not found | Write the method name in lowercase: `crm.smartprocess.get` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-list-rest-v3.md)
- [{#T}](./crm-smart-process-update-rest-v3.md)
- [{#T}](./crm-smart-process-field-list-rest-v3.md)
- [{#T}](../items/crm-entity-get-rest-v3.md)
