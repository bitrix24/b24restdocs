# How to Add a Template and Create a Document Based on It

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

You can automate document handling in CRM using a script. It will perform the complete cycle of document generation: create a number generator, upload a template in `.docx` format, and generate a document for a specific deal.

To create a document, we will sequentially execute the following methods:

1. [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) — we will create a document number generator,

2. [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) — we will upload the document template,

3. [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) — we will generate the document.

## Prepare Variables

Let's define the main variables that we will use during the document generation process.

-  `filePath` — path to the template file. We will specify `template.docx`.

-  `iDealID` — deal identifier. We will create a document for the deal with the identifier `1`.

-  `sDocName` — name of the document being created. We will specify `Product Demonstration Implementation`.

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

   ```javascript
   let filePath = 'template.docx'; 
   let iDealID = 1; 
   let sDocName = 'Product Demonstration Implementation';
   ```

-  PHP

   ```php
   $filePath = __DIR__ . '/template.docx';  
   $iDealID = 1;  
   $sDocName = 'Product Demonstration Implementation';
   ```

{% endlist %}

## 1\. Create a Document Number Generator

We will create a number generator for documents using [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md). We will pass two parameters to the method.

-  `name` — name of the number generator. We will specify `Rest Numerator`.

-  `template` — the template from which the document number will be generated. We will specify `{NUMBER}` — this is a variable that will be replaced with the sequential number. Other variables can be used, for example, `{DAY}` — current day, `{CLIENT_ID}` — client identifier, `{RANDOM}` — random number.

{% list tabs %}

-  JS

   ```javascript
   BX24.callMethod(
       'crm.documentgenerator.numerator.add',
       {
           'fields': {
               'name': 'Rest Numerator',
               'template': '{NUMBER}'
           }
       },
       function(resNum) {
           if (resNum.error()) {
               console.error(resNum.error());
               alert('Number generator not added: ' + resNum.error_description());
               return;
           }
       }
   );
   ```

-  PHP

   ```php
   $resNum = CRest::call(
       'crm.documentgenerator.numerator.add',
       [
           'fields' => [
               'name' => 'Rest Numerator',
               'template' => '{NUMBER}',
           ]
       ]
   );
   ```

{% endlist %}

The method [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md) will return an object `resNum` with information about the created number generator.

```json
"numerator": {
    "name": "Rest Numerator",
    "template": "{NUMBER}",
    "id": 43,
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
```

## 2\. Upload the Document Template

If the number generator is created, we will add the document template using the method [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md).

{% note warning "" %}

The content of the template file needs to be converted to [Base64](../../../api-reference/files/how-to-upload-files.md).

{% endnote %}

In [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md), we need to pass the following data:

-  `name` — name of the template. We will specify the variable `sDocName`.

-  `numeratorId` — identifier of the number generator. We will pass it from the object `resNum`, which was obtained in the first step.

-  `region` — region of the template. This affects localization, such as currency and date. We will specify `de` — Germany.

-  `users` — array of access permissions. Defines which user groups can see and use the template. We will specify `UA` — all authorized users.

