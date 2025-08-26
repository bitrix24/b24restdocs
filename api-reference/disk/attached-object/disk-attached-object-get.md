# Get Information About the Attached File disk.attachedObject.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no parameter descriptions
- parameter types not specified
- parameter requirements not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.attachedObject.get` returns information about the attached file through a user property by the binding identifier.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"disk.attachedObject.get",
    		{
    			id: 318
    		}
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
                'disk.attachedObject.get',
                [
                    'id' => 318,
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
        echo 'Error getting attached object: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.attachedObject.get",
        {
            id: 318
        },
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

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in Case of Success

> 200 OK

```json
result: {
    ID: "318",
    OBJECT_ID: "13215", //file identifier from Drive
    MODULE_ID: "blog", //module that owns the user property
    ENTITY_TYPE: "blog_comment", //entity type
    ENTITY_ID: "157", //identifier of the entity to which the attachment is made
    CREATE_TIME: "2018-10-31T10:57:35+02:00", //creation time
    CREATED_BY: "1", //identifier of the user who created the binding
    DOWNLOAD_URL: "https://test.bitrix24.com/bitrix/tools/disk/uf.php?attachedId=318&auth%5Baplogin%5D=1&auth%5Bap%5D=******&action=download&ncc=1",
    NAME: "Test.docx", //file name
    SIZE: "3867" //file size in bytes
}
```

## Continue Exploring

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)