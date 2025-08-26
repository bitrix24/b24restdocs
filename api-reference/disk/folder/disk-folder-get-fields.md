# Get Folder Fields Description disk.folder.getfields

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.getfields` returns the description of folder fields.

- `TYPE` — field type;
- `USE_IN_FILTER` — whether the field can be used for filtering the selection;
- `USE_IN_SHOW` — whether this field is available in the response.

## Parameters

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"disk.folder.getfields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.folder.getfields',
                []
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
        echo 'Error getting folder fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.getfields",
        {},
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK

```json
"result": {
    "ID": {
        "TYPE": "integer",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "NAME": {
        "TYPE": "string",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "TYPE": {
        "TYPE": "enum",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "CODE": {
        "TYPE": "string",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "STORAGE_ID": {
        "TYPE": "integer",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "REAL_OBJECT_ID": {
        "TYPE": "integer",
        "USE_IN_FILTER": false,
        "USE_IN_SHOW": true
    },
    "PARENT_ID": {
        "TYPE": "integer",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "CREATE_TIME": {
        "TYPE": "datetime",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "UPDATE_TIME": {
        "TYPE": "datetime",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "DELETE_TIME": {
        "TYPE": "datetime",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    },
    "CREATED_BY": {
        "TYPE": "integer",
        "USE_IN_FILTER": false,
        "USE_IN_SHOW": true
    },
    "UPDATED_BY": {
        "TYPE": "integer",
        "USE_IN_FILTER": false,
        "USE_IN_SHOW": true
    },
    "DELETED_BY": {
        "TYPE": "integer",
        "USE_IN_FILTER": false,
        "USE_IN_SHOW": true
    },
    "DELETED_TYPE": {
        "TYPE": "enum",
        "USE_IN_FILTER": true,
        "USE_IN_SHOW": true
    }
}
```