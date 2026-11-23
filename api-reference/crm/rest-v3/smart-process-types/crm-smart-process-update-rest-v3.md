# Update an SPA Type v 3.0 crm.smartprocess.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with permission to manage this SPA in CRM access permissions

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.smartprocess.update` method updates the settings of an SPA type by its `entityTypeId`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | SPA type ID, for example, `1038`.

Type IDs are returned by the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with new values in the format `{"field": "value"}`. The fields you can pass are described in the [fields parameter table](./crm-smart-process-add-rest-v3.md#fields) of the `crm.smartprocess.add` method ||
|#

The method changes only the fields that are passed; the other type settings remain the same. If you pass an empty `fields` object, the method returns `true` and changes nothing.

The `title` field is optional when updating, but you cannot pass an empty string in it. To remove the description, pass `null` or an empty string in `description`. To remove the item completion deadline, pass `"daysBeforeClose": null`.

In `startCatalogFormId`, you can pass the ID of a CRM form of this type that is available on the "Start workflow" page. The value `null` or `0` clears the form selection.

The method updates a single type and does not accept the `filter` parameter: with it, the method returns a [validation error](#request-validation).

The method name is written in lowercase: `crm.smartprocess.update`. For the name `crm.smartProcess.update`, the method returns the ["Method Not Found"](#method-not-found) error.

{% note info "" %}

If you disable pipelines, stages, the "Customer" field, or product binding for a type, its items lose the corresponding fields and method groups. What depends on each setting is shown on the [SPA fields](../items/fields-smart-process.md) page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.update`

{% endnote %}

The examples change the description of the type with `entityTypeId = 1038`, enable the "Observers" field, and set an item completion deadline of 30 days. Replace it with the ID of your type from the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) response.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038,"fields":{"description":"Requests for product supplies","isObserversEnabled":true,"daysBeforeClose":30}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038,"fields":{"description":"Requests for product supplies","isObserversEnabled":true,"daysBeforeClose":30},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SmartProcessUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SmartProcessUpdateResult>(
        'crm.smartprocess.update',
        {
          entityTypeId: 1038,
          fields: {
            description: 'Requests for product supplies',
            isObserversEnabled: true,
            daysBeforeClose: 30,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info('Updated:', response.getData()!.result.result)
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
      async function updateSmartProcessType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.update',
            {
              entityTypeId: 1038,
              fields: {
                description: 'Requests for product supplies',
                isObserversEnabled: true,
                daysBeforeClose: 30,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info('Updated:', response.getData().result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSmartProcessType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "entityTypeId": 1038,
                "fields": {
                    "description": "Requests for product supplies",
                    "isObserversEnabled": True,
                    "daysBeforeClose": 30,
                },
            },
            prefer_version=3,
        )
        print("Updated:", response["result"]["result"])
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
                'crm.smartprocess.update',
                [
                    'entityTypeId' => 1038,
                    'fields' => [
                        'description' => 'Requests for product supplies',
                        'isObserversEnabled' => true,
                        'daysBeforeClose' => 30,
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
        'crm.smartprocess.update',
        {
            entityTypeId: 1038,
            fields: {
                description: 'Requests for product supplies',
                isObserversEnabled: true,
                daysBeforeClose: 30,
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
        'crm.smartprocess.update',
        [
            'entityTypeId' => 1038,
            'fields' => [
                'description' => 'Requests for product supplies',
                'isObserversEnabled' => true,
                'daysBeforeClose' => 30,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess.update", b24.Params{
    	"entityTypeId": 1038,
    	"fields": b24.Params{
    		"description":        "Requests for product supplies",
    		"isObserversEnabled": true,
    		"daysBeforeClose":    30,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.update: %w", err)
    }

    var updated struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("Updated:", updated.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1795431177,
        "finish": 1795431177.890654,
        "duration": 0.8906540870666504,
        "processing": 0,
        "date_start": "2026-11-23T11:52:57+01:00",
        "date_finish": "2026-11-23T11:52:57+01:00",
        "operating_reset_at": 1795431720,
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
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the type has been updated. The new settings are returned by the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Title must not be empty.",
                "field": "fields.title"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Type Not Found {#not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `0` was not found | There is no type with this `entityTypeId`, it is a system type, for example, `2` for a deal, or the user has no permission to manage this SPA. Check the ID with the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method. The error message always contains `0`, not the passed ID ||
|#

#### Request Validation Errors {#request-validation}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `entityTypeId` | Required field `entityTypeId` is missing | Pass `entityTypeId` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `filter` | Update addresses one smart process by entityTypeId and takes no filter. | Remove `filter`: the method updates a single type by `entityTypeId` ||
|| `fields.title` | Title must not be empty. | Pass a non-empty string in `title` or do not pass the field ||
|| `isClientEnabled` | This request requires data type `bool` in field `isClientEnabled` | Pass the flag as `true` or `false`, not `"Y"` ||
|| `fields.daysBeforeClose` | Invalid smart process field. | Pass a number from `0` to `365` or `null` in `daysBeforeClose`. The same error is returned for a `code` longer than 255 characters ||
|| `DESCRIPTION_TOO_LONG` | Description cannot exceed 1000 characters | Shorten `description` to 1000 characters ||
|| `relations` | This request requires data type `SmartProcessRelationsDto` in field `relations` | Remove `relations` from `fields`: this method does not change links. The method returns the same error for `linkedUserFields` ||
|| `RESTRICTION` | RESTRICTION | Bitrix24 did not save the type. For example, the type settings are not available on the plan, or the passed flag cannot be enabled: the method returns this error for `"isPersonEnabled": true` in a regular SPA ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `customSectionId` | Field "customSectionId" is read-only | Remove the field from `fields`: it is read-only. The method returns the same error for `entityTypeId`, `isPaymentsEnabled`, and other read-only fields ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `SmartProcessDto` | Check the field name against the [fields parameter table](./crm-smart-process-add-rest-v3.md#fields). An error with the field name `0` occurs when `fields` is passed as a string instead of an object ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Starting SPAs from CRM forms is disabled in Bitrix24, and `isStartCatalogEnabled`, `isCreateByFormEnabled`, or `startCatalogFormId` is passed in `fields` with a value different from the current one. Remove these fields from `fields` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.update` was not found | Write the method name in lowercase: `crm.smartprocess.update` ||
|#

#### Internal Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INTERNAL_INTERNALEXCEPTION`, HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Something's gone wrong | Bitrix24 did not finish saving the type. Check the settings with the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) method and repeat the request ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-add-rest-v3.md)
- [{#T}](./crm-smart-process-get-rest-v3.md)
- [{#T}](./crm-smart-process-list-rest-v3.md)
- [{#T}](../items/fields-smart-process.md)
