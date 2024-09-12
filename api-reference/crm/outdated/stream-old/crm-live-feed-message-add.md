# Send a Message to the CRM Feed crm.livefeedmessage.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a message to the CRM feed.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_TITLE**
[`string`](../../../data-types.md) | Message title ||
|| **MESSAGE**
[`text`](../../../data-types.md) | Message text. ||
|| **SPERM** | Permissions to view the message, example:
```
"SPERM": {
    "CRMCONTACT": ["CRMCONTACT3", "CRMCONTACT7"], // CRM contacts
    "CRMCOMPANY": ["CRMCOMPANY1", "CRMCOMPANY3"], // CRM companies
    "CRMDEAL": ["CRMDEAL3", "CRMDEAL5"], // CRM deals
    "CRMLEAD": ["CRMLEAD9", "CRMLEAD11"], // CRM leads
    "SG": ["SG5", "SG9"], // social network working groups
    "U": ["U1", "U3"], // users
    "DR": ["DR1", "DR7"], // departments with subdivisions
}
``` 
||
|| **ENTITYTYPEID** 
[`integer`](../../../data-types.md)| Type of the entity in which the message is published:
- 1 - lead;
- 2 - deal;
- 3 - contact;
- 4 - company ||
|| **ENTITYID** 
[`integer`](../../../data-types.md)| ID of the specific lead/deal/contact/company in which the message is published. ||
|| **FILES**
[`file`](../../../data-types.md) | Message files ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example 1

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"POST_TITLE":"A Bit About the Service","MESSAGE":"Bitrix24 is built on the Bitrix Framework.","SPERM":{"CRMCONTACT":["CRMCONTACT3","CRMCONTACT7"],"CRMCOMPANY":["CRMCOMPANY1","CRMCOMPANY3"],"CRMDEAL":["CRMDEAL3","CRMDEAL5"],"CRMLEAD":["CRMLEAD9","CRMLEAD11"],"SG":["SG5","SG9"],"U":["U1","U3"],"DR":["DR1","DR7"]},"ENTITYTYPEID":3,"ENTITYID":3}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.livefeedmessage.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"POST_TITLE":"A Bit About the Service","MESSAGE":"Bitrix24 is built on the Bitrix Framework.","SPERM":{"CRMCONTACT":["CRMCONTACT3","CRMCONTACT7"],"CRMCOMPANY":["CRMCOMPANY1","CRMCOMPANY3"],"CRMDEAL":["CRMDEAL3","CRMDEAL5"],"CRMLEAD":["CRMLEAD9","CRMLEAD11"],"SG":["SG5","SG9"],"U":["U1","U3"],"DR":["DR1","DR7"]},"ENTITYTYPEID":3,"ENTITYID":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.livefeedmessage.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.livefeedmessage.add",
        {
            fields:
            {
                "POST_TITLE": "A Bit About the Service",
                "MESSAGE": "Bitrix24 is built on the Bitrix Framework.",
                "SPERM": {
                    "CRMCONTACT": ["CRMCONTACT3", "CRMCONTACT7"],
                    "CRMCOMPANY": ["CRMCOMPANY1", "CRMCOMPANY3"],
                    "CRMDEAL": ["CRMDEAL3", "CRMDEAL5"],
                    "CRMLEAD": ["CRMLEAD9", "CRMLEAD11"],
                    "SG": ["SG5", "SG9"],
                    "U": ["U1", "U3"],
                    "DR": ["DR1", "DR7"],
                },
                "ENTITYTYPEID": 3,
                "ENTITYID": 3,
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Message created with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.livefeedmessage.add',
        [
            'fields' => [
                'POST_TITLE' => 'A Bit About the Service',
                'MESSAGE' => 'Bitrix24 is built on the Bitrix Framework.',
                'SPERM' => [
                    'CRMCONTACT' => ['CRMCONTACT3', 'CRMCONTACT7'],
                    'CRMCOMPANY' => ['CRMCOMPANY1', 'CRMCOMPANY3'],
                    'CRMDEAL' => ['CRMDEAL3', 'CRMDEAL5'],
                    'CRMLEAD' => ['CRMLEAD9', 'CRMLEAD11'],
                    'SG' => ['SG5', 'SG9'],
                    'U' => ['U1', 'U3'],
                    'DR' => ['DR1', 'DR7'],
                ],
                'ENTITYTYPEID' => 3,
                'ENTITYID' => 3,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Example 2

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"POST_TITLE":"POST_TITLE","MESSAGE":"MESSAGE","SPERM":{"CRMLEAD":["CRMLEAD9","CRMLEAD11"],"U":["U1"]},"ENTITYTYPEID":1,"ENTITYID":56374,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","..."]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.livefeedmessage.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"POST_TITLE":"POST_TITLE","MESSAGE":"MESSAGE","SPERM":{"CRMLEAD":["CRMLEAD9","CRMLEAD11"],"U":["U1"]},"ENTITYTYPEID":1,"ENTITYID":56374,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","..."]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.livefeedmessage.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.livefeedmessage.add",
        {
            fields:
            {
                "POST_TITLE": "POST_TITLE",
                "MESSAGE": "MESSAGE",
                "SPERM": {
                    "CRMLEAD": ["CRMLEAD9", "CRMLEAD11"],
                    "U": ["U1"],
                },
                "ENTITYTYPEID": 1,
                "ENTITYID": 56374,
                "FILES": [
                    ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                    ["2.gif", "..."]
                ],
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Message created with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.livefeedmessage.add',
        [
            'fields' => [
                'POST_TITLE' => 'POST_TITLE',
                'MESSAGE' => 'MESSAGE',
                'SPERM' => [
                    'CRMLEAD' => ['CRMLEAD9', 'CRMLEAD11'],
                    'U' => ['U1'],
                ],
                'ENTITYTYPEID' => 1,
                'ENTITYID' => 56374,
                'FILES' => [
                    ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
                    ['2.gif', '...']
                ],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Additional Information

- [`crm.timeline.comment.add`](../../timeline/comments/crm-timeline-comment-add.md)