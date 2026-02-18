# How to Update and Delete Files

In Bitrix24, there are two types of file fields.

- **File.** The field is not linked to Drive; files are uploaded directly through the [Base64](./how-to-upload-files.md) format. After uploading, the file ID is stored in the field.
  
- **File (Drive).** The field is linked to Drive, and the Drive object ID is stored in the field. This field does not process the Base64 format. To update Drive files, use the [disk.file.*](../disk/file/index.md) methods.

## How to Update a File

If the field is not multiple, upload a new file to the field using the `*.update` method. Use the [Base64](./how-to-upload-files.md) data transfer format. When a new file is uploaded, the old file will be automatically deleted.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"ID":525,"FIELDS":{"TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.template.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"ID":525,"FIELDS":{"TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.workflow.template.update',
    		{
    			ID: 525,
    			FIELDS: {
    				// Content of the file with the new workflow template
    				TEMPLATE_DATA: [
    					"bp-379.bpt", // The first element of the array - file name
    					"base64_encoded_content_here" // The second element of the array - file content encoded in base64
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	// Required logic for processing the result
    	processResult(result);
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
                'bizproc.workflow.template.update',
                [
                    'ID'     => 525,
                    'FIELDS' => [
                        'TEMPLATE_DATA' => [
                            "bp-379.bpt", // The first element of the array - file name
                            "base64_encoded_content_here" // The second element of the array - file content encoded in base64
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating workflow template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.update',
        {
            ID: 525,
            FIELDS: {
                // Content of the file with the new workflow template
                TEMPLATE_DATA: [
                    "bp-379.bpt", // The first element of the array - file name
                    "base64_encoded_content_here" // The second element of the array - file content encoded in base64
                ]
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.template.update',
        [
            'ID' => 525,
            'FIELDS' => [
                'TEMPLATE_DATA' => [
                    'bp-379.bpt',
                    'base64_encoded_content_here'
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To clear the field, pass an empty value.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.item.update",
    		{
    			id: 9,
    			entityTypeId: 177,
    			fields: {
    				ufCrm_7_1739432938: [ // empty value to remove the file from the field
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
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
                'crm.item.update',
                [
                    'id'           => 9,
                    'entityTypeId' => 177,
                    'fields'       => [
                        'ufCrm_7_1739432938' => [], // empty value to remove the file from the field
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating CRM item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.item.update",
        {
            id: 9,
            entityTypeId: 177,
            fields: {
                ufCrm_7_1739432938: [ // empty value to remove the file from the field
                ]
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.update',
        [
            'id' => 9,
            'entityTypeId' => 177,
            'fields' => [
                'ufCrm_7_1739432938' => []
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Update File in a Multiple Field

If the field is multiple, it stores an array of file IDs. When updating multiple file-type fields, consider the specifics of the methods.

### crm.item.update — update field in CRM object

To update fields in CRM objects, use the universal method [crm.item.update](../crm/universal/crm-item-update.md).

{% note info "" %}

It is not recommended to use the methods [crm.deal.update](../crm/deals/crm-deal-update.md), [crm.lead.update](../crm/leads/crm-lead-update.md), [crm.contact.update](../crm/contacts/crm-contact-update.md), [crm.company.update](../crm/companies/crm-company-update.md) for updating file fields.

{% endnote %}

#### 1. Get File IDs in the Field

Before updating the field, get the current file IDs to save them. You can use the [crm.item.get](../crm/universal/crm-item-get.md) method, which will return all fields of the item, or the [crm.item.list](../crm/universal/crm-item-list.md) method with a selection of only the required file-type field in `select`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"entityTypeId":177,"select":["ufCrm_7_1739432938"],"filter":{"id":"29"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"entityTypeId":177,"select":["ufCrm_7_1739432938"],"filter":{"id":"29"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.item.list',
        {
          entityTypeId: 177,
          select: [
            "ufCrm_7_1739432938", // file-type field
          ],
          filter: {
            "id": "29",
          },
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('crm.item.list', { entityTypeId: 177, select: ["ufCrm_7_1739432938"], filter: { "id": "29" } }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.item.list', { entityTypeId: 177, select: ["ufCrm_7_1739432938"], filter: { "id": "29" } }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.list',
                [
                    'entityTypeId' => 177,
                    'select' => [
                        "ufCrm_7_1739432938", // file-type field
                    ],
                    'filter' => [
                        "id" => "29",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 177,
            select: [
                "ufCrm_7_1739432938", // file-type field
            ],
            filter: {
                "id": "29",
            },
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 177,
            'select' => [
                'ufCrm_7_1739432938' // file-type field
            ],
            'filter' => [
                'id' => '29'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

In the response, we will receive information about the files: `ID` and download links.

```json
{
    "result": {
        "items": [
            {
                "ufCrm_7_1739432938": [
                    {
                        "id": 30577, // file ID, used to save the file in the field
                        "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=177&id=29&fieldName=UF_CRM_7_1739432938&fileId=30577",
                        "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b006e2cf200000001f0f107061147e530dda74d4e556cae7642992c&token=crm%7CYWN0aW9uPWNybS5jb25ZTU1NmNhZTc2NDI5OTJjIg%3D%3D.cR012fYj2JpQSObAORU0G8ZDvVc1Osnv0foUpBpaJVY%3D"
                    },
                    {
                        "id": 30581, // file ID, used to save the file in the field
                        "url": "https:///your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=177&id=29&fieldName=UF_CRM_7_1739432938&fileId=30581",
                        "urlMachine": "https:///your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b006e2cf200000001f0f107061147e530dda74d4e556cae7642992c&token=crm%7CYWNNmNhZTc2NDI5OTJjIg%3D%3D.l6GB1qKENuwQYtQHse4GK1r%2F3zps%2FQdh%2BlFsopOuJdU%3D"
                    }
                ]
            }
        ]
    },
}
```

#### 2. Update Files in the Field

Depending on the parameters passed, the [crm.item.update](../crm/universal/crm-item-update.md) method performs operations:

- uploading new files — pass content in [Base64](./how-to-upload-files.md) format,

- deleting old files — do not pass the `ID` of these files in the array,

- saving files — pass the `ID` in the array of files.

Files will be saved if their `ID` are listed in the request. Files will be deleted if their `ID` are not present in the request.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.item.update",
    		{
    			id: 9,
    			entityTypeId: 177,
    			fields: {
    				ufCrm_7_1739432938: [
    					{
    						id: 30577 // ID of the old file that will be saved in the field
    					},
    					[
    						"myNewFile.pdf", // Name of the new file
    						"base64_encoded_content_here" // Content of the new file in base64 format
    					]
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	// Required logic for processing data
    	processResult(result);
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
                'crm.item.update',
                [
                    'id'           => 9,
                    'entityTypeId' => 177,
                    'fields'       => [
                        'ufCrm_7_1739432938' => [
                            [
                                'id' => 30577 // ID of the old file that will be saved in the field
                            },
                            [
                                'myNewFile.pdf', // Name of the new file
                                'base64_encoded_content_here' // Content of the new file in base64 format
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating CRM item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.item.update",
        {
            id: 9,
            entityTypeId: 177,
            fields: {
                ufCrm_7_1739432938: [
                    {
                        id: 30577 // ID of the old file that will be saved in the field
                    },
                    [
                        "myNewFile.pdf", // Name of the new file
                        "base64_encoded_content_here" // Content of the new file in base64 format
                    ]
                ]
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.update',
        [
            'id' => 9,
            'entityTypeId' => 177,
            'fields' => [
                'ufCrm_7_1739432938' => [
                    [
                        'id' => 30577 // ID of the old file that will be saved in the field
                    },
                    [
                        'myNewFile.pdf', // Name of the new file
                        'base64_encoded_content_here' // Content of the new file in base64 format
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To delete all files, pass an empty array in the field.

### crm.timeline.comment.update — update files in a comment

To update files in comments of CRM items, use the [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md) method. Old files are always deleted when updating the field value. Upload new files to the field in [Base64](./how-to-upload-files.md) format.

To delete all files, pass an empty array in the `FILES` field.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":62589,"fields":{"COMMENT":"Comment was changed","FILES":[]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":62589,"fields":{"COMMENT":"Comment was changed","FILES":[]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.timeline.comment.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.timeline.comment.update',
    		{
    			id: 62589,
    			fields: {
    				"COMMENT": "Comment was changed",
    				"FILES": [ // empty value to remove files
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
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
                'crm.timeline.comment.update',
                [
                    'id' => 62589,
                    'fields' => [
                        'COMMENT' => 'Comment was changed',
                        'FILES' => [], // empty value to remove files
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating timeline comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.comment.update",
        {
            id: 62589,
            fields: {
                "COMMENT": "Comment was changed",
                "FILES": [ // empty value to remove files
                ]
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.comment.update',
        [
            'id' => 62589,
            'fields' => [
                'COMMENT' => 'Comment was changed',
                'FILES' => [] // empty value to remove files
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### lists.element.update — update field in the list

To upload new files to a list item field, pass files using the [lists.element.update](../lists/elements/lists-element-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the field.

To delete files, the `ID` of the property value will be needed.

#### 1. Get Property Value ID

To get the `ID` for deleting a file, execute the [lists.element.get](../lists/elements/lists-element-get.md) method, which will return all fields of the item.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"37","ELEMENT_ID":"6783"}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"37","ELEMENT_ID":"6783","auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/lists.element.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.element.get',
    		{
    			IBLOCK_TYPE_ID: 'lists',
    			IBLOCK_ID: '37',
    			ELEMENT_ID: '6783'
    		}
    	);
    	
    	const result = response.getData().result;
    	// Your required logic for processing data
    	processResult(result);
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
                'lists.element.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID'      => '37',
                    'ELEMENT_ID'     => '6783',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting list element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.element.get',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: '37',
            ELEMENT_ID: '6783'
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 37,
            'ELEMENT_ID' => 6783
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

The "file" field in the example is `PROPERTY_1075`. The field will contain information:

- the first value `"3693"` — this is the `ID` of the value, used for deletion,

- the second value `"31219"`— this is the `ID` of the file.

```json
{
    "result": [
        {
            "ID": "6783",
            "PROPERTY_1075": {
                "3693": "31219", // 3693 — ID of the value, used for deletion
                "3697": "31221", // 3697 — ID of the value, used for deletion
                "3699": "31223"  // 3699 — ID of the value, used for deletion
            }
        }
    ],
    "total": 1,
}
```

#### 2. Delete File from the Field

Pass the property field with the `_DEL` suffix to the [lists.element.update](../lists/elements/lists-element-update.md) method, for example, `PROPERTY_1075_DEL`. In the field, specify a list of `ID` property values that will be deleted:

- key — `ID` of the property value,

- value — `Y`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":6783,"FIELDS":{"NAME":"rest files","PROPERTY_1075_DEL":{"3693":"Y"}}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":6783,"FIELDS":{"NAME":"rest files","PROPERTY_1075_DEL":{"3693":"Y"}},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"lists.element.update",
    		{
    			IBLOCK_TYPE_ID: "lists",
    			IBLOCK_ID: 37,
    			ELEMENT_ID: 6783,
    			FIELDS: {
    				NAME: "rest files",
    				PROPERTY_1075_DEL: { // _DEL suffix for delete operation
    					3693: "Y" // list of values to delete
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	// Your required logic for processing data
    	processResult(result);
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
                'lists.element.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID'      => 37,
                    'ELEMENT_ID'     => 6783,
                    'FIELDS'         => [
                        'NAME'            => 'rest files',
                        'PROPERTY_1075_DEL' => [ // _DEL suffix for delete operation
                            3693 => 'Y' // list of values to delete
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating list element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "lists.element.update",
        {
            IBLOCK_TYPE_ID: "lists",
            IBLOCK_ID: 37,
            ELEMENT_ID: 6783,
            FIELDS: {
                NAME: "rest files",
                PROPERTY_1075_DEL: { // _DEL suffix for delete operation
                    3693: "Y" // list of values to delete
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 37,
            'ELEMENT_ID' => 6783,
            'FIELDS' => [
                'NAME' => 'rest files',
                'PROPERTY_1075_DEL' => [
                    3693 => 'Y' // _DEL suffix for delete operation
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### log.blogpost.update — update files in a post

To upload new files to a post in the feed, pass files using the [log.blogpost.update](../log/log-blogpost-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the post.

To delete files, their `ID` will be needed.

#### 1. Get File ID in the Post

To get the `ID` for deleting a file, execute the [log.blogpost.get](../log/log-blogpost-get.md) method, which will return all fields of the post, including `FILES`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"log.blogpost.get",
    		{
    			POST_ID: 211
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'log.blogpost.get',
                [
                    'POST_ID' => 211
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "log.blogpost.get",
        {
            POST_ID: 211
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogpost.get',
        [
            'POST_ID' => 211
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

In the response, we will receive an array of objects:

- the first value `0` — this is the sequential `ID` of the file in the post,

- the second value `437`— this is the `ID` of the file.

```json
[FILES] => Array
    (
        [0] => 437 
        [1] => 439
        [2] => 441
```

#### 2. Delete File from the Post

Pass the `FILES` field to the [log.blogpost.update](../log/log-blogpost-update.md) method. In the field, specify an array of `ID` files that will be deleted:

- key — `ID` of the file,

- value — `del`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","FILES":{"445":"del"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","FILES":{"445":"del"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"log.blogpost.update",
    		{
    			POST_ID: 211,
    			POST_TITLE: "New Post Title",
    			FILES: {
    				"445": "del" // ID of files to delete
    			}
    		}
    	);
    	
    	const result = response.getData().result;
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
                'log.blogpost.update',
                [
                    'POST_ID'    => 211,
                    'POST_TITLE' => 'New Post Title',
                    'FILES'      => [
                        '445' => 'del' // ID of files to delete
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "log.blogpost.update",
        {
            POST_ID: 211,
            POST_TITLE: "New Post Title",
            FILES: {
                "445": "del" // ID of files to delete
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogpost.update',
        [
            'POST_ID' => 211,
            'POST_TITLE' => 'New Post Title',
            'FILES' => [
                '445' => 'del' // ID of files to delete
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To delete all files from the post, pass the `UF_BLOG_POST_FILE` field to the [log.blogpost.update](../log/log-blogpost-update.md) method. In the field value, specify `["empty"]`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","UF_BLOG_POST_FILE":["empty"]}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","UF_BLOG_POST_FILE":["empty"],"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"log.blogpost.update",
    		{
    			POST_ID: 211,
    			POST_TITLE: "New Post Title",
    			UF_BLOG_POST_FILE: ["empty"] // delete all files from the post
    		}
    	);
    	
    	const result = response.getData().result;
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
                'log.blogpost.update',
                [
                    'POST_ID'         => 211,
                    'POST_TITLE'      => 'New Post Title',
                    'UF_BLOG_POST_FILE' => ['empty'], // delete all files from the post
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "log.blogpost.update",
        {
            POST_ID: 211,
            POST_TITLE: "New Post Title",
            UF_BLOG_POST_FILE: ["empty"] // delete all files from the post
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogpost.update',
        [
            'POST_ID' => 211,
            'POST_TITLE' => 'New Post Title',
            'UF_BLOG_POST_FILE' => ['empty'] // delete all files from the post
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### catalog.product.update — update field in the product

To upload new files to the product card, pass files using the [catalog.product.update](../catalog/product/catalog-product-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the field.

To delete files, the `ID` of the property value will be needed.

#### 1. Get Property Value ID

To get the `ID` for deleting a file, execute the [catalog.product.get](../catalog/product/catalog-product-get.md) method. The method will return all fields of the product.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/catalog.product.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.get',
    		{
    			'id': 541
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'catalog.product.get',
                [
                    'id' => 541
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting product information: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.get',
        {
            'id': 541
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.get',
        [
            'id' => 541
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

The "file" field in the example is `property1077`. The field contains an array of objects:

- `value` — this is the file information: `ID` and download links,

- `valueId` — this is the `ID` of the property value.

```json
{
    "result": {
        "product": {
            "iblockId": 25,
            "id": 541,
            "property1077": [
                {
                    "value": {
                        "id": "31251",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31251&fields%5BproductId%5D=541",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31251&fields%5BproductId%5D=541"
                    },
                    "valueId": "3705" // ID of the value, used for deletion
                },
                {
                    "value": {
                        "id": "31253",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541"
                    },
                    "valueId": "3707" // ID of the value, used for deletion
                }
            ],

        }
    },
}
```

#### 2. Delete File from the Field

To delete a file, pass the field to the [catalog.product.update](../catalog/product/catalog-product-update.md) method with the values:

- `value` — specify `remove` as the key, `Y` as the value,

- `valueId` — specify the `ID` of the property value whose file will be deleted.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"fields":{"property1077":[{"value":{"remove":"Y"},"valueId":"3705"}]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"fields":{"property1077":[{"value":{"remove":"Y"},"valueId":"3705"}]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/catalog.product.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.update',
    		{
    			id: 541,
    			fields: {
    				property1077: [
    					{
    						"value": {
    							'remove': 'Y', // operation to delete the file
    						},
    						'valueId': '3705', // ID of the value to delete
    					}
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Updated product with ID:', result);
    	// Your required logic for processing data
    	processResult(result);
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
                'catalog.product.update',
                [
                    'id' => 541,
                    'fields' => [
                        'property1077' => [
                            [
                                'value' => [
                                    'remove' => 'Y', // operation to delete the file
                                },
                                'valueId' => '3705', // ID of the value to delete
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.update',
        {
            id: 541,
            fields: {
                property1077: [
                    {
                        "value": {
                            'remove': 'Y', // operation to delete the file
                        },
                        'valueId': '3705', // ID of the value to delete
                    }
                ]
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.update',
        [
            'id' => 541,
            'fields' => [
                'property1077' => [
                    [
                        'value' => [
                            'remove' => 'Y' // operation to delete the file
                        },
                        'valueId' => '3705' // ID of the value to delete
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

