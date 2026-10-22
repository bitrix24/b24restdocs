# Update CRM Form Placement Settings crm.form.embedding.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit CRM forms

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `crm.form.embedding.update` saves the on-click display and auto-display settings of a CRM form.

{% note info "" %}

The method changes the form version `updatedTime`. Before the next change to the form or its placement, retrieve the new version with the [crm.form.get](../crm-form-get.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | CRM form identifier.

The identifier can be obtained using the [crm.form.list](../crm-form-list.md) method ||
|| **updatedTime***
[`datetime`](../../../data-types.md) | Form version — the date of the last change in ISO 8601 format, for example `2026-10-21T17:48:29+02:00`.

The value can be obtained from the `updatedTime` field using the [crm.form.get](../crm-form-get.md) method. If the form was changed after you retrieved the value, the method returns the `CRM_FORM_VERSION_MISMATCH` error ||
|| **fields***
[`object`](../../../data-types.md) | Settings to change. Settings that are not passed remain unchanged. To reset a setting, pass `null` for it. An empty object `{}` is allowed: in this case, the method changes nothing, [detailed description](#fields) ||
|#

### Parameter fields {#fields}

The `fields` structure follows the structure of the [crm.form.embedding.get](./crm-form-embedding-get.md#item) response: the option settings are passed in the `settings` object inside `click` or `auto`. Other response fields cannot be changed, and the `inline` option has no settings.

#|
|| **Name**
`type` | **Description** ||
|| **click.settings**
[`object`](../../../data-types.md) | On-click display settings of the form, [detailed description](#click-settings) ||
|| **auto.settings**
[`object`](../../../data-types.md) | Auto-display settings of the form, [detailed description](#auto-settings) ||
|#

#### Click.settings Object {#click-settings}

The allowed values of `type`, `position`, `vertical`, `button.font`, and `button.align` are returned by the [crm.form.embedding.get](./crm-form-embedding-get.md) method in the `click.options` object.

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Form window type.

Possible values:

- `popup` — a pop-up window
- `panel` — a side panel ||
|| **position**
[`string`](../../../data-types.md) | Horizontal position of the window.

Possible values:

- `left` — on the left
- `center` — in the center
- `right` — on the right ||
|| **vertical**
[`string`](../../../data-types.md) | Vertical position of the window.

Possible values:

- `bottom` — at the bottom
- `top` — at the top ||
|| **button**
[`object`](../../../data-types.md) | Button settings, [detailed description](#button) ||
|#

#### Click.settings.button Object {#button}

#|
|| **Name**
`type` | **Description** ||
|| **use**
[`boolean`](../../../data-types.md) | Whether to show the Bitrix24 button on the website.

Possible values:

- `true` — show the button with the settings from this object
- `false` — do not show the button ||
|| **text**
[`string`](../../../data-types.md) | Button text, no longer than 255 characters ||
|| **font**
[`string`](../../../data-types.md) | Button font.

Possible values:

- `modern`
- `classic`
- `elegant` ||
|| **align**
[`string`](../../../data-types.md) | Button alignment.

Possible values:

- `inline` — inline with the text
- `left` — left-aligned
- `center` — centered
- `right` — right-aligned ||
|| **plain**
[`boolean`](../../../data-types.md) | Button without a background.

Possible values:

- `true` — the button has no background
- `false` — the button has a background ||
|| **rounded**
[`boolean`](../../../data-types.md) | Rounded button corners.

Possible values:

- `true` — the corners are rounded
- `false` — the corners are square ||
|| **outlined**
[`boolean`](../../../data-types.md) | Button outline.

Possible values:

- `true` — the button has an outline
- `false` — the button has no outline ||
|| **decoration**
[`string`](../../../data-types.md) | Button text underline, no longer than 20 characters. The interface uses the values `dotted` and `solid` ||
|| **color**
[`object`](../../../data-types.md) | Button colors in HEX format: `#3bc8f5` or, with transparency, `#3eddffa6`. 3, 4, 6, or 8 hexadecimal digits after `#` are allowed:

- `text` — text color
- `textHover` — text color on hover
- `background` — background color
- `backgroundHover` — background color on hover ||
|#

#### Auto.settings Object {#auto-settings}

The allowed values are returned by the [crm.form.embedding.get](./crm-form-embedding-get.md) method in the `auto.options` object.

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Form window type: `popup` or `panel` ||
|| **position**
[`string`](../../../data-types.md) | Horizontal position of the window: `left`, `center`, or `right` ||
|| **vertical**
[`string`](../../../data-types.md) | Vertical position of the window: `bottom` or `top` ||
|| **delay**
[`integer`](../../../data-types.md) | Auto-display delay in seconds.

Possible values: `3`, `5`, `7`, `10`, `15`, `20`, `25`, `30`, `40`, `60`, `120` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.embedding.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":49,"updatedTime":"2026-10-21T17:48:29+02:00","fields":{"click":{"settings":{"type":"popup","position":"left","vertical":"bottom","button":{"use":true,"text":"Submit a request","font":"modern","align":"right","rounded":true,"color":{"text":"#ffffff","background":"#3bc8f5"}}}},"auto":{"settings":{"type":"panel","delay":10}}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.embedding.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":49,"updatedTime":"2026-10-21T17:48:29+02:00","fields":{"click":{"settings":{"type":"popup","position":"left","vertical":"bottom","button":{"use":true,"text":"Submit a request","font":"modern","align":"right","rounded":true,"color":{"text":"#ffffff","background":"#3bc8f5"}}}},"auto":{"settings":{"type":"panel","delay":10}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.embedding.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FormEmbeddingUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<FormEmbeddingUpdateResult>({
        method: 'crm.form.embedding.update',
        params: {
          id: 49,
          updatedTime: '2026-10-21T17:48:29+02:00',
          fields: {
            click: {
              settings: {
                type: 'popup',
                position: 'left',
                vertical: 'bottom',
                button: {
                  use: true,
                  text: 'Submit a request',
                  font: 'modern',
                  align: 'right',
                  rounded: true,
                  color: {
                    text: '#ffffff',
                    background: '#3bc8f5',
                  },
                },
              },
            },
            auto: {
              settings: {
                type: 'panel',
                delay: 10,
              },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

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
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateFormEmbedding() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.embedding.update',
            params: {
              id: 49,
              updatedTime: '2026-10-21T17:48:29+02:00',
              fields: {
                click: {
                  settings: {
                    type: 'popup',
                    position: 'left',
                    vertical: 'bottom',
                    button: {
                      use: true,
                      text: 'Submit a request',
                      font: 'modern',
                      align: 'right',
                      rounded: true,
                      color: {
                        text: '#ffffff',
                        background: '#3bc8f5',
                      },
                    },
                  },
                },
                auto: {
                  settings: {
                    type: 'panel',
                    delay: 10,
                  },
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

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

      document.addEventListener('DOMContentLoaded', updateFormEmbedding)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "click": {
            "settings": {
                "type": "popup",
                "position": "left",
                "vertical": "bottom",
                "button": {
                    "use": True,
                    "text": "Submit a request",
                    "font": "modern",
                    "align": "right",
                    "rounded": True,
                    "color": {
                        "text": "#ffffff",
                        "background": "#3bc8f5",
                    },
                },
            },
        },
        "auto": {
            "settings": {
                "type": "panel",
                "delay": 10,
            },
        },
    }

    try:
        bitrix_response = client.crm.form.embedding.update(
            id=49,
            updatedTime="2026-10-21T17:48:29+02:00",
            fields=fields,
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
                'crm.form.embedding.update',
                [
                    'id' => 49,
                    'updatedTime' => '2026-10-21T17:48:29+02:00',
                    'fields' => [
                        'click' => [
                            'settings' => [
                                'type' => 'popup',
                                'position' => 'left',
                                'vertical' => 'bottom',
                                'button' => [
                                    'use' => true,
                                    'text' => 'Submit a request',
                                    'font' => 'modern',
                                    'align' => 'right',
                                    'rounded' => true,
                                    'color' => [
                                        'text' => '#ffffff',
                                        'background' => '#3bc8f5',
                                    ],
                                ],
                            ],
                        ],
                        'auto' => [
                            'settings' => [
                                'type' => 'panel',
                                'delay' => 10,
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
        'crm.form.embedding.update',
        {
            id: 49,
            updatedTime: '2026-10-21T17:48:29+02:00',
            fields: {
                click: {
                    settings: {
                        type: 'popup',
                        position: 'left',
                        vertical: 'bottom',
                        button: {
                            use: true,
                            text: 'Submit a request',
                            font: 'modern',
                            align: 'right',
                            rounded: true,
                            color: {
                                text: '#ffffff',
                                background: '#3bc8f5',
                            },
                        },
                    },
                },
                auto: {
                    settings: {
                        type: 'panel',
                        delay: 10,
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
        'crm.form.embedding.update',
        [
            'id' => 49,
            'updatedTime' => '2026-10-21T17:48:29+02:00',
            'fields' => [
                'click' => [
                    'settings' => [
                        'type' => 'popup',
                        'position' => 'left',
                        'vertical' => 'bottom',
                        'button' => [
                            'use' => true,
                            'text' => 'Submit a request',
                            'font' => 'modern',
                            'align' => 'right',
                            'rounded' => true,
                            'color' => [
                                'text' => '#ffffff',
                                'background' => '#3bc8f5',
                            ],
                        ],
                    ],
                ],
                'auto' => [
                    'settings' => [
                        'type' => 'panel',
                        'delay' => 10,
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
    res, err := client.Core().Call(ctx, "crm.form.embedding.update", b24.Params{
    	"id":          49,
    	"updatedTime": "2026-10-21T17:48:29+02:00",
    	"fields": map[string]any{
    		"click": map[string]any{
    			"settings": map[string]any{
    				"type":     "popup",
    				"position": "left",
    				"vertical": "bottom",
    				"button": map[string]any{
    					"use":     true,
    					"text":    "Submit a request",
    					"font":    "modern",
    					"align":   "right",
    					"rounded": true,
    					"color": map[string]any{
    						"text":       "#ffffff",
    						"background": "#3bc8f5",
    					},
    				},
    			},
    		},
    		"auto": map[string]any{
    			"settings": map[string]any{
    				"type":  "panel",
    				"delay": 10,
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.form.embedding.update: %w", err)
    }

    var result struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(result.Result)
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
        "start": 1792594145,
        "finish": 1792594146.982679,
        "duration": 1.9826788902282715,
        "processing": 1,
        "date_start": "2026-10-21T17:49:05+02:00",
        "date_finish": "2026-10-21T17:49:06+02:00",
        "operating_reset_at": 1792594740,
        "operating": 1.0246188640594482
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response.

Contains an object with the `result` key and the value `true` if the settings are saved ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "CRM_FORM_VERSION_MISMATCH",
        "message": "Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request.",
        "currentUpdatedTime": "2026-10-21T17:49:05+02:00"
    }
}
```

In the `CRM_FORM_VERSION_MISMATCH` error, the `currentUpdatedTime` field contains the current form version. You can pass it in `updatedTime` when repeating the request if you are sure that another user's changes can be overwritten.

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

If `fields` contains several errors, the method returns them together in the `validation` array.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `updatedTime` | Required field `updatedTime` is missing | Pass `updatedTime` from the [crm.form.get](../crm-form-get.md) response ||
|| `updatedTime` | This request requires data type `DateTime` in field `updatedTime` | Pass the date in ISO 8601 format, for example `2026-10-21T17:48:29+02:00` ||
|| `filter` | crm.form.embedding.update changes the embedding of one form addressed by id and takes no filter. | Remove `filter` from the request and pass the form `id` ||
|| `inline` | `inline` carries no settings of its own | Do not pass `inline`: this option has no settings ||
|| `click` | `click` is cleared through its own settings, not as a whole | Do not pass `null` for the whole option. To reset a setting, pass `null` for it, for example `click.settings.type` ||
|| `click.settings.delay` | `click.settings.delay` is no setting of the `click` variant | Pass `delay` only in `auto.settings` and `button` only in `click.settings` ||
|| `click.code` | `click.code` is no setting of the `click` variant | Pass only the `settings` object inside the option ||
|| `publicUrl` | `publicUrl` is published for reading only | Do not pass `formActive`, `resourcesReady`, and `publicUrl`: they are read-only ||
|| `click.settings.type` | `click.settings.type` accepts one of: popup, panel | Pass a value from `click.options` of the [crm.form.embedding.get](./crm-form-embedding-get.md) response ||
|| `auto.settings.delay` | `auto.settings.delay` accepts one of: 3, 5, 7, 10, 15, 20, 25, 30, 40, 60, 120 | Pass a value from `auto.options.delay` ||
|| `click.settings.button.color.text` | `click.settings.button.color.text` expects a hex colour, `#3bc8f5` or `#3eddffa6` | Pass the color in HEX format ||
|| `click.settings.button.text` | `click.settings.button.text` is longer than 255 characters | Shorten the button text to 255 characters ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `click.settings.size` found in entity `EmbeddingDto` | Pass only the settings from the `fields` parameter description ||
|#

#### Form Version and State Errors

Error Code: `CRM_FORM_VERSION_MISMATCH`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request | Retrieve the current `updatedTime` with the [crm.form.get](../crm-form-get.md) method or take it from `currentUpdatedTime`, and repeat the request ||
|#

Error Code: `CRM_FORM_SYSTEM_IMMUTABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Cannot edit or delete this CRM form | A system form cannot be changed. Choose a form whose `system` field is `false` in the [crm.form.get](../crm-form-get.md) response ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `999999` was not found | Check the form identifier with the [crm.form.list](../crm-form-list.md) method ||
|#

#### Access and Plan Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has permission to edit CRM forms ||
|#

Error Code: `CRM_FORM_WRITE_RESTRICTED`

HTTP status: **402**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You cannot modify CRM forms on your plan | Switch to a plan that includes editing CRM forms ||
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
|| `-` | Operation failed but CRM form data was modified. Check the form before trying again | The settings are saved, but the form files were not rebuilt. Check the settings with the [crm.form.embedding.get](./crm-form-embedding-get.md) method and repeat the request ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-embedding-get.md)
- [{#T}](./crm-form-embedding-field-list.md)
- [{#T}](./index.md)
- [{#T}](../crm-form-update.md)
