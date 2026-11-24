# Create an SPA Type v 3.0 crm.smartprocess.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage Smart Process Automation" permission in CRM access permissions

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.smartprocess.add` method creates an SPA type and returns its `entityTypeId`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the settings of the new type in the format `{"field": "value"}`. The `title` field is required. [Parameter description](#fields) ||
|#

The method name is written in lowercase: `crm.smartprocess.add`. For the name `crm.smartProcess.add`, the method returns the ["Method Not Found"](#method-not-found) error.

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../../data-types.md) | Type name. The method trims leading and trailing spaces. A string of spaces only causes an error ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the type, up to 255 characters ||
|| **description**
[`string`](../../../data-types.md) | Type description, up to 1000 characters. The method trims leading and trailing spaces and line breaks. An empty string is saved as `null` ||
|| **daysBeforeClose**
[`integer`](../../../data-types.md) | Number of days from creation allotted to an item until completion: from `0` to `365`. To set no deadline, pass `null` ||
|| **isCategoriesEnabled**
[`boolean`](../../../data-types.md) | Enable custom pipelines ||
|| **isStagesEnabled**
[`boolean`](../../../data-types.md) | Enable custom stages and kanban ||
|| **isBeginCloseDatesEnabled**
[`boolean`](../../../data-types.md) | Enable the "Start date" and "End date" fields ||
|| **isClientEnabled**
[`boolean`](../../../data-types.md) | Enable the "Customer" field. Together with it, the type gets system links to contacts and companies ||
|| **isUseInUserfieldEnabled**
[`boolean`](../../../data-types.md) | Allow the type to be used in a custom field ||
|| **isLinkWithProductsEnabled**
[`boolean`](../../../data-types.md) | Enable binding to catalog products ||
|| **isMycompanyEnabled**
[`boolean`](../../../data-types.md) | Enable the "Your company details" field ||
|| **isDocumentsEnabled**
[`boolean`](../../../data-types.md) | Enable document printing ||
|| **isSourceEnabled**
[`boolean`](../../../data-types.md) | Enable the "Source" and "Source information" fields ||
|| **isObserversEnabled**
[`boolean`](../../../data-types.md) | Enable the "Observers" field ||
|| **isRecurringEnabled**
[`boolean`](../../../data-types.md) | Enable recurring items ||
|| **isRecyclebinEnabled**
[`boolean`](../../../data-types.md) | Move deleted items to the Recycle Bin ||
|| **isAutomationEnabled**
[`boolean`](../../../data-types.md) | Enable automation rules and triggers ||
|| **isBizProcEnabled**
[`boolean`](../../../data-types.md) | Enable the workflow designer ||
|| **isCountersEnabled**
[`boolean`](../../../data-types.md) | Enable the Activities view mode and counters ||
|| **isMultifieldsEnabled**
[`boolean`](../../../data-types.md) | Enable communication fields: phone, email, website, messenger ||
|| **isPersonEnabled**
[`boolean`](../../../data-types.md) | Enable person tracking ||
|| **isStartCatalogEnabled**
[`boolean`](../../../data-types.md) | Show the type on the "Start workflow" page ||
|| **isCreateByFormEnabled**
[`boolean`](../../../data-types.md) | Allow items to be created from a CRM form ||
|| **isSetOpenPermissions**
[`boolean`](../../../data-types.md) | Make new pipelines available to everyone ||
|| **startCatalogFormId**
[`integer`](../../../data-types.md) | CRM form for the "Start workflow" page. When creating a type, you can pass only `null` or `0`: the form is selected with the [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) method ||
|#

Flags are passed as `true` or `false`. If a flag is not passed, it is disabled, except for `isSetOpenPermissions`, which is enabled by default. The default values of the created type are returned by the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) method.

The method does not accept the `entityTypeId`, `createdBy`, `updatedBy`, `createdTime`, `updatedTime`, `customSectionId`, and `isPaymentsEnabled` fields: Bitrix24 assigns the type ID itself, and the other fields are read-only. This method also does not set the `relations` links, the `linkedUserFields` bindings, or the `customSections` digital workplaces. How to configure them is described in the [group overview](./index.md#limits).

{% note info "" %}

The `isStartCatalogEnabled` and `isCreateByFormEnabled` flags and the `startCatalogFormId` field can be changed only if starting SPAs from CRM forms is enabled in Bitrix24. Otherwise, for a value different from the current one, the method returns an [access error](#access-denied).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.add`

{% endnote %}

