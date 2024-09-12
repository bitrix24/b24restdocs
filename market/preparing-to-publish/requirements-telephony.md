# Integrate Your Telephony or Contact Center with Bitrix24

The REST API for telephony makes it easy to incorporate your PBX or contact center into business scenarios for Bitrix24 users.

## Required Scenarios

To be featured on the [Bitrix24 Market](https://www.bitrix24.com/apps/?category=telephony), your integration must support several mandatory user scenarios:

1. Incoming/outgoing calls must be registered in Bitrix24 using the [telephony.externalcall.register](../../api-reference/telephony/telephony-external-call-register.md) method. Bitrix24 will automatically create the necessary objects in the CRM: lead, contact, deal, etc. You do not need to add this data; Bitrix24 will handle it automatically upon call registration.
2. When registering an incoming call, you need to pass the incoming line number to the [telephony.externalcall.register](../../api-reference/telephony/telephony-external-call-register.md) method to support Sales Intelligence in Bitrix24.
3. After a successful call, you need to send the call recording to Bitrix24 using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method.
4. You need to support the click2call scenario by adding an outgoing call event handler [OnExternalCallStart](../../api-reference/telephony/events/on-external-call-start.md).
5. You need to support the callback request scenario from Bitrix24 CRM forms by adding an outgoing call event handler [OnExternalCallBackStart](../../api-reference/telephony/events/on-external-call-back-start.md).

{% note info "" %}

Telephony solutions that support all mandatory user scenarios will be highlighted in the Bitrix24 Marketplace catalog with the "Recommended" label.

{% endnote %}

## Recommended Scenarios

In addition to the mandatory telephony integration scenarios, Bitrix24 users expect additional features from quality solutions:

1. Call forwarding to the responsible manager. For incoming calls, the integration can find the client in Bitrix24 by phone number using the [telephony.externalCall.searchCrmEntities](../../api-reference/telephony/telephony-external-call-search-crm-entities.md) method. This method will return information about the found client as well as the employee responsible for the client, allowing you to route the call to that employee.
2. Managing the display of the standard call card during call forwarding. If, after registering a call in Bitrix24, your PBX's queue processing rules (dial plan) require transferring the voice traffic to another employee, you can show the call card to that employee by calling the [telephony.externalcall.show](../../api-reference/telephony/telephony-external-call-show.md) method. At the same time, you should hide the call card from other employees by calling the [telephony.externalcall.hide](../../api-reference/telephony/telephony-external-call-hide.md) method.

## Application Review by Moderators

Before publishing your integration, our moderators will review the previously described telephony operation scenarios. Additionally, they will check the compliance of the integration card descriptions:

1. The descriptions and data in the card must meet the [application publication requirements for Bitrix24 Marketplace](./publication-requirements.md).
2. The card must contain a pricing grid for the services provided or a link to the pricing page.
3. The card must include a detailed description of the integration's functionality and setup instructions.

{% note info "" %}

To expedite the review process after submitting the application for moderation, it is recommended to provide the moderator with access to the PBX's account in the chat, along with an associated external number and several internal numbers.

{% endnote %}

## Continue Learning

- [{#T}](common-requirements.md)