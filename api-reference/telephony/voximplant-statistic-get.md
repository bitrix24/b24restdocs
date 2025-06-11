# Get Call History List voximplant.statistic.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `voximplant.statistic.get` returns a list of call history. It is a list method. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Call Statistics - View` according to the value of this permission.

#|
|| **Parameter** | **Description** ||
|| **FILTER** | Fields for filtering ||
|| **SORT** | Field by which sorting is performed ||
|| **ORDER** | Sorting order (ASC/DESC) ||
|#

## Returned Data

#|
|| **Parameter** | **Description** ||
|| **CALL_ID** | Call identifier. ||
|| **ID** | Call identifier (for internal purposes). ||
|| **CALL_TYPE** | Type of call: 
- `1` — outgoing,
- `2` — incoming,
- `3` — incoming redirected to a mobile or landline phone,
- `4` — callback ||
|| **CALL_VOTE** | Default - 0. Call rating is used only for internal telephony. ||
|| **COMMENT** | Comment on the call. ||
|| **PORTAL_USER_ID** | Identifier of the operator who answered (if this is call type: 2 - Incoming) or identifier of the calling operator (if this is call type: 1 - Outgoing). ||
|| **PORTAL_NUMBER** | Number that received the call (if this is call type: 2 - Incoming) or number from which the call was made (1 - Outgoing). ||
|| **PHONE_NUMBER** | Number from which the subscriber is calling (if this is call type: 2 - Incoming) or number to which the operator is calling (1 - Outgoing). ||
|| **CALL_DURATION** | Duration of the call in seconds. ||
|| **CALL_START_DATE** | Time of call initiation. When sorting by this field, the date must be specified in ISO-8601 format. ||
|| **COST** | Cost of the call. ||
|| **COST_CURRENCY** | Currency of the call (USD, EUR). ||
|| **CALL_FAILED_CODE** | Call code (see the call code table). ||
|| **CALL_FAILED_REASON** | Text description of the call code (in Latin letters). ||
|| **CRM_ACTIVITY_ID** | Identifier of the CRM activity created based on the call. ||
|| **CRM_ENTITY_ID** | Identifier of the CRM entity to which the activity is attached. ||
|| **CRM_ENTITY_TYPE** | Type of CRM entity to which the activity is attached, for example: LEAD. ||
|| **REST_APP_ID** | Identifier of the external telephony integration application. ||
|| **REST_APP_NAME** | Name of the external telephony integration application. ||
|| **REDIAL_ATTEMPT** | Number of the redial attempt (for callbacks). ||
|| **SESSION_ID** | Identifier of the call session on the Voximplant side. ||
|| **TRANSCRIPT_ID** | Identifier of the call transcript. ||
|| **TRANSCRIPT_PENDING** | Y\N. Indicator that the transcript will be received later. ||
|| **RECORD_FILE_ID** | Identifier of the call recording file. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.statistic.get',
        {
            "FILTER": {">CALL_DURATION":60},
            "SORT": "CALL_DURATION",
            "ORDER": "DESC",
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}