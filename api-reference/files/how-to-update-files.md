# How to Update and Delete Files

In Bitrix24, there are two types of file fields.

- **File.** This field is not linked to Drive; files are uploaded directly through the [Base64](./how-to-upload-files.md) format. After uploading, the file ID is stored in the field.
  
- **File (Drive).** This field is linked to Drive, and it stores the Drive object ID. This field does not process the Base64 format. To update Drive files, use the [disk.file.*](../disk/file/index.md) methods.

## How to Update a File

If the field is not multiple, upload a new file to the field using the `*.update` method. Use the [Base64](./how-to-upload-files.md) data transfer format. When uploading a new file, the old file will be automatically deleted.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.update',
        {
            ID: 525,
            FIELDS: {
                // Content of the file with the new business process template
                TEMPLATE_DATA: [
                    "bp-379.bpt", // The first element of the array - file name
                    "base64_encoded_content_here" // The second element of the array - file content encoded in base64
                ]
            }
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"ID":525,"FIELDS":{"TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/bizproc.workflow.template.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"ID":525,"FIELDS":{"TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.update
    ```

- PHP

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

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- PHP

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

If the field is multiple, it stores an array of file `IDs`. When updating multiple file fields, consider the specifics of the methods.

### crm.item.update — update field in CRM object

To update fields in CRM objects, use the universal method [crm.item.update](../crm/universal/crm-item-update.md).

{% note info "" %}

It is not recommended to use the methods [crm.deal.update](../crm/deals/crm-deal-update.md), [crm.lead.update](../crm/leads/crm-lead-update.md), [crm.contact.update](../crm/contacts/crm-contact-update.md), [crm.company.update](../crm/companies/crm-company-update.md) for updating file fields.

{% endnote %}

#### 1. Get File IDs in the Field

Before updating the field, get the `IDs` of the current files to save them. You can use the [crm.item.get](../crm/universal/crm-item-get.md) method, which will return all fields of the item, or the [crm.item.list](../crm/universal/crm-item-list.md) method with selection of only the required file-type field in `select`.

{% list tabs %}

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"entityTypeId":177,"select":["ufCrm_7_1739432938"],"filter":{"id":"29"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"entityTypeId":177,"select":["ufCrm_7_1739432938"],"filter":{"id":"29"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.list
    ```

- PHP

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

- uploading new files — pass the content in [Base64](./how-to-upload-files.md) format,

- deleting old files — do not pass the `IDs` of these files in the array,

- saving files — pass the `IDs` in the array of files.

Files will be saved if their `IDs` are listed in the request. Files will be deleted if their `IDs` are not present in the request.

{% list tabs %}

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":9,"entityTypeId":177,"fields":{"ufCrm_7_1739432938":[{"id":30577},["myNewFile.pdf","base64_encoded_content_here"]]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.item.update
    ```

- PHP

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

{% endlist %}

To delete all files, pass an empty array in the field.

### crm.timeline.comment.update — update files in a comment

To update files in comments of CRM items, use the [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md) method. Old files are always deleted when updating the field value. Upload new files to the field in [Base64](./how-to-upload-files.md) format.

To delete all files, pass an empty array in the `FILES` field.

{% list tabs %}

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":62589,"fields":{"COMMENT":"Comment was changed","FILES":[]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.timeline.comment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":62589,"fields":{"COMMENT":"Comment was changed","FILES":[]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.timeline.comment.update
    ```

- PHP

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

### lists.element.update — update field in a list

To upload new files to a list item field, pass the files using the [lists.element.update](../lists/elements/lists-element-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the field.

To delete files, you will need the `ID` of the property value.

#### 1. Get Property Value ID

To get the `ID` for deleting a file, execute the [lists.element.get](../lists/elements/lists-element-get.md) method, which will return all fields of the item.

{% list tabs %}

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"37","ELEMENT_ID":"6783"}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"37","ELEMENT_ID":"6783","auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/lists.element.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '37',
            'ELEMENT_ID' => '6783'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

The "file" field in the example is `PROPERTY_1075`. The field will contain information:

- the first value `"3693"` — this is the `ID` of the property value, 

- the second value `"31219"`— this is the `ID` of the file.

```json
{
    "result": [
        {
            "ID": "6783",
            "PROPERTY_1075": {
                "3693": "31219", // 3693 — property value ID, used for deletion
                "3697": "31221", // 3697 — property value ID, used for deletion
                "3699": "31223"  // 3699 — property value ID, used for deletion
            }
        }
    ],
    "total": 1,
}
```

#### 2. Delete File from the Field

Pass the property value ID to the [lists.element.update](../lists/elements/lists-element-update.md) method with the `_DEL` suffix, for example, `PROPERTY_1075_DEL`. In the field, specify the list of property value `IDs` that will be deleted:

- key — property value `ID`,

- value — `Y`.

{% list tabs %}

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":6783,"FIELDS":{"NAME":"rest files","PROPERTY_1075_DEL":{"3693":"Y"}}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":6783,"FIELDS":{"NAME":"rest files","PROPERTY_1075_DEL":{"3693":"Y"}},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- PHP

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

To upload new files to a post in the feed, pass the files using the [log.blogpost.update](../log/log-blogpost-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the post.

To delete files, you will need their `IDs`.

#### 1. Get File ID in the Post

To get the `ID` for deleting a file, execute the [log.blogpost.get](../log/log-blogpost-get.md) method, which will return all fields of the post, including `FILES`.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "log.blogpost.get",
        {
            POST_ID: 211
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/log.blogpost.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.get
    ```

- PHP

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

Pass the `FILES` field to the [log.blogpost.update](../log/log-blogpost-update.md) method. In the field, specify an array of `IDs` of the files that will be deleted:

- key — `ID` of the file,

- value — `del`.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "log.blogpost.update",
        {
            POST_ID: 211,
            POST_TITLE: "New Post Title",
            FILES: {
                "445": "del" // IDs of files to delete
            }
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","FILES":{"445":"del"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","FILES":{"445":"del"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogpost.update',
        [
            'POST_ID' => 211,
            'POST_TITLE' => 'New Post Title',
            'FILES' => [
                '445' => 'del' // IDs of files to delete
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

- JS

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

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","UF_BLOG_POST_FILE":["empty"]}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"POST_ID":211,"POST_TITLE":"New Post Title","UF_BLOG_POST_FILE":["empty"],"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- PHP

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

### catalog.product.update — update field in a product

To upload new files to a product card, pass the files using the [catalog.product.update](../catalog/product/catalog-product-update.md) method in [Base64](./how-to-upload-files.md) format. Old files will remain unchanged in the field.

To delete files, you will need the `ID` of the property value.

#### 1. Get Property Value ID

To get the `ID` for deleting a file, execute the [catalog.product.get](../catalog/product/catalog-product-get.md) method. The method will return all fields of the product.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.product.get',
        {
            'id': 541
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/catalog.product.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/catalog.product.get
    ```

- PHP

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

To delete a file, pass the property value to the [catalog.product.update](../catalog/product/catalog-product-update.md) method with the values:

- `value` — specify `remove` as the key, `Y` as the value,

- `valueId` — specify the `ID` of the property value whose file will be deleted.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.product.update',
        {
            id: 541,
            fields: {
                property1077: [
                    {
                        "value": {
                            'remove': 'Y', // file deletion operation
                        },
                        'valueId': '3705', // ID of the value for deletion
                    }
                ]
            }
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"fields":{"property1077":[{"value":{"remove":"Y"},"valueId":"3705"}]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/catalog.product.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":541,"fields":{"property1077":[{"value":{"remove":"Y"},"valueId":"3705"}]},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/catalog.product.update
    ```

- PHP

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
                            'remove' => 'Y' // file deletion operation
                        },
                        'valueId' => '3705' // ID of the value for deletion
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