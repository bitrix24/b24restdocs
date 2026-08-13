# Generate a Document with Date and Name Modifiers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

Modifiers in document templates are formatting rules that control how field values, such as dates or full names, are displayed.

Input data can be pre-formatted in the application or passed through REST.

## When to Use

- You need to format a date using the document generator.
- You need to display a full name in a specified format.

## What to Include in the Request

Date and name modifiers are applied to the values of the template placeholders through the `values` and `fields` parameters of the [documentgenerator.document.add](../document-generator-document-add.md) method. In addition, the request requires `templateId` — the template identifier, and `value` — the external identifier of the object.

{% note tip "User Documentation" %}

- [Modifiers in document templates](https://helpdesk.bitrix24.com/open/25799327/)

{% endnote %}

### For Date

- In `values`, pass the date in Atom format, for example, `2026-03-18T00:00:00+03:00`.
- In `fields`, specify the field type: `TYPE` = `DATE`.
- If necessary, set the default output format via `FORMAT['format']`:

    - `d` — day of the month with leading zero
    - `j` — day of the month without leading zero
    - `m` — month number with leading zero
    - `n` — month number without leading zero
    - `F` — month name in the form used within a date, for example `March`
    - `f` — month name in its base form, for example `March`
    - `y` — year in two digits
    - `Y` — year in four digits
    - `H` — hours in 24-hour format with leading zero
    - `i` — minutes with leading zero
    - `s` — seconds with leading zero

Date and time formatting symbols can be combined:

- `d.m.y` — `28.03.26`
- `j F Y` — `28 March 2026`
- `H:i:s` — `10:24:18`
- `Y-m-d H:i:s` — `2026-03-28 10:24:18`

The month name is displayed in the language of the region specified in the template.

### For Name

- In `values`, pass the name as an array with parts of the full name:

    ```php
    [
        'NAME' => 'Igor', // first name
        'LAST_NAME' => 'Ivanov', // last name
        'SECOND_NAME' => 'Petrovich', // patronymic
        'GENDER' => 'M', // gender
    ]
    ```

The `GENDER` key can explicitly specify the gender: `M` or `F`. If the gender is not specified, the module will attempt to determine it based on the patronymic. If both `GENDER` and the patronymic are not provided, the gender will not be determined, and declension will not work.

- In `fields`, specify the field type: `TYPE` = `NAME`.
- In `FORMAT['format']`, you can pass the output template:

    - `#TITLE#` — salutation
    - `#NAME#` — first name
    - `#LAST_NAME#` — last name
    - `#SECOND_NAME#` — patronymic
    - `#NAME_SHORT#` — first letter of the first name with a dot
    - `#LAST_NAME_SHORT#` — first letter of the last name with a dot
    - `#SECOND_NAME_SHORT#` — first letter of the patronymic with a dot

## Example

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"templateId":203,"value":"ORDER_1024","values":{"SomeDate":"2026-03-18T00:00:00+03:00","SomeName":{"NAME":"Vladislav","LAST_NAME":"Gorelkin","GENDER":"M"}},"fields":{"SomeDate":{"TYPE":"DATE","FORMAT":{"format":"d.m.Y H:i"}},"SomeName":{"TYPE":"NAME","FORMAT":{"format":"#NAME# #LAST_NAME#"}}}}' \
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
          // the date is passed in the atom format
          SomeDate: '2026-03-18T00:00:00+03:00',
          // the name is passed as an object with name parts
          SomeName: {
            NAME: 'Vladislav',
            LAST_NAME: 'Gorelkin',
            GENDER: 'M',
          },
        },
        fields: {
          SomeDate: {
            TYPE: 'DATE',
            FORMAT: {
              format: 'd.m.Y H:i',
            },
          },
          SomeName: {
            TYPE: 'NAME',
            FORMAT: {
              format: '#NAME# #LAST_NAME#',
            },
          },
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
                    // the value is passed in the atom format
                    'SomeDate' => '2026-03-18T00:00:00+03:00',
                    // the name is passed as an array
                    'SomeName' => [
                        'NAME' => 'Vladislav',
                        'LAST_NAME' => 'Gorelkin',
                        'GENDER' => 'M',
                    ],
                ],
                'fields' => [
                    // field type - date
                    'SomeDate' => [
                        'TYPE' => 'DATE',
                        'FORMAT' => [
                            'format' => 'd.m.Y H:i',
                        ],
                    ],
                    // field type - name
                    'SomeName' => [
                        'TYPE' => 'NAME',
                        'FORMAT' => [
                            'format' => '#NAME# #LAST_NAME#',
                        ],
                    ],
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

The method returns the data of the created document. The `values` field returns the original values you passed, while the formatted values are inserted into the document file. The response example is abbreviated; a complete description of the fields is available on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

```json
{
    "result": {
        "document": {
            "id": 51,
            "title": "ORDER Template 51",
            "templateId": "203",
            "value": "ORDER_1024",
            "values": {
                "SomeDate": "2026-03-18T00:00:00+03:00",
                "_creationMethod": "rest"
            },
            "isTransformationError": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068"
        }
    }
}
```

## Verify the Result

1. Download the file by `downloadUrl` from the response
2. Check that the date is displayed in the format from `FORMAT['format']`, and the name follows the output template
3. If the format was not applied, compare the field codes in `values` and `fields`: they must match each other and the template placeholders

## If the Method Returns an Error

- `Empty required parameter "value"` — the required parameter `value` is not provided
- `Template not found` — no template exists with the specified `templateId`

The date is displayed as is — check that it is passed in `values` in Atom format and that `TYPE` = `DATE` is specified for this field in `fields`.

The name is not declined — pass `GENDER` explicitly or add the patronymic in `SECOND_NAME`.

The complete list of errors is available in the "Error Handling" section on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)