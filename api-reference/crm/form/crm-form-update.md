# Update a CRM Form crm.form.update

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

The `crm.form.update` method partially updates a CRM form: only the fields passed in `fields` are changed.

{% note info "" %}

The method requires the `updatedTime` form version from the [crm.form.get](./crm-form-get.md) response. If the form has been changed in the meantime, the method returns the `CRM_FORM_VERSION_MISMATCH` error: retrieve the form again and repeat the request.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Form identifier.

The identifier can be obtained using the [crm.form.list](./crm-form-list.md) method ||
|| **updatedTime***
[`string`](../../data-types.md) | Form version — the `updatedTime` value from the [crm.form.get](./crm-form-get.md) response in ISO 8601 format, for example `2026-10-21T17:44:00+02:00` ||
|| **fields***
[`object`](../../data-types.md) | Fields to change. The field format is the same as in the [crm.form.get](./crm-form-get.md) response. See the [CRM Form Structure](./data-types.md#crm_form) section.

Partial update rules:

- a passed section field changes only that field, for example `appearance.title`
- the `fields`, `dependencies`, `presetFields`, `responsible.userIds`, `agreements.items` collections and others are replaced entirely. An empty array clears the collection
- a section cannot be cleared with `null`
- the `active` field and read-only fields cannot be passed. Activity is changed by the [crm.form.activate](./crm-form-activate.md) and [crm.form.deactivate](./crm-form-deactivate.md) methods

Fields the client fills in are passed in the `fields` key inside this parameter. The `name`, `type`, `label`, `required`, `multiple`, `placeholder`, `valueType`, `value` element properties are described in the [fields.fields element structure](./crm-form-add.md#form-fields).

An empty object `{}` changes nothing but checks that the form exists and can be modified ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.update`

{% endnote %}

The example changes the form title, the button text, and the message shown after submission. The other form settings remain unchanged.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"updatedTime":"2026-10-21T17:44:00+02:00","fields":{"appearance":{"title":"Leave a Request","buttonCaption":"Submit Request"},"submission":{"success":{"text":"Thank you! We will call you back within an hour."}}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"updatedTime":"2026-10-21T17:44:00+02:00","fields":{"appearance":{"title":"Leave a Request","buttonCaption":"Submit Request"},"submission":{"success":{"text":"Thank you! We will call you back within an hour."}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<FormUpdateResult>({
        method: 'crm.form.update',
        params: {
          id: 47,
          updatedTime: '2026-10-21T17:44:00+02:00',
          fields: {
            appearance: {
              title: 'Leave a Request',
              buttonCaption: 'Submit Request',
            },
            submission: {
              success: {
                text: 'Thank you! We will call you back within an hour.',
              },
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
        console.info(result.result)
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
      async function updateForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.update',
            params: {
              id: 47,
              updatedTime: '2026-10-21T17:44:00+02:00',
              fields: {
                appearance: {
                  title: 'Leave a Request',
                  buttonCaption: 'Submit Request',
                },
                submission: {
                  success: {
                    text: 'Thank you! We will call you back within an hour.',
                  },
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
          console.info(result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateForm)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.update(
            id=47,
            updatedTime="2026-10-21T17:44:00+02:00",
            fields={
                "appearance": {
                    "title": "Leave a Request",
                    "buttonCaption": "Submit Request",
                },
                "submission": {
                    "success": {
                        "text": "Thank you! We will call you back within an hour.",
                    },
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
                'crm.form.update',
                [
                    'id' => 47,
                    'updatedTime' => '2026-10-21T17:44:00+02:00',
                    'fields' => [
                        'appearance' => [
                            'title' => 'Leave a Request',
                            'buttonCaption' => 'Submit Request',
                        ],
                        'submission' => [
                            'success' => [
                                'text' => 'Thank you! We will call you back within an hour.',
                            ],
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
        'crm.form.update',
        {
          id: 47,
          updatedTime: '2026-10-21T17:44:00+02:00',
          fields: {
            appearance: {
              title: 'Leave a Request',
              buttonCaption: 'Submit Request',
            },
            submission: {
              success: {
                text: 'Thank you! We will call you back within an hour.',
              },
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
        'crm.form.update',
        [
            'id' => 47,
            'updatedTime' => '2026-10-21T17:44:00+02:00',
            'fields' => [
                'appearance' => [
                    'title' => 'Leave a Request',
                    'buttonCaption' => 'Submit Request',
                ],
                'submission' => [
                    'success' => [
                        'text' => 'Thank you! We will call you back within an hour.',
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
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.update", b24.Params{
    	"id": 47,
    	"updatedTime": "2026-10-21T17:44:00+02:00",
    	"fields": map[string]any{
    		"appearance": map[string]any{
    			"title": "Leave a Request",
    			"buttonCaption": "Submit Request",
    		},
    		"submission": map[string]any{
    			"success": map[string]any{
    				"text": "Thank you! We will call you back within an hour.",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.form.update: %w", err)
    }

    var data struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.Result)
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
        "start": 1792593882,
        "finish": 1792593883.117865,
        "duration": 1.1178650856018066,
        "processing": 1,
        "date_start": "2026-10-21T17:44:42+02:00",
        "date_finish": "2026-10-21T17:44:43+02:00",
        "operating_reset_at": 1792594440,
        "operating": 0.9873239994049072
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response.

Contains an object with the `result` key set to `true` if the form has been updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

After the update, the form has a new `updatedTime` version. Retrieve it using the [crm.form.get](./crm-form-get.md) method before the next update.

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "CRM_FORM_VERSION_MISMATCH",
        "message": "Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request.",
        "currentUpdatedTime": "2026-10-21T17:44:00+02:00"
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
|| `updatedTime` | Required field `updatedTime` is missing | Pass the form version from the [crm.form.get](./crm-form-get.md) response ||
|| `updatedTime` | This request requires data type `DateTime` in field `updatedTime` | Pass the date in ISO 8601 format, for example `2026-10-21T17:44:00+02:00` ||
|| `active` | `active` is published for reading only | Change activity using the [crm.form.activate](./crm-form-activate.md) and [crm.form.deactivate](./crm-form-deactivate.md) methods ||
|| `booking` | `booking` is published for reading only | Do not pass read-only fields ||
|| `filter` | crm.form.update changes one form addressed by id and takes no filter. | Pass the identifier of a single form in `id` without `filter` ||
|| `fields[N].type` | A stored field can not be turned into a `list` one: the variants it offers are not part of this contract | Do not change the field type to `list`, `checkbox`, `radio`, `product`, `booking` ||
|| `-` | Other configuration violations | The messages are the same as for [crm.form.add](./crm-form-add.md). Check the changes in advance using the [crm.form.validate](./crm-form-validate.md) method ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `sort` found in entity `FormDto` | Pass only the fields listed in the [CRM Form Structure](./data-types.md#crm_form) section ||
|#

#### Outdated Form Version Error

Error Code: `CRM_FORM_VERSION_MISMATCH`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `updatedTime` | Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request | Retrieve the form using the [crm.form.get](./crm-form-get.md) method, apply the changes to the current version, and repeat the request. The current version is in the `currentUpdatedTime` key of the response ||
|#

#### System Form Error

Error Code: `CRM_FORM_SYSTEM_IMMUTABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Cannot edit or delete this CRM form | A system form cannot be modified. Check the `system` field using the [crm.form.get](./crm-form-get.md) method ||
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

Error Code: `CRM_FORM_PHONE_VERIFICATION_REQUIRED`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You have to confirm your Bitrix24 phone number to use CRM forms | Verify the phone number in Bitrix24 and repeat the request ||
|#

#### Post-Save Error

Error Code: `CRM_FORM_SIDE_EFFECT_FAILED`

HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Operation failed but CRM form data was modified. Check the form before trying again | Retrieve the form using the [crm.form.get](./crm-form-get.md) method and check its state ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-get.md)
- [{#T}](./crm-form-validate.md)
- [{#T}](./data-types.md)
- [{#T}](./placement/crm-form-embedding-update.md)
- [{#T}](./index.md)
