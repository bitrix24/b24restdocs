# Get the List of Fields for documentgenerator.template.getfields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.getfields` returns the template fields: placeholders from the template file and system document fields.

{% note info "" %}

The method works only with templates uploaded via the REST API, which have `moduleId` set to `rest`. For CRM templates and templates of other modules, the method returns the `DOCGEN_ACCESS_ERROR` error.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the template.

You can obtain the template identifier after [creating a template](./document-generator-template-add.md) or by using the [method to get the list of templates](./document-generator-template-list.md).

The template must not be marked as deleted. You can check this in the `isDeleted` field of the [documentgenerator.template.get](./document-generator-template-get.md) method response ||
|#

{% note info "" %}

Instead of `id`, you can pass the `templateId` parameter with the same value. If neither is passed, the method returns an error with code `100`.

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.getfields
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.getfields
  ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TemplateField = {
      title?: string
      value: string
      default: string
      required?: 'Y'
      type?: 'IMAGE' | 'STAMP' | 'DATE' | 'TEXT' | 'NAME' | 'PHONE'
      group?: string[]
      chain?: string | unknown[]
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateGetFieldsResult = {
      templateFields: Record<string, TemplateField>
    }

    try {
      const response = await $b24.actions.v2.call.make<TemplateGetFieldsResult>({
        method: 'documentgenerator.template.getfields',
        params: {
          id: 57,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result.templateFields))
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
      async function getTemplateFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'documentgenerator.template.getfields',
            params: {
              id: 57,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result.templateFields))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplateFields)
    </script>
    ```

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.documentgenerator.template.getfields(
          bitrix_id=57,
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
      $response = $b24Service->core->call(
          'documentgenerator.template.getfields',
          [
              'id' => 57,
          ]
      );

      $result = $response->getResponseData()->getResult();
      print_r($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.template.getfields',
      {
          id: 57
      },
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.log(result.data());
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'documentgenerator.template.getfields',
      [
          'id' => 57,
      ]
  );

  print_r($result);
  ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "documentgenerator.template.getfields", b24.Params{
    	"id": 57,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("documentgenerator.template.getfields: %w", err)
    }

    // The method wraps the response in an object with the "templateFields" key.
    raw, ok := b24.Unwrap(res.Result, "templateFields")
    if !ok {
    	return fmt.Errorf("no templateFields key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "templateFields": {
            "DocumentNumber": {
                "title": "Number",
                "value": "A015//11/2026",
                "required": "Y",
                "group": [
                    "Document"
                ],
                "chain": "this.DOCUMENT.DOCUMENT_NUMBER",
                "default": "A015//11/2026"
            },
            "DocumentCreateTime": {
                "title": "Created on",
                "value": "2026-11-11",
                "type": "DATE",
                "group": [
                    "Document"
                ],
                "chain": "this.DOCUMENT.DOCUMENT_CREATE_TIME",
                "default": "2026-11-11"
            },
            "ClientName": {
                "value": "",
                "default": ""
            },
            "Total": {
                "value": "",
                "default": ""
            },
            "DocumentTitle": {
                "title": "Document name",
                "value": "Supply contract A015//11/2026",
                "group": [
                    "Document"
                ],
                "chain": [
                    {},
                    "getTitle"
                ],
                "required": "Y",
                "default": "Supply contract A015//11/2026"
            }
        }
    },
    "time": {
        "start": 1794393620,
        "finish": 1794393620.625241,
        "duration": 0.6252410411834717,
        "processing": 0,
        "date_start": "2026-11-11T13:40:20+01:00",
        "date_finish": "2026-11-11T13:40:20+01:00",
        "operating_reset_at": 1794394080,
        "operating": 0.3588840961456299
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templateFields**
[`object`](../../data-types.md) | Template fields. Key — the placeholder name without curly braces, for example `DocumentNumber` for `{DocumentNumber}`, value — field description [(detailed description)](#templatefield).

Keys:
- `DocumentTitle` — document title, always returned
- `DocumentNumber` — document number, returned if the template file contains the `{DocumentNumber}` placeholder
- `DocumentCreateTime` — document creation date, returned if the template file contains the `{DocumentCreateTime}` placeholder
- other placeholders from the template file, for example `{ClientName}`. Only the `value` and `default` fields with an empty string are returned for them — the values are passed when creating a document with the [documentgenerator.document.add](../document-generator-document-add.md) method
||
|#

#### Object templateField {#templatefield}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name. Returned only for the `DocumentTitle`, `DocumentNumber`, and `DocumentCreateTime` fields ||
|| **value**
[`string`](../../data-types.md) | Field value to be inserted into a new document. For `DocumentNumber`, the next number from the template numerator; for `DocumentCreateTime`, the current date in the `YYYY-MM-DD` format; for `DocumentTitle`, the template name and the document number separated by a space. For other placeholders, an empty string ||
|| **default**
[`string`](../../data-types.md) | Default value of the field. In the method response, it matches `value` ||
|| **required**
[`char`](../../data-types.md) | Required field flag. Returned with the value `Y` for the `DocumentTitle` and `DocumentNumber` fields. If the field is optional, the key is absent ||
|| **type**
[`string`](../../data-types.md) | Field type. Returned only for some fields, for example `DATE` for `DocumentCreateTime`.

Possible values:
- `DATE` — date
- `TEXT` — text
- `NAME` — name
- `PHONE` — phone
- `IMAGE` — image
- `STAMP` — stamp or signature
||
|| **group**
[`array`](../../data-types.md) | Names of the groups to which the field belongs, for example `["Document"]` ||
|| **chain**
[`string`](../../data-types.md) \| [`array`](../../data-types.md) | Path to the field value in the data provider, for example `this.DOCUMENT.DOCUMENT_NUMBER`.

For the `DocumentTitle` field, the array `[{}, "getTitle"]` is returned ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Cannot get fields from deleted template"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `id` or `templateId` not provided ||
|| `400` | `0` | Template was not found | A template with the specified `id` does not exist, or `id` is not a positive integer, for example `0` or `"abc"` ||
|| `400` | `0` | Cannot get fields from deleted template | The template is marked as deleted: the `isDeleted` field is `Y` ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | The template does not belong to the `rest` module, for example it is a CRM template ||
|| `400` | `0` | You do not have permissions to modify templates | The user does not have permission to modify document generator templates ||
|| `400` | `0` | You do not have permissions to modify this template | The user does not have permission to modify this template ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-delete.md)