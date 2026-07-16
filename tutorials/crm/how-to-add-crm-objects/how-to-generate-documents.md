# How to Add a Template and Create a Document Based on It

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can automate working with documents in the CRM using a script. It will perform the full document generation cycle: create a numerator, upload a template in `.docx` format, and generate a document for a specific deal.

To create a document, we will call the following methods in sequence:

1. [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) — create a document numerator,

2. [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) — upload a document template,

3. [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) — generate a document.

## Prepare Variables

Define the main variables that will be used during the document generation process.

-  `filePath` — path to the template file. We will specify `template.docx`.

-  `iDealID` — deal identifier. We will create a document for a deal with identifier `1`.

-  `sDocName` — name of the document being created. We will specify Demonstration product implementation.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

   ```javascript
   let filePath = 'template.docx'; 
   let iDealID = 1; 
   let sDocName = 'Demonstration product implementation';
   ```

-  PHP

   ```php
   $filePath = __DIR__ . '/template.docx';  
   $iDealID = 1;  
   $sDocName = 'Demonstration product implementation';
   ```

- Python

   ```python
   file_path = "template.docx"
   deal_id = 1
   document_name = "Demonstration product implementation"
   ```

{% endlist %}

## 1\. Create a Document Numerator

Create a numerator for documents using [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md). Pass two parameters to the method.

-  `name` — numerator name. We will specify `Rest Numerator`.

-  `template` — the template used to generate the document number. We will specify `{NUMBER}` — this is a variable that will be replaced by a sequential number. You can use other variables, such as `{DAY}` — current day, `{CLIENT_ID}` — customer identifier, `{RANDOM}` — random number.

{% list tabs %}

-  JS

   ```javascript
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   const resNum = await $b24.actions.v2.call.make({
       method: 'crm.documentgenerator.numerator.add',
       params: {
           fields: {
               'name': 'Enumerator from REST',
               'template': '{NUMBER}'
           }
       },
       requestId: 'numerator-add'
   });
   ```

-  PHP

   ```php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Psr\Log\NullLogger;

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   $resNum = $sb->getCRMScope()->documentgeneratorNumerator()->add(
       [
           'name' => 'Enumerator from REST',
           'template' => '{NUMBER}',
       ]
   );
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

   res_num = client.crm.documentgenerator.numerator.add(
       fields={
           "name": "Enumerator from REST",
           "template": "{NUMBER}",
       }
   ).response.result
   ```

{% endlist %}

The [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) method returns an `resNum` object containing information about the created numerator.

```json
"numerator":{
    "name":"Enumerator from REST",
    "template":"{NUMBER}",
    "id":43,
    "code":null,
    "settings":{
        "Bitrix_Main_Numerator_Generator_SequentNumberGenerator":{
            "start":1,
            "step":1,
            "length":0,
            "padString":"0",
            "periodicBy":null,
            "timezone":null,
            "isDirectNumeration":false
        }
    }
}
```

## 2\. Upload a Document Template

Once the numerator is created, add a document template using the [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) method.

{% note warning "" %}

The template file content must be converted to [Base64](../../../api-reference/files/how-to-upload-files.md) format.

{% endnote %}

Pass the following data to [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md):

-  `name` — template name. We will specify the variable `sDocName`.

-  `numeratorId` — numerator identifier. Pass this from the `resNum` object obtained in the first step.

-  `region` — template region. This affects localization, such as currency and date. We will specify `de` — Germany.

-  `users` — access rights array. Defines which user groups can view and use the template. We will specify `UA` — all authorized users.

-  `entityTypeId` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We will specify `2` — deal. A full list of object types can be retrieved using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

-  `file` — content of file `filePath`, converted to Base64 format.

{% list tabs %}

