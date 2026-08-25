# How to Add a Template and Create a Document Based on It

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, both permissions are required — to modify document generator templates and to modify document generator documents
>
> - [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) and [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) — a user with permission to modify document generator templates
> - [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) — a user with permission to modify document generator documents
> - [crm.documentgenerator.document.get](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-get.md) — a user with permission to view document generator documents

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The document generator builds a printable form from a `.docx` template and the data of a CRM object. Through REST, all three steps — creating a numerator, uploading a template, and generating a document — can be performed by a script.

The numerator defines how document numbers are counted, so it is created first: without it, the template is not retained. The template holds the file and the list of objects it is available for. The document is created from a ready template for a single object, so it comes last.

As a result of the scenario, a document with a number from the numerator appears in the deal card, and the method returns a link to download it.

The scenario consists of three steps. Steps 1 and 2 configure the generator and are performed once, step 3 is repeated for every new deal with the `templateId` that is already available.

1. Create the numerator using the [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) method and retrieve its `id`
2. Upload the template using the [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) method, passing the numerator `id` and the file in Base64, and retrieve the template `id`
3. Generate the document using the [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) method, passing the template `id` and the deal identifier

## Before You Start

- The webhook is created on behalf of a user who has permissions to modify document generator templates and documents. Verifying the result additionally requires the permission to view documents

- The `crm` scope is selected in the webhook permissions

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

- A `.docx` template file with document generator fields is located on the disk next to the script

- Bitrix24 contains the deal the document is created for, and you know its `id`. The deal can be found using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with `entityTypeId`: `2`

- The document generator module is available in Bitrix24, and the plan allows creating documents

Fields in the template file are written in curly braces, for example `{DocumentNumber}` — the document number, `{DocumentCreateTime}` — the generation date, `{TotalSum}` — the total amount. A file without fields is uploaded successfully, but the document built from it comes out empty.

The examples use three values. Replace them with your own.

- `templatePath` — the path to the template file, `template.docx` in the example

- `templateName` — the template name, `Demonstration product implementation` in the example

- `dealId` — the deal identifier, `8287` in the example

## 1. Create the Numerator

Use the [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) method. The method accepts a `fields` object with the following parameters:

- `name` — the numerator name, a required parameter. Specify `Numerator from REST`

- `template` — the number template, a required parameter. Specify `{NUMBER}` — the generator replaces this variable with the sequential number of the document. Variables can be combined in the number template, for example `{DAY}` — the current day, `{CLIENT_ID}` — the client identifier, `{RANDOM}` — a random number

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { readFile } from 'node:fs/promises'
    import { basename } from 'node:path'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const templatePath = 'template.docx'; // path to the template file
    const templateName = 'Demonstration product implementation'; // template name
    const dealId = 8287; // deal identifier

    const resNum = await $b24.actions.v2.call.make({
        method: 'crm.documentgenerator.numerator.add',
        params: {
            fields: {
                name: 'Numerator from REST', // Numerator name
                template: '{NUMBER}' // Document number template
            }
        },
        requestId: 'numerator-add'
    });

    const numeratorId = resNum.getData().result.numerator.id;
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    template_path = "template.docx"  # path to the template file
    template_name = "Demonstration product implementation"  # template name
    deal_id = 8287  # deal identifier

    numerator = client.crm.documentgenerator.numerator.add(
        fields={
            "name": "Numerator from REST",  # Numerator name
            "template": "{NUMBER}",  # Document number template
        },
    ).response.result["numerator"]

    numerator_id = numerator["id"]
    ```


- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $templatePath = __DIR__ . '/template.docx'; // path to the template file
    $templateName = 'Demonstration product implementation'; // template name
    $dealId = 8287; // deal identifier

    $resNum = $sb->getCRMScope()->documentgeneratorNumerator()->add(
        [
            'name' => 'Numerator from REST', // Numerator name
            'template' => '{NUMBER}', // Document number template
        ]
    );

    $numeratorId = $resNum->getId();
    ```
{% endlist %}

