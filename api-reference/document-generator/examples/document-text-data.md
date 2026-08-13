# Generate a Document with Text

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

Text values for the template placeholders are passed to the method [documentgenerator.document.add](../document-generator-document-add.md) via the `values` parameter without additional field type settings.

## When to Use

- The template contains only text placeholders without type modifiers.
- There is no need to specify `TYPE`, `FORMAT`, and providers in `fields`.

## What to Pass in the Request

- `templateId` — identifier of the template used to create the document
- `value` — external identifier of the object for which the document is created
- `values` — an object of the form `"FieldCode": "TextValue"`
- `fields` can be omitted if all fields are inserted as plain text without formatting.

Keys in `values` must match the field codes from the template; for example, for the placeholder `{SomeName}`, you need to pass `'SomeName'`.

You can obtain the field codes of the template using the method [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md).

The data provider `Bitrix\DocumentGenerator\DataProvider\Rest` is applied automatically, so `providerClassName` can be omitted.

## Example

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"templateId":203,"value":"ORDER_1024","values":{"DocumentNumber":"DG-2026-001","CurrentDate":"03/18/2026","ClientName":"Ltd. Superbank","Comment":"Payment within 5 business days after signing"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentAddResult = {
      document: {
        id: number
        title: string
        downloadUrl: string
      }
    }

    const response = await $b24.actions.v2.call.make<DocumentAddResult>({
      method: 'documentgenerator.document.add',
      params: {
        templateId: 203,
        value: 'ORDER_1024',
        values: {
          DocumentNumber: 'DG-2026-001',
          CurrentDate: '03/18/2026',
          ClientName: 'Ltd. Superbank',
          Comment: 'Payment within 5 business days after signing',
        },
      },
      requestId: Text.getUuidRfc4122()
    })

    if (!response.isSuccess) {
      console.error(response.getErrorMessages().join('; '))
    } else {
      console.info('Created document id:', response.getData()!.result.document.id)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'documentgenerator.document.add',
            [
                'templateId' => 203,
                'value' => 'ORDER_1024',
                'values' => [
                    'DocumentNumber' => 'DG-2026-001',
                    'CurrentDate' => '03/18/2026',
                    'ClientName' => 'Ltd. Superbank',
                    'Comment' => 'Payment within 5 business days after signing',
                ],
            ]
        );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

{% endlist %}

## What Is Returned

The method returns the data of the created document. The response example is abbreviated; a complete description of the fields is available on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

```json
{
    "result": {
        "document": {
            "id": 51,
            "title": "ORDER Template DG-2026-001",
            "number": "DG-2026-001",
            "templateId": "203",
            "value": "ORDER_1024",
            "isTransformationError": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068",
            "publicUrl": null
        }
    }
}
```

What to take from the response:

- `id` — document identifier for further calls
- `downloadUrl` — DOCX download link for the user, `downloadUrlMachine` — the same link for the application
- `publicUrl` — public link, it equals `null` until it is enabled with the [documentgenerator.document.enablepublicurl](../document-generator-document-enable-public-url.md) method

## Verify the Result

1. Retrieve the document with the [documentgenerator.document.get](../document-generator-document-get.md) method using the `id` from the response
2. Download the file by `downloadUrl` and make sure that the passed values are inserted in the document text instead of the placeholders
3. If you need a PDF, check the `pdfUrl` field in the response of the [documentgenerator.document.get](../document-generator-document-get.md) method. The conversion is performed asynchronously, so the field is not filled immediately

## If the Method Returns an Error

- `Empty required parameter "value"` — the required parameter `value` is not provided
- `Template not found` — no template exists with the specified `templateId`
- `Cannot create document on deleted template` — the template is marked as deleted, create the document from another template

The document was created, but the fields are empty — the codes in `values` do not match the template placeholders. Compare them with the response of the [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md) method.

The complete list of errors is available in the "Error Handling" section on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

## Continue Learning

- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)