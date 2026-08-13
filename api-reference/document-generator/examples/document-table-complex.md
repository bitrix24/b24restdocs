# Generate a Document with Complex Tables

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

In the REST document generator, you can fill template placeholders with nested lists, where one of the first-level list values contains another list. The method used to create the document is [documentgenerator.document.add](../document-generator-document-add.md).

## When to Use

- When you need to insert multiple tables of the same structure but with a different number of rows
- When you need to build a table within a repeating block

In such scenarios, the outer list defines the structure, while the inner lists are passed as separate fields and linked through chains like `Events.Event.Title` and `Event1Speakers.Speaker.Name`.

## What to Pass in the Request

- The required request parameters are `templateId` and `value`: the template identifier and the external identifier of the object for which the document is created
- In `values`, pass the outer list, for example, `Events`
- In the elements of the outer list, pass placeholders for the related inner lists, for example, `{Event1SpeakersSpeakerName}`
- For the fields of the outer list, pass access chains like `Events.Event.Title`
- For each inner list, pass a separate array in `values`, for example, `Event1Speakers`
- For the fields of the inner lists, pass access chains like `Event1Speakers.Speaker.Name`
- In `fields`, describe the outer and inner lists through `ArrayDataProvider`
- In the `OPTIONS` of each list, specify `ITEM_NAME` and `ITEM_PROVIDER` = `Bitrix\DocumentGenerator\DataProvider\HashDataProvider`

## Points to Note

- In `fields`, the outer list should come before the related inner lists so that the providers are resolved in the correct order
- The codes of the inner lists must match the placeholders passed in the outer list

## Example