In the response, the method returns a `numerator` object. Retain the `id` — it has to be passed to step 2. In the example, `id`: `1095`.

```json
{
    "result": {
        "numerator": {
            "name": "Numerator from REST",
            "template": "{NUMBER}",
            "id": 1095,
            "code": null,
            "settings": {
                "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                    "start": 1,
                    "step": 1,
                    "length": 0,
                    "padString": "0",
                    "periodicBy": null,
                    "timezone": null,
                    "isDirectNumeration": false
                }
            }
        }
    }
}
```

## 2. Upload the Document Template

Use the [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) method.

{% note warning "" %}

The content of the template file has to be converted to the [Base64](../../../api-reference/files/how-to-upload-files.md) format.

{% endnote %}

The method accepts a `fields` object with the following parameters:

- `name` — the template name, a required parameter. Pass the `templateName` value

- `numeratorId` — the numerator identifier, a required parameter. Take the `id` from the step 1 response, `1095` in the example

- `region` — the template region, a required parameter. It affects localization, such as the currency and date format. Specify `de`. The list of regions available in your Bitrix24 is returned by the [documentgenerator.region.list](../../../api-reference/document-generator/region/document-generator-region-list.md) method — it requires the separate `documentgenerator` scope

- `entityTypeId` — an array of [CRM object type identifiers](../../../api-reference/crm/data-types.md#object_type) the template is available for, a required parameter. Specify `2` — a deal. The full list of types is returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

- `file` — the template file in the format `["file_name.docx", "base64-content"]`, a required parameter. The first element of the array sets the name the file is retained under in Bitrix24, the second one holds the file content in Base64. Take the name from `templatePath` so that it does not diverge from the name of the uploaded file

- `users` — an array of access permission codes, an optional parameter. It defines who sees the template and can use it. Specify `UA` — the access code for all authorized users. To narrow the access down, pass the codes of specific users or groups instead of `UA`

{% list tabs %}

- JS

    ```javascript
    const fileContent = (await readFile(templatePath)).toString('base64');

    const resTemplate = await $b24.actions.v2.call.make({
        method: 'crm.documentgenerator.template.add',
        params: {
            fields: {
                name: templateName, // Template name
                numeratorId: numeratorId, // Numerator identifier from step 1
                region: 'de', // Template region
                users: ['UA'], // Access permissions: all authorized users
                entityTypeId: ['2'], // 2 — deal
                file: [basename(templatePath), fileContent] // File name and content in Base64
            }
        },
        requestId: 'template-add'
    });

    const templateId = resTemplate.getData().result.template.id;
    ```

- Python

    ```python
    import base64
    from pathlib import Path

    with open(template_path, "rb") as file:
        file_content = base64.b64encode(file.read()).decode("ascii")

    template = client.crm.documentgenerator.template.add(
        fields={
            "name": template_name,  # Template name
            "numeratorId": numerator_id,  # Numerator identifier from step 1
            "region": "de",  # Template region
            "users": ["UA"],  # Access permissions: all authorized users
            "entityTypeId": ["2"],  # 2 — deal
            "file": [Path(template_path).name, file_content],  # File name and content in Base64
        },
    ).response.result["template"]

    template_id = int(template["id"])
    ```


- PHP

    ```php
    $fileContent = base64_encode(file_get_contents($templatePath));

    $resTemplate = $sb->getCRMScope()->documentgeneratorTemplate()->add(
        [
            'name' => $templateName, // Template name
            'numeratorId' => $numeratorId, // Numerator identifier from step 1
            'region' => 'de', // Template region
            'users' => ['UA'], // Access permissions: all authorized users
            'entityTypeId' => ['2'], // 2 — deal
            'file' => [basename($templatePath), $fileContent] // File name and content in Base64
        ]
    );

    $templateId = $resTemplate->getId();
    ```
{% endlist %}

In the response, the method returns a `template` object. Retain the `id` — it has to be passed to step 3. In the example, `id`: `249`.

```json
{
    "result": {
        "template": {
            "id": "249",
            "name": "Demonstration product implementation",
            "region": "de",
            "code": null,
            "download": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=249",
            "active": "Y",
            "moduleId": "crm",
            "numeratorId": "1095",
            "withStamps": "N",
            "users": {
                "UA": "UA"
            },
            "isDeleted": "N",
            "sort": "500",
            "createTime": "2026-08-19T14:55:23+03:00",
            "updateTime": "2026-08-19T14:55:23+03:00",
            "entityTypeId": [
                "2"
            ],
            "downloadMachine": "https://your-domain.bitrix24.com/rest/crm.documentgenerator.template.download.json?..."
        }
    }
}
```

The numerator identifier came back in the `numeratorId` field as a string — this is the same numerator that was created in step 1. The value types in the response differ:

- the template's own `id` also arrives as a string: it can be passed to `templateId` in step 3 as is or converted to a number

- the template's `entityTypeId` array holds strings, while the document in step 3 accepts a number

The names of the fields that the template substitutes into the document are returned by the [crm.documentgenerator.template.getfields](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-get-fields.md) method for the `id` from this response. Compare that list with the fields in the `.docx` file if the document comes out empty.

## 3. Generate the Document

Build the document from the template and the deal data using the [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) method with the following parameters:

- `templateId` — the template identifier, a required parameter. Take the `id` from the step 2 response, `249` in the example

- `entityTypeId` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type), a required parameter. Specify `2` — a deal. The value has to be included in the template's `entityTypeId` array, otherwise the document is not created

