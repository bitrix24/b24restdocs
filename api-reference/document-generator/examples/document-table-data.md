# Generate a Document with Tabular Data

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

Tabular data for template placeholders is passed to the method [documentgenerator.document.add](../document-generator-document-add.md) as an array of strings in `values`. In `fields`, you need to specify the table provider so that the generator processes the array as repeating rows.

## When to Use

- When you need to fill a single table with rows of the same structure
- Each row of the table contains the same set of values, such as name, price, and image
- When you need to display the row number within the table

## What to Pass in the Request

- The required request parameters are `templateId` and `value`: the template identifier and the external identifier of the object for which the document is created
- In `fields['Table']['PROVIDER']`, specify `Bitrix\DocumentGenerator\DataProvider\ArrayDataProvider` so that the generator processes `values['Table']` as a list of table rows
- In `fields['Table']['OPTIONS']`, specify:
  - `ITEM_NAME` — the internal name of the array element
  - `ITEM_PROVIDER` — `Bitrix\DocumentGenerator\DataProvider\HashDataProvider`
- In `values['Table']`, pass the list of table rows
- For table placeholders, such as `TableItemName` and `TableItemPrice`, provide the data access chain: `Table.Item.Name`, `Table.Item.Price`
- For images in the table, specify `TYPE = IMAGE` in `fields`
- For the row number, you can use `Table.INDEX`

## Example

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"templateId":203,"value":"ORDER_1024","values":{"Table":[{"Name":"Item name 1","Price":"$111.23","Image":"https://myrestapp.example/upload/product-1.png"},{"Name":"Item name 2","Price":"$222.34","Image":"https://myrestapp.example/upload/product-2.png"}],"TableItemName":"Table.Item.Name","TableItemImage":"Table.Item.Image","TableItemPrice":"Table.Item.Price","TableIndex":"Table.INDEX"},"fields":{"Table":{"PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider","OPTIONS":{"ITEM_NAME":"Item","ITEM_PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider"}},"TableItemImage":{"TYPE":"IMAGE"}}}' \
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
        downloadUrl: string
      }
    }

    const response = await $b24.actions.v2.call.make<DocumentAddResult>({
      method: 'documentgenerator.document.add',
      params: {
        templateId: 203,
        value: 'ORDER_1024',
        values: {
          Table: [
            {
              Name: 'Item name 1',
              Price: '$111.23',
              Image: 'https://myrestapp.example/upload/product-1.png',
            },
            {
              Name: 'Item name 2',
              Price: '$222.34',
              Image: 'https://myrestapp.example/upload/product-2.png',
            },
          ],
          TableItemName: 'Table.Item.Name',
          TableItemImage: 'Table.Item.Image',
          TableItemPrice: 'Table.Item.Price',
          TableIndex: 'Table.INDEX',
        },
        fields: {
          Table: {
            PROVIDER: 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
            OPTIONS: {
              ITEM_NAME: 'Item',
              ITEM_PROVIDER: 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
            },
          },
          TableItemImage: { TYPE: 'IMAGE' },
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
                    'Table' => [
                        [
                            'Name' => 'Item name 1',
                            'Price' => '$111.23',
                            'Image' => 'https://myrestapp.example/upload/product-1.png',
                        ],
                        [
                            'Name' => 'Item name 2',
                            'Price' => '$222.34',
                            'Image' => 'https://myrestapp.example/upload/product-2.png',
                        ],
                    ],
                    'TableItemName' => 'Table.Item.Name',
                    'TableItemImage' => 'Table.Item.Image',
                    'TableItemPrice' => 'Table.Item.Price',
                    'TableIndex' => 'Table.INDEX',
                ],
                'fields' => [
                    'Table' => [
                        'PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
                        'OPTIONS' => [
                            'ITEM_NAME' => 'Item',
                            'ITEM_PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
                        ],
                    ],
                    'TableItemImage' => ['TYPE' => 'IMAGE'],
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

## How It Works

In the example, it is assumed that the template contains a table with fields `{TableItemName}`, `{TableItemImage}`, `{TableItemPrice}`.

1. The `Table` field is used as a container for the array of rows. This placeholder may not exist in the template, but it is necessary to pass the array of values for the table.
2. By `ITEM_NAME = Item`, the provider reads each row element as an `Item` object.
3. By `ITEM_PROVIDER = HashDataProvider`, the elements are read as a flat associative array.
4. Fields like `TableItem...` reference values through the chain `Table.Item.<Key>`, where `<Key>` is the key of the internal associative array, such as `Name`, `Price`, or `Image`.

`Table.INDEX` returns the current row number, starting from `1`.

If a regular string is specified as the value of a field, it will be inserted into the table as is in all rows.

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
            "isTransformationError": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068"
        }
    }
}
```

The response does not contain the expanded table rows: it only confirms that the document was created. Check the substitution result in the file itself.

## Verify the Result

1. Download the file by `downloadUrl` from the response
2. Make sure that the table contains as many rows as there are items passed in `values['Table']`
3. Check the row numbering if the template uses a placeholder with `Table.INDEX`

## If the Method Returns an Error

- `Empty required parameter "value"` — the required parameter `value` is not provided
- `Template not found` — no template exists with the specified `templateId`

The table has one row instead of several — `ArrayDataProvider` is not specified in `fields['Table']['PROVIDER']`, so the array was processed as a regular value.

Strings like `Table.Item.Name` are displayed in the cells — the data access chain does not match `ITEM_NAME`, or the field codes do not match the template placeholders.

The complete list of errors is available in the "Error Handling" section on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)