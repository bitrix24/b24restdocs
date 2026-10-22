# Get a List of CRM Form Scenarios crm.form.scenario.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.form.scenario.list` method returns CRM form scenarios — form templates for specific tasks, such as a callback or event registration.

## Method Parameters

No parameters. The method returns the entire directory. Filtering, sorting, pagination, and `select` are not supported: if you pass them in the request, the method ignores them.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.scenario.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.scenario.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.scenario.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormScenarioListResult = {
      items: Array<{
        id: string
        title: string
        description: string | null
        categoryId: string
        iconUrl: string | null
        sections: string[]
        expertSections: string[]
        defaultSection: string | null
        openable: boolean
        soon: boolean
        actions: string[]
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FormScenarioListResult>({
        method: 'crm.form.scenario.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        // Scenarios that open the form editor right away
        console.info(result.items.filter(scenario => scenario.openable && !scenario.soon))
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
      async function getFormScenarios() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.scenario.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFormScenarios)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.form.scenario.list().response
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
                'crm.form.scenario.list',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.scenario.list',
        {},
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
        'crm.form.scenario.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.form.scenario.list", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.scenario.list: %w", err)
    }

    // The method wraps the list in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	ID         string `json:"id"`
    	Title      string `json:"title"`
    	CategoryID string `json:"categoryId"`
    	Openable   bool   `json:"openable"`
    	Soon       bool   `json:"soon"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, scenario := range items {
    	fmt.Println(scenario.ID, scenario.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The example is shortened: three of the 26 scenarios are left in the list.

```json
{
    "result": {
        "items": [
            {
                "id": "callback",
                "title": "Callback",
                "description": "Obtain customer's phone number and call them back",
                "categoryId": "crm",
                "iconUrl": "/bitrix/images/crm/webform/icons/revertcall.svg",
                "sections": ["fields", "agreements", "header-and-button", "crm", "callback", "design", "other"],
                "expertSections": ["fields", "booking-resource-auto-selection", "agreements", "crm", "pay-systems", "header-and-button", "spam-protection", "fields-rules", "actions", "default-values", "analytics", "callback", "design", "other"],
                "defaultSection": "callback",
                "openable": true,
                "soon": false,
                "actions": []
            },
            {
                "id": "contacts",
                "title": "Contact information",
                "description": "Obtain customer's name, phone number and email address",
                "categoryId": "crm",
                "iconUrl": "/bitrix/images/crm/webform/icons/contacts.svg",
                "sections": ["fields", "agreements", "header-and-button", "crm", "design", "other"],
                "expertSections": ["fields", "booking-resource-auto-selection", "agreements", "crm", "pay-systems", "header-and-button", "spam-protection", "fields-rules", "actions", "default-values", "analytics", "callback", "design", "other"],
                "defaultSection": "fields",
                "openable": true,
                "soon": false,
                "actions": []
            },
            {
                "id": "personalisation",
                "title": "Personalized form",
                "description": "Fill in the known form fields for a recognized customer",
                "categoryId": "crm_automation",
                "iconUrl": "/bitrix/images/crm/webform/icons/personalization.svg",
                "sections": ["fields", "agreements", "header-and-button", "crm", "design", "other"],
                "expertSections": [],
                "defaultSection": "fields",
                "openable": false,
                "soon": false,
                "actions": ["showHelp"]
            }
        ]
    },
    "time": {
        "start": 1792593642,
        "finish": 1792593642.129926,
        "duration": 0.12992596626281738,
        "processing": 0,
        "date_start": "2026-10-21T17:40:42+02:00",
        "date_finish": "2026-10-21T17:40:42+02:00",
        "operating_reset_at": 1792594200,
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
|| **items**
[`object[]`](../../../data-types.md) | Array of scenarios [(detailed description)](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Scenario code, for example `callback`, `contacts`, `event_registration` ||
|| **title**
[`string`](../../../data-types.md) | Scenario name ||
|| **description**
[`string`](../../../data-types.md) | Scenario description or `null` ||
|| **categoryId**
[`string`](../../../data-types.md) | Scenario category code. Category names are returned by the [crm.form.scenariocategory.list](./crm-form-scenariocategory-list.md) method ||
|| **iconUrl**
[`string`](../../../data-types.md) | Path to the scenario icon in Bitrix24, or `null` if there is no icon ||
|| **sections**
[`string[]`](../../../data-types.md) | Builder sections the scenario opens for configuration, for example `fields`, `agreements`, `crm`, `design` ||
|| **expertSections**
[`string[]`](../../../data-types.md) | Builder sections the scenario opens in advanced mode. May be empty ||
|| **defaultSection**
[`string`](../../../data-types.md) | Section the builder opens on, or `null` ||
|| **openable**
[`boolean`](../../../data-types.md) | Indicates whether the scenario opens the form builder.

Possible values:

- `true` — the scenario opens the builder
- `false` — the scenario does not open the builder but performs an action from `actions` ||
|| **soon**
[`boolean`](../../../data-types.md) | Indicates an announced scenario.

Possible values:

- `true` — the scenario is announced but not yet available
- `false` — the scenario is available ||
|| **actions**
[`string[]`](../../../data-types.md) | Additional scenario actions, for example `showHelp` or `check`. For most scenarios, the array is empty ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION",
        "message": "Access denied"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Grant the user permission to view CRM forms ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-scenariocategory-list.md)
- [{#T}](./crm-form-configuration-get.md)
- [{#T}](../crm-form-add.md)
- [{#T}](./crm-form-scenario-field-list.md)
- [{#T}](./index.md)
