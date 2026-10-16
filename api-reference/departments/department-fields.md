# Get a list of department field names department.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: any user

Returns department field names.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.fields
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/department.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"department.fields", {}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.department.fields().response
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
                'department.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching department fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "department.fields", {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('department.fields');

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "department.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("department.fields: %w", err)
    }

    var item struct {
    	ID     string `json:"ID"`
    	Name   string `json:"NAME"`
    	Sort   string `json:"SORT"`
    	Parent string `json:"PARENT"`
    	UFHead string `json:"UF_HEAD"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "ID",
        "NAME": "Department Name",
        "SORT": "Sort Order",
        "PARENT": "Parent Department",
        "UF_HEAD": "Manager"
    },
    "time": {
        "start": 1736926600.426408,
        "finish": 1736926600.667272,
        "duration": 0.24086403846740723,
        "processing": 0.0050868988037109375,
        "date_start": "2025-01-15T07:36:40+00:00",
        "date_finish": "2025-01-15T07:36:40+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response. Its keys are department field codes, and its values are their names ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

### Fields of the `result` Object

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Name of the department identifier field. The value is `ID` ||
|| **NAME**
[`string`](../data-types.md) | Name of the department name field ||
|| **SORT**
[`string`](../data-types.md) | Name of the department sort order field ||
|| **PARENT**
[`string`](../data-types.md) | Name of the parent department field ||
|| **UF_HEAD**
[`string`](../data-types.md) \| [`null`](../data-types.md) | Name of the department manager field from the custom field settings. If the field is not configured, the value may be `null` ||
|#

The names of `NAME`, `SORT`, and `PARENT` are localized. The name of `UF_HEAD` comes from the custom field setting for the current language and may differ from the response example.

## Error Handling

HTTP status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the webhook token"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

The method has no error codes of its own. If the webhook or application does not have the `department` scope, the REST API returns `insufficient_scope`. The example shows the response for a webhook. For an application, `error_description` ends with `provided by the access token`. Other system errors are listed below.

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./department-add.md)
- [{#T}](./department-update.md)
- [{#T}](./department-get.md)
- [{#T}](./department-delete.md)
