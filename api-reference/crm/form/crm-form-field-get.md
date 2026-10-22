# Get a CRM Form Field crm.form.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `crm.form.field.get` method returns the description of a CRM form field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description you want to retrieve.

Available fields:

- `id` — form identifier
- `code` — form symbolic code
- `name` — form name
- `active` — whether the form is active
- `system` — system form flag
- `languageId` — form language
- `templateId` — form scenario
- `createdTime` — creation date and time
- `updatedTime` — form version, date and time of the last change
- `activeChangedById` — user who last activated or deactivated the form
- `activeChangedTime` — date and time of the last activation or deactivation
- `appearance` — form title, description, and button
- `crm` — CRM objects that the form creates
- `dependencies` — dependent field rules
- `presetFields` — CRM field values that the form fills in automatically
- `responsible` — queue of responsible persons
- `agreements` — consents
- `submission` — messages and redirect after submission
- `payment` — payment after submission
- `callback` — callback after submission
- `booking` — online booking
- `analytics` — web analytics counters
- `spamProtection` — spam protection
- `integrations` — integrations with messengers and advertising services
- `fields` — fields that the client fills in

Field descriptions are in the [CRM Form Structure](./data-types.md#crm_form) section ||
|| **select**
[`array`](../../data-types.md) | List of description properties to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — whether the field is available in the filter
- `sortable` — whether the field is available for sorting
- `editable` — whether the field is editable
- `multiple` — whether the field has multiple values
- `elementType` — element type for composite fields

By default, all description properties are returned ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"crm","select":["name","type","title","filterable","sortable","editable","multiple"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"crm","select":["name","type","title","filterable","sortable","editable","multiple"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.field.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormFieldGetResult = {
      item: {
        name?: string
        type?: string
        title?: string
        filterable?: boolean
        sortable?: boolean
        editable?: boolean
        multiple?: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FormFieldGetResult>({
        method: 'crm.form.field.get',
        params: {
          name: 'crm',
          select: [
            'name',
            'type',
            'title',
            'filterable',
            'sortable',
            'editable',
            'multiple',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item)
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
      async function getFormField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.field.get',
            params: {
              name: 'crm',
              select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable',
                'editable',
                'multiple',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFormField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.field.get(
            name="crm",
            select=[
                "name",
                "type",
                "title",
                "filterable",
                "sortable",
                "editable",
                "multiple",
            ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.field.get',
                [
                    'name' => 'crm',
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'filterable',
                        'sortable',
                        'editable',
                        'multiple',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.field.get',
        {
          name: 'crm',
          select: [
            'name',
            'type',
            'title',
            'filterable',
            'sortable',
            'editable',
            'multiple',
          ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.form.field.get',
        [
            'name' => 'crm',
            'select' => [
                'name',
                'type',
                'title',
                'filterable',
                'sortable',
                'editable',
                'multiple',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.field.get", b24.Params{
    	"name": "crm",
    	"select": []string{"name", "type", "title", "filterable", "sortable", "editable", "multiple"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var field struct {
    	Name       string `json:"name"`
    	Type       string `json:"type"`
    	Title      string `json:"title"`
    	Filterable bool   `json:"filterable"`
    	Sortable   bool   `json:"sortable"`
    }
    if err := json.Unmarshal(raw, &field); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(field.Name, field.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "crm",
            "type": "object",
            "multiple": false,
            "title": "crm",
            "filterable": false,
            "sortable": false,
            "editable": false
        }
    },
    "time": {
        "start": 1792593934,
        "finish": 1792593934.763353,
        "duration": 0.7633531093597412,
        "processing": 0,
        "date_start": "2026-10-21T17:45:34+02:00",
        "date_finish": "2026-10-21T17:45:34+02:00",
        "operating_reset_at": 1792594500,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **item**
[`object`](../../data-types.md) | Object with the field description. The response structure depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"foo\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has permission to view CRM forms ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `foo` was not found | Specify a field name from the [crm.form.field.list](./crm-form-field-list.md) response ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter with an existing field name ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `CrmFieldMetadataDto` | Only pass fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `abc` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-field-list.md)
- [{#T}](./data-types.md)
- [{#T}](./index.md)