-  JS

   ```javascript
   function fileToBase64(filePath) {
       return new Promise((resolve, reject) => {
           fetch(filePath)
               .then(response => response.blob())
               .then(blob => {
                   let reader = new FileReader();
                   reader.onloadend = () => resolve(reader.result.split(',')[1]);
                   reader.onerror = reject;
                   reader.readAsDataURL(blob);
               });
       });
   }
   
   let fileContent = await fileToBase64(filePath);
   
   const resTemplate = await $b24.actions.v2.call.make({
       method: 'crm.documentgenerator.template.add',
       params: {
           fields: {
               'name': sDocName,
               'numeratorId': resNum.getData().result.numerator.id,
               'region': 'de',
               'users': ['UA'],
               'entityTypeId': ['2'],
               'file': fileContent
           }
       },
       requestId: 'template-add'
   });
   ```

-  PHP

   ```php
   $resTemplate = $sb->getCRMScope()->documentgeneratorTemplate()->add(
       [
           'name' => $sDocName,
           'numeratorId' => $resNum->getId(), // crm.documentgenerator.numerator.add
           'region' => 'de', // eu,de,ua,by,ru
           'users' => [
               'UA'//User All
           ],
           'entityTypeId' => ['2'], // 2 — deal (crm.enum.ownertype)
           'file' => base64_encode(file_get_contents($filePath))
       ]
   );
   ```

- Python

   ```python
   import base64

   with open(file_path, "rb") as file:
       file_content = base64.b64encode(file.read()).decode("ascii")

   res_template = client.crm.documentgenerator.template.add(
       fields={
           "name": document_name,
           "numeratorId": res_num["numerator"]["id"],
           "region": "de",
           "users": ["UA"],
           "entityTypeId": ["2"],
           "file": file_content,
       }
   ).response.result
   ```

{% endlist %}

The [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) method returns an `resTemplate` object containing information about the template.

```json
template: { 
    "id": "39", 
    "name": "Demonstration product implementation", 
    "region": "de",
    "​​active": "Y",
    "​​code": null,
    "​​createTime": "2025-07-09T16:12:13+03:00",
    "​​download": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=39",
    "​​downloadMachine": "https://some-domain.bitrix24.com/rest/crm.documentgenerator.template.download.json?sessid=c4ad892d7583ead4fd38666a0af85cb7&token=crm%7CYWN0aW9uPWNybS5kb2N1bWVudGdlbmVyYXRvci50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTM5Jl89azNRNlFuVVRvUGl5VzNLaExTVDJCR3g1WjdyQ0tSSFA%3D%7CImNybS5kb2N1bWVudGdlbmVyYXRvci50ZW1wbGF0ZS5kb3dubG9hZHxjcm18WVdOMGFXOXVQV055YlM1a2IyTjFiV1Z1ZEdkbGJtVnlZWFJ2Y2k1MFpXMXdiR0YwWlM1a2IzZHViRzloWkNaVFNWUkZYMGxFUFhNeEptbGtQVE01Smw4OWF6TlJObEZ1VlZSdlVHbDVWek5MYUV4VFZESkNSM2cxV2pkeVEwdFNTRkE9fGM0YWQ4OTJkNzU4M2VhZDRmZDM4NjY2YTBhZjg1Y2I3Ig%3D%3D.GMgjAbCT099xlo8CJN9n5mP2s7MBbqfU%2BbEM%2FAzpoYE%3D",
    "​​entityTypeId": [ "0": "2" ],
    "​​​length": 1,
    "​​​numeratorId": "43",
    "​​users": [ "0": "UA" ],
    "sort": 500
}
​​
```

## 3. Generate a Document

If the template is successfully uploaded, create a document for the deal using the [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) method. Specify three parameters in the method.

1. `templateId` — the template identifier. Pass it from the `resTemplate` object obtained in step two.

2. `entityTypeId` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). Specify `2` — deal. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

3. `entityId` — the deal identifier. Specify the `iDealID` variable.

{% list tabs %}

