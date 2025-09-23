# Get a list of available access levels disk.rights.getTasks

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.rights.getTasks` allows you to retrieve a list of access levels that can be used for assigning permissions. It returns the available access levels.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the access level. ||
|| **NAME**
[`unknown`](../../data-types.md) |  Symbolic code. ||
|| **TITLE**
[`unknown`](../../data-types.md) |  Title. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"disk.rights.getTasks",
    		{}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.rights.getTasks',
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
        echo 'Error getting tasks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.rights.getTasks",
        {},
        function (result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    )
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on success

> 200 OK

```json
{
    "result": [
        {
            "ID": "42",
            "NAME": "disk_access_full",
            "TITLE": "Full access"
        },
        {
            "ID": "40",
            "NAME": "disk_access_edit",
            "TITLE": "Editing"
        },
        {
            "ID": "38",
            "NAME": "disk_access_read",
            "TITLE": "Reading"
        }
    ]
}
```