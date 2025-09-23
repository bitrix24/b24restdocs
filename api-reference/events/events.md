# Get a list of available events

> Who can execute the method: any user

The `events` method returns a comprehensive list of available events.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SCOPE**
[`string`](../data-types.md) | The method will return events belonging to the specified permission ||
|| **FULL**
[`boolean`](../data-types.md) | The method will return the complete list of events. This parameter will be ignored if the `SCOPE` parameter is provided ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    Example #1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "SCOPE": "user",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/events
    ```
    
    Example #2
    
    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "FULL": true,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/events
    ```

- JS

    Example #1

    ```js
    BX24.callMethod(
        "events",
        {
            "SCOPE": "user"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```
    
    Example #2
    
    ```js
    BX24.callMethod(
        "events",
        {
            "FULL": true
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    Example #1
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'events',
        [
            'SCOPE' => 'user'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    Example #2
    
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'events',
        [
            'FULL' => true
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":[
        "ONAPPUNINSTALL",
        "ONAPPINSTALL",
        "ONAPPUPDATE",
        "ONAPPPAYMENT",
        "ONAPPTEST",
        "ONAPPMETHODCONFIRM",
        "ONOFFLINEEVENT",
        "ONUSERADD",
        "ONCRMINVOICEADD",
        "ONCRMINVOICEUPDATE",
        "ONCRMINVOICEDELETE",
        "ONCRMINVOICESETSTATUS",
        "ONCRMLEADADD",
        "ONCRMLEADUPDATE",
        "ONCRMLEADDELETE",
        "ONCRMLEADUSERFIELDADD",
        "ONCRMLEADUSERFIELDUPDATE",
        "ONCRMLEADUSERFIELDDELETE",
        "ONCRMLEADUSERFIELDSETENUMVALUES",
        "ONCRMDEALADD",
        "ONCRMDEALUPDATE",
        "ONCRMDEALDELETE",
        "ONCRMDEALMOVETOCATEGORY",
        "ONCRMDEALUSERFIELDADD",
        "ONCRMDEALUSERFIELDUPDATE",
        "ONCRMDEALUSERFIELDDELETE",
        "ONCRMDEALUSERFIELDSETENUMVALUES",
        "ONCRMCOMPANYADD",
        "ONCRMCOMPANYUPDATE",
        "ONCRMCOMPANYDELETE",
        "ONCRMCOMPANYUSERFIELDADD",
        "ONCRMCOMPANYUSERFIELDUPDATE",
        "ONCRMCOMPANYUSERFIELDDELETE",
        "ONCRMCOMPANYUSERFIELDSETENUMVALUES",
        "ONCRMCONTACTADD",
        "ONCRMCONTACTUPDATE",
        "ONCRMCONTACTDELETE",
        "ONCRMCONTACTUSERFIELDADD",
        "ONCRMCONTACTUSERFIELDUPDATE",
        "ONCRMCONTACTUSERFIELDDELETE",
        "ONCRMCONTACTUSERFIELDSETENUMVALUES",
        "ONCRMQUOTEADD",
        "ONCRMQUOTEUPDATE",
        "ONCRMQUOTEDELETE",
        "ONCRMQUOTEUSERFIELDADD",
        "ONCRMQUOTEUSERFIELDUPDATE",
        "ONCRMQUOTEUSERFIELDDELETE",
        "ONCRMQUOTEUSERFIELDSETENUMVALUES",
        "ONCRMINVOICEUSERFIELDADD",
        "ONCRMINVOICEUSERFIELDUPDATE",
        "ONCRMINVOICEUSERFIELDDELETE",
        "ONCRMINVOICEUSERFIELDSETENUMVALUES",
        "ONCRMCURRENCYADD",
        "ONCRMCURRENCYUPDATE",
        "ONCRMCURRENCYDELETE",
        "ONCRMPRODUCTADD",
        "ONCRMPRODUCTUPDATE",
        "ONCRMPRODUCTDELETE",
        "ONCRMPRODUCTPROPERTYADD",
        "ONCRMPRODUCTPROPERTYUPDATE",
        "ONCRMPRODUCTPROPERTYDELETE",
        "ONCRMPRODUCTSECTIONADD",
        "ONCRMPRODUCTSECTIONUPDATE",
        "ONCRMPRODUCTSECTIONDELETE",
        "ONCRMACTIVITYADD",
        "ONCRMACTIVITYUPDATE",
        "ONCRMACTIVITYDELETE",
        "ONCRMREQUISITEADD",
        "ONCRMREQUISITEUPDATE",
        "ONCRMREQUISITEDELETE",
        "ONCRMREQUISITEUSERFIELDADD",
        "ONCRMREQUISITEUSERFIELDUPDATE",
        "ONCRMREQUISITEUSERFIELDDELETE",
        "ONCRMREQUISITEUSERFIELDSETENUMVALUES",
        "ONCRMBANKDETAILADD",
        "ONCRMBANKDETAILUPDATE",
        "ONCRMBANKDETAILDELETE",
        "ONCRMADDRESSREGISTER",
        "ONCRMADDRESSUNREGISTER",
        "ONCRMMEASUREADD",
        "ONCRMMEASUREUPDATE",
        "ONCRMMEASUREDELETE",
        "ONCRMDEALRECURRINGADD",
        "ONCRMDEALRECURRINGUPDATE",
        "ONCRMDEALRECURRINGDELETE",
        "ONCRMDEALRECURRINGEXPOSE",
        "ONCRMINVOICERECURRINGADD",
        "ONCRMINVOICERECURRINGUPDATE",
        "ONCRMINVOICERECURRINGDELETE",
        "ONCRMINVOICERECURRINGEXPOSE",
        "ONCRMTIMELINECOMMENTADD",
        "ONCRMTIMELINECOMMENTUPDATE",
        "ONCRMTIMELINECOMMENTDELETE",
        "ONCRMDYNAMICITEMADD",
        "ONCRMDYNAMICITEMUPDATE",
        "ONCRMDYNAMICITEMDELETE",
        "ONCRMDYNAMICITEMADD_147",
        "ONCRMDYNAMICITEMUPDATE_147",
        "ONCRMDYNAMICITEMDELETE_147",
        "ONCRMTYPEADD",
        "ONCRMTYPEUPDATE",
        "ONCRMTYPEDELETE",
        "ONCRMDOCUMENTGENERATORDOCUMENTADD",
        "ONCRMDOCUMENTGENERATORDOCUMENTUPDATE",
        "ONCRMDOCUMENTGENERATORDOCUMENTDELETE",
        "ONTASKADD",
        "ONTASKUPDATE",
        "ONTASKDELETE",
        "ONTASKCOMMENTADD",
        "ONTASKCOMMENTUPDATE",
        "ONTASKCOMMENTDELETE"
    ]
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)