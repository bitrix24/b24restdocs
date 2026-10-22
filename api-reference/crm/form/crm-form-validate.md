# Validate a CRM Form Configuration crm.form.validate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `crm.form.validate` method validates a CRM form configuration without saving it and returns all violations at once.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of a saved form.

The identifier can be obtained using the [crm.form.list](./crm-form-list.md) method ||
|| **fields**
[`object`](../../data-types.md) | Form configuration in the same format as in [crm.form.add](./crm-form-add.md) and [crm.form.update](./crm-form-update.md). The fields are described in the [CRM Form Structure](./data-types.md#crm_form) section.

If `id` is passed, the `active` field cannot be specified, just as in [crm.form.update](./crm-form-update.md) ||
|#

Pass at least one parameter: `id` or `fields`. The combination determines what the method validates:

- only `fields` — the configuration of a new form, the way [crm.form.add](./crm-form-add.md) validates it
- only `id` — the saved form in its current state
- `id` and `fields` — changes applied on top of the saved form, the way [crm.form.update](./crm-form-update.md) validates them

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.validate`

{% endnote %}

The example validates the configuration of a new form that has no name and specifies a nonexistent CRM scheme.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"crm":{"entityScheme":999}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.validate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"crm":{"entityScheme":999}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.validate
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormValidateResult = {
      valid: boolean
      violations: Array<{
        field: string
        message: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FormValidateResult>({
        method: 'crm.form.validate',
        params: {
          fields: {
            crm: {
              entityScheme: 999,
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
        console.info(result.valid, result.violations)
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
      async function validateForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.validate',
            params: {
              fields: {
                crm: {
                  entityScheme: 999,
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
          console.info(result.valid, result.violations)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', validateForm)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.validate(
            fields={
                "crm": {
                    "entityScheme": 999,
                },
            },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.validate',
                [
                    'fields' => [
                        'crm' => [
                            'entityScheme' => 999,
                        ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.form.validate',
        {
          fields: {
            crm: {
              entityScheme: 999,
            },
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
        'crm.form.validate',
        [
            'fields' => [
                'crm' => [
                    'entityScheme' => 999,
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
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.validate", b24.Params{
    	"fields": map[string]any{
    		"crm": map[string]any{
    			"entityScheme": 999,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.form.validate: %w", err)
    }

    var data struct {
    	Valid      bool `json:"valid"`
    	Violations []struct {
    		Field   string `json:"field"`
    		Message string `json:"message"`
    	} `json:"violations"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.Valid, data.Violations)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "valid": false,
        "violations": [
            {
                "field": "name",
                "message": "Name of the CRM form is required"
            },
            {
                "field": "crm.entityScheme",
                "message": "CRM scheme of the form can not be used"
            }
        ]
    },
    "time": {
        "start": 1792593823,
        "finish": 1792593823.067024,
        "duration": 0.06702399253845215,
        "processing": 0,
        "date_start": "2026-10-21T17:43:43+02:00",
        "date_finish": "2026-10-21T17:43:43+02:00",
        "operating_reset_at": 1792594380,
        "operating": 0
    }
}
```

If there are no violations:

```json
{
    "result": {
        "valid": true,
        "violations": []
    }
}
```

Configuration violations are not treated as a request error: the method responds with HTTP status `200` and lists the violations in `violations`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the validation result ||
|| **valid**
[`boolean`](../../data-types.md) | Validation result.

Possible values:

- `true` — the configuration is valid, and the form can be saved
- `false` — there are violations ||
|| **violations**
[`array`](../../data-types.md) | Violations. Each element contains:

- `field` — path to the value, for example `crm.entityScheme` or `fields[0].type`
- `message` — description of the violation in English

Possible messages are listed on the [crm.form.add](./crm-form-add.md) page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
                "message": "At least one of the required fields must be not empty: id, fields."
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, fields | Pass `id`, `fields`, or both parameters ||
|| `active` | `active` is published for reading only | Do not pass `active` when validating a saved form ||
|| `filter` | crm.form.validate checks one form addressed by id and takes no filter. | Pass the identifier of a single form in `id` without `filter` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `sort` found in entity `FormDto` | Pass only the fields listed in the [CRM Form Structure](./data-types.md#crm_form) section ||
|#

#### Form Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [crm.form.list](./crm-form-list.md) method ||
|#

#### Plan Restrictions

Error Code: `CRM_FORM_WRITE_RESTRICTED`

HTTP status: **402**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You cannot modify CRM forms on your plan | Change the Bitrix24 plan ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to edit CRM forms ||
|#

Error Code: `CRM_FORM_SYSTEM_IMMUTABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Cannot edit or delete this CRM form | A system form cannot be modified ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-add.md)
- [{#T}](./crm-form-update.md)
- [{#T}](./data-types.md)
- [{#T}](./index.md)