The examples create the "Supplier Requests" type with pipelines, stages, and the "Customer" field.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Supplier Requests","code":"supplier_requests","description":"Requests for product supplies","isCategoriesEnabled":true,"isStagesEnabled":true,"isClientEnabled":true}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Supplier Requests","code":"supplier_requests","description":"Requests for product supplies","isCategoriesEnabled":true,"isStagesEnabled":true,"isClientEnabled":true},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SmartProcessAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SmartProcessAddResult>(
        'crm.smartprocess.add',
        {
          fields: {
            title: 'Supplier Requests',
            code: 'supplier_requests',
            description: 'Requests for product supplies',
            isCategoriesEnabled: true,
            isStagesEnabled: true,
            isClientEnabled: true,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created smart process type, entityTypeId:', result.id)
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
      async function addSmartProcessType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.add',
            {
              fields: {
                title: 'Supplier Requests',
                code: 'supplier_requests',
                description: 'Requests for product supplies',
                isCategoriesEnabled: true,
                isStagesEnabled: true,
                isClientEnabled: true,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created smart process type, entityTypeId:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addSmartProcessType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "title": "Supplier Requests",
                    "code": "supplier_requests",
                    "description": "Requests for product supplies",
                    "isCategoriesEnabled": True,
                    "isStagesEnabled": True,
                    "isClientEnabled": True,
                },
            },
            prefer_version=3,
        )
        print("entityTypeId:", response["result"]["id"])
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
                'crm.smartprocess.add',
                [
                    'fields' => [
                        'title' => 'Supplier Requests',
                        'code' => 'supplier_requests',
                        'description' => 'Requests for product supplies',
                        'isCategoriesEnabled' => true,
                        'isStagesEnabled' => true,
                        'isClientEnabled' => true,
                    ],
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
        'crm.smartprocess.add',
        {
            fields: {
                title: 'Supplier Requests',
                code: 'supplier_requests',
                description: 'Requests for product supplies',
                isCategoriesEnabled: true,
                isStagesEnabled: true,
                isClientEnabled: true,
            },
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
        'crm.smartprocess.add',
        [
            'fields' => [
                'title' => 'Supplier Requests',
                'code' => 'supplier_requests',
                'description' => 'Requests for product supplies',
                'isCategoriesEnabled' => true,
                'isStagesEnabled' => true,
                'isClientEnabled' => true,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess.add", b24.Params{
    	"fields": b24.Params{
    		"title":               "Supplier Requests",
    		"code":                "supplier_requests",
    		"description":         "Requests for product supplies",
    		"isCategoriesEnabled": true,
    		"isStagesEnabled":     true,
    		"isClientEnabled":     true,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.add: %w", err)
    }

    var created struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &created); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("entityTypeId:", created.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 1250
    },
    "time": {
        "start": 1795431025,
        "finish": 1795431028.336084,
        "duration": 3.3360838890075684,
        "processing": 3,
        "date_start": "2026-11-23T11:50:25+01:00",
        "date_finish": "2026-11-23T11:50:28+01:00",
        "operating_reset_at": 1795431600,
        "operating": 3.2407209873199463
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.id**
[`integer`](../../../data-types.md) | The `entityTypeId` of the created type. It is used in the item method names, for example, `crm.smartprocess1250.add`. The type settings are returned by the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"title\" is empty.",
                "field": "title"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields.title` | Title must not be empty. | Pass a non-empty string in `title` ||
|| `title` | This request requires data type `string` in field `title` | Pass a value of the type specified in the [field description](#fields) ||
|| `isClientEnabled` | This request requires data type `bool` in field `isClientEnabled` | Pass the flag as `true` or `false`, not `"Y"` ||
|| `fields.daysBeforeClose` | Invalid smart process field. | Pass a number from `0` to `365` or `null` in `daysBeforeClose`. The same error is returned for a `code` longer than 255 characters ||
|| `fields` | Invalid smart process fields. | Remove a `startCatalogFormId` greater than `0`: the form is selected after the type is created, with the [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) method ||
|| `DESCRIPTION_TOO_LONG` | Description cannot exceed 1000 characters | Shorten `description` to 1000 characters ||
|| `relations` | This request requires data type `SmartProcessRelationsDto` in field `relations` | Remove `relations` from `fields`: this method does not set links. The method returns the same error for `linkedUserFields` ||
|| `RESTRICTION` | RESTRICTION | Bitrix24 did not save the type. For example, the SPA limit of the plan has been reached, or the passed flag cannot be enabled, for example, `isPersonEnabled` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `title` | Required field "title" is empty | Pass `title` in `fields` ||
|| `entityTypeId` | Field "entityTypeId" is read-only | Remove `entityTypeId` from `fields`: the type ID is assigned automatically. The method returns the same error for other read-only fields, for example, `isPaymentsEnabled` and `customSectionId` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `SmartProcessDto` | Check the field name against the [fields parameter table](#fields). An error with the field name `0` occurs when `fields` is passed as a string instead of an object ||
|#

#### Access Error {#access-denied}

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with permission to manage SPAs. The error is also caused by `isStartCatalogEnabled` or `isCreateByFormEnabled` set to `true` if starting SPAs from CRM forms is disabled in Bitrix24 ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.add` was not found | Write the method name in lowercase: `crm.smartprocess.add` ||
|#

#### Internal Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INTERNAL_INTERNALEXCEPTION`, HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Something's gone wrong | Bitrix24 did not finish saving the type. Check whether the type has been created with the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method, and repeat the request ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-update-rest-v3.md)
- [{#T}](./crm-smart-process-get-rest-v3.md)
- [{#T}](../items/crm-entity-add-rest-v3.md)
- [{#T}](../fields/crm-entity-field-custom-add-rest-v3.md)