- `entityId` — the object identifier, a required parameter. Pass the `dealId` value — the document is built from the data of this deal

{% note warning "" %}

The method does not check whether the object exists. If the `entityId` of a non-existent deal is passed, the document is still created: there is no error, but the deal fields in it remain empty and the numerator value is spent. Make sure the deal exists before the call.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const resDoc = await $b24.actions.v2.call.make({
        method: 'crm.documentgenerator.document.add',
        params: {
            templateId: templateId, // Template identifier from step 2
            entityTypeId: 2, // 2 — deal
            entityId: dealId // Deal identifier
        },
        requestId: 'document-add'
    });

    const documentId = resDoc.getData().result.document.id;
    ```

- Python

    ```python
    document = client.crm.documentgenerator.document.add(
        template_id=template_id,  # Template identifier from step 2
        entity_type_id=2,  # 2 — deal
        entity_id=deal_id,  # Deal identifier
    ).response.result["document"]

    document_id = document["id"]
    ```


- PHP

    ```php
    $resDoc = $sb->getCRMScope()->documentgeneratorDocument()->add(
        $templateId, // Template identifier from step 2
        2, // 2 — deal
        $dealId // Deal identifier
    );

    $documentId = $resDoc->getId();
    ```
{% endlist %}

In the response, the method returns a `document` object. The response is shortened, showing the fields that confirm the result.

```json
{
    "result": {
        "document": {
            "id": 1919,
            "title": "Demonstration product implementation 1",
            "number": "1",
            "templateId": "249",
            "entityTypeId": "2",
            "entityId": 8287,
            "createTime": "2026-08-19T14:55:42+03:00",
            "createdBy": 1,
            "publicUrl": null,
            "downloadUrl": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=1919",
            "downloadUrlMachine": "https://your-domain.bitrix24.com/rest/crm.documentgenerator.document.download.json?...",
            "products": {
                "currencyId": "EUR",
                "totalSum": "0.00",
                "totalRows": 0
            }
        }
    }
}
```

The `title` field is built from the template name and the number `1` issued by the numerator. The `downloadUrl` link opens the document in a browser, `downloadUrlMachine` returns the file when it is downloaded from an application. The `publicUrl` field is empty: the public link is enabled by the separate [crm.documentgenerator.document.enablepublicurl](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-enable-public-url.md) method.

## Verify the Result

Open the deal card in Bitrix24 — the new document appears in the deal document list under the name from the `title` field. From there it can be downloaded or sent to the client.

Through REST, the document is returned by the [crm.documentgenerator.document.get](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-get.md) method for the identifier from step 3.

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'crm.documentgenerator.document.get',
        params: { id: documentId },
        requestId: 'document-get'
    });

    console.dir(checkResult.getData().result.document);
    ```

