# Upload a new file to the root of the storage disk.storage.uploadfile

{% if build == 'dev' %}

{% note alert "TO-DO _not uploaded to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.uploadfile` uploads a new file to the root of the storage.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Storage identifier. ||
|| **fileContent**
[`unknown`](../../data-types.md) | Upload file in [Base64](../../files/how-to-upload-files.md) format. ||
|| **data**
[`unknown`](../../data-types.md) | Array describing the file. Required field `NAME` - name of the new file. ||
|| **generateUniqueName**
[`unknown`](../../data-types.md) | Optional, defaults to `false`. If set to `true`, a unique name will be generated for the uploaded file by adding a suffix (1), (2), etc. Example: avatar (1).jpg, avatar (2).jpg. ||
|| **rights**
[`unknown`](../../data-types.md) | Optional, defaults to an empty array. Array of access permissions for the uploaded file. ||
|#

## Example

{% note info %}

Please note that the list of available `TASK_ID` identifiers for setting permissions can be obtained using the method [disk.rights.getTasks](../rights/disk-rights-get-tasks.md).

{% endnote %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"disk.storage.uploadFile",
    		{
    			id: 4,
    			data: {
    				NAME: "avatar.jpg"
    			},
    			fileContent: document.getElementById('test_file_input'),
    			generateUniqueName: true,
    			rights: [
    				{
    					TASK_ID: 42,
    					ACCESS_CODE: 'U35' //access for user with ID=35
    				},
    				{
    					TASK_ID: 38,
    					ACCESS_CODE: 'U2' //access for user with ID=2
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
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
                'disk.storage.uploadFile',
                [
                    'id'               => 4,
                    'data'             => [
                        'NAME' => 'avatar.jpg',
                    ],
                    'fileContent'      => $_FILES['test_file_input'],
                    'generateUniqueName' => true,
                    'rights'           => [
                        [
                            'TASK_ID'     => 42,
                            'ACCESS_CODE' => 'U35', //access for user with ID=35
                        ],
                        [
                            'TASK_ID'     => 38,
                            'ACCESS_CODE' => 'U2', //access for user with ID=2
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error uploading file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.uploadFile",
        {
            id: 4,
            data: {
                NAME: "avatar.jpg"
            },
            fileContent: document.getElementById('test_file_input'),
            generateUniqueName: true,
            rights: [
                {
                    TASK_ID: 42,
                    ACCESS_CODE: 'U35' //access for user with ID=35
                },
                {
                    TASK_ID: 38,
                    ACCESS_CODE: 'U2' //access for user with ID=2
                }
            ]
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

## Response on success

> 200 OK

On success, returns a structure similar to that provided in [disk.file.get](../file/disk-file-get.md).

```json
"result": {
    "ID": "10",
    "NAME": "2511.jpg",
    "CODE": null,
    "STORAGE_ID": "4",
    "TYPE": "file",
    "PARENT_ID": "8",
    "DELETED_TYPE": "0",
    "CREATE_TIME": "2015-04-24T10:41:51+03:00",
    "UPDATE_TIME": "2015-04-24T15:52:43+03:00",
    "DELETE_TIME": null,
    "CREATED_BY": "1",
    "UPDATED_BY": "1",
    "DELETED_BY": "0",
    "DOWNLOAD_URL": "https://test.bitrix24.com/disk/downloadFile/10/?&ncc=1&filename=2511.jpg&auth=******",
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/file/2511.jpg"
}
```