# Update Lead crm.lead.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to edit CRM leads.

The method `crm.lead.update` updates an existing lead.

{% note warning %}

It is strongly recommended to pass the full set of address fields when updating the address. The specifics of updating address fields are described [here](../data-types.md).

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Integer identifier of the lead. The method to obtain it is described below. ||
|| **fields**^*^
[`object`](../../data-types.md) | A set of fields in the format `["updated field" => "value"[, ...]]`. The method to obtain the list of possible fields is described below. ||
|| **options**
[`object`](../../data-types.md) | An optional set of options. (`"optionName"=>"value"[, ...]`). The list of possible fields is described below. ||
|#

## Parameter id

To find out the full list of available identifiers, execute the method [crm.lead.list](./crm-lead-list.md).

## Parameter fields

#|
|| **Field** / **Type** | **Description** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Contact address. ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is customary to split the address into 2 parts. ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City. ||
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country. ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code. ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code. ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province. ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region. ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person. ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Date of birth. ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments. ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link of the lead to the company. ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | Company name specified in the corresponding lead field. To link an existing company, pass its id in the COMPANY_ID field. ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link of the lead to the contact. ||
|| **CONTACT_IDS**
[`crm_contact`](../../data-types.md) | Identifier of the linked contact. Multiple. ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier. ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address. Multiple. ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Form of address. ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messenger. Multiple. ||
|| **LINK**
[`crm_multifield`](../../data-types.md) | User ID linked through an open channel. Multiple. ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name. ||
|| **NAME**
[`string`](../../data-types.md) | First name. ||
|| **OPENED**
[`char`](../../data-types.md) | Indicates the availability of the lead to everyone. Allowed values are Y or N. ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount. ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates manual mode for calculating the amount. Allowed values are Y or N. ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source. ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone number. Multiple. ||
|| **POST**
[`string`](../../data-types.md) | Position. ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name. ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source. ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Identifier of the source.
Default values:

#|
||SOURCE_ID|Name||
||CALL|Call||
||EMAIL|E-mail||
||WEB|Website||
||ADVERTISING|Advertising||
||PARTNER|Existing client||
||RECOMMENDATION|By recommendation||
||TRADE_SHOW|Trade show||
||WEBFORM|CRM form||
||CALLBACK|Callback||
||RC_GENERATOR|Sales generator||
||STORE|Online store||
||OTHER|Other||
|#

The list of all possible identifiers from the directory can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=SOURCE`. ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage. ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Default stages:

#|
||STATUS_ID|Name||
||NEW | New||
||IN_PROCESS | In process||
||PROCESSED | Processed||
||JUNK | Junk lead||
||CONVERTED | Qualified lead||
|#

The list of all possible stages from the directory can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=STATUS`. ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the lead. ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Identifier of the advertising campaign. ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners). ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google AdWords and others. ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising. ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Website. Multiple. ||
|| **ufCrm_ххх** | Custom fields. See section [{#T}](../universal/user-defined-fields/index.md) ||
|#

{% note info %}

Also, to find out the required format of the fields, you can execute the method [crm.lead.fields](crm-lead-fields.md) and check the format of the received values for these fields.

{% endnote %}

{% note info %}

When adding a lead, you cannot explicitly set the repeat lead indicator (the `IS_RETURN_CUSTOMER` field), however, this field automatically takes the value Y if you specify a value for `COMPANY_ID` or `CONTACT_ID` when adding the lead.

{% endnote %}

## Parameter options

#|
|| **Parameter** / **Type** | **Description** ||
|| **REGISTER_SONET_EVENT**
[`char`](../../data-types.md) | Register the event of adding a lead in the live feed. A notification will also be sent to the responsible person for the lead. Allowed values are Y or N. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1608,"fields":{"TITLE":"John Smith","PHONE":{0:{"VALUE":"555888","VALUE_TYPE":"WORK"}}} ,"auth":"**put_access_token_here**"}' \
    https://xxx.bitrix24.com/rest/crm.lead.update
    ```

- JS

    ```javascript
    BX24.callMethod(
        "crm.lead.update",
        {
            id: 1608,
            fields:
            {
                TITLE: "John Smith",
                NAME: "Gleb",
                SECOND_NAME: "Yegorovich",
                LAST_NAME: "Smith",
                STATUS_ID: "NEW",
                OPENED: "Y",
                ASSIGNED_BY_ID: 1,
                CURRENCY_ID: "USD",
                OPPORTUNITY: 12500,
                PHONE: [
                    {
                        VALUE: "555888",
                        VALUE_TYPE: "WORK",
                    },
                ],
                WEB: [
                    {
                        VALUE: "www.mysite.com",
                        VALUE_TYPE: "WORK",
                    }
                ],
            },
            options: {
                REGISTER_SONET_EVENT: "Y",
            }
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());return;
            }
    
            console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    $fields = [
        'TITLE' => $sTitle,
        'COMPANY_ID' => 123,
        'PHONE' => [
            [
                'VALUE' => '555888',
                'VALUE_TYPE' => 'WORK',
            ],
        ],
    ];
    
    $result = CRest::call(
        'crm.lead.update',
        [
            'id' => 1608,
            'fields' => $fields,
        ],
        [
            'REGISTER_SONET_EVENT' => 'Y',
        ]     
    );
    ```

- HTTPS

    ```http
    https://xxx.bitrix24.com/rest/1/5***/crm.lead.update.json?id=1734&fields[NAME]=Vasily&fields[SECOND_NAME]=Petrovich&fields[LAST_NAME]=Cosmonaut&fields[PHONE][0][VALUE]=89994445556&fields[PHONE][0][VALUE_TYPE]=WORK&fields[EMAIL][0][VALUE]=test@example.com&fields[EMAIL][0][VALUE_TYPE]=WORK
    ```

- B24-PHP-SDK

    ```php        
    try {
        $id = 123; // Example lead ID
        $fields = [
            'TITLE' => 'Updated Lead Title',
            'NAME' => 'John',
            'LAST_NAME' => 'Doe',
            'BIRTHDATE' => (new DateTime('1980-01-01'))->format(DateTime::ATOM),
            'COMPANY_TITLE' => 'Example Company',
            'STATUS_ID' => 'NEW',
            'COMMENTS' => 'Updated comments for the lead.',
            'PHONE' => '1234567890',
            'EMAIL' => 'john.doe@example.com',
        ];
        $params = [
            'REGISTER_SONET_EVENT' => 'Y',
        ];
        $result = $serviceBuilder->getCRMScope()->lead()->update($id, $fields, $params);
        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## See also

- [How to update a field of type "resource booking"](../../calendar/calendar-resource-booking-list.md)

## Response on success

> 200 OK

```json
{
    "result": true,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+01:00",
        "date_finish": "2024-01-20T18:35:37+01:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
`boolean`| Result of the request. ||
|| **time**
[`array`](../../data-types.md) | Information about the execution time of the request. ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the moment the request was initialized. ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the moment the request was completed. ||
|| **duration**
[`double`](../../data-types.md) | How long in milliseconds the request took (finish - start). ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the moment the request was initialized. ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the moment the request was completed. ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md). ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds will the limit on REST API resources be reset? Read more in the article [operation limits](../../../limits.md). ||
|#

## Response on error

> HTTP status: 40x, 50x Error

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

### Possible errors

#|  
|| **Error text** | **Description** ||
|| ID is not defined or invalid. | The unique identifier of the lead is not specified or is not a positive number. ||
|| Access denied. | The user does not have permission to edit the lead. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}