- Python

    ```python
    check_result = client.crm.documentgenerator.document.get(
        int(document_id),
    ).response.result["document"]

    print(check_result)
    ```


- PHP

    ```php
    $checkResult = $sb->getCRMScope()->documentgeneratorDocument()->get($documentId);

    print_r($checkResult->document());
    ```
{% endlist %}

The scenario is complete if the response contains non-empty `id` and `downloadUrl` fields and the `templateId` matches the template identifier from step 2. The `number` field shows the number issued by the numerator: for the first document it is `1`, for the next one `2`.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error text** | **Reason and action** ||
|| `Empty required fields: template` | The `template` number template was not passed in step 1 ||
|| `Empty required fields: name, numeratorId, region, entityTypeId` | The required fields of the `fields` object were not passed in step 2. The message lists only the ones that are missing ||
|| `Missing file content` | The file content was not passed in `fields.file` in step 2. Make sure that the template file exists and is read before the call ||
|| `Could not save file` | The template file was not retained. Make sure that the whole `.docx` file got into Base64, without line breaks and the `data:` prefix ||
|| `You do not have permissions to modify templates` | The webhook user does not have permission to modify document generator templates ||
|| `Template not found` | The `templateId` of a non-existent or deleted template was passed in step 3 ||
|| `No provider for entityTypeId` | The `entityTypeId` passed in step 3 has no data source. For a deal, it is `2` ||
|| `Empty required parameter "value"` | The `entityId` was not passed in step 3, or it is empty ||
|| `Cannot create document` | The document was not built from the template. A common reason is that the file is not in the `.docx` format ||
|| The document is created, but the deal fields are empty | This is not an error. Either a non-existent deal was passed in `entityId`, or the template names fields that the object does not have ||
|| `DOCGEN_ACCESS_ERROR` | The webhook user does not have permission to modify document generator documents ||
|| `DOCGEN_LIMIT_ERROR` | The document limit of the plan has been reached ||
|| `Module documentgenerator is not installed` | The document generator module is not available in Bitrix24 ||
|#

The steps run as a chain, so repeat only the failed step and the ones that follow it. If step 2 returned the error, the numerator is already created — take its `id` from the step 1 response instead of creating a new one. If step 3 returned the error, the template is already uploaded and steps 1 and 2 do not need to be repeated.

## Key Considerations

- Running all three steps repeatedly accumulates duplicate numerators and templates in the CRM settings. The existing ones are returned by the [crm.documentgenerator.numerator.list](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-list.md) and [crm.documentgenerator.template.list](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-list.md) methods, and the extra templates are deleted by [crm.documentgenerator.template.delete](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-delete.md)

- The numerator counts numbers with a continuous counter. Documents created from different templates with the same numerator continue the shared numbering

- To build documents for invoices or smart processes, add their identifiers to the `entityTypeId` array when uploading the template, or update the template using the [crm.documentgenerator.template.update](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-update.md) method

## Code Example

The script sequentially creates the numerator, uploads the template, and builds the document for the deal. Each following call runs only after the previous one has succeeded.

{% list tabs %}

