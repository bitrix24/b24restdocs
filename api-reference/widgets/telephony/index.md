# Call Card Tab CALL_CARD

> Scope: [`telephony`](../../scopes/permissions.md)

You can add your item in the call card tab.

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the widget is embedded

#|
|| **Widget Code** | **Location** ||
|| `CALL_CARD` | Item in the call card tab ||
|#

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 588b8a98e848778a4ffb38fbcf70f2b9
    [AUTH_ID] => 4172bb6600705a0700005a4b00000001f0f107c42ca5bd5f61030c5d9c3e4d60d11b5a
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 31f1e26600705a0700005a4b00000001f0f107b1918506d8a2ed9ecf76e8fdac962471
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CALL_CARD
    [PLACEMENT_OPTIONS] => {"CALL_ID":"externalCall.c3ee67f1a63f6e6117c230ab59cc49ea.1723556778","PHONE_NUMBER":"555555","LINE_NUMBER":"","LINE_NAME":"","CRM_ENTITY_TYPE":"","CRM_ENTITY_ID":"0","CRM_ACTIVITY_ID":"undefined","CALL_DIRECTION":"incoming","CALL_STATE":"connected","CALL_LIST_MODE":"false"}
)

```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **CALL_ID*** 
[`string`](../../data-types.md) | Identifier of the call during which the widget was opened.

||
|| **PHONE_NUMBER*** 
[`string`](../../data-types.md) | The phone number of the client with whom the conversation is taking place.

||
|| **LINE_NUMBER** 
[`string`](../../data-types.md) | The company's phone number used for the conversation with the client.

||
|| **LINE_NAME** 
[`string`](../../data-types.md) | The name of the company's phone line used for the conversation with the client.

Lines are added by applications for integrating telephony using the [telephony.externalLine.add](../../telephony/telephony-external-line-add.md) method and are used for user convenience in Sales Intelligence.

||
|| **CRM_ENTITY_TYPE** 
[`integer`](../../data-types.md) | [Type of the CRM entity](../../crm/data-types.md#object_type) to which the current call is linked.

Knowing the type and identifier of the CRM entity (specified in the `CRM_ENTITY_ID` parameter), you can retrieve information about the entity.

||
|| **CRM_ENTITY_ID** 
[`string`](../../data-types.md) | Identifier of the CRM entity to which the current call is linked.

Knowing the type (specified in the `CRM_ENTITY_TYPE` parameter) and the identifier of the CRM entity (specified in the `CRM_ENTITY_ID` parameter), you can retrieve information about the entity using the corresponding methods:

- any object type [crm.item.get](../../crm/universal/crm-item-get.md) with entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)

||
|| **CRM_ACTIVITY_ID** 
[`string`](../../data-types.md) | Identifier of the [CRM activity](../../crm/timeline/activities/index.md) related to the current call.

Can be used to obtain additional information using the [user.get](../../user/user-get.md) method.

||
|| **CALL_DIRECTION*** 
[`string`](../../data-types.md) | Defines the type of call. Can take the following values:

- 'incoming', incoming call;
- 'outcoming', outgoing call.

||
|| **CALL_STATE** 
[`string`](../../data-types.md) | Defines the state of the call. Can take the following values:

- 'connected', active call;

Other possible values will be published later.

||
|| **CALL_LIST_MODE** 
[`string`](../../data-types.md) | Indicates whether the call is part of a [call campaign](https://helpdesk.bitrix24.com/open/17520342/) or not.

Can take the following values:

- `False`, the call is not part of a call campaign;
- `True`, the call is made as part of a call campaign.

||
|#

## Continue exploring

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)