# Get CRM Form Placement Options crm.form.embedding.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `crm.form.embedding.get` returns the code for placing a CRM form on a website and the public link to the form.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | CRM form identifier.

The identifier can be obtained using the [crm.form.list](../crm-form-list.md) method ||
|| **variant**
[`string`](../../../data-types.md) | The placement option to return. If the parameter is not passed, the method returns all options.

Possible values:

- `inline` — the form is embedded in a website page
- `click` — the form opens when a button or link is clicked
- `auto` — the form opens automatically after a set delay
- `link` — a public link to the form ||
|| **buttonText**
[`string`](../../../data-types.md) | Text of the button or link that opens the form. The value is inserted only into the returned code and is not saved in the form.

Use it if `variant` is `click`. If only `buttonType` is passed, the method takes the button text from the form settings, and if there is none, uses the default text ||
|| **buttonType**
[`string`](../../../data-types.md) | Type of the element that opens the form. The value is inserted only into the returned code and is not saved in the form.

Possible values:

- `button` — a button
- `link` — a link

Default: `button`

Use it if `variant` is `click` ||
|| **delay**
[`integer`](../../../data-types.md) | Auto-display delay of the form in seconds. The value is inserted only into the returned code and is not saved in the form.

Allowed values are listed in `auto.options.delay` of the method response without the `variant` parameter, for example `3`, `5`, `7`, `10`, `15`, `20`, `25`, `30`, `40`, `60`, `120`.