- JS

    ```javascript
    import { readFile } from 'node:fs/promises'
    import { basename } from 'node:path'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const templatePath = 'template.docx'; // path to the template file
    const templateName = 'Demonstration product implementation'; // template name
    const dealId = 8287; // deal identifier

    async function createDocument() {
        try {
            const fileContent = (await readFile(templatePath)).toString('base64');

            const resNum = await $b24.actions.v2.call.make({
                method: 'crm.documentgenerator.numerator.add',
                params: {
                    fields: { name: 'Numerator from REST', template: '{NUMBER}' }
                },
                requestId: 'numerator-add'
            });
            const numeratorId = resNum.getData().result.numerator.id;

            const resTemplate = await $b24.actions.v2.call.make({
                method: 'crm.documentgenerator.template.add',
                params: {
                    fields: {
                        name: templateName,
                        numeratorId: numeratorId,
                        region: 'de',
                        users: ['UA'],
                        entityTypeId: ['2'],
                        file: [basename(templatePath), fileContent]
                    }
                },
                requestId: 'template-add'
            });
            const templateId = resTemplate.getData().result.template.id;

            const resDoc = await $b24.actions.v2.call.make({
                method: 'crm.documentgenerator.document.add',
                params: {
                    templateId: templateId,
                    entityTypeId: 2,
                    entityId: dealId
                },
                requestId: 'document-add'
            });

            const document = resDoc.getData().result.document;
            console.log('Document created:', document.title, document.downloadUrl);
        } catch (error) {
            console.error('Document not created:', error.message);
        }
    }

    createDocument();
    ```

- Python

    ```python
    import base64
    from pathlib import Path

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    template_path = "template.docx"  # path to the template file
    template_name = "Demonstration product implementation"  # template name
    deal_id = 8287  # deal identifier

    try:
        with open(template_path, "rb") as file:
            file_content = base64.b64encode(file.read()).decode("ascii")
    except OSError as error:
        print(f"Template file not read: {error}")
    else:
        try:
            numerator = client.crm.documentgenerator.numerator.add(
                fields={
                    "name": "Numerator from REST",
                    "template": "{NUMBER}",
                },
            ).response.result["numerator"]

            template = client.crm.documentgenerator.template.add(
                fields={
                    "name": template_name,
                    "numeratorId": numerator["id"],
                    "region": "de",
                    "users": ["UA"],
                    "entityTypeId": ["2"],
                    "file": [Path(template_path).name, file_content],
                },
            ).response.result["template"]

            document = client.crm.documentgenerator.document.add(
                template_id=int(template["id"]),
                entity_type_id=2,
                entity_id=deal_id,
            ).response.result["document"]
        except BitrixAPIError as error:
            print(f"Document not created: {error}")
        else:
            print(f"Document created: {document['title']} {document['downloadUrl']}")
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $templatePath = __DIR__ . '/template.docx'; // path to the template file
    $templateName = 'Demonstration product implementation'; // template name
    $dealId = 8287; // deal identifier

    try {
        if (!is_readable($templatePath)) {
            throw new \RuntimeException('Template file not found: ' . $templatePath);
        }
        $fileContent = base64_encode(file_get_contents($templatePath));

        $resNum = $sb->getCRMScope()->documentgeneratorNumerator()->add(
            [
                'name' => 'Numerator from REST',
                'template' => '{NUMBER}',
            ]
        );
        $numeratorId = $resNum->getId();

        $resTemplate = $sb->getCRMScope()->documentgeneratorTemplate()->add(
            [
                'name' => $templateName,
                'numeratorId' => $numeratorId,
                'region' => 'de',
                'users' => ['UA'],
                'entityTypeId' => ['2'],
                'file' => [basename($templatePath), $fileContent]
            ]
        );
        $templateId = $resTemplate->getId();

        $resDoc = $sb->getCRMScope()->documentgeneratorDocument()->add(
            $templateId,
            2,
            $dealId
        );

        echo 'Document created: ' . $sb->getCRMScope()
            ->documentgeneratorDocument()
            ->get($resDoc->getId())
            ->document()
            ->title;
    } catch (\Throwable $e) {
        echo 'Document not created: ' . $e->getMessage();
    }
    ```
{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md)
- [{#T}](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-list.md)
- [{#T}](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md)
- [{#T}](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-list.md)
- [{#T}](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-get-fields.md)
- [{#T}](../../../api-reference/document-generator/region/document-generator-region-list.md)
- [{#T}](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md)
- [{#T}](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-get.md)
- [{#T}](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-enable-public-url.md)
- [{#T}](../../../api-reference/files/how-to-upload-files.md)
