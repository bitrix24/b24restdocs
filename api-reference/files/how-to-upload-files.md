# How to Upload Files

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This page describes how to upload a new file to Bitrix24 via the Bitrix24 REST API: how to encode a file in Base64, which format to use when passing it to a method, and which limitations to consider.

There is no single format for all methods: some accept a Base64 string, others accept an array containing a filename and such a string, and others use a separate parameter. Before making a call, check the [How to Choose a Format](#formats) table.

Updating, replacing, and deleting files are described in the [How to Update and Delete Files](./how-to-update-files.md) article. To retrieve an already uploaded file, see the page for the relevant method.

The User permissions and [scope](../scopes/permissions.md) required for the call are specified at the beginning of each method's page — check them before uploading a file.

## Types of File Fields

In Bitrix24, there are two types of file fields.

- **File.** This field is not linked to Drive. The file is passed directly into the field — either as a Base64 string or as an array containing a filename and such a string. Bitrix24 decodes the string and saves the file, while the field retains the `ID` of the file.

- **File (Drive).** This field is linked to Drive, and the field stores the `ID` of an object on Drive. Some methods accept Base64 and upload the file to Drive themselves — this is how "file (Drive)" type fields work in the CRM. If the method expects a ready-made `ID`, first upload the file to Drive, and then pass `ID` to the field. For more details, see the [How to Pass a File to a Field Linked to Drive](#disk-field) section.

## How to Encode a File in Base64

Base64 is an encoding standard that represents binary data as a text string. Encoding is necessary to pass a file through text-based protocols, such as HTTP.

In JavaScript, use the built-in [FileReader](https://www.w3.org/TR/FileAPI/) object. The code reads the file selected by the user and converts it to Base64.

```JavaScript
const fileInput = document.getElementById('fileInput'); // File selection field

fileInput.addEventListener('change', function() {
    const file = fileInput.files[0]; // Get selected file
    const reader = new FileReader();

    reader.onload = function() {
        const base64 = reader.result.split(',')[1]; // Get base64 without prefix
        console.log(base64); // Display result
    };

    reader.readAsDataURL(file); // Encode file to base64
});
```

In PHP, use the [base64_encode](https://www.php.net/manual/en/function.base64-encode.php) function. The code reads the file from the disk and encodes it in Base64.

```PHP
$filePath = 'path/to/your/file.jpg'; // File path
$fileData = file_get_contents($filePath); // Read file
$base64 = base64_encode($fileData); // Encode to base64
```

The result of the encoding will be a string like `YmFzZSDRgtC10YHRgg==`. The larger the file size, the longer the string.

Consider the characteristics of the format.

- A Base64 string is approximately one-third longer than the original file: every 3 bytes are converted into 4 characters. A 1.5 MB file will occupy about 2 MB in a request.

- Pass the string without the `data:image/png;base64,` prefix. In the JavaScript example, the prefix is stripped by the method `split(',')[1]`.

- Validate the string before sending. Bitrix24 decodes it and saves the result as a file exactly as is: a corrupted string will result in a corrupted file, and an empty string will result in a method error.

- In most formats, the filename is passed separately. If you pass only the Base64 string without a name, Bitrix24 will generate a name automatically — the file will be difficult to identify in the interface.

## How to Choose a Transfer Format {#formats}

The format depends on the method and whether the field is a multiple field or not.

#|
|| **Method** | **One file** | **Multiple files** ||
|| [documentgenerator.template.add](../document-generator/templates/document-generator-template-add.md) | [Base64 string](#string) in field `file` | — ||
|| [crm.documentgenerator.template.add](../crm/document-generator/templates/crm-document-generator-template-add.md) | [Base64 string](#string) in field `file` | — ||
|| [bizproc.workflow.template.add](../bizproc/template/bizproc-workflow-template-add.md) | ["name — Base64" array](#array) in field `TEMPLATE_DATA` | — ||
|| [user.add](../user/user-add.md) | ["name — Base64" array](#array) in field `PERSONAL_PHOTO` | — ||
|| [crm.item.add](../crm/universal/crm-item-add.md) | ["name — Base64" array](#array) in "file" type field | [array of pairs](#multiple-array) ||
|| [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md) | [array of pairs](#multiple-array) of one item in field `FILES` | [array of pairs](#multiple-array) in field `FILES` ||
|| [log.blogpost.add](../log/log-blogpost-add.md) | [array of pairs](#multiple-array) of one item in field `FILES` | [array of pairs](#multiple-array) in field `FILES` ||
|| [lists.element.add](../lists/elements/lists-element-add.md) | ["name — Base64" array](#array) in "file" type property | [array of pairs](#multiple-array) ||
|| [entity.item.add](../entity/items/entity-item-add.md) | ["name — Base64" array](#array) in "file" type property | [array of pairs](#multiple-array) ||
|| [crm.lead.add](../crm/leads/crm-lead-add.md), [crm.deal.add](../crm/deals/crm-deal-add.md), [crm.contact.add](../crm/contacts/crm-contact-add.md), [crm.company.add](../crm/companies/crm-company-add.md) | [object `fileData`](#filedata) in "file" type field | [array of objects `fileData`](#multiple-filedata) ||
|| [catalog.product.add](../catalog/product/catalog-product-add.md) | [object `fileData`](#filedata) in fields `previewPicture`, `detailPicture` | [array of objects `value.fileData`](#multiple-value) ||
|| [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md), [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md), [disk.file.uploadversion](../disk/file/disk-file-upload-version.md) | [parameter `fileContent`](#filecontent) | — ||
|| [catalog.productImage.add](../catalog/product-image/catalog-product-image-add.md) | [parameter `fileContent`](#filecontent) | — ||
|| [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md) | [parameters `FILENAME` and `FILE_CONTENT`](#filename) | — ||
|#

If the required method is not in the table, check the parameter descriptions on the method's page for the format.

## File Transfer Formats

{% include [Note on examples](../../_includes/examples.md) %}

### Base64 String in the File Field {#string}

Pass a Base64 string in the `file` field. The filename is not passed in this format.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Template example","file":"base64_encoded_content_here","code":"example_template_code"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Template example","file":"base64_encoded_content_here","code":"example_template_code"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'documentgenerator.template.add',
            {
                fields: {
                    name: "Template example",
                    file: "base64_encoded_content_here", // File content encoded in base64
                    code: "example_template_code"
                }
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
                'documentgenerator.template.add',
                [
                    'fields' => [
                        'name' => 'Template example',
                        'file' => 'base64_encoded_content_here', // File content encoded in base64
                        'code' => 'example_template_code'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'documentgenerator.template.add',
        {
            fields: {
                name: "Template example",
                file: "base64_encoded_content_here", // File content encoded in base64
                code: "example_template_code"
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'documentgenerator.template.add',
        [
            'fields' => [
                'name' => 'Template example',
                'file' => 'base64_encoded_content_here', // File content encoded in base64
                'code' => 'example_template_code'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### "Filename — Base64" Array {#array}

Pass an array of two items: the first is the filename with its extension, and the second is the Base64 string.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DOCUMENT_TYPE":["lists","BizprocDocument","iblock_164"],"NAME":"App template","TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.template.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DOCUMENT_TYPE":["lists","BizprocDocument","iblock_164"],"NAME":"App template","TEMPLATE_DATA":["bp-379.bpt","base64_encoded_content_here"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'bizproc.workflow.template.add',
            {
                DOCUMENT_TYPE: ['lists', 'BizprocDocument', 'iblock_164'],
                NAME: 'App template',
                TEMPLATE_DATA: [
                    "bp-379.bpt", // First array element — filename
                    "base64_encoded_content_here" // Second array element — file content in base64
                ]
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
                'bizproc.workflow.template.add',
                [
                    'DOCUMENT_TYPE' => ['lists', 'BizprocDocument', 'iblock_164'],
                    'NAME'          => 'App template',
                    'TEMPLATE_DATA' => [
                        'bp-379.bpt', // First array element — filename
                        'base64_encoded_content_here' // Second array element — file content in base64
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding workflow template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.add',
        {
            DOCUMENT_TYPE: ['lists', 'BizprocDocument', 'iblock_164'],
            NAME: 'App template',
            TEMPLATE_DATA: [
                "bp-379.bpt", // First array element — filename
                "base64_encoded_content_here" // Second array element — file content in base64
            ]
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.template.add',
        [
            'DOCUMENT_TYPE' => ['lists', 'BizprocDocument', 'iblock_164'],
            'NAME' => 'App template',
            'TEMPLATE_DATA' => [
                'bp-379.bpt', // Filename
                'base64_encoded_content_here' // File content in base64
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### fileData Object {#filedata}

Pass an object with the `fileData` key. The key contains an array consisting of the filename and the Base64 string.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":"24","name":"Product example","previewPicture":{"fileData":["example.jpg","base64_encoded_content_here"]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":"24","name":"Product example","previewPicture":{"fileData":["example.jpg","base64_encoded_content_here"]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'catalog.product.add',
            {
                fields: {
                    iblockId: '24',
                    name: "Product example",
                    previewPicture: {
                        fileData: [
                            "example.jpg", // Image filename
                            "base64_encoded_content_here" // Image content in base64
                        ]
                    }
                }
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
                'catalog.product.add',
                [
                    'fields' => [
                        'iblockId'       => '24',
                        'name'           => 'Product example',
                        'previewPicture' => [
                            'fileData' => [
                                'example.jpg', // Image filename
                                'base64_encoded_content_here' // Image content in base64
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.add',
        {
            fields: {
                iblockId: '24',
                name: "Product example",
                previewPicture: {
                    fileData: [
                        "example.jpg", // Image filename
                        "base64_encoded_content_here" // Image content in base64
                    ]
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.add',
        [
            'fields' => [
                'iblockId' => '24',
                'name' => 'Product example',
                'previewPicture' => [
                    'fileData' => [
                        'example.jpg', // Image filename
                        'base64_encoded_content_here' // Image content in base64
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

### Parameter fileContent {#filecontent}

Pass a separate `fileContent` parameter containing an array of the filename and the Base64 string. The parameter is passed at the top level of the request, rather than inside `fields`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4,"fileContent":["1.gif","base64_encoded_content_here"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.uploadversion
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4,"fileContent":["1.gif","base64_encoded_content_here"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.uploadversion
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.uploadversion',
            {
                id: 4, // File ID for which a new version is being uploaded
                fileContent: [
                    '1.gif', // First array element — filename
                    'base64_encoded_content_here' // Second array element — file content in base64
                ]
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
                'disk.file.uploadversion',
                [
                    'id'          => 4, // File ID for which a new version is being uploaded
                    'fileContent' => [
                        '1.gif', // Filename
                        'base64_encoded_content_here' // File content in base64
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error uploading file version: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'disk.file.uploadversion',
        {
            id: 4, // File ID for which a new version is being uploaded
            fileContent: [
                '1.gif', // First array element — filename
                'base64_encoded_content_here' // Second array element — file content in base64
            ]
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.file.uploadversion',
        [
            'id' => 4, // File ID for which a new version is being uploaded
            'fileContent' => [
                '1.gif', // Filename
                'base64_encoded_content_here' // File content in base64
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### FILENAME and FILE_CONTENT Parameters {#filename}

The [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md) method accepts the filename and its content in two separate parameters: `FILENAME` and `FILE_CONTENT`.

If `FILENAME` is passed without `FILE_CONTENT`, the method will return `uploadUrl` — the file is uploaded via a separate request to this address. This method is suitable for large call recordings.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","FILENAME":"call-001.mp3","FILE_CONTENT":"base64_encoded_content_here"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.externalCall.attachRecord
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","FILENAME":"call-001.mp3","FILE_CONTENT":"base64_encoded_content_here","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.attachRecord
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalCall.attachRecord',
            {
                CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                FILENAME: 'call-001.mp3', // Record filename
                FILE_CONTENT: 'base64_encoded_content_here' // Record content in base64
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
                'telephony.externalCall.attachRecord',
                [
                    'CALL_ID'      => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                    'FILENAME'     => 'call-001.mp3', // Record filename
                    'FILE_CONTENT' => 'base64_encoded_content_here' // Record content in base64
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error attaching call record: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'telephony.externalCall.attachRecord',
        {
            CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            FILENAME: 'call-001.mp3', // Record filename
            FILE_CONTENT: 'base64_encoded_content_here' // Record content in base64
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'telephony.externalCall.attachRecord',
        [
            'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            'FILENAME' => 'call-001.mp3', // Record filename
            'FILE_CONTENT' => 'base64_encoded_content_here' // Record content in base64
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## How to Upload Multiple Files to a Multiple Field

If a field has the "multiple" flag, you can upload several files in a single request. The format depends on the method — refer to the "Multiple Files" column in the [How to Choose a Format](#formats) table.

The `FILES` field in the [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md) and [log.blogpost.add](../log/log-blogpost-add.md) methods always accepts an array, even when there is only one file. Files from these methods are saved to Drive in a system folder for uploaded files.

### Array of "Filename — Base64" Pairs {#multiple-array}

Pass an array where each item is an array consisting of the filename and the Base64 string.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","ufCrm_123456":[["green_pixel.png","base64_encoded_content_here"],["blue_pixel.png","base64_encoded_content_here"],["red_pixel.png","base64_encoded_content_here"]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"fields":{"title":"New deal (specifically for REST method examples)","ufCrm_123456":[["green_pixel.png","base64_encoded_content_here"],["blue_pixel.png","base64_encoded_content_here"],["red_pixel.png","base64_encoded_content_here"]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.item.add',
            {
                entityTypeId: 2,
                fields: {
                    title: "New deal (specifically for REST method examples)",
                    ufCrm_123456: [ // Multiple field with an array of files
                        [
                            "green_pixel.png", // Filename № 1
                            "base64_encoded_content_here" // Content of the first file
                        ],
                        [
                            "blue_pixel.png", // Filename № 2
                            "base64_encoded_content_here" // Content of the second file
                        ],
                        [
                            "red_pixel.png", // Filename № 3
                            "base64_encoded_content_here" // Content of the third file
                        ]
                    ]
                }
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
                'crm.item.add',
                [
                    'entityTypeId' => 2,
                    'fields'       => [
                        'title'        => 'New deal (specifically for REST method examples)',
                        'ufCrm_123456' => [ // Multiple field with an array of files
                            [
                                'green_pixel.png', // Filename № 1
                                'base64_encoded_content_here' // Content of the first file
                            ],
                            [
                                'blue_pixel.png', // Filename № 2
                                'base64_encoded_content_here' // Content of the second file
                            ],
                            [
                                'red_pixel.png', // Filename № 3
                                'base64_encoded_content_here' // Content of the third file
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding CRM item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.add',
        {
            entityTypeId: 2,
            fields: {
                title: "New deal (specifically for REST method examples)",
                ufCrm_123456: [ // Multiple field with an array of files
                    [
                        "green_pixel.png", // Filename № 1
                        "base64_encoded_content_here" // Content of the first file
                    ],
                    [
                        "blue_pixel.png", // Filename № 2
                        "base64_encoded_content_here" // Content of the second file
                    ],
                    [
                        "red_pixel.png", // Filename № 3
                        "base64_encoded_content_here" // Content of the third file
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
        'crm.item.add',
        [
            'entityTypeId' => 2,
            'fields' => [
                'title' => 'New deal (specifically for REST method examples)',
                'ufCrm_123456' => [
                    [
                        'green_pixel.png', // Filename № 1
                        'base64_encoded_content_here' // Content of the first file
                    ],
                    [
                        'blue_pixel.png', // Filename № 2
                        'base64_encoded_content_here' // Content of the second file
                    ],
                    [
                        'red_pixel.png', // Filename № 3
                        'base64_encoded_content_here' // Content of the third file
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

### Array of value.fileData Objects {#multiple-value}

Pass an array of objects. Each object contains the `value` field with the `fileData` key.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":1,"name":"Product example","PROPERTY_1077":[{"value":{"fileData":["blue_pixel.txt","YmFzZSDRgtC10YHRgg=="]}},{"value":{"fileData":["red_pixel.txt","YmFzZSDRgtC10YHRgg=="]}}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":1,"name":"Product example","PROPERTY_1077":[{"value":{"fileData":["blue_pixel.txt","YmFzZSDRgtC10YHRgg=="]}},{"value":{"fileData":["red_pixel.txt","YmFzZSDRgtC10YHRgg=="]}}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'catalog.product.add',
            {
                fields: {
                    iblockId: 1,
                    name: "Product example",
                    PROPERTY_1077: [
                        {
                            value: {
                                fileData: [
                                    "blue_pixel.txt", // Filename
                                    "YmFzZSDRgtC10YHRgg==" // File content in base64
                                ]
                            }
                        },
                        {
                            value: {
                                fileData: [
                                    "red_pixel.txt",
                                    "YmFzZSDRgtC10YHRgg=="
                                ]
                            }
                        }
                    ]
                }
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
                'catalog.product.add',
                [
                    'fields' => [
                        'iblockId'      => 1,
                        'name'          => 'Product example',
                        'PROPERTY_1077' => [
                            [
                                'value' => [
                                    'fileData' => [
                                        'blue_pixel.txt', // Filename
                                        'YmFzZSDRgtC10YHRgg==' // File content in base64
                                    ]
                                ]
                            ],
                            [
                                'value' => [
                                    'fileData' => [
                                        'red_pixel.txt',
                                        'YmFzZSDRgtC10YHRgg=='
                                    ]
                                ]
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.add',
        {
            fields: {
                iblockId: 1,
                name: "Product example",
                PROPERTY_1077: [
                    {
                        value: {
                            fileData: [
                                "blue_pixel.txt", // Filename
                                "YmFzZSDRgtC10YHRgg==" // File content in base64
                            ]
                        }
                    },
                    {
                        value: {
                            fileData: [
                                "red_pixel.txt",
                                "YmFzZSDRgtC10YHRgg=="
                            ]
                        }
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
        'catalog.product.add',
        [
            'fields' => [
                'iblockId' => 1,
                'name' => 'Product example',
                'PROPERTY_1077' => [
                    [
                        'value' => [
                            'fileData' => [
                                'blue_pixel.txt',
                                'YmFzZSDRgtC10YHRgg=='
                            ]
                        ]
                    ],
                    [
                        'value' => [
                            'fileData' => [
                                'red_pixel.txt',
                                'YmFzZSDRgtC10YHRgg=='
                            ]
                        ]
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

### Array of fileData Objects {#multiple-filedata}

Pass an array of objects, where each object contains the `fileData` key with the filename and the Base64 string.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Lead example","UF_CRM_1711610801":[{"fileData":["file1.png","base64_encoded_content_here"]},{"fileData":["file2.png","base64_encoded_content_here"]}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Lead example","UF_CRM_1711610801":[{"fileData":["file1.png","base64_encoded_content_here"]},{"fileData":["file2.png","base64_encoded_content_here"]}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.lead.add',
            {
                fields: {
                    TITLE: "Lead example",
                    UF_CRM_1711610801: [
                        {
                            fileData: [
                                "file1.png", // Filename
                                "base64_encoded_content_here" // File content in base64
                            ]
                        },
                        {
                            fileData: [
                                "file2.png",
                                "base64_encoded_content_here"
                            ]
                        }
                    ]
                }
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
                'crm.lead.add',
                [
                    'fields' => [
                        'TITLE'             => 'Lead example',
                        'UF_CRM_1711610801' => [
                            [
                                'fileData' => [
                                    'file1.png', // Filename
                                    'base64_encoded_content_here' // File content in base64
                                ]
                            ],
                            [
                                'fileData' => [
                                    'file2.png',
                                    'base64_encoded_content_here'
                                ]
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding lead: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.lead.add',
        {
            fields: {
                TITLE: "Lead example",
                UF_CRM_1711610801: [
                    {
                        fileData: [
                            "file1.png", // Filename
                            "base64_encoded_content_here" // File content in base64
                        ]
                    },
                    {
                        fileData: [
                            "file2.png",
                            "base64_encoded_content_here"
                        ]
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
        'crm.lead.add',
        [
            'fields' => [
                'TITLE' => 'Lead example',
                'UF_CRM_1711610801' => [
                    [
                        'fileData' => [
                            'file1.png',
                            'base64_encoded_content_here'
                        ]
                    ],
                    [
                        'fileData' => [
                            'file2.png',
                            'base64_encoded_content_here'
                        ]
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

## How to Pass a File to a Field Linked to Drive {#disk-field}

A "file (Drive)" type field stores the `ID` of an object on Drive. If a method does not accept Base64 in such a field, the upload takes two steps.

1. Upload the file to Drive using the [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md) or [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md) method — the file is passed in the [fileContent](#filecontent) parameter.

2. Take the `ID` from the response and pass it to the object field. For example, the [tasks.task.file.attach](../tasks/tasks-task-file-attach.md) method attaches a file that is already stored on Drive to a task.

"File (Drive)" type fields in the CRM are an exception. They accept a [ `fileData`](#filedata) object with Base64, and Bitrix24 automatically saves the file to Drive in a system folder for REST files.

## Response Content

`disk.*` methods return a Drive file object: `ID`, name, size, and download link `DOWNLOAD_URL`.

```json
{
    "result": {
        "ID": 9035,
        "NAME": "picture.png",
        "TYPE": "file",
        "STORAGE_ID": "1357",
        "FILE_ID": 32895,
        "SIZE": "1679",
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?auth=b8d880690000071b006e2cf2000004f5...",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1269/disk/file/picture.png"
    }
}
```

Methods that upload a file to an object field return the identifier of the created object, not the file. To retrieve the `ID` of a file and its download links, request the object using a read method — for example, [crm.item.get](../crm/universal/crm-item-get.md). The file field will return an array of objects.

```json
{
    "ufCrm_123456": [
        {
            "id": 30577,
            "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&fileId=30577",
            "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=c2a8ad670000071b..."
        }
    ]
}
```

These `ID` will be required when you need to [update or delete files](./how-to-update-files.md).

## Limitations When Working with Files

- GET requests are limited by the URL length — approximately 2048 characters. This is a general limitation of browsers and web servers, not a specific feature of Bitrix24. A Base64 string is almost always longer, so pass files via a POST request.

- The POST request size in Bitrix24 Cloud is limited by server settings — 2 GB. A file larger than this size will not be processed. If multiple files are passed in a single request and their total size exceeds the limit, the request will be interrupted — pass such files in separate requests. Refer to the size of the Base64 string rather than the original file: the string is approximately one-third longer.

- In the Self-hosted version, the request size limit is determined by your server settings, not Bitrix24. Check this with your portal administrator.

- The request execution time limit is 60 seconds for Bitrix24 Cloud. The request will time out if processing takes longer. You can check the execution time in the [time](../data-types.md#time) object of the response, parameter `duration`.

- If the method is executed via a GET request in the address bar or through cURL, the Base64 string must be additionally [URL-encoded](../../settings/how-to-call-rest-api/data-encoding.md), otherwise the file will not be read.

## Next Steps

- [How to Update and Delete Files](./how-to-update-files.md) — replacing a file, deleting, and retaining other files in a multiple field

- [How to Work with Files](./index.md) — a section overview: field types, linking files to Bitrix24 objects, and core methods

- [Data Encoding](../../settings/how-to-call-rest-api/data-encoding.md) — how to pass data in GET requests and cURL