Use it if `variant` is `auto` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.embedding.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.embedding.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.embedding.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmbeddingVariant = {
      code: string | null
      previewUrl: string
      settings: Record<string, unknown> | null
      options: Record<string, unknown> | null
    }

    type FormEmbeddingGetResult = {
      item: {
        formActive: boolean
        resourcesReady: boolean
        publicUrl: string
        inline: EmbeddingVariant
        click: EmbeddingVariant
        auto: EmbeddingVariant
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FormEmbeddingGetResult>({
        method: 'crm.form.embedding.get',
        params: {
          id: 45,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.publicUrl, result.item.inline.code)
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
      async function getFormEmbedding() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.embedding.get',
            params: {
              id: 45,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.publicUrl, result.item.inline.code)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFormEmbedding)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.form.embedding.get(
            id=45,
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
                'crm.form.embedding.get',
                [
                    'id' => 45,
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
        'crm.form.embedding.get',
        {
            id: 45,
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
        'crm.form.embedding.get',
        [
            'id' => 45,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "crm.form.embedding.get", b24.Params{
    	"id": 45,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.embedding.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	FormActive     bool   `json:"formActive"`
    	ResourcesReady bool   `json:"resourcesReady"`
    	PublicURL      string `json:"publicUrl"`
    	Inline         struct {
    		Code       *string `json:"code"`
    		PreviewURL string  `json:"previewUrl"`
    	} `json:"inline"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.PublicURL)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response without the `variant` parameter:

```json
{
    "result": {
        "item": {
            "formActive": true,
            "resourcesReady": true,
            "publicUrl": "https://example.bitrix24site.com/crm_form_nscnl/?telemetry_sign=v1%3A45%3Adirect_link.0960d174ab4aa87739668fbe63deae0a790b4f7254670a3b36d646588fa04242",
            "inline": {
                "code": "<script data-b24-form=\"inline/45/6el0zj\" data-b24-form-telemetry-sign=\"v1:45:site.d77c41d60d0649b2310bbe359cfd319772d16a2387b2e14f10c9e830ae76eb39\" data-skip-moving=\"true\">\n\t(function(w,d,u){\n\t\tvar s=d.createElement('script');s.async=true;s.src=u+'?'+(Date.now()/180000|0);\n\t\tvar h=d.getElementsByTagName('script')[0];h.parentNode.insertBefore(s,h);\n\t})(window,document,'https://cdn-com.bitrix24.com/b1234567/crm/form/loader_45.js');\n</script>",
                "previewUrl": "https://test.bitrix24.com/pub/form/45/6el0zj/?view=preview&preview=inline",
                "settings": null,
                "options": null
            },
            "click": {
                "code": "<script data-b24-form=\"click/45/6el0zj\" data-b24-form-telemetry-sign=\"v1:45:site.d77c41d60d0649b2310bbe359cfd319772d16a2387b2e14f10c9e830ae76eb39\" data-skip-moving=\"true\">\n\t(function(w,d,u){\n\t\tvar s=d.createElement('script');s.async=true;s.src=u+'?'+(Date.now()/180000|0);\n\t\tvar h=d.getElementsByTagName('script')[0];h.parentNode.insertBefore(s,h);\n\t})(window,document,'https://cdn-com.bitrix24.com/b1234567/crm/form/loader_45.js');\n</script>",
                "previewUrl": "https://test.bitrix24.com/pub/form/45/6el0zj/?view=preview&preview=click",
                "settings": {
                    "type": "panel",
                    "position": "right",
                    "vertical": "bottom",
                    "button": {
                        "use": false,
                        "text": null,
                        "font": null,
                        "align": null,
                        "plain": false,
                        "rounded": false,
                        "outlined": false,
                        "decoration": null,
                        "color": {
                            "text": null,
                            "textHover": null,
                            "background": null,
                            "backgroundHover": null
                        }
                    }
                },
                "options": {
                    "type": ["popup", "panel"],
                    "position": ["left", "center", "right"],
                    "vertical": ["bottom", "top"],
                    "button": {
                        "font": ["modern", "classic", "elegant"],
                        "align": ["inline", "left", "center", "right"]
                    }
                }
            },
            "auto": {
                "code": "<script data-b24-form=\"auto/45/6el0zj\" data-b24-form-telemetry-sign=\"v1:45:site.d77c41d60d0649b2310bbe359cfd319772d16a2387b2e14f10c9e830ae76eb39\" data-skip-moving=\"true\">\n\t(function(w,d,u){\n\t\tvar s=d.createElement('script');s.async=true;s.src=u+'?'+(Date.now()/180000|0);\n\t\tvar h=d.getElementsByTagName('script')[0];h.parentNode.insertBefore(s,h);\n\t})(window,document,'https://cdn-com.bitrix24.com/b1234567/crm/form/loader_45.js');\n</script>",
                "previewUrl": "https://test.bitrix24.com/pub/form/45/6el0zj/?view=preview&preview=auto",
                "settings": {
                    "type": "popup",
                    "position": "center",
                    "vertical": "bottom",
                    "delay": 5
                },
                "options": {
                    "type": ["popup", "panel"],
                    "position": ["left", "center", "right"],
                    "vertical": ["bottom", "top"],
                    "delay": [3, 5, 7, 10, 15, 20, 25, 30, 40, 60, 120]
                }
            }
        }
    },
    "time": {
        "start": 1792593809,
        "finish": 1792593809.843669,
        "duration": 0.8436689376831055,
        "processing": 0,
        "date_start": "2026-10-21T17:43:29+02:00",
        "date_finish": "2026-10-21T17:43:29+02:00",
        "operating_reset_at": 1792594380,
        "operating": 0
    }
}
```

Response with the `"variant":"auto"` and `"delay":10` parameters:

```json
{
    "result": {
        "item": {
            "variant": "auto",
            "code": "<script id=\"bx24_form_delay\" data-skip-moving=\"true\">\n\t(function(w,d,u,b){w['Bitrix24FormObject']=b;w[b] = w[b] || function(){arguments[0].ref=u;\n\t\t(w[b].forms=w[b].forms||[]).push(arguments[0])};\n\t\tif(w[b]['forms']) return;\n\t\tvar s=d.createElement('script');s.async=1;s.src=u+'?'+(1*new Date());\n\t\tvar h=d.getElementsByTagName('script')[0];h.parentNode.insertBefore(s,h);\n\t})(window,document,'https://test.bitrix24.com/bitrix/js/crm/form_loader.js','b24form');\n\n\tb24form({\"id\":45,\"lang\":\"en\",\"sec\":\"6el0zj\",\"type\":\"delay\",\"delay\":10});\n</script>"
        }
    },
    "time": {
        "start": 1792594117,
        "finish": 1792594117.571196,
        "duration": 0.5711960792541504,
        "processing": 0,
        "date_start": "2026-10-21T17:48:37+02:00",
        "date_finish": "2026-10-21T17:48:37+02:00",
        "operating_reset_at": 1792594680,
        "operating": 0.4092140197753906
    }
}
```

Response with the `"variant":"link"` parameter:

```json
{
    "result": {
        "item": {
            "variant": "link",
            "url": "https://example.bitrix24site.com/crm_form_nscnl/?telemetry_sign=v1%3A45%3Adirect_link.0960d174ab4aa87739668fbe63deae0a790b4f7254670a3b36d646588fa04242"
        }
    },
    "time": {
        "start": 1792594113,
        "finish": 1792594113.75192,
        "duration": 0.7519199848175049,
        "processing": 0,
        "date_start": "2026-10-21T17:48:33+02:00",
        "date_finish": "2026-10-21T17:48:33+02:00",
        "operating_reset_at": 1792594680,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **item**
[`object`](../../../data-types.md) | Form placement options. The content depends on the `variant` parameter:

- without `variant` — all options, [detailed description](#item)
- with `variant` — one selected option, [detailed description](#item-variant) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object Without the variant Parameter {#item}

#|
|| **Name**
`type` | **Description** ||
|| **formActive**
[`boolean`](../../../data-types.md) | Whether the form is active.

Possible values:

- `true` — the form is active and accepts submissions
- `false` — the form is deactivated. You can activate it with the [crm.form.activate](../crm-form-activate.md) method ||
|| **resourcesReady**
[`boolean`](../../../data-types.md) | Whether the form files are built for publication.

Possible values:

- `true` — the files are built, the embed codes are ready
- `false` — the files are not built yet, `code` is `null` for all options ||
|| **publicUrl**
[`string`](../../../data-types.md) | Public link to the form ||
|| **inline**
[`object`](../../../data-types.md) | Option for embedding the form in a website page. This option has no settings, so `settings` and `options` are `null`, [detailed description](#variant) ||
|| **click**
[`object`](../../../data-types.md) | Option for showing the form when a button or link is clicked, [detailed description](#variant) ||
|| **auto**
[`object`](../../../data-types.md) | Option for showing the form automatically after a set delay, [detailed description](#variant) ||
|#

#### Inline, Click, Auto Objects {#variant}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../../data-types.md) | HTML code to insert on a website. It is `null` until the form files are built ||
|| **previewUrl**
[`string`](../../../data-types.md) | Link to the form preview in this placement option ||
|| **settings**
[`object`](../../../data-types.md) | Current display settings. Present only for the `click` and `auto` options, [detailed description](#settings) ||
|| **options**
[`object`](../../../data-types.md) | Allowed values of the display settings. Present only for the `click` and `auto` options, [detailed description](#options) ||
|#

#### Settings Object {#settings}

If a setting value is not saved in the form, `null` is returned. In this case, the default value is applied on the website.

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
|| **delay**
[`integer`](../../../data-types.md) | Auto-display delay in seconds. Present only for the `auto` option ||
|| **button**
[`object`](../../../data-types.md) | Settings of the button that opens the form. Present only for the `click` option, [detailed description](#button) ||
|#

#### Button Object {#button}

#|
|| **Name**
`type` | **Description** ||
|| **use**
[`boolean`](../../../data-types.md) | Whether to show the Bitrix24 button on the website.

Possible values:

- `true` — show the button with the settings from this object
- `false` — do not show the button ||
|| **text**
[`string`](../../../data-types.md) | Button text ||
|| **font**
[`string`](../../../data-types.md) | Button font. Allowed values are listed in `click.options.button.font`, for example `modern`, `classic`, `elegant` ||
|| **align**
[`string`](../../../data-types.md) | Button alignment. Allowed values are listed in `click.options.button.align`, for example `inline`, `left`, `center`, `right` ||
|| **plain**
[`boolean`](../../../data-types.md) | Indicates a button without a background.

Possible values:

- `true` — the button has no background
- `false` — the button has a background ||
|| **rounded**
[`boolean`](../../../data-types.md) | Indicates rounded corners.

Possible values:

- `true` — the corners are rounded
- `false` — the corners are square ||
|| **outlined**
[`boolean`](../../../data-types.md) | Indicates a button outline.

Possible values:

- `true` — the button has an outline
- `false` — the button has no outline ||
|| **decoration**
[`string`](../../../data-types.md) | Button text underline. The interface offers the values `dotted`, `solid`, or an empty value ||
|| **color**
[`object`](../../../data-types.md) | Button colors in HEX format, for example `#3bc8f5` or `#3eddffa6` with transparency:

- `text` — text color
- `textHover` — text color on hover
- `background` — background color
- `backgroundHover` — background color on hover ||
|#

#### Options Object {#options}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string[]`](../../../data-types.md) | Allowed values of `settings.type` ||
|| **position**
[`string[]`](../../../data-types.md) | Allowed values of `settings.position` ||
|| **vertical**
[`string[]`](../../../data-types.md) | Allowed values of `settings.vertical` ||
|| **delay**
[`integer[]`](../../../data-types.md) | Allowed values of `settings.delay`. Present only for the `auto` option ||
|| **button**
[`object`](../../../data-types.md) | Allowed values of the button settings: `font` and `align`. Present only for the `click` option ||
|#

#### Item Object with the variant Parameter {#item-variant}

#|
|| **Name**
`type` | **Description** ||
|| **variant**
[`string`](../../../data-types.md) | Selected placement option: `inline`, `click`, `auto`, or `link` ||
|| **code**
[`string`](../../../data-types.md) | HTML code to insert on a website. Returned for the `inline`, `click`, and `auto` options ||
|| **url**
[`string`](../../../data-types.md) | Public link to the form. Returned for the `link` option ||
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
                "message": "delay is available only for the auto embedding variant.",
                "field": "delay"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the form `id` ||
|| `variant` | Field value is not one of the possible values: inline, click, auto, link | Pass one of the values `inline`, `click`, `auto`, `link` ||
|| `buttonType` | Field value is not one of the possible values: button, link | Pass `button` or `link` ||
|| `buttonText` | buttonText is available only for the click embedding variant. | Pass `buttonText` only together with `"variant":"click"` ||
|| `buttonType` | buttonType is available only for the click embedding variant. | Pass `buttonType` only together with `"variant":"click"` ||
|| `delay` | delay is available only for the auto embedding variant. | Pass `delay` only together with `"variant":"auto"` ||
|| `delay` | Embedding delay `11` is not supported | Pass a value from `auto.options.delay` ||
|#

#### Form Not Found Error

HTTP status: **404**

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `999999` was not found | Check the form identifier with the [crm.form.list](../crm-form-list.md) method ||
|#

#### Access Errors

HTTP status: **403**

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has permission to view CRM forms ||
|#

Error Code: `BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_PHONEVERIFICATIONREQUIREDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Confirm your phone number to add CRM forms | Verify the phone number in Bitrix24 and repeat the request ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-embedding-update.md)
- [{#T}](./crm-form-embedding-field-list.md)
- [{#T}](../crm-form-activate.md)
- [{#T}](./index.md)
- [{#T}](../crm-form-get.md)
