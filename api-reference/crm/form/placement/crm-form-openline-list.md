# Get a List of Open Channels for a CRM Form crm.form.openline.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms and to edit Open Channels

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.form.openline.list` method returns Open Channels and shows whether the specified CRM form is the welcome form in them.

{% note info "" %}

The method only reads data: you cannot set a form as the welcome form via REST.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter by CRM form. Accepts exactly one condition on the `formId` field with the `=` operator:

```json
[["formId", "=", 45]]
```

Other fields, operators, `or` logic, and negation are not supported.

The form identifier can be obtained using the [crm.form.list](../crm-form-list.md) method ||
|| **order**
[`object`](../../../data-types.md) | Sorting by Open Channel identifier. Accepts only the `id` field.

Possible values:

- `ASC` — ascending
- `DESC` — descending

For example, `{"id": "DESC"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:

- `limit` — the number of Open Channels per page, from 1 to 1000. The default is `50`
- `offset` — how many Open Channels to skip from the start of the selection. The default is `0` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.openline.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["formId","=",49]],"order":{"id":"ASC"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.openline.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["formId","=",49]],"order":{"id":"ASC"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.openline.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FormOpenLineListResult = {
      items: Array<{
        formId: number
        id: number
        name: string
        bound: boolean
        welcomeFormEnabled: boolean
        welcomeFormId: number | null
        welcomeFormDelayed: boolean
        previewUrl: string | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FormOpenLineListResult>({
        method: 'crm.form.openline.list',
        params: {
          filter: [['formId', '=', 49]],
          order: { id: 'ASC' },
          pagination: { limit: 3 },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(item => console.info(item.id, item.name, item.bound))
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
      async function getFormOpenLines() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.openline.list',
            params: {
              filter: [['formId', '=', 49]],
              order: { id: 'ASC' },
              pagination: { limit: 3 },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(item => console.info(item.id, item.name, item.bound))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFormOpenLines)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.form.openline.list(
            filter=[["formId", "=", 49]],
            order={"id": "ASC"},
            pagination={"limit": 3},
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
                'crm.form.openline.list',
                [
                    'filter' => [['formId', '=', 49]],
                    'order' => ['id' => 'ASC'],
                    'pagination' => ['limit' => 3],
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
        'crm.form.openline.list',
        {
            filter: [['formId', '=', 49]],
            order: { id: 'ASC' },
            pagination: { limit: 3 },
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
        'crm.form.openline.list',
        [
            'filter' => [['formId', '=', 49]],
            'order' => ['id' => 'ASC'],
            'pagination' => ['limit' => 3],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "crm.form.openline.list", b24.Params{
    	"filter":     [][]any{{"formId", "=", 49}},
    	"order":      map[string]string{"id": "ASC"},
    	"pagination": map[string]int{"limit": 3},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.openline.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	FormID             int     `json:"formId"`
    	ID                 int     `json:"id"`
    	Name               string  `json:"name"`
    	Bound              bool    `json:"bound"`
    	WelcomeFormEnabled bool    `json:"welcomeFormEnabled"`
    	WelcomeFormID      *int    `json:"welcomeFormId"`
    	WelcomeFormDelayed bool    `json:"welcomeFormDelayed"`
    	PreviewURL         *string `json:"previewUrl"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.ID, item.Name, item.Bound)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "formId": 49,
                "id": 1,
                "name": "Open Channel ",
                "bound": false,
                "welcomeFormEnabled": false,
                "welcomeFormId": 25,
                "welcomeFormDelayed": true,
                "previewUrl": "https://test.bitrix24.com/pub/form/49/7vo9z0/?view=preview&preview=ol&preview_id=1"
            },
            {
                "formId": 49,
                "id": 3,
                "name": "Wix",
                "bound": false,
                "welcomeFormEnabled": true,
                "welcomeFormId": 25,
                "welcomeFormDelayed": true,
                "previewUrl": "https://test.bitrix24.com/pub/form/49/7vo9z0/?view=preview&preview=ol&preview_id=3"
            },
            {
                "formId": 49,
                "id": 5,
                "name": "Open Channel 4",
                "bound": false,
                "welcomeFormEnabled": false,
                "welcomeFormId": 25,
                "welcomeFormDelayed": true,
                "previewUrl": null
            }
        ]
    },
    "time": {
        "start": 1792594222,
        "finish": 1792594222.68616,
        "duration": 0.6861600875854492,
        "processing": 0,
        "date_start": "2026-10-21T17:50:22+02:00",
        "date_finish": "2026-10-21T17:50:22+02:00",
        "operating_reset_at": 1792594800,
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
|| **items**
[`array`](../../../data-types.md) | List of Open Channels, [detailed description](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Array Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **formId**
[`integer`](../../../data-types.md) | CRM form identifier from the request filter ||
|| **id**
[`integer`](../../../data-types.md) | Open Channel identifier ||
|| **name**
[`string`](../../../data-types.md) | Open Channel name ||
|| **bound**
[`boolean`](../../../data-types.md) | Whether the form from the filter is the welcome form of this channel.

Possible values:

- `true` — the form is set as the welcome form
- `false` — the form is not set as the welcome form ||
|| **welcomeFormEnabled**
[`boolean`](../../../data-types.md) | Whether a welcome form is enabled in the channel: this one or another.

Possible values:

- `true` — the welcome form is enabled
- `false` — the welcome form is disabled ||
|| **welcomeFormId**
[`integer`](../../../data-types.md) | Identifier of the channel's welcome form. Equals `null` if no form is selected ||
|| **welcomeFormDelayed**
[`boolean`](../../../data-types.md) | When the welcome form is shown.

Possible values:

- `true` — after the visitor's first message
- `false` — before the visitor's first message ||
|| **previewUrl**
[`string`](../../../data-types.md) | Link to the preview of the website widget with this Open Channel. Equals `null` if the channel is not connected to any active widget. The form from the filter appears in the preview only if `bound` is `true` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"formId\" is required.",
                "field": "filter.formId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Filter and Request Parameter Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass `filter` with the condition `[["formId", "=", <form_id>]]` ||
|| `id` | DTO `OpenLineDto` in field `id` requires attribute `Filterable` to perform this request | Filter only by `formId` ||
|| `name` | DTO `OpenLineDto` in field `name` requires attribute `Sortable` to perform this request | Sort only by `id` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.formId` | Filter for required field `formId` is required | Pass a condition on `formId` with the `=` operator ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `[["formId","=",0]]` | Pass a single condition `[["formId", "=", <form_id>]]` with a positive identifier, without `or` logic or negation ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `UP` | Pass `order` as `{"id": "ASC"}` or `{"id": "DESC"}` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":1001}` | Pass `limit` from 1 to 1000 and `offset` of at least 0 ||
|#

#### Form Not Found Error

HTTP status: **404**

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `formId` | Entity with ID `999999` was not found | Check the form identifier using the [crm.form.list](../crm-form-list.md) method ||
|#

#### Access Errors

HTTP status: **403**

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view CRM forms ||
|#

Error Code: `BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_OPENLINEACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient permission to edit Open Channels | Grant the user permission to edit Open Channels ||
|#

Error Code: `BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_OPENLINESUNAVAILABLEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The "Open Channels" module is required | Install the Open Channels module in Bitrix24 ||
|#

Error Code: `BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_PHONEVERIFICATIONREQUIREDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Confirm your phone number to add CRM forms | Verify the phone number in Bitrix24 and repeat the request ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-openline-field-list.md)
- [{#T}](./crm-form-sitebutton-list.md)
- [{#T}](./crm-form-embedding-get.md)
- [{#T}](./index.md)
