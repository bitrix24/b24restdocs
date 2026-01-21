# How to Upload Files

In Bitrix24, there are two types of file fields.

- **File.** This field is not linked to Drive; files are uploaded directly through a Base64 formatted string.

- **File (Drive).** This field is linked to Drive, and it stores the ID of the Drive object. The Base64 format is not processed in this field, so the file must first be uploaded to Bitrix24 Drive using the methods [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md) or [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md).

To upload files in Bitrix24, use the standard Base64 encoding. Encoding is used when you need to transmit a file over text protocols, such as HTTP.

## How to Encode a File in Base64

In JavaScript, you can use the built-in [FileReader](https://www.w3.org/TR/FileAPI/) object. The code reads the file selected by the user and converts it to Base64.

```JavaScript
const fileInput = document.getElementById('fileInput'); // File selection field

fileInput.addEventListener('change', function() {
    const file = fileInput.files[0]; // Get the selected file
    const reader = new FileReader();

    reader.onload = function() {
        const base64 = reader.result.split(',')[1]; // Get base64 without prefix
        console.log(base64); // Output the result
    };

    reader.readAsDataURL(file); // Encode the file in base64
});
```

In PHP, you can use the [base64_encode](https://www.php.net/manual/en/function.base64-encode.php) function. The code reads a file from the disk and encodes it in Base64.

```PHP
$filePath = 'path/to/your/file.jpg'; // Path to the file
$fileData = file_get_contents($filePath); // Read the file
$base64 = base64_encode($fileData); // Encode to base64
```

As a result of encoding the file, you will get a string like `YmFzZSDRgtC10YHRgg==`. The larger the file size, the longer the string will be.

## How to Pass a Base64 String to a Field

In Bitrix24, there are 4 features for uploading files.

1. Pass the Base64 string to the `file` field if you are using the methods:

   - [documentgenerator.template.add](../document-generator/templates/document-generator-template-add.md)

   - [crm.documentgenerator.template.add](../crm/document-generator/templates/crm-document-generator-template-add.md)

    {% list tabs %}

    - JS
    
        ```JavaScript
        BX24.callMethod(
            'documentgenerator.template.add',
            {
                fields: {
                    name: "Template Example",
                    file: "base64_encoded_content_here", // File content encoded in base64
                    code: "example_template_code"
                }
            }
        );
        ```

    - PHP
    
        ```php
        require_once('crest.php');

        $result = CRest::call(
            'documentgenerator.template.add',
            [
                'fields' => [
                    'name' => 'Template Example',
                    'file' => 'base64_encoded_content_here', // File content encoded in base64
                    'code' => 'example_template_code' 
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"name":"Template Example","file":"base64_encoded_content_here","code":"example_template_code"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/documentgenerator.template.add
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"name":"Template Example","file":"base64_encoded_content_here","code":"example_template_code"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.add
        ```

    {% endlist %}

2. Pass an array consisting of the file name and the Base64 string if you are using the methods:

   - [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md) — in the `FILES` field

   - [crm.item.add](../crm/universal/crm-item-add.md) — in file-type fields of CRM objects
  
   - [user.add](../user/user-add.md) — in the `PERSONAL_PHOTO` field

   - [log.blogpost.add](../log/log-blogpost-add.md) — in the `FILES` field

   - [lists.element.add](../lists/elements/lists-element-add.md) — in file-type properties

   - [entity.item.add](../entity/items/entity-item-add.md) — in file-type properties

   - [bizproc.workflow.template.add](../bizproc/template/bizproc-workflow-template-add.md) — in the `TEMPLATE_DATA` field

    {% list tabs %}

    - JS
    
        ```JavaScript
        BX24.callMethod(
            'bizproc.workflow.template.add',
            {
                DOCUMENT_TYPE: ['lists', 'BizprocDocument', 'iblock_164'],
                NAME: 'App template', 
                // File content with the business process template
                TEMPLATE_DATA: [   
                    "bp-379.bpt", // First element of the array - file name
                    "base64_encoded_content_here" // Second element of the array - file content encoded in base64
                ]
            }
        );
        ```

    - PHP
    
        ```php
        require_once('crest.php');

        $result = CRest::call(
            'bizproc.workflow.template.add',
            [
                'DOCUMENT_TYPE' => ['lists', 'BizprocDocument', 'iblock_164'],
                'NAME' => 'App template',
                // File content with the business process template
                'TEMPLATE_DATA' => [
                    'bp-379.bpt', // File name
                    'base64_encoded_content_here' // File content encoded in base64
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"DOCUMENT_TYPE":["lists","BizprocDocument","iblock_164"],"NAME":"App template","TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.add
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"DOCUMENT_TYPE":["lists","BizprocDocument","iblock_164"],"NAME":"App template","TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.template.add
        ```

    {% endlist %}

3. Pass an object with the key `fileData`, which contains an array of the file name and the Base64 string if you are using the methods:

   - [catalog.product.add](../catalog/product/catalog-product-add.md) — in the `previewPicture`, `detailPicture` fields
   
   - [crm.lead.add](../crm/leads/crm-lead-add.md) — in file-type fields

   - [crm.deal.add](../crm/deals/crm-deal-add.md) — in file-type fields

   - [crm.contact.add](../crm/contacts/crm-contact-add.md) — in file-type fields

   - [crm.company.add](../crm/companies/crm-company-add.md) — in file-type fields

    {% list tabs %}

    - JS
    
        ```JavaScript
        BX24.callMethod(
            "catalog.product.add",
            {
                fields: {
                    iblockId: '24', 
                    name: "Product Example",
                    // Preview image of the product, fileData - array where the first element is the file name, the second is the file content in base64 format
                    previewPicture: {
                        fileData: [
                            "example.jpg", // Image file name
                            "base64_encoded_content_here" // Image content in base64 format
                        ]
                    }
                }
            }
        );
        ```

    - PHP
    
        ```php
        require_once('crest.php');

        $result = CRest::call(
            'catalog.product.add',
            [
                'fields' => [
                    'iblockId' => '24', 
                    'name' => 'Product Example', 
                    // Preview image of the product, fileData - array where the first element is the file name, the second is the file content in base64 format            
                    'previewPicture' => [
                        'fileData' => [
                            'example.jpg', // Image file name
                            'base64_encoded_content_here' // Image content in base64 format
                        ]
                    ]
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"iblockId":"24","name":"Product Example","previewPicture":{"fileData":["example.jpg","base64_encoded_content_here"]}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/catalog.product.add
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"iblockId":"24","name":"Product Example","previewPicture":{"fileData":["example.jpg","base64_encoded_content_here"]}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
        ```

    {% endlist %}

4. Pass the `fileContent` parameter, which contains an array of the file name and the Base64 string if you are using the methods:

   - [disk.file.uploadversion](../disk/file/disk-file-upload-version.md)

   - [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md)

   - [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md)

   - [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md)

   - [catalog.productImage.add](../catalog/product-image/catalog-product-image-add.md)

    {% list tabs %}

    - JS
    
        ```JavaScript
        BX24.callMethod(
            "disk.file.uploadversion",
            {    
                id: 4, // Identifier of the file for which a new version is being uploaded
                // Content of the file being uploaded as a new version
                fileContent: [
                    '1.gif', // First element of the array - file name
                    'base64_encoded_content_here' // Second element of the array - file content in base64 format
                ]
            }
        );
        ```

    - PHP
    
        ```php
        require_once('crest.php');

        $result = CRest::call(
            'disk.file.uploadversion',
            [
                'id' => 4, // Identifier of the file for which a new version is being uploaded
                'fileContent' => [
                    '1.gif', // File name
                    'base64_encoded_content_here' // File content in base64 format
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":4,"fileContent":["1.gif","base64_encoded_content_here"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/disk.file.uploadversion
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":4,"fileContent":["1.gif","base64_encoded_content_here"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.uploadversion
        ```

    {% endlist %}

## How to Upload Files to a Multiple Field

If the field has a "multiple" flag, you can upload multiple files to it. The format depends on the method.

1. Pass an array where each element is a file name and file in Base64 format if you are using the methods:

   - [crm.item.add](../crm/universal/crm-item-add.md) — file-type fields

   - [lists.element.add](../lists/elements/lists-element-add.md) — file-type properties

   - [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md) — in the `FILES` field

   - [log.blogpost.add](../log/log-blogpost-add.md) — in the `FILES` field

    {% list tabs %}

    - JS
    
        ```JavaScript
        BX24.callMethod(
            'crm.item.add',
            {
                entityTypeId: 2, 
                fields: {
                    title: "New Deal (specifically for REST method example)", 
                    // Multiple field with an array of files
                    ufCrm_123456: [ 
                        [
                            "green_pixel.png", // File name #1
                            "base64_encoded_content_here" // Base64 content of the first file
                        ],
                        [
                            "blue_pixel.png", // File name #2
                            "base64_encoded_content_here" // Base64 content of the second file
                        ],
                        [
                            "red_pixel.png", // File name #3
                            "base64_encoded_content_here" // Base64 content of the third file
                        ]
                    ]
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.add',
            [
                'entityTypeId' => 2, 
                'fields' => [
                    'title' => 'New Deal (specifically for REST method example)', 
                    // Multiple field with an array of files
                    'ufCrm_123456' => [
                        [
                            'green_pixel.png', // File name #1
                            'base64_encoded_content_here' // Base64 content of the first file
                        ],
                        [
                            'blue_pixel.png', // File name #2
                            'base64_encoded_content_here' // Base64 content of the second file
                        ],
                        [
                            'red_pixel.png', // File name #3
                            'base64_encoded_content_here' // Base64 content of the third file
                        ]
                    ]
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New Deal (specifically for REST method example)","ufCrm_123456":[["green_pixel.png","base64_encoded_content_here"],["blue_pixel.png","base64_encoded_content_here"],["red_pixel.png","base64_encoded_content_here"]]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.add
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New Deal (specifically for REST method example)","ufCrm_123456":[["green_pixel.png","base64_encoded_content_here"],["blue_pixel.png","base64_encoded_content_here"],["red_pixel.png","base64_encoded_content_here"]]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
        ```

    {% endlist %}

2. Pass an array of objects where each object contains a `value` field with the key `fileData` if you are using the method:

   - [catalog.product.add](../catalog/product/catalog-product-add.md) — file-type fields

    {% list tabs %}

    - JS

        ```js
        BX24.callMethod(
            'catalog.product.add',
            {
                fields: {
                    iblockId: 1,
                    name: "Product Example",
                    PROPERTY_1077: [
                        {
                            value: {
                                fileData: [
                                    "blue_pixel.txt", // File name
                                    "YmFzZSDRgtC10YHRgg==" // Base64 content
                                ]
                            }
                        },
                        {
                            value: {
                                fileData: [
                                    "red_pixel.txt", // File name
                                    "YmFzZSDRgtC10YHRgg==" // Base64 content
                                ]
                            }
                        }
                    ]
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'catalog.product.add',
            [
                'fields' => [
                    'iblockId' => 1,
                    'name' => 'Product Example',
                    'PROPERTY_1077' => [
                        [
                            'value' => [
                                'fileData' => [
                                    'blue_pixel.txt',
                                    'YmFzZSDRgtC10YHRgg=='
                                ]
                            }
                        ],
                        [
                            'value' => [
                                'fileData' => [
                                    'red_pixel.txt',
                                    'YmFzZSDRgtC10YHRgg=='
                                ]
                            }
                        ]
                    ]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```        

    - cURL (Webhook)

        ```bash
        curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"fields": {"iblockId": 1, "name": "Product Example", "PROPERTY_1077": [{"value": {"fileData": ["blue_pixel.txt", "YmFzZSDRgtC10YHRgg=="]}}, {"value": {"fileData": ["red_pixel.txt", "YmFzZSDRgtC10YHRgg=="]}}]}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"fields": {"iblockId": 1, "name": "Product Example", "PROPERTY_1077": [{"value": {"fileData": ["blue_pixel.txt", "YmFzZSDRgtC10YHRgg=="]}}, {"value": {"fileData": ["red_pixel.txt", "YmFzZSDRgtC10YHRgg=="]}}]}, "auth": "**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/catalog.product.add
        ```

    {% endlist %}

3. Pass an array of objects with the `fileData` key if you are using the methods:

   - [crm.lead.add](../crm/leads/crm-lead-add.md) — file-type fields

   - [crm.deal.add](../crm/deals/crm-deal-add.md) — file-type fields

   - [crm.contact.add](../crm/contacts/crm-contact-add.md) — file-type fields

   - [crm.company.add](../crm/companies/crm-company-add.md) — file-type fields

    {% list tabs %}

    - JS

        ```JavaScript
        BX24.callMethod(
            'crm.lead.add',
            {
                fields: {
                    TITLE: "Lead Example",
                    UF_CRM_1711610801: [
                        {
                            fileData: [
                                "file1.png", // File name
                                "base64_1" // Base64 content
                            ]
                        },
                        {
                            fileData: [
                                "file2.png", // File name
                                "base64_2" // Base64 content
                            ]
                        },
                        {
                            fileData: [
                                "file3.png", // File name
                                "base64_3" // Base64 content
                            ]
                        }
                    ]
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.lead.add',
            [
                'fields' => [
                    'TITLE' => 'Lead Example',
                    'UF_CRM_1711610801' => [
                        [
                            'fileData' => [
                                'file1.png', // File name
                                'base64_1' // Base64 content
                            ]
                        },
                        [
                            'fileData' => [
                                'file2.png', // File name
                                'base64_2' // Base64 content
                            ]
                        },
                        [
                            'fileData' => [
                                'file3.png', // File name
                                'base64_3' // Base64 content
                            ]
                        ]
                    ]
                ]
            ]
        );
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"TITLE":"Lead Example","UF_CRM_1711610801":[{"fileData":["file1.png","base64_1"]},{"fileData":["file2.png","base64_2"]},{"fileData":["file3.png","base64_3"]}]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.lead.add
        ```

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"TITLE":"Lead Example","UF_CRM_1711610801":[{"fileData":["file1.png","base64_1"]},{"fileData":["file2.png","base64_2"]},{"fileData":["file3.png","base64_3"]}]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.add
        ```

    {% endlist %}

## Limitations When Working with Files

- The limit on the length of a GET request is 2048 characters, which is the length of the URL. Files encoded in Base64 often exceed this value. To transmit large files, use a POST request.

- The limit on the size of a POST request in Bitrix24 is 2GB. Files larger than 2GB will not be processed. If multiple files are transmitted in one request totaling more than 2GB, the request will be aborted. To upload multiple large files, send the data in separate requests.

- The execution time limit for a request is 60 seconds for cloud Bitrix24. The request will time out if processing takes longer than 60 seconds. You can check the execution time of the request in the [time](../data-types.md#time) object of the response, parameter `duration`.

- If you are transmitting a file encoded in Base64, and the method is executed in the address bar as a GET request or the method is executed via curl, the Base64 must be additionally [encoded in urlencode](../../settings/how-to-call-rest-api/data-encoding.md), otherwise the file will not be read.