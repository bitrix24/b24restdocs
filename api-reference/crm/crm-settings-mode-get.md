# Determine the Current CRM Operating Mode crm.settings.mode.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Method name: **crm.settings.mode.get**
>
> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method returns the current settings of the CRM operating mode: **classic CRM mode** (with leads) or **simple CRM mode** (without leads).

This mode affects a number of CRM operation scenarios, and for better understanding, we recommend reading the [relevant article](https://helpdesk.bitrix24.com/open/24207198/) in the user documentation.

## Method Parameters

The method is called without parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.settings.mode.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.settings.mode.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.settings.mode.get',
            {}
        );

        const result = response.getData().result;
        console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.settings.mode.get().response
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
        $response = $b24Service
            ->core
            ->call(
                'crm.settings.mode.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'CRM mode: ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting CRM settings mode: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod("crm.settings.mode.get", {}, result => {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.settings.mode.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.settings.mode.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.settings.mode.get: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the current CRM operating mode. Possible values are described [below](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Possible result Values {#result}

#|
|| **Value** | **Mode** | **Description** ||
|| `1` | Classic | CRM operates with leads ||
|| `2` | Simple | CRM operates without leads: new inquiries are converted directly into deals and contacts or companies ||
|#

The [crm.enum.settings.mode](./auxiliary/enum/crm-enum-settings-mode.md) method returns the current list of modes and their names.

## Error Handling

HTTP Status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the webhook token"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `insufficient_scope` | Insufficient token scope | The token does not include the `crm` scope ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./auxiliary/enum/crm-enum-settings-mode.md)
- [{#T}](./index.md)
- [{#T}](./leads/index.md)
