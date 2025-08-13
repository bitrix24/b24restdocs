# Register a call in Bitrix24 telephony.externalcall.register

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalcall.register` registers a call in Bitrix24 by searching for the corresponding object in CRM based on the phone number. If found, it adds the call linked to the identified object. If not found, it can automatically create a lead.

When using `telephony.externalcall.register`, the first responsible person for this client will be automatically assigned as the responsible for the new lead. This responsible person can be changed later through [telephony.externalcall.finish](telephony-external-call-finish.md).

Simultaneously with the call registration, the method can optionally display the call card to the user. The user to whom the card is shown is identified either by `USER_ID` or `USER_PHONE_INNER`. (That is, the fields are marked as required, but in fact, only one of the two is needed.)

There is no need to call this method again for calls received on the event [OnExternalCallStart](./events/on-external-call-start.md). These calls are already registered in the system, and only [telephony.externalcall.finish](telephony-external-call-finish.md) should be called at the end of the call.

{% note warning %}

A repeated call to `telephony.externalcall.register` with the same parameters, without closing the previous call using the method `telephony.externalcall.finish`, will return the same `CALL_ID` within 30 minutes.

{% endnote %}

To create an activity "call," it is also necessary to call the method `telephony.externalcall.finish`.

#|
|| **Parameter** / **Type** | **Description** ||
|| **USER_PHONE_INNER**^*^ 
[`string`](../data-types.md) | User's internal phone number. ||
|| **USER_ID**^*^ 
[`int`](../data-types.md) | User identifier. ||
|| **PHONE_NUMBER**^*^ 
[`string`](../data-types.md) | Phone number. ||
|| **CALL_START_DATE** 
[`string`](../data-types.md) | Call date/time in iso8601 format. Note that the timezone must be included in the date to avoid distortion of the call time. Example: `2021-02-03T18:25:10+03:00`. ||
|| **CRM_CREATE** 
[`int`](../data-types.md) | [0/1] - Automatic creation in CRM of the entity related to the call. Creates a lead or deal in CRM as needed, depending on [settings and CRM operation mode](*mode).

 Note that the call activity is created with any value of this parameter if creation is possible. ||
|| **CRM_SOURCE** 
[`string`](../data-types.md) | STATUS_ID of the source from the sources directory. You can get a list of sources using the `crm.status.list` method with a filter by "ENTITY_ID": "SOURCE". ||
|| **CRM_ENTITY_TYPE** 
[`string`](../data-types.md) | Type of the CRM object from which the call is made - CONTACT \| COMPANY \| LEAD. ||
|| **CRM_ENTITY_ID** 
[`int`](../data-types.md) | Identifier of the CRM object, the type of which is specified in CRM_ENTITY_TYPE. ||
|| **SHOW** 
[`int`](../data-types.md) | [0/1] Whether to show the call card (default is 1). ||
|| **CALL_LIST_ID** 
[`int`](../data-types.md) | Identifier of the [call list](../crm/call-list/index.md) to which the call should be linked. ||
|| **LINE_NUMBER** 
[`string`](../data-types.md) | Number of the external line through which the call was made (see [telephony.externalLine.add](telephony-external-line-add.md)).

{% note warning %}

Values from this parameter are used in Bitrix24's Sales Intelligence scenarios. Therefore, integration solutions with telephony for the Applications24 catalog must necessarily pass the number to which the registered incoming call was made.

{% endnote %}

||
|| **TYPE**^*^ 
[`integer`](../data-types.md) | Type of call:
1 - outgoing
2 - incoming
3 - incoming with redirection
4 - callback ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

## Returned data

#|
|| **Value** / **Type** | **Description** ||
|| **CALL_ID** 
[`string`](../data-types.md) | Identifier of the call within Bitrix24. ||
|| **CRM_CREATED_LEAD** 
[`int`](../data-types.md) | Identifier of the created lead (created if no object is found in CRM for the incoming number). ||
|| **CRM_ENTITY_ID** 
[`int`](../data-types.md) | Identifier of the object found in CRM. ||
|| **CRM_ENTITY_TYPE** 
[`string`](../data-types.md) | Type of the object found in CRM by the incoming number CONTACT \| COMPANY \| LEAD. ||
|| **CRM_CREATED_ENTITIES** 
[`array`](../data-types.md) | Array of entities automatically created in CRM when registering the call. Format:
- ENTITY_TYPE - type of the created entity
- ENTITY_ID - identifier of the created entity ||
|| **LEAD_CREATION_ERROR** 
[`string`](../data-types.md) | Error message that occurred while attempting to create a lead in CRM. ||
|#

[*mode]: There are:
- simple mode (without leads) - in which a deal will be created instead of a lead;
- repeat sales mode, in which a deal/lead will be created even if the entity is found in CRM. (But it will not be created if there is an active deal/lead or the number is on the CRM blacklist).