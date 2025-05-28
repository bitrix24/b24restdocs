# Get client information by phone number from CRM telephony.externalCall.searchCrmEntities

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalCall.searchCrmEntities` allows you to retrieve client information from the CRM by phone number in a single request. This information helps to decide which employee should be redirected to answer the incoming call at that moment. The method returns a suitable list of CRM entities sorted by internal priorities. If different employees are responsible for the entities associated with the number (one employee for the lead, another for the company), it is recommended to take the first object returned in the list. If the integration implies custom logic, a selection is possible since all objects are returned.

The list of CRM objects immediately provides all information about the responsible employee for each object (so there is no need to retrieve this data with additional REST requests). All contact phone numbers specified by the user are returned: the employee's internal phone, mobile, work phone, etc.

The status of the employee's workday is also returned (if time tracking is enabled in Bitrix24). The integration can check whether the employee is at their workplace (or on a break) and either redirect the incoming call to a queue or forward the call to the employee's mobile, etc.

It is recommended to call this method before calling [telephony.externalcall.register](telephony-external-call-register.md).

#|
|| **Parameter** | **Description** ||
|| **PHONE_NUMBER**^*^ | Client's phone number. ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

## Response in case of success

> 200 OK

Example of returned data

```json
Array
(
    [0] => Array
    (
        [CRM_ENTITY_TYPE] => CONTACT
        [CRM_ENTITY_ID] => 1
        [ASSIGNED_BY_ID] => 1
        [ASSIGNED_BY] => Array
            (
                [ID] => 1
                [TIMEMAN_STATUS] => CLOSED
                [USER_PHONE_INNER] => 102
                [WORK_PHONE] =>
                [PERSONAL_PHONE] =>
                [PERSONAL_MOBILE] => 19062195047
            )

    )

    [1] => Array
    (
        [CRM_ENTITY_TYPE] => COMPANY
        [CRM_ENTITY_ID] => 4
        [ASSIGNED_BY_ID] => 1
        [ASSIGNED_BY] => Array
            (
                [ID] => 1
                [TIMEMAN_STATUS] => CLOSED
                [USER_PHONE_INNER] => 102
                [WORK_PHONE] =>
                [PERSONAL_PHONE] =>
                [PERSONAL_MOBILE] => 19062195047
            )

    )

)
```