-  `entityTypeId` — [identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We will specify `2` — deal. A complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

-  `file` — content of the file `filePath`, which has been converted to Base64.

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
   
   BX24.callMethod(
       'crm.documentgenerator.template.add',
       {
           'fields': {
               'name': sDocName,
               'numeratorId': resNum.data().numerator.id,
               'region': 'de',
               'users': ['UA'],
               'entityTypeId': ['2'],
               'file': fileContent
           }
       },
       function(resTemplate) {
           if (resTemplate.error()) {
               console.error(resTemplate.error());
               alert('Template not added: ' + resTemplate.error_description());
               return;
           }
       }
   );
   ```

-  PHP

   ```php
   $resTemplate = CRest::call(
       'crm.documentgenerator.template.add',
       [
           'fields' => [
               'name' => $sDocName,
               'numeratorId' => $resNum['result']['numerator']['id'],  
               'region' => 'de', 
               'users' => [
                   'UA' // User All
               ],
               'entityTypeId' => ['2'], 
               'file' => base64_encode(file_get_contents($filePath))
           ]
       ]
   );
   ```

{% endlist %}

The method [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md) will return an object `resTemplate` with information about the template.

```json
template: { 
    "id": "39", 
    "name": "Product Demonstration Implementation", 
    "region": "de",
    "active": "Y",
    "code": null,
    "createTime": "2025-07-09T16:12:13+02:00",
    "download": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=39",
    "downloadMachine": "https://some-domain.bitrix24.com/rest/crm.documentgenerator.template.download.json?sessid=c4ad892d7583ead4fd38666a0af85cb7&token=crm%7CYWN0aW9uPWNybS5kb2N1bWVudGdlbmVyYXRvci50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTM5Jl89azNRNlFuVVRvUGl5VzNLaExTVDJCR3g1WjdyQ0tSSFA%3D%7CImNybS5kb2N1bWVudGdlbmVyYXRvci50ZW1wbGF0ZS5kb3dubG9hZHxjcm18WVdOMGFXOXVQV055YlM1a2IyTjFiV1Z1ZEdkbGJtVnlZWFJ2Y2k1MFpXMXdiR0YwWlM1a2IzZHViRzloWkNaVFNWUkZYMGxFUFhNeEptbGtQVE01Smw4OWF6TlJObEZ1VlZSdlVHbDVWek5MYUV4VFZESkNSM2cxV2pkeVEwdFNTRkE9fGM0YWQ4OTJkNzU4M2VhZDRmZDM4NjY2YTBhZjg1Y2I3Ig%3D%3D.GMgjAbCT099xlo8CJN9n5mP2s7MBbqfU%2BbEM%2FAzpoYE%3D",
    "entityTypeId": [ "0": "2" ],
    "length": 1,
    "numeratorId": "43",
    "users": [ "0": "UA" ],
    "sort": 500
}
```

## 3\. Generate the Document

If the template is successfully uploaded, we will create a document for the deal using the method [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md). We will specify three parameters in the method.

1. `templateId` — identifier of the template. We will pass it from the object `resTemplate`, which was obtained in the second step.

2. `entityTypeId` — [identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We will specify `2` — deal. A complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

3. `entityId` — identifier of the deal. We will specify the variable `iDealID`.

{% list tabs %}

-  JS

   ```js
   BX24.callMethod(
       'crm.documentgenerator.document.add',
       {
           'templateId': resTemplate.data().template.id,
           'entityTypeId': '2',
           'entityId': iDealID
       },
       function(resDoc) {
           if (resDoc.error()) {
               console.error(resDoc.error());
               alert('Document not created: ' + resDoc.error_description());
           } else {
               alert('Document created');
           }
       }
   );
   ```

-  PHP

   ```php
   $resDoc = CRest::call(
       'crm.documentgenerator.document.add',
       [
           'templateId' => $resTemplate['result']['template']['id'],
           'entityTypeId' => '2', 
           'entityId' => $iDealID,
       ]
   );
   ```

{% endlist %}

The document will be generated, and the method [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) will return its parameters.

```json
"document": {
    "products": {
        "currencyId": "EUR",
        "totalSum": 1500,
        "totalRows": 1
    },
    "downloadUrl": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=29",
    "publicUrl": null,
    "title": "Product Demonstration Implementation 1",
    "number": "1",
    "id": 29,
    "createTime": "2025-07-09T16:29:27+02:00",
    "createdBy": 27,
    "updateTime": "2025-07-09T16:29:27+02:00",
    "templateId": "39",
    "emailDiskFile": 4917,
    "entityId": "1",
    "entityTypeId": "2",
    "downloadUrlMachine": "https://some-domain.bitrix24.com/rest/crm.documentgenerator.document.download.json?sessid=c4ad892d7583ead4fd38666a0af85cb7&token=crm%7CYWN0aW9uPWNybS5kb2N1bWVudGdlbmVyYXRvci5kb2N1bWVudC5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTI5Jl89YlQ2SU9XeGVnR2s3NnZ5M0hGVlRxTDVaRlJtdFgyNTE%3D%7CImNybS5kb2N1bWVudGdlbmVyYXRvci5kb2N1bWVudC5kb3dubG9hZHxjcm18WVdOMGFXOXVQV055YlM1a2IyTjFiV1Z1ZEdkbGJtVnlZWFJ2Y2k1a2IyTjFiV1Z1ZEM1a2IzZHViRzloWkNaVFNWUkZYMGxFUFhNeEptbGtQVEk1Smw4OVlsUTJTVTlYZUdWblIyczNOblo1TTBoR1ZsUnhURFZhUmxKdGRGZ3lOVEU9fGM0YWQ4OTJkNzU4M2VhZDRmZDM4NjY2YTBhZjg1Y2I3Ig%3D%3D.H575mM4Mf%2Fj4PVH2Ngzb1kmkQhdScsAL75ZJkbYkALk%3D"
}
```

## Code Example

{% list tabs %}

-  JS

   ```javascript
   document.addEventListener('DOMContentLoaded', function() {
       let filePath = 'template.docx'; // path to the local template file
       let iDealID = 1; // deal identifier
       let sDocName = 'Product Demonstration Implementation';
   
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
   
               BX24.callMethod(
                   'crm.documentgenerator.numerator.add',
                   {
                       'fields': {
                           'name': 'Rest Numerator',
                           'template': '{NUMBER}'
                       }
                   },
                   function(resNum) {
                       if (resNum.error()) {
                           console.error(resNum.error());
                           alert('Number generator not added: ' + resNum.error_description());
                           return;
                       }
   
                       if (resNum.data().numerator.id) {
                           BX24.callMethod(
                               'crm.documentgenerator.template.add',
                               {
                                   'fields': {
                                       'name': sDocName,
                                       'numeratorId': resNum.data().numerator.id,
                                       'region': 'de',
                                       'users': ['UA'],
                                       'entityTypeId': ['2'],
                                       'file': fileContent
                                   }
                               },
                               function(resTemplate) {
                                   if (resTemplate.error()) {
                                       console.error(resTemplate.error());
                                       alert('Template not added: ' + resTemplate.error_description());
                                       return;
                                   }
   
                                   if (resTemplate.data().template.id) {
                                       BX24.callMethod(
                                           'crm.documentgenerator.document.add',
                                           {
                                               'templateId': resTemplate.data().template.id,
                                               'entityTypeId': '2',
                                               'entityId': iDealID
                                           },
                                           function(resDoc) {
                                               if (resDoc.error()) {
                                                   console.error(resDoc.error());
                                                   alert('Document not created: ' + resDoc.error_description());
                                               } else {
                                                   alert('Document created');
                                               }
                                           }
                                       );
                                   }
                               }
                           );
                       }
                   }
               );
           } catch (error) {
               console.error(error);
               alert('Error: ' + error.message);
           }
       }
   
       createDocument();
   });
   ```

-  PHP

   ```php
   $filePath = __DIR__ . '/template.docx'; // path to the local template file
   $iDealID = 1; // deal identifier
   $sDocName = 'Product Demonstration Implementation';
   $resNum = CRest::call(
       'crm.documentgenerator.numerator.add',
       [
           'fields' => [
               'name' => 'Rest Numerator',
               'template' => '{NUMBER}',
           ]
       ]
   );
   if (!empty($resNum['result']['numerator']['id'])) {
       $resTemplate = CRest::call(
           'crm.documentgenerator.template.add',
           [
               'fields' => [
                   'name' => $sDocName,
                   'numeratorId' => $resNum['result']['numerator']['id'], // crm.documentgenerator.numerator.add
                   'region' => 'de', // eu,de,ua,by,ru
                   'users' => [
                       'UA' // User All
                   ],
                   'entityTypeId' => ['2'], // 2 — deal in CRest::call('crm.enum.ownertype');
                   'file' => base64_encode(file_get_contents($filePath))
               ]
           ]
       );
       if (!empty($resTemplate['result']['template']['id'])) {
           $resDoc = CRest::call(
               'crm.documentgenerator.document.add',
               [
                   'templateId' => $resTemplate['result']['template']['id'],
                   'entityTypeId' => '2', // 2 — deal in CRest::call('crm.enum.ownertype');
                   'entityId' => $iDealID,
               ]
           );
       }
   }
   if (!empty($resDoc['result'])) {
       echo json_encode(['message' => 'Document created']);
   } elseif (!empty($resDoc['error_description'])) {
       echo json_encode(['message' => 'Document not created: ' . $resDoc['error_description']]);
   } else {
       echo json_encode(['message' => 'Document not created']);
   }
   ```

{% endlist %}