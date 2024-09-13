# Import a Batch of CRM Records - crm.item.batchImport

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "import" access permission for the CRM object

A universal method for importing objects into CRM. The differences from adding an object are described in more detail [`here`](./index.md).

The logic for adding elements works similarly to the [crm.item.import](crm-item-import.md) method.

{% note warning "Attention!" %}

A maximum of 20 elements can be imported in a single request.

{% endnote %}

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the system or [user-defined type](../user-defined-object-types/index.md) for which the element needs to be created ||
|| **data***
[`array`][1] | An array of field values for the elements. It can be viewed as an array where each element contains a set of `fields`, as described in the [crm.item.import](crm-item-import.md) method ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

1. How to import deals

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]},{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":4,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]}]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.batchImport
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]},{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":4,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":[1,2]}],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.batchImport
        ```

    - JS

        ```js
        const formatDate = (date) => {
            return date.toISOString().slice(0, 10);
        };

        const day = 60 * 60 * 24 * 1000;

        const now = new Date();
        const twelveDaysInAdvance = new Date(now.getTime() + 12 * day);
        const monthAgo = new Date(now.getTime() - 30 * day);

        const commentsExample = `
        Example comment within the deal

        [B]Bold text[/B]
        [I]Italic[/I]
        [U]Underlined[/U]
        [S]Strikethrough[/S]
        [B][I][U][S]Mix[/S][/U][/I][/B]

        [LIST]
        [*]List item #1
        [*]List item #2
        [*]List item #3
        [/LIST]

        [LIST=1]
        [*]Numbered list item #1
        [*]Numbered list item #2
        [*]Numbered list item #3
        [/LIST]
        `;
      
        const deal = {
            title: "New Deal (specifically for REST method example)",
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
            sourceDescription: "There should be additional description about the source",
            leadId: 102,
            additionalInfo: "There should be additional information",
            observers: [2, 3],
            utmSource: "google",
            utmMedium: "CPC",
            ufCrm_1721244707107: 1111.1,
            parentId1220: [
                1,
                2,
            ],
        };

        const secondDeal = {
            title: "New Deal (specifically for REST method example)",
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
            sourceDescription: "There should be additional description about the source",
            leadId: 102,
            additionalInfo: "There should be additional information",
            observers: [2, 3],
            utmSource: "google",
            utmMedium: "CPC",
            ufCrm_1721244707107: 1111.1,
            parentId1220: [
                1,
                2,
            ],
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

    - PHP

        ```php
        require_once('crest.php');
        
        $deal = [
            'title' => "New Deal (specifically for REST method example)",
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
            'sourceDescription' => "There should be additional description about the source",
            'leadId' => 102,
            'additionalInfo' => "There should be additional information",
            'observers' => [2, 3],
            'utmSource' => "google",
            'utmMedium' => "CPC",
            'ufCrm_1721244707107' => 1111.1,
            'parentId1220' => [
                1,
                2,
            ]
        ];
        
        $secondDeal = [
            'title' => "New Deal (specifically for REST method example)",
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
            'sourceDescription' => "There should be additional description about the source",
            'leadId' => 102,
            'additionalInfo' => "There should be additional information",
            'observers' => [2, 3],
            'utmSource' => "google",
            'utmMedium' => "CPC",
            'ufCrm_1721244707107' => 1111.1,
            'parentId1220' => [
                1,
                2,
            ]
        ];
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


2. How to create an SPA element with a set of custom fields

    {% cut "Custom fields used in the example" %}

    {% include [Set of custom fields](../../_include/user-fields-for-examples-cut.md) %}

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
                "ufCrm44_1721812760630": "String for custom field of type String",
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
                "ufCrm44_1721812760630": "String for custom field of type String",
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
                "ufCrm44_1721812760630": "String for custom field of type String",
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
                "ufCrm44_1721812760630": "String for custom field of type String",
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

    - JS

        ```js
        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==";

        BX24.callMethod(
            'crm.item.batchImport', 
            {
                entityTypeId: 1302,
                data: [
                    {
                        ufCrm44_1721812760630: "String for custom field of type String",
                        ufCrm44_1721812814433: 81,
                        ufCrm44_1721812853419: (new Date()).toISOString().slice(0, 10),
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
                        ufCrm44_1721812853419: (new Date()).toISOString().slice(0, 10),
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
                    }
                ],
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

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 1302,
                'data' => [
                    [
                        'ufCrm44_1721812760630' => "String for custom field of type String",
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
                        'ufCrm44_1721812760630' => "String for custom field of type String",
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

The method will return an array `items`, containing objects where each object in this array will contain the identifier of the created element in case of success, or an error message object.

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
                "error_description": "The field \"Title\" is required."
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
[`object`][1] | Information about the created element.

Contains a single key `id` ||
|| **id**
[`int`][1] | Identifier of the created element ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#


## Error Handling

HTTP status: **401**, **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "SPA not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `400`      | `NOT_FOUND`                       | SPA not found.                                                      | Occurs when an invalid `entityTypeId` is passed.                                            ||
|| `400`      | `ACCESS_DENIED`                   | Access denied.                                                      | The user does not have permission to add elements of type `entityTypeId`.                   ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for the field "`field`".                              | An incorrect value for the field `field` was passed.

For system fields of type `createdTime`, if the request is not from an administrator. ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead. | One of the multiple fields received a value of type `type`, while an iterable type was expected. This can also occur with an incorrect request (incorrect JSON or request headers). ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new element due to your plan's restrictions. | The plan's restrictions do not allow creating SPA elements.                                 ||
|| `400`      | `MAX_IMPORT_BATCH_SIZE_EXCEEDED`  | You cannot import more than 20 elements.                           | Occurs when more than 20 elements are passed during import.                                 ||
|| `401`      | `INVALID_CREDENTIALS`             | Invalid authorization data for the request.                         | Incorrect `user ID` and/or code in the request path.                                       ||
|| `403`      | `allowed_only_intranet_user`      | Action allowed only for intranet users.                             | The user is not an intranet user.                                                          ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-item-import.md)

[1]: ../../data-types.md