-  JS

   ```js
   const resDoc = await $b24.actions.v2.call.make({
       method: 'crm.documentgenerator.document.add',
       params: {
           'templateId': resTemplate.getData().result.template.id,
           'entityTypeId': '2',
           'entityId': iDealID
       },
       requestId: 'document-add'
   });
   ```

-  PHP

   ```php
   $resDoc = $sb->getCRMScope()->documentgeneratorDocument()->add(
       templateId: $resTemplate->getId(),
       entityTypeId: 2, // 2 — deal (crm.enum.ownertype)
       entityId: $iDealID,
   );
   ```

- Python

   ```python
   res_doc = client.crm.documentgenerator.document.add(
       template_id=int(res_template["template"]["id"]),
       entity_type_id=2,
       entity_id=deal_id,
       values={},
       stamps_enabled=False,
   ).response.result
   ```

{% endlist %}

The document will be generated, and the [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) method will return its parameters.

```json
"document":{
    "products":{
        "currencyId":"EUR",
        "totalSum":1500,
        "totalRows":1
    },
    "downloadUrl":"https:\\/\\/some-domain.bitrix24.com\\/bitrix\\/services\\/main\\/ajax.php?action=crm.documentgenerator.document.download\\u0026SITE_ID=s1\\u0026id=29",
    "publicUrl":null,
    "title":"Demonstration product implementation 1",
    "number":"1",
    "id":29,
    "createTime":"2025-07-09T16:29:27+03:00",
    "createdBy":27,
    "updateTime":"2025-07-09T16:29:27+03:00",
    "templateId":"39",
    "emailDiskFile":4917,
    "entityId":"1",
    "entityTypeId":"2",
    "downloadUrlMachine":"https:\\/\\/some-domain.bitrix24.com\\/rest\\/crm.documentgenerator.document.download.json?sessid=c4ad892d7583ead4fd38666a0af85cb7\\u0026token=crm%7CYWN0aW9uPWNybS5kb2N1bWVudGdlbmVyYXRvci5kb2N1bWVudC5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTI5Jl89YlQ2SU9XeGVnR2s3NnZ5M0hGVlRxTDVaRlJtdFgyNTE%3D%7CImNybS5kb2N1bWVudGdlbmVyYXRvci5kb2N1bWVudC5kb3dubG9hZHxjcm18WVdOMGFXOXVQV055YlM1a2IyTjFiV1Z1ZEdkbGJtVnlZWFJ2Y2k1a2IyTjFiV1Z1ZEM1a2IzZHViRzloWkNaVFNWUkZYMGxFUFhNeEptbGtQVEk1Smw4OVlsUTJTVTlYZUdWblIyczNOblo1TTBoR1ZsUnhURFZhUmxKdGRGZ3lOVEU9fGM0YWQ4OTJkNzU4M2VhZDRmZDM4NjY2YTBhZjg1Y2I3Ig%3D%3D.H575mM4Mf%2Fj4PVH2Ngzb1kmkQhdScsAL75ZJkbYkALk%3D"
}
```

## Code Example

{% list tabs %}

-  JS

   ```javascript
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   let filePath = 'template.docx'; // path to local template file
   let iDealID = 1; // deal identifier
   let sDocName = 'Demonstration product implementation';

   function fileToBase64(filePath) {
       return new Promise((resolve, reject) => {
           fetch(filePath)
               .then(response => response.blob())
               .then(blob => {
                   let reader = new FileReader();
                   reader.onloadend = () => resolve(reader.result.split(',')[1]);
                   reader.onerror = reject;
                   reader.readAsDataURL(blob);
               });
       });
   }

   async function createDocument() {
       try {
           let fileContent = await fileToBase64(filePath);

           const resNum = await $b24.actions.v2.call.make({
               method: 'crm.documentgenerator.numerator.add',
               params: { fields: { 'name': 'Enumerator from REST', 'template': '{NUMBER}' } },
               requestId: 'numerator-add'
           });

           if (resNum.getData().result.numerator.id) {
               const resTemplate = await $b24.actions.v2.call.make({
                   method: 'crm.documentgenerator.template.add',
                   params: {
                       fields: {
                           'name': sDocName,
                           'numeratorId': resNum.getData().result.numerator.id,
                           'region': 'de',
                           'users': ['UA'],
                           'entityTypeId': ['2'],
                           'file': fileContent
                       }
                   },
                   requestId: 'template-add'
               });

               if (resTemplate.getData().result.template.id) {
                   await $b24.actions.v2.call.make({
                       method: 'crm.documentgenerator.document.add',
                       params: {
                           'templateId': resTemplate.getData().result.template.id,
                           'entityTypeId': '2',
                           'entityId': iDealID
                       },
                       requestId: 'document-add'
                   });
                   alert('Document created');
               }
           }
       } catch (error) {
           console.error(error);
           alert('Error: ' + error.message);
       }
   }

   createDocument();
   ```

