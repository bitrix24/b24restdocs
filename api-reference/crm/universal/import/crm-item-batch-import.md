# Importing a Batch of CRM Records: crm.item.batchImport

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with the "import" access permission for the CRM object

This is a universal method for importing objects into CRM. The differences from adding an object are described in more detail [`here`](./index.md).

The logic for adding entities is similar to the [crm.item.import](crm-item-import.md) method.

{% note warning "Attention!" %}

A maximum of 20 entities can be imported in a single request.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../data-types.md#object_type) or [custom type](../user-defined-object-types/index.md) for which the entity needs to be created.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are provided in the [CRM object types reference](../../data-types.md#object_type). The identifier for the SPA can be obtained using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method. ||
|| **data***
[`array`][1] | An array of field values for the entities. It can be viewed as an array where each element contains a set of `fields`, as described in the [crm.item.import](crm-item-import.md) method. ||
|| **useOriginalUfNames**
[`boolean`][1] | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. How to Import Deals

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":4,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.batchImport
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},{"title":"New deal (specifically for REST method examples)","typeId":"SERVICE","categoryId":4,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be an additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.batchImport
        ```

    - Python

        Example

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.item.batch_import(
                entity_type_id=2,
                data=[
                    {
                        "title": "New deal (specifically for REST method examples)",
                        "typeId": "SERVICE",
                        "categoryId": 9,
                        "stageId": "C9:UC_KN8KFI",
                        "isReccurring": "Y",
                        "probability": 50,
                        "currencyId": "USD",
                        "isManualOpportunity": "Y",
                        "opportunity": 999.99,
                        "taxValue": 99.9,
                        "companyId": 5,
                        "contactId": 4,
                        "contactIds": [4, 5],
                        "quoteId": 7,
                        "begindate": "formatDate(monthAgo)",
                        "closedate": "formatDate(twelveDaysInAdvance)",
                        "opened": "N",
                        "comments": "commentsExample",
                        "assignedById": 6,
                        "sourceId": "WEB",
                        "sourceDescription": "There should be an additional description about the source",
                        "leadId": 102,
                        "additionalInfo": "There should be additional information",
                        "observers": [2, 3],
                        "utmSource": "google",
                        "utmMedium": "CPC",
                        "ufCrm_1721244707107": 1111.1,
                        "parentId1220": 2,
                    },
                    {
                        "title": "New deal (specifically for REST method examples)",
                        "typeId": "SERVICE",
                        "categoryId": 4,
                        "stageId": "C9:UC_KN8KFI",
                        "isReccurring": "Y",
                        "probability": 50,
                        "currencyId": "USD",
                        "isManualOpportunity": "Y",
                        "opportunity": 999.99,
                        "taxValue": 99.9,
                        "companyId": 5,
                        "contactId": 4,
                        "contactIds": [4, 5],
                        "quoteId": 7,
                        "begindate": "formatDate(monthAgo)",
                        "closedate": "formatDate(twelveDaysInAdvance)",
                        "opened": "N",
                        "comments": "commentsExample",
                        "assignedById": 6,
                        "sourceId": "WEB",
                        "sourceDescription": "There should be an additional description about the source",
                        "leadId": 102,
                        "additionalInfo": "There should be additional information",
                        "observers": [2, 3],
                        "utmSource": "google",
                        "utmMedium": "CPC",
                        "ufCrm_1721244707107": 1111.1,
                        "parentId1220": 2,
                    },
                ],
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    - BX24.js

        ```js
        const formatDate = (date) => {
            return date.toISOString().slice(0, 10);
        };

        const day = 60 * 60 * 24 * 1000;

        const now = new Date();
        const twelveDaysInAdvance = new Date(now.getTime() + 12 * day);
        const monthAgo = new Date(now.getTime() - 30 * day);

        const commentsExample = `
        Пример комментария внутри сделки

        [B]Жирный текст[/B]
        [I]Курсив[/I]
        [U]Подчеркнутый[/U]
        [S]Зачеркнутый[/S]
        [B][I][U][S]Микс[/S][/U][/I][/B]

        [LIST]
        [*]Элемент списка #1
        [*]Элемент списка #2
        [*]Элемент списка #3
        [/LIST]

        [LIST=1]
        [*]Нумерованный элемент списка #1
        [*]Нумерованный элемент списка #2
        [*]Нумерованный элемент списка #3
        [/LIST]
        `;
      
        const deal = {
            title: "New deal (specifically for REST method examples)",
            typeId: "SERVICE",
            categoryId: 9,
            stageId: "C9:UC_KN8KFI",
            isReccurring: "Y",
            probability: 50,
            currencyId: "USD",
            isManualOpportunity: "Y",
            opportunity: 999.99,
            taxValue: 99.9,
            companyId: 5,
            contactId: 4,
            contactIds: [4, 5],
            quoteId: 7,
            begindate: formatDate(monthAgo),
            closedate: formatDate(twelveDaysInAdvance),
            opened: "N",
            comments: commentsExample,
            assignedById: 6,
            sourceId: "WEB",
            sourceDescription: "There should be an additional description about the source",
            leadId: 102,
            additionalInfo: "There should be additional information",
            observers: [2, 3],
            utmSource: "google",
            utmMedium: "CPC",
            ufCrm_1721244707107: 1111.1,
            parentId1220: 2,
        };

        const secondDeal = {
            title: "New deal (specifically for REST method examples)",
            typeId: "SERVICE",
            categoryId: 4,
            stageId: "C9:UC_KN8KFI",
            isReccurring: "Y",
            probability: 50,
            currencyId: "USD",
            isManualOpportunity: "Y",
            opportunity: 999.99,
            taxValue: 99.9,
            companyId: 5,
            contactId: 4,
            contactIds: [4, 5],
            quoteId: 7,
            begindate: formatDate(monthAgo),
            closedate: formatDate(twelveDaysInAdvance),
            opened: "N",
            comments: commentsExample,
            assignedById: 6,
            sourceId: "WEB",
            sourceDescription: "There should be an additional description about the source",
            leadId: 102,
            additionalInfo: "There should be additional information",
            observers: [2, 3],
            utmSource: "google",
            utmMedium: "CPC",
            ufCrm_1721244707107: 1111.1,
            parentId1220: 2,
        };

        BX24.callMethod(
            'crm.item.batchImport', 
            {
                entityTypeId: 2,
                data: [
                    deal,
                    secondDeal
                ]
            },
            (result) => 
            {
                result.error() 
                    ? console.error(result.error()) 
                    : console.info(result.data())
                ;
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');
        
        $deal = [
            'title' => "New deal (specifically for REST method examples)",
            'typeId' => "SERVICE",
            'categoryId' => 9,
            'stageId' => "C9:UC_KN8KFI",
            'isReccurring' => "Y",
            'probability' => 50,
            'currencyId' => "USD",
            'isManualOpportunity' => "Y",
            'opportunity' => 999.99,
            'taxValue' => 99.9,
            'companyId' => 5,
            'contactId' => 4,
            'contactIds' => [4, 5],
            'quoteId' => 7,
            'begindate' => formatDate(monthAgo),
            'closedate' => formatDate(twelveDaysInAdvance),
            'opened' => "N",
            'comments' => $commentsExample,
            'assignedById' => 6,
            'sourceId' => "WEB",
            'sourceDescription' => "There should be an additional description about the source",
            'leadId' => 102,
            'additionalInfo' => "There should be additional information",
            'observers' => [2, 3],
            'utmSource' => "google",
            'utmMedium' => "CPC",
            'ufCrm_1721244707107' => 1111.1,
            'parentId1220' => 2
        ]
        
        $secondDeal = [
            'title' => "New deal (specifically for REST method examples)",
            'typeId' => "SERVICE",
            'categoryId' => 4,
            'stageId' => "C9:UC_KN8KFI",
            'isReccurring' => "Y",
            'probability' => 50,
            'currencyId' => "USD",
            'isManualOpportunity' => "Y",
            'opportunity' => 999.99,
            'taxValue' => 99.9,
            'companyId' => 5,
            'contactId' => 4,
            'contactIds' => [4, 5],
            'quoteId' => 7,
            'begindate' => formatDate(monthAgo),
            'closedate' => formatDate(twelveDaysInAdvance),
            'opened' => "N",
            'comments' => $commentsExample,
            'assignedById' => 6,
            'sourceId' => "WEB",
            'sourceDescription' => "There should be an additional description about the source",
            'leadId' => 102,
            'additionalInfo' => "There should be additional information",
            'observers' => [2, 3],
            'utmSource' => "google",
            'utmMedium' => "CPC",
            'ufCrm_1721244707107' => 1111.1,
            'parentId1220' => 2
        ]
        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 2,
                'data' => [
                        $deal,
                        $secondDeal,
                    ],
            ],
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}


2. How to Create an SPA Element with a Set of Custom Fields

    {% cut "Custom fields involved in the example" %}

    {% include [Set of Custom Fields](../../_include/user-fields-for-examples-cut.md) %}

    {% endcut %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "data": [{
                "ufCrm44_1721812760630": "String for a string-type custom field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },{
                "ufCrm44_1721812760630": "String for a string-type custom field",
                "ufCrm44_1721812814433": 45,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel2.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "600|USD",
                "ufCrm44_1721812935209": "N",
                "ufCrm44_1721812948498": 9999.9
            }]
        }' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.batchImport
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "data": [{
                "ufCrm44_1721812760630": "String for a string-type custom field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },{
                "ufCrm44_1721812760630": "String for a string-type custom field",
                "ufCrm44_1721812814433": 45,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel2.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "600|USD",
                "ufCrm44_1721812935209": "N",
                "ufCrm44_1721812948498": 9999.9
            }],
            "auth": "**put_access_token_here**"
        }' \
        https://**put_your_bitrix24_address**/rest/crm.item.batchImport
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type BatchImportResult = {
          items: Array<
            | { item: { id: number } }
            | { error: string; error_description: string }
          >
        }

        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="

        try {
          const response = await $b24.actions.v2.call.make<BatchImportResult>({
            method: 'crm.item.batchImport',
            params: {
              entityTypeId: 1302,
              data: [
                {
                  ufCrm44_1721812760630: "String for custom field of type String",
                  ufCrm44_1721812814433: 81,
                  ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                  ufCrm44_1721812885588: [
                    "example.com",
                    "second-example.com",
                  ],
                  ufCrm44_1721812898903: [
                    "green_pixel.png",
                    greenPixelInBase64,
                  ],
                  ufCrm44_1721812915476: "300|USD",
                  ufCrm44_1721812935209: "Y",
                  ufCrm44_1721812948498: 9999.9,
                },
                {
                  ufCrm44_1721812760630: "String for custom field of type String",
                  ufCrm44_1721812814433: 45,
                  ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                  ufCrm44_1721812885588: [
                    "example.com",
                    "second-example.com",
                  ],
                  ufCrm44_1721812898903: [
                    "green_pixel2.png",
                    greenPixelInBase64,
                  ],
                  ufCrm44_1721812915476: "600|USD",
                  ufCrm44_1721812935209: "N",
                  ufCrm44_1721812948498: 9999.9,
                },
              ],
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Imported items:', result.items)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function batchImportItems() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.batchImport',
                params: {
                  entityTypeId: 1302,
                  data: [
                    {
                      ufCrm44_1721812760630: "String for custom field of type String",
                      ufCrm44_1721812814433: 81,
                      ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                      ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                      ],
                      ufCrm44_1721812898903: [
                        "green_pixel.png",
                        greenPixelInBase64,
                      ],
                      ufCrm44_1721812915476: "300|USD",
                      ufCrm44_1721812935209: "Y",
                      ufCrm44_1721812948498: 9999.9,
                    },
                    {
                      ufCrm44_1721812760630: "String for custom field of type String",
                      ufCrm44_1721812814433: 45,
                      ufCrm44_1721812853419: new Date().toISOString().slice(0, 10),
                      ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                      ],
                      ufCrm44_1721812898903: [
                        "green_pixel2.png",
                        greenPixelInBase64,
                      ],
                      ufCrm44_1721812915476: "600|USD",
                      ufCrm44_1721812935209: "N",
                      ufCrm44_1721812948498: 9999.9,
                    },
                  ],
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Imported items:', result.items)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', batchImportItems)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 1302,
                'data' => [
                    [
                        'ufCrm44_1721812760630' => "String for a string-type custom field",
                        'ufCrm44_1721812814433' => 81,
                        'ufCrm44_1721812853419' => date('Y-m-d'),
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "300|USD",
                        'ufCrm44_1721812935209' => "Y",
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                    [
                        'ufCrm44_1721812760630' => "String for a string-type custom field",
                        'ufCrm44_1721812814433' => 45,
                        'ufCrm44_1721812853419' => date('Y-m-d'),
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel2.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "600|USD",
                        'ufCrm44_1721812935209' => "N",
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}


## Response Handling

The method will return an array `items`, containing objects where each object in this array will either contain the identifier of the created entity in case of success, or an error message object.

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "item": {
                    "id": 15
                }
            },
            {
                "error": "CRM_FIELD_ERROR_REQUIRED",
                "error_description": "The \"Name\" field is required"
            }
        ]
    },
    "time": {
        "start": 1723414961.913589,
        "finish": 1723414964.652124,
        "duration": 2.738534927368164,
        "processing": 2.376383066177368,
        "date_start": "2024-08-11T22:22:41+00:00",
        "date_finish": "2024-08-11T22:22:44+00:00",
        "operating": 2.3762991428375244
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response.

Contains a single key `item` ||
|| **items**
[`array`][1] | An array containing `item` objects or errors ||
|| **item**
[`object`][1] | Information about the created entity.

Contains a single key `id` ||
|| **id**
[`int`][1] | Identifier of the created entity ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP status: **401**, **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                            | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | User does not have permission to add items of type `entityTypeId`                             ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | An incorrect value for the `field` was passed.

For system fields of type `createdTime`, if the request is not from an administrator. ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | One of the multiple fields received a value of type `type`, while an iterable type was expected. This can also occur with an incorrect request (invalid JSON or request headers). ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items                              ||
|| `400`      | `MAX_IMPORT_BATCH_SIZE_EXCEEDED`  | You cannot import more than 20 items                     | Occurs when more than 20 entities are passed during import.                                        ||
|| `401`      | `INVALID_CREDENTIALS`             | Invalid authorization data for the request                            | Incorrect `ID` and/or code in the request path.                                       ||
|| `403`      | `allowed_only_intranet_user`      | This action is allowed only for intranet users                   | User is not an intranet user                                                 ||
|#

{% include [System errors](./../../../../_includes/system-errors.md) %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-import.md)

[1]: ../../data-types.md