The example creates a document with a block of events, and each event contains its own table of speakers.

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"templateId":203,"value":"EVENTS_2026","values":{"Title":"Welcome to my template","Description":"<b>Description is here</b>","Events":[{"Title":"Automation","Description":"Some description of the automation event","SpeakerName":"{Event1SpeakersSpeakerName}","SpeakerCompany":"{Event1SpeakersSpeakerCompany}","SpeakerPosition":"{Event1SpeakersSpeakerPosition}"},{"Title":"Documents","Description":"This event is about document processing","SpeakerName":"{Event2SpeakersSpeakerName}","SpeakerCompany":"{Event2SpeakersSpeakerCompany}","SpeakerPosition":"{Event2SpeakersSpeakerPosition}"}],"EventsEventTitle":"Events.Event.Title","EventsEventDescription":"Events.Event.Description","EventsEventSpeakerName":"Events.Event.SpeakerName","EventsEventSpeakerCompany":"Events.Event.SpeakerCompany","EventsEventSpeakerPosition":"Events.Event.SpeakerPosition","Event1Speakers":[{"Name":"John Smith","Company":"Cool Ltd.","Position":"Core developer"},{"Name":"Michael Johnson","Company":"Cool Ltd.","Position":"Product Manager"}],"Event1SpeakersSpeakerName":"Event1Speakers.Speaker.Name","Event1SpeakersSpeakerCompany":"Event1Speakers.Speaker.Company","Event1SpeakersSpeakerPosition":"Event1Speakers.Speaker.Position","Event2Speakers":[{"Name":"David Brown","Company":"Devils corp.","Position":"Chief"}],"Event2SpeakersSpeakerName":"Event2Speakers.Speaker.Name","Event2SpeakersSpeakerCompany":"Event2Speakers.Speaker.Company","Event2SpeakersSpeakerPosition":"Event2Speakers.Speaker.Position"},"fields":{"Events":{"PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider","OPTIONS":{"ITEM_NAME":"Event","ITEM_PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider"}},"Event1Speakers":{"PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider","OPTIONS":{"ITEM_NAME":"Speaker","ITEM_PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider"}},"Event2Speakers":{"PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider","OPTIONS":{"ITEM_NAME":"Speaker","ITEM_PROVIDER":"Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider"}}}}' \
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

    const arrayProvider = 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider'
    const hashProvider = 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider'

    const response = await $b24.actions.v2.call.make<DocumentAddResult>({
      method: 'documentgenerator.document.add',
      params: {
        templateId: 203,
        value: 'EVENTS_2026',
        values: {
          Title: 'Welcome to my template',
          Description: '<b>Description is here</b>',
          // the outer list: each item carries placeholders of its own speakers table
          Events: [
            {
              Title: 'Automation',
              Description: 'Some description of the automation event',
              SpeakerName: '{Event1SpeakersSpeakerName}',
              SpeakerCompany: '{Event1SpeakersSpeakerCompany}',
              SpeakerPosition: '{Event1SpeakersSpeakerPosition}',
            },
            {
              Title: 'Documents',
              Description: 'This event is about document processing',
              SpeakerName: '{Event2SpeakersSpeakerName}',
              SpeakerCompany: '{Event2SpeakersSpeakerCompany}',
              SpeakerPosition: '{Event2SpeakersSpeakerPosition}',
            },
          ],
          EventsEventTitle: 'Events.Event.Title',
          EventsEventDescription: 'Events.Event.Description',
          EventsEventSpeakerName: 'Events.Event.SpeakerName',
          EventsEventSpeakerCompany: 'Events.Event.SpeakerCompany',
          EventsEventSpeakerPosition: 'Events.Event.SpeakerPosition',
          // the inner lists are passed as separate fields
          Event1Speakers: [
            { Name: 'John Smith', Company: 'Cool Ltd.', Position: 'Core developer' },
            { Name: 'Michael Johnson', Company: 'Cool Ltd.', Position: 'Product Manager' },
          ],
          Event1SpeakersSpeakerName: 'Event1Speakers.Speaker.Name',
          Event1SpeakersSpeakerCompany: 'Event1Speakers.Speaker.Company',
          Event1SpeakersSpeakerPosition: 'Event1Speakers.Speaker.Position',
          Event2Speakers: [
            { Name: 'David Brown', Company: 'Devils corp.', Position: 'Chief' },
          ],
          Event2SpeakersSpeakerName: 'Event2Speakers.Speaker.Name',
          Event2SpeakersSpeakerCompany: 'Event2Speakers.Speaker.Company',
          Event2SpeakersSpeakerPosition: 'Event2Speakers.Speaker.Position',
        },
        fields: {
          Events: {
            PROVIDER: arrayProvider,
            OPTIONS: { ITEM_NAME: 'Event', ITEM_PROVIDER: hashProvider },
          },
          Event1Speakers: {
            PROVIDER: arrayProvider,
            OPTIONS: { ITEM_NAME: 'Speaker', ITEM_PROVIDER: hashProvider },
          },
          Event2Speakers: {
            PROVIDER: arrayProvider,
            OPTIONS: { ITEM_NAME: 'Speaker', ITEM_PROVIDER: hashProvider },
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
        $arrayProvider = 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider';
        $hashProvider = 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider';

        $response = $b24Service->core->call(
            'documentgenerator.document.add',
            [
                'templateId' => 203,
                'value' => 'EVENTS_2026',
                'values' => [
                    'Title' => 'Welcome to my template',
                    'Description' => '<b>Description is here</b>',
                    // the outer list: each item carries placeholders of its own speakers table
                    'Events' => [
                        [
                            'Title' => 'Automation',
                            'Description' => 'Some description of the automation event',
                            'SpeakerName' => '{Event1SpeakersSpeakerName}',
                            'SpeakerCompany' => '{Event1SpeakersSpeakerCompany}',
                            'SpeakerPosition' => '{Event1SpeakersSpeakerPosition}',
                        ],
                        [
                            'Title' => 'Documents',
                            'Description' => 'This event is about document processing',
                            'SpeakerName' => '{Event2SpeakersSpeakerName}',
                            'SpeakerCompany' => '{Event2SpeakersSpeakerCompany}',
                            'SpeakerPosition' => '{Event2SpeakersSpeakerPosition}',
                        ],
                    ],
                    'EventsEventTitle' => 'Events.Event.Title',
                    'EventsEventDescription' => 'Events.Event.Description',
                    'EventsEventSpeakerName' => 'Events.Event.SpeakerName',
                    'EventsEventSpeakerCompany' => 'Events.Event.SpeakerCompany',
                    'EventsEventSpeakerPosition' => 'Events.Event.SpeakerPosition',
                    // the inner lists are passed as separate fields
                    'Event1Speakers' => [
                        [
                            'Name' => 'John Smith',
                            'Company' => 'Cool Ltd.',
                            'Position' => 'Core developer',
                        ],
                        [
                            'Name' => 'Michael Johnson',
                            'Company' => 'Cool Ltd.',
                            'Position' => 'Product Manager',
                        ],
                    ],
                    'Event1SpeakersSpeakerName' => 'Event1Speakers.Speaker.Name',
                    'Event1SpeakersSpeakerCompany' => 'Event1Speakers.Speaker.Company',
                    'Event1SpeakersSpeakerPosition' => 'Event1Speakers.Speaker.Position',
                    'Event2Speakers' => [
                        [
                            'Name' => 'David Brown',
                            'Company' => 'Devils corp.',
                            'Position' => 'Chief',
                        ],
                    ],
                    'Event2SpeakersSpeakerName' => 'Event2Speakers.Speaker.Name',
                    'Event2SpeakersSpeakerCompany' => 'Event2Speakers.Speaker.Company',
                    'Event2SpeakersSpeakerPosition' => 'Event2Speakers.Speaker.Position',
                ],
                'fields' => [
                    'Events' => [
                        'PROVIDER' => $arrayProvider,
                        'OPTIONS' => [
                            'ITEM_NAME' => 'Event',
                            'ITEM_PROVIDER' => $hashProvider,
                        ],
                    ],
                    'Event1Speakers' => [
                        'PROVIDER' => $arrayProvider,
                        'OPTIONS' => [
                            'ITEM_NAME' => 'Speaker',
                            'ITEM_PROVIDER' => $hashProvider,
                        ],
                    ],
                    'Event2Speakers' => [
                        'PROVIDER' => $arrayProvider,
                        'OPTIONS' => [
                            'ITEM_NAME' => 'Speaker',
                            'ITEM_PROVIDER' => $hashProvider,
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

The method returns the data of the created document. The response example is abbreviated; a complete description of the fields is available on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

```json
{
    "result": {
        "document": {
            "id": 51,
            "title": "EVENTS Template 51",
            "templateId": "203",
            "value": "EVENTS_2026",
            "isTransformationError": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=documentgenerator.api.document.getfile&SITE_ID=s1&id=51&ts=1773844068"
        }
    }
}
```

The response only confirms that the document was created. Expanded nested lists are not returned in it, so check the substitution result in the file itself.

## Verify the Result

1. Download the file by `downloadUrl` from the response
2. Make sure that the repeating block is built according to the number of items in the outer list `Events`
3. Check that inside each block the speakers table contains the rows of its own inner list, not a shared set

## If the Method Returns an Error

- `Empty required parameter "value"` — the required parameter `value` is not provided
- `Template not found` — no template exists with the specified `templateId`

The same list of speakers is displayed in all blocks — the codes of the inner lists do not match the placeholders passed in the items of the outer list.

The inner tables remain empty — in `fields`, the inner list is described before the outer one, or `ArrayDataProvider` is not specified for it.

The complete list of errors is available in the "Error Handling" section on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method.

## Continue Your Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)