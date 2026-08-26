# How to Update and Delete Files

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Replacing a file in a Bitrix24 field, deleting a single file from a multiple field, and retaining the rest of the files are all done by the `*.update` methods of the object that owns the file. Passing a new file is covered in the [How to Upload Files](./how-to-upload-files.md) article.

If you need to download a file before replacing or deleting it, retrieve the link using the [How to Download Files](./how-to-download-files.md) guide.

There is no single rule for all methods. Some methods delete the old files as soon as the field is passed in the request, others keep them and delete a file only on an explicit command. Before updating, check the [How Methods Handle Files](#behavior) table: an incorrect request format does not raise a method error — the files simply disappear from the field.

Updating a file means updating the object, so you need permissions to modify the object rather than the file itself. The permissions and the `scope` of every method are specified on its page.

Replace the identifiers in the examples with your own values:

- the names of file fields are returned by the [crm.item.fields](../crm/universal/crm-item-fields.md), [lists.field.get](../lists/fields/lists-field-get.md), and [catalog.productProperty.list](../catalog/product-property/catalog-product-property-list.md) methods

- the `entityTypeId` of a CRM object is returned by the [crm.enum.ownertype](../crm/auxiliary/enum/crm-enum-owner-type.md) method, and for Smart Processes by [crm.type.list](../crm/universal/user-defined-object-types/crm-type-list.md)

- the `IBLOCK_ID` of a list is returned by the [lists.get](../lists/lists/lists-get.md) method, and the `id` of a product by the [catalog.product.list](../catalog/product/catalog-product-list.md) method

- the identifiers of files, property values, and attachments are returned by the object read methods, which are collected in the [Where to Get the Identifiers](#identifiers) table

## Types of File Fields

- **File.** The field is not linked to Drive. A new file is passed directly into the field as a [Base64](./how-to-upload-files.md) string or as an array containing the file name and such a string. The field stores the `ID` of the file.

- **File (Drive).** The field is linked to Drive, and the field stores the `ID` of an object on Drive. Such fields exist in lists, tasks, and the feed. A new version of the file itself is uploaded with the [disk.file.uploadVersion](../disk/file/disk-file-upload-version.md) method, and all other operations with the Drive object are performed with the [disk.file.*](../disk/file/index.md) methods.

There are no "file (Drive)" type fields in the CRM: the file fields of CRM items are of the "file" type. Timeline comment attachments work differently not because they are a different field type, but because of how the method is built: parameter `FILES` accepts the file content, and Bitrix24 saves the file to Drive.

## How Methods Handle Files {#behavior}

#|
|| **Method** | **What happens to the old files** | **How to delete a file** ||
|| [crm.item.update](../crm/universal/crm-item-update.md) | Files whose `ID` are not passed in the array are deleted | Do not pass the `ID` of the file in the array ||
|| [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md) | All old files are deleted as soon as field `FILES` is passed in the request | Pass an empty array in field `FILES` ||
|| [lists.element.update](../lists/elements/lists-element-update.md) | Files remain in the field unchanged, but the other item fields that are not passed in the request are cleared | Pass the `ID` of the property value in a field with the `_DEL` suffix ||
|| [log.blogpost.update](../log/log-blogpost-update.md) | They remain in the post unchanged | Pass the attachment identifier with the `del` value in field `FILES` ||
|| [catalog.product.update](../catalog/product/catalog-product-update.md) | The values of a file property that are not passed are retained, but after a deletion the `valueId` of the remaining values changes | Pass `valueId` and `remove` with the `Y` value, then read the product again ||
|| [entity.item.update](../entity/items/entity-item-update.md) | The same as in lists: files remain in the property unchanged | Pass the `ID` of the property value in a field with the `_DEL` suffix ||
|| [tasks.task.update](../tasks/tasks-task-update.md) | Field `UF_TASK_WEBDAV_FILES` is overwritten entirely: only the files passed remain. If the field is not passed, the task files are not changed | Do not pass the attachment identifier in the array, or pass an empty array ||
|| [user.update](../user/user-update.md) | A new photo in field `PERSONAL_PHOTO` replaces the old one | Pass an empty string in the field ||
|| [bizproc.workflow.template.update](../bizproc/template/bizproc-workflow-template-update.md) | The old file in field `TEMPLATE_DATA` is replaced by the new one | It cannot be deleted, the field does not accept an empty value ||
|#

If a method is not in the table, the general rule applies: a single-value file field is replaced by a new file, and a multiple field is set by the request as a whole. No object method changes the files in a "file (Drive)" type field — they are handled by Drive methods, as described in the [Update a File in a "File (Drive)" Field](#disk-field) section.

## Where to Get the Identifiers {#identifiers}

An update request carries different numbers, and they must not be confused: the `ID` of the file, the `ID` of the property value, and the identifier of the file attachment to the object. What exactly a read method returns depends on the tool.

#|
|| **What is being updated** | **What the file field holds** | **What to read it with** ||
|| Field of a CRM object | `ID` of the file | [crm.item.get](../crm/universal/crm-item-get.md) ||
|| Timeline comment | `ID` of the file on Drive | [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) ||
|| Property of a list item or of a data storage item | The object key is the `ID` of the property value, and the value is the `ID` of the file | [lists.element.get](../lists/elements/lists-element-get.md), [entity.item.get](../entity/items/entity-item-get.md) ||
|| Feed post | The identifiers of the Drive file attachments to the post | [log.blogpost.get](../log/log-blogpost-get.md) ||
|| Product property | `value.id` is the `ID` of the file, and `valueId` is the `ID` of the property value | [catalog.product.get](../catalog/product/catalog-product-get.md) ||
|| Task | The identifiers of the Drive file attachments to the task | [tasks.task.get](../tasks/tasks-task-get.md) ||
|| "File (Drive)" field outside the CRM | The attachment identifier; the `ID` of the file is taken from `OBJECT_ID` | [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) ||
|#

In a list item property and in a product property, it is not the file that is deleted but the property value: the request carries the `ID` of the value rather than the `ID` of the file. Deleting a value does not delete the object on Drive — the file is deleted by [disk.file.delete](../disk/file/disk-file-delete.md).

## Update a File in a Single-Value Field

A new file is uploaded to a single-value field with the `*.update` method in the [Base64](./how-to-upload-files.md#array) format. The old file is deleted automatically.

In the example, the [bizproc.workflow.template.update](../bizproc/template/bizproc-workflow-template-update.md) method replaces the file of the workflow template with `ID` 525. The template identifier is returned by the [bizproc.workflow.template.add](../bizproc/template/bizproc-workflow-template-add.md) and [bizproc.workflow.template.list](../bizproc/template/bizproc-workflow-template-list.md) methods.

{% note warning "" %}

The method works only in the application context and only with the templates created by that same application. It cannot be called with a webhook — this returns the `ACCESS_DENIED` error with the `Application context required` message.

{% endnote %}

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

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
    					"bp-379.bpt", // The first element of the array — file name
    					"base64_encoded_content_here" // The second element of the array — file content encoded in base64
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
                            "bp-379.bpt", // The first element of the array — file name
                            "base64_encoded_content_here" // The second element of the array — file content encoded in base64
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
                    "bp-379.bpt", // The first element of the array — file name
                    "base64_encoded_content_here" // The second element of the array — file content encoded in base64
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "bizproc.workflow.template.update", b24.Params{
    	"ID": 525,
    	"FIELDS": b24.Params{
    		// The first element of the array is the file name, the second is the content in base64
    		"TEMPLATE_DATA": []string{"bp-379.bpt", "base64_encoded_content_here"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("bizproc.workflow.template.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The response returns the identifier of the updated template.

```json
{
    "result": 525
}
```

Not every method allows a single-value field to be cleared. In a file field of a CRM object, the value is deleted by an empty string, as described in the [How to Update a File Type Custom Field](../crm/universal/crm-item-update.md#how-to-update-a-file-type-custom-field) section. A new file is passed to a single-value field of a CRM object with the [crm.item.update](../crm/universal/crm-item-update.md) method as follows.

```json
{
    "entityTypeId": 177,
    "id": 29,
    "fields": {
        "ufCrm_7_1739432938": [
            "myNewFile.pdf",
            "base64_encoded_content_here"
        ]
    }
}
```

Such a field is cleared by an empty string.

```json
{
    "entityTypeId": 177,
    "id": 29,
    "fields": {
        "ufCrm_7_1739432938": ""
    }
}
```

The file of a workflow template cannot be deleted: an empty `TEMPLATE_DATA` value returns the `ERROR_TEMPLATE_VALIDATION_FAILURE` error with the `Incorrect field TEMPLATE_DATA!` message.

## Update a File in a Multiple Field

The order of work is the same: first retrieve the current state of the field with a read method, then call the update method. The request content differs from method to method: in the CRM, you pass the `ID` of the files to retain; in lists and the catalog, the identifiers of the property values; in the feed, the `ID` of the files with the `del` value. The `ID` of an existing file cannot be passed back into a list property — the method returns an error. Check the [How Methods Handle Files](#behavior) table.

### crm.item.update — Update a Field in a CRM Object {#crm-item-update}

To update fields in CRM objects, use the universal method [crm.item.update](../crm/universal/crm-item-update.md).

{% note info "" %}

It is not recommended to use the methods [crm.deal.update](../crm/deals/crm-deal-update.md), [crm.lead.update](../crm/leads/crm-lead-update.md), [crm.contact.update](../crm/contacts/crm-contact-update.md), [crm.company.update](../crm/companies/crm-company-update.md) for updating file fields.

{% endnote %}

#### 1. Get the File IDs in the Field

Before updating the field, retrieve the `ID` of the current files in order to retain them. You can use the [crm.item.get](../crm/universal/crm-item-get.md) method, which returns all fields of the item, or the [crm.item.list](../crm/universal/crm-item-list.md) method with only the required "file" type field in `select`.

The response returns the file information: the `ID` and the download links. The response is shortened, only the file field is shown. Retain the `id` values — they will be required in the next step. The `urlMachine` link contains an authorization token: do not publish it and do not record it in logs.

```json
{
    "result": {
        "items": [
            {
                "ufCrm_7_1739432938": [
                    {
                        "id": 30577,
                        "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=177&id=29&fieldName=UF_CRM_7_1739432938&fileId=30577",
                        "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b006e2cf200000001f0f107061147e530dda74d4e556cae7642992c&token=crm%7CYWN0aW9uPWNybS5jb25ZTU1NmNhZTc2NDI5OTJjIg%3D%3D.cR012fYj2JpQSObAORU0G8ZDvVc1Osnv0foUpBpaJVY%3D"
                    },
                    {
                        "id": 30581,
                        "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=177&id=29&fieldName=UF_CRM_7_1739432938&fileId=30581",
                        "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b006e2cf200000001f0f107061147e530dda74d4e556cae7642992c&token=crm%7CYWNNmNhZTc2NDI5OTJjIg%3D%3D.l6GB1qKENuwQYtQHse4GK1r%2F3zps%2FQdh%2BlFsopOuJdU%3D"
                    }
                ]
            }
        ]
    }
}
```

#### 2. Update the Files in the Field

Depending on the parameters passed, the [crm.item.update](../crm/universal/crm-item-update.md) method performs the following operations:

- uploading new files — pass the content in the [Base64](./how-to-upload-files.md#multiple-array) format

- deleting old files — do not pass the `ID` of those files in the array

- retaining files — pass their `ID` in the array of files

In the example, the file with `ID` 30577 remains in the field, the new file `myNewFile.pdf` is added, and the file with `ID` 30581 is deleted because it is not in the request.

{% note warning "" %}

The request sets the field as a whole. A file whose `ID` is not in the array is deleted without a warning and without an error in the response — retrieve the current list of files with step 1 before updating.

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":29,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":29,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.item.update",
    		{
    			id: 29,
    			entityTypeId: 177,
    			fields: {
    				ufCrm_7_1739432938: [
    					{
    						id: 30577 // ID of the old file that will be retained in the field
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
                    'id'           => 29,
                    'entityTypeId' => 177,
                    'fields'       => [
                        'ufCrm_7_1739432938' => [
                            [
                                'id' => 30577 // ID of the old file that will be retained in the field
                            ],
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
            id: 29,
            entityTypeId: 177,
            fields: {
                ufCrm_7_1739432938: [
                    {
                        id: 30577 // ID of the old file that will be retained in the field
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
            'id' => 29,
            'entityTypeId' => 177,
            'fields' => [
                'ufCrm_7_1739432938' => [
                    [
                        'id' => 30577 // ID of the old file that will be retained in the field
                    ],
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.update", b24.Params{
    	"id":           29,
    	"entityTypeId": 177,
    	"fields": b24.Params{
    		"ufCrm_7_1739432938": []any{
    			b24.Params{"id": 30577},                                     // ID of the old file that will be retained in the field
    			[]string{"myNewFile.pdf", "base64_encoded_content_here"},    // name and content of the new file
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.item.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The method returns an `item` object with the fields of the item. The response is shortened, only the file field is shown: the retained file 30577 and the new file with its own `id`, while file 30581 is no longer in the field.

```json
{
    "result": {
        "item": {
            "id": 29,
            "ufCrm_7_1739432938": [
                {
                    "id": 30577,
                    "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&fileId=30577",
                    "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b..."
                },
                {
                    "id": 30591,
                    "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&fileId=30591",
                    "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b..."
                }
            ]
        }
    }
}
```

To delete all files from the field, pass an empty array. In the example, the [crm.item.update](../crm/universal/crm-item-update.md) method clears field `ufCrm_7_1739432938` of the same item with `entityTypeId` 177 and `id` 29.

```json
{
    "id": 29,
    "entityTypeId": 177,
    "fields": {
        "ufCrm_7_1739432938": []
    }
}
```

The method returns an `item` object with the fields of the item. The response is shortened, only the file field is shown.

```json
{
    "result": {
        "item": {
            "id": 29,
            "ufCrm_7_1739432938": []
        }
    }
}
```

### crm.timeline.comment.update — Update the Files in a Comment {#crm-timeline-comment-update}

The files of a comment are updated by the [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md) method. The scenario has two steps.

1. Retrieve the `ID` of the comment with the [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) method
2. Pass the final set of files to `FILES` in the [Base64](./how-to-upload-files.md#multiple-array) format

The second step works differently from the other methods on this page: as soon as field `FILES` is in the request, all the previous files of the comment are deleted, and only what is passed in that request remains in the comment. The only way to retain an old file is to upload it again together with the new ones.

#### 1. Get the Comment ID

The [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) method requires a filter with two mandatory fields: `ENTITY_ID` — the identifier of the CRM item, and `ENTITY_TYPE` — the [CRM object type](../crm/data-types.md#object_type), for example `deal`.

The response returns the list of the item's comments. The response is shortened: it shows the `ID` of the comment for step 2 and the content of field `FILES`. The key of the `FILES` object matches the `id` field — this is the `ID` of the file on Drive.

```json
{
    "result": [
        {
            "ID": "62589",
            "ENTITY_ID": "2",
            "ENTITY_TYPE": "deal",
            "COMMENT": "Comment with files",
            "FILES": {
                "930": {
                    "id": 930,
                    "name": "1.gif",
                    "size": 43,
                    "urlDownload": "https://your-domain.bitrix24.com/disk/downloadFile/930/?&ncc=1&filename=1.gif"
                }
            }
        }
    ]
}
```

To keep a file in the comment, it has to be uploaded again — download it in a separate step.

The `urlDownload` link is not suitable for this: it leads to the Drive interface and works only within a user session. The key of the `FILES` object and the `id` field inside it are the `ID` of the file on Drive. Pass it to the [disk.file.get](../disk/file/disk-file-get.md) method, and it will return a signed `DOWNLOAD_URL` link.

```json
{
    "id": 930
}
```
The file is downloaded from that link with a separate GET request that carries the `User-Agent`, `Accept`, `Accept-Language`, and `Referer` headers according to the rules in the [How a Request Is Executed](../../settings/how-to-call-rest-api/general-principles.md#headers) article. The `DOWNLOAD_URL` link contains an authorization token: do not publish it and do not record it in logs. Encode the downloaded file in [Base64](./how-to-upload-files.md#how-to-encode-a-file-in-base64) and pass it in step 2 together with the new files.

#### 2. Update the Files in the Comment

To delete all files, pass an empty array in field `FILES`. Pass field `COMMENT` together with the files: the method does not accept an empty comment.

{% note warning "" %}

Field `FILES` overwrites the comment attachments entirely. All the files that are not in this request will be deleted, even if the field is passed for the sake of a single new file.

{% endnote %}

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.timeline.comment.update", b24.Params{
    	"id": 62589,
    	"fields": b24.Params{
    		"COMMENT": "Comment was changed",
    		"FILES":   []any{}, // empty value to remove files
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.comment.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The response returns the identifier of the updated comment.

```json
{
    "result": 62589
}
```

### lists.element.update — Update a Field in a List {#lists-element-update}

The [lists.element.update](../lists/elements/lists-element-update.md) method accepts new files in the same property in the [Base64](./how-to-upload-files.md#multiple-array) format — the old files remain in the field. Deletion is covered below: it requires a separate field and a preliminary request.

To delete files, you need the `ID` of the property value.

{% note warning "" %}

The method overwrites the item: regular fields whose values are not passed are cleared, so pass the other item fields, such as `NAME`, together with the request. The file property itself does not need to be passed — the files in it are retained, and passing the `ID` of an existing file raises an error.

{% endnote %}

#### 1. Get the Property Value ID

To retrieve the `ID` for deleting a file, call the [lists.element.get](../lists/elements/lists-element-get.md) method, which returns all fields of the item.

The "file" field in the example is `PROPERTY_1075`. The property arrives as an object in which:

- key `"3693"` is the `ID` of the property value, and it is what you pass for deletion

- value `"31219"` is the `ID` of the file

```json
{
    "result": [
        {
            "ID": "6783",
            "NAME": "rest files",
            "PROPERTY_1075": {
                "3693": "31219",
                "3697": "31221",
                "3699": "31223"
            }
        }
    ],
    "total": 1
}
```

#### 2. Delete a File from the Field

Pass a field with the `_DEL` suffix, for example `PROPERTY_1075_DEL`, to the [lists.element.update](../lists/elements/lists-element-update.md) method. In the field, specify the list of the `ID` of the property values that are to be deleted:

- key — the `ID` of the property value

- value — `Y`

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
    				PROPERTY_1075_DEL: { // _DEL suffix for the delete operation
    					"3693": "Y" // list of values to delete
    				}
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
                'lists.element.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID'      => 37,
                    'ELEMENT_ID'     => 6783,
                    'FIELDS'         => [
                        'NAME'            => 'rest files',
                        'PROPERTY_1075_DEL' => [ // _DEL suffix for the delete operation
                            '3693' => 'Y' // list of values to delete
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
                PROPERTY_1075_DEL: { // _DEL suffix for the delete operation
                    "3693": "Y" // list of values to delete
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
                'PROPERTY_1075_DEL' => [ // _DEL suffix for the delete operation
                    '3693' => 'Y' // list of values to delete
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "lists.element.update", b24.Params{
    	"IBLOCK_TYPE_ID": "lists",
    	"IBLOCK_ID":      37,
    	"ELEMENT_ID":     6783,
    	"FIELDS": b24.Params{
    		"NAME": "rest files",
    		// _DEL suffix for the delete operation, the key is the ID of the property value
    		"PROPERTY_1075_DEL": b24.Params{"3693": "Y"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("lists.element.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The method returns `true`.

```json
{
    "result": true
}
```

### log.blogpost.update — Update the Files in a Post {#log-blogpost-update}

The [log.blogpost.update](../log/log-blogpost-update.md) method accepts new files in field `FILES` in the [Base64](./how-to-upload-files.md#multiple-array) format — the old files remain in the post. Deletion is covered below: the `ID` of the file with the `del` value is passed in the same `FILES` field.

To delete files, you need their `ID`.

#### 1. Get the File ID in the Post

To retrieve the `ID` for deleting a file, call the [log.blogpost.get](../log/log-blogpost-get.md) method, which returns all fields of the post, including `FILES`.

Field `FILES` returns an array of attachment identifiers — the records stating that Drive files are attached to the post. These values are passed as keys on deletion, so take them from the `log.blogpost.get` response. To retrieve the file itself, pass the identifier to [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) and take `OBJECT_ID` from the response.

```json
{
    "result": [
        {
            "ID": "211",
            "TITLE": "New Regulations",
            "FILES": [
                437,
                439,
                441
            ]
        }
    ],
    "total": 1
}
```

#### 2. Delete a File from the Post

Pass field `FILES` to the [log.blogpost.update](../log/log-blogpost-update.md) method. In the field, specify the array of the `ID` of the files that are to be deleted:

- key — the `ID` of the file

- value — `del`

The method requires a title or a message text: if `POST_TITLE` or `POST_MESSAGE` is not passed, the `EMPTY_TITLE` error is returned. Pass the value of field `TITLE` from the [log.blogpost.get](../log/log-blogpost-get.md) response to the `POST_TITLE` parameter — the field names of the read and update methods differ. Otherwise the post will be renamed.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Regulations","FILES":{"437":"del"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Regulations","FILES":{"437":"del"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"log.blogpost.update",
    		{
    			POST_ID: 211,
    			POST_TITLE: "New Regulations",
    			FILES: {
    				"437": "del" // ID of the files to delete
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
                    'POST_TITLE' => 'New Regulations',
                    'FILES'      => [
                        '437' => 'del' // ID of the files to delete
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
            POST_TITLE: "New Regulations",
            FILES: {
                "437": "del" // ID of the files to delete
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
            'POST_TITLE' => 'New Regulations',
            'FILES' => [
                '437' => 'del' // ID of the files to delete
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "log.blogpost.update", b24.Params{
    	"POST_ID":    211,
    	"POST_TITLE": "New Regulations",
    	"FILES": b24.Params{
    		"437": "del", // ID of the files to delete
    	},
    })
    if err != nil {
    	return fmt.Errorf("log.blogpost.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The response returns the identifier of the updated post.

```json
{
    "result": 211
}
```

To delete all files from a post, pass field `UF_BLOG_POST_FILE` to the [log.blogpost.update](../log/log-blogpost-update.md) method. Specify `["empty"]` as the field value.

{% note warning "" %}

Do not pass `FILES` and `UF_BLOG_POST_FILE` in the same request. If field `FILES` is passed, the value of `UF_BLOG_POST_FILE` is not processed, and the files will remain in the post.

{% endnote %}

```json
{
    "POST_ID": 211,
    "POST_TITLE": "New Regulations",
    "UF_BLOG_POST_FILE": ["empty"]
}
```

As with the deletion of a single file, the response returns the identifier of the updated post.

```json
{
    "result": 211
}
```

### catalog.product.update — Update a Field in a Product {#catalog-product-update}

The [catalog.product.update](../catalog/product/catalog-product-update.md) method accepts new files in a product property in the [Base64](./how-to-upload-files.md#multiple-value) format. Deleting a file from a property is covered below.

To delete files, you need the `ID` of the field value.

{% note info "" %}

The values of a multiple file property that are not passed in the request are retained: there is no need to list the other values in order to delete a single file. At the same time, the `valueId` of the remaining values changes after a deletion — read the product again before the next deletion.

{% endnote %}

#### 1. Get the Field Value ID

To retrieve the `ID` for deleting a file, call the [catalog.product.get](../catalog/product/catalog-product-get.md) method. The method returns all fields of the product.

The "file" field in the example is `property1077`. The field contains an array of objects:

- `value` is the file information: the `ID` and the download links

- `valueId` is the `ID` of the field value

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
                    "valueId": "3705"
                },
                {
                    "value": {
                        "id": "31253",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541"
                    },
                    "valueId": "3707"
                }
            ]
        }
    }
}
```

#### 2. Delete a File from a Product Property

To delete a file, pass the field to the [catalog.product.update](../catalog/product/catalog-product-update.md) method with the values:

- `value` — specify `remove` as the key and `Y` as the value

- `valueId` — specify the `ID` of the field value whose file is to be deleted

In the example, value `3705` is deleted from property `property1077`. The second value, `3707`, is not passed in the request and remains in the property.

A single request can delete an old file and upload a new one: the array holds an element with `remove` next to an element with the content in the [Base64](./how-to-upload-files.md#multiple-value) format.

```json
{
    "id": 541,
    "fields": {
        "property1077": [
            {
                "value": {
                    "remove": "Y"
                },
                "valueId": "3705"
            },
            {
                "value": {
                    "fileData": [
                        "blue_pixel.txt",
                        "base64_encoded_content_here"
                    ]
                }
            }
        ]
    }
}
```

The example below only deletes value `3705`.

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
    						value: {
    							remove: 'Y' // operation to delete the file
    						},
    						valueId: '3705' // ID of the value to delete
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
                                ],
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
                        value: {
                            remove: 'Y' // operation to delete the file
                        },
                        valueId: '3705' // ID of the value to delete
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
                        ],
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.update", b24.Params{
    	"id": 541,
    	"fields": b24.Params{
    		"property1077": []b24.Params{
    			{
    				"value":   b24.Params{"remove": "Y"}, // operation to delete the file
    				"valueId": "3705",                   // ID of the value to delete
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

The method returns an `element` object with the fields of the product. The response is shortened, only the file property is shown: the deleted value is gone, and the second file remains but has taken the `valueId` of the deleted value.

```json
{
    "result": {
        "element": {
            "id": 541,
            "property1077": [
                {
                    "value": {
                        "id": "31253",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property1077&fields%5BfileId%5D=31253&fields%5BproductId%5D=541"
                    },
                    "valueId": "3705"
                }
            ]
        }
    }
}
```

## Update a File in a "File (Drive)" Field {#disk-field}

Such a field holds not the file itself but an **attachment identifier** — the record stating that a Drive file is attached to this object. The object method does not change it, so you have to work with the Drive object. The attachment is returned by [lists.element.get](../lists/elements/lists-element-get.md), [log.blogpost.get](../log/log-blogpost-get.md), [tasks.task.get](../tasks/tasks-task-get.md), and other read methods.

A CRM timeline comment is not part of this scenario: the [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) method returns the `ID` of the file on Drive right away, so the attachment step is not needed.

The scenario has three steps.

1. Retrieve the attachment identifier with the object read method

2. Pass it to the [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) method and take `OBJECT_ID` from the response — this is the `ID` of the file on Drive

    ```json
    {
        "result": {
            "ID": 495,
            "OBJECT_ID": 9035,
            "DOWNLOAD_URL": "https://your-domain.bitrix24.com/bitrix/tools/disk/uf.php?attachedId=495&auth[auth]=d78a4a69...&action=download&ncc=1"
        }
    }
    ```

    The `DOWNLOAD_URL` link in this response contains the authorization token itself, and for a webhook the token has no expiration date. Do not publish such a link and do not record it in logs.

3. Upload a new version of the file with the [disk.file.uploadVersion](../disk/file/disk-file-upload-version.md) method: the file is passed in the [fileContent](./how-to-upload-files.md#filecontent) parameter as an array containing the file name and a Base64 string

    ```json
    {
        "id": 9035,
        "fileContent": ["report.pdf", "base64_encoded_content_here"]
    }
    ```

The object field is not changed in the process — it keeps the same attachment, while the file on Drive gets a new version. To remove the file, delete the object with the [disk.file.delete](../disk/file/disk-file-delete.md) method or clear the object field according to the rules of its method from the [How Methods Handle Files](#behavior) table.

Check the result with the [disk.file.get](../disk/file/disk-file-get.md) method — it returns the name, size, and `DOWNLOAD_URL` link of the current version.

The file fields of CRM items are not part of this scenario either: they are of the "file" type and are updated together with the item by the [crm.item.update](../crm/universal/crm-item-update.md) method.

## Files in Other Objects {#other-objects}

The other Bitrix24 objects have no dedicated scenario on this page: updating either repeats the scenarios above or is not available via REST.

#|
|| **Object** | **Replace a file** | **Delete a file** ||
|| Task | [tasks.task.update](../tasks/tasks-task-update.md) — field `UF_TASK_WEBDAV_FILES`; [tasks.task.file.attach](../tasks/tasks-task-file-attach.md) adds a file to the ones already attached | Via [tasks.task.update](../tasks/tasks-task-update.md), there is no dedicated method for detaching ||
|| User photo | [user.update](../user/user-update.md) — field `PERSONAL_PHOTO` | An empty string in field `PERSONAL_PHOTO` ||
|| Additional product images | There is no update method: delete the image and add a new one with the [catalog.productImage.add](../catalog/product-image/catalog-product-image-add.md) method | [catalog.productImage.delete](../catalog/product-image/catalog-product-image-delete.md), both the `id` of the image and `productId` are required ||
|| Document template | [documentgenerator.template.update](../document-generator/templates/document-generator-template-update.md) — the field is single-value, and a new file replaces the old one | The file is not deleted separately, the whole template is deleted instead ||
|| Knowledge base | Not possible: REST provides only [note.file.add](../note/file/note-file-add.md) and [note.file.get](../note/file/note-file-get.md) | Not possible ||
|| Chat | Not possible: the file is uploaded again with the [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method | Not possible ||
|| Call recording | Not possible | Not possible ||
|| Site | By uploading again with the [landing.block.uploadfile](../landing/block/methods/landing-block-upload-file.md) method | There is no dedicated method ||
|#

In a task, the field stores attachment identifiers rather than the `ID` of the files on Drive. To retain an attached file, pass its attachment identifier as a number, and pass a new file as a string such as `"n9851"`, where the number after `n` is the `ID` of the file on Drive.

```json
{
    "taskId": 4017,
    "fields": {
        "UF_TASK_WEBDAV_FILES": [567, "n9851"]
    }
}
```

Clearing the field detaches the files from the task but does not delete them from Drive: the file itself is deleted by [disk.file.delete](../disk/file/disk-file-delete.md).

## Next Steps

- [How to Upload Files](./how-to-upload-files.md) — file transfer formats and uploading multiple files to a multiple field

- [How to Download Files](./how-to-download-files.md) — retrieving a file link and downloading it with a separate `GET` request

- [How to Work with Files](./index.md) — a section overview: field types, linking files to Bitrix24 objects, and the core Drive methods

- [Data Encoding](../../settings/how-to-call-rest-api/data-encoding.md) — how to pass data in GET requests and cURL