-  PHP

   ```php
   <?php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Psr\Log\NullLogger;

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   $filePath = __DIR__ . '/template.docx'; // path to local template file
   $iDealID = 1; // deal identifier
   $sDocName = 'Demonstration product implementation';
   try {
       $resNum = $sb->getCRMScope()->documentgeneratorNumerator()->add(
           [
               'name' => 'Enumerator from REST',
               'template' => '{NUMBER}',
           ]
       );
       $resDoc = null;
       if (!empty($resNum->getId()))
       {
           $resTemplate = $sb->getCRMScope()->documentgeneratorTemplate()->add(
               [
                   'name' => $sDocName,
                   'numeratorId' => $resNum->getId(), // crm.documentgenerator.numerator.add
                   'region' => 'de', // eu,de,ua,by,ru
                   'users' => [
                       'UA'//User All
                   ],
                   'entityTypeId' => ['2'], // 2 — deal (crm.enum.ownertype)
                   'file' => base64_encode(file_get_contents($filePath))
               ]
           );
           if (!empty($resTemplate->getId()))
           {
               $resDoc = $sb->getCRMScope()->documentgeneratorDocument()->add(
                   templateId: $resTemplate->getId(),
                   entityTypeId: 2, // 2 — deal (crm.enum.ownertype)
                   entityId: $iDealID,
               );
           }
       }
       if (!empty($resDoc) && !empty($resDoc->getId()))
       {
           echo json_encode(['message' => 'Document created']);
       }
       else
       {
           echo json_encode(['message' => 'Document not created']);
       }
   } catch (\Throwable $e) {
       echo json_encode(['message' => 'Document not created: ' . $e->getMessage()]);
   }
   ```

- Python

    ```python
    import base64

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    template_path = "template.docx"
    deal_id = 1
    document_name = "Demonstration product implementation"

    try:
        numerator = client.crm.documentgenerator.numerator.add(
            fields={
                "name": "Enumerator from REST",
                "template": "{NUMBER}",
            }
        ).response.result["numerator"]
    except BitrixAPIError as error:
        print(f"Number generator not added: {error}")
    else:
        with open(template_path, "rb") as file:
            template_content = base64.b64encode(file.read()).decode("ascii")

        try:
            template = client.crm.documentgenerator.template.add(
                fields={
                    "name": document_name,
                    "numeratorId": numerator["id"],
                    "region": "de",
                    "users": ["UA"],
                    "entityTypeId": ["2"],
                    "file": template_content,
                }
            ).response.result["template"]
        except BitrixAPIError as error:
            print(f"Template not added: {error}")
        else:
            try:
                document = client.crm.documentgenerator.document.add(
                    template_id=int(template["id"]),
                    entity_type_id=2,
                    entity_id=deal_id,
                    values={},
                    stamps_enabled=False,
                ).response.result["document"]
            except BitrixAPIError as error:
                print(f"Document not created: {error}")
            else:
                if document:
                    print("Document created")
                else:
                    print("Document not created")
    ```

{% endlist %}
