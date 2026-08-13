# Generate a Document with Images and Stamps

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

Images, stamps, and signatures for template placeholders are passed to the method [documentgenerator.document.add](../document-generator-document-add.md) as file links in `values`. The files are downloaded from the specified URL and inserted into the document during generation.

## When to Use

- When you need to insert an image from an external link
- When you need to add a stamp or signature in a template field

## What to Include in the Request

- The required request parameters are `templateId` and `value`: the template identifier and the external identifier of the object for which the document is created
- In `values`, provide absolute URLs for the files. The file URL must be accessible by Bitrix24 without additional authorization.
- In `fields`, specify the type for the field code:
  - `IMAGE` — image field
  - `STAMP` — stamp or signature field
- The field codes in `values` and `fields` must match the placeholder codes in the template.

## Example

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"templateId":203,"value":"ORDER_1024","stampsEnabled":1,"values":{"Stamp":"https://myrestapp.example/upload/stamp.png","Image":"https://myrestapp.example/upload/image.jpg"},"fields":{"Stamp":{"TYPE":"STAMP"},"Image":{"TYPE":"IMAGE"}}}' \
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
        stampsEnabled: boolean
        downloadUrl: string
      }
    }

    const response = await $b24.actions.v2.call.make<DocumentAddResult>({
      method: 'documentgenerator.document.add',
      params: {
        templateId: 203,
        value: 'ORDER_1024',
        stampsEnabled: 1,
        values: {
          // external link to the stamp file
          Stamp: 'https://myrestapp.example/upload/stamp.png',
          // external link to the image file
          Image: 'https://myrestapp.example/upload/image.jpg',
        },
        fields: {
          Stamp: { TYPE: 'STAMP' },
          Image: { TYPE: 'IMAGE' },
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
                'stampsEnabled' => 1,
                'values' => [
                    // external path to the stamp file
                    'Stamp' => 'https://myrestapp.example/upload/stamp.png',
                    // external path to the image file
                    'Image' => 'https://myrestapp.example/upload/image.jpg',
                ],
                'fields' => [
                    // field type - stamp
                    'Stamp' => ['TYPE' => 'STAMP'],
                    // field type - image
                    'Image' => ['TYPE' => 'IMAGE'],
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

The `stampsEnabled` parameter controls the display of `STAMP` type fields in the document. If it is not passed, the value from the template is applied.

## What Is Returned

The method returns the data of the created document. The response example is abbreviated; a complete description of the fields is available on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

```json
{
    "result": {
        "document": {
            "id": 51,
            "title": "ORDER Template 51",
            "templateId": "203",
            "value": "ORDER_1024",
            "stampsEnabled": true,
            "isTransformationError": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068"
        }
    }
}
```

The `stampsEnabled` field in the response shows whether stamps and signatures are enabled for the created document.

## Verify the Result

1. Download the file by `downloadUrl` from the response
2. Make sure that the image and the stamp are inserted into the intended placeholders and do not remain text links
3. If a field is empty, check that the file URL opens without authorization and returns an image

## If the Method Returns an Error

- `Empty required parameter "value"` — the required parameter `value` is not provided
- `Template not found` — no template exists with the specified `templateId`

A link is shown in the document instead of an image — `TYPE` = `IMAGE` or `TYPE` = `STAMP` is not specified for this field in `fields`.

The stamp did not appear — check `stampsEnabled` in the response. If it is `false`, pass `stampsEnabled` = `1` in the request or enable stamps in the template.

The complete list of errors is available in the "Error Handling" section on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./index.md)