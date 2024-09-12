# Interaction with the CALL_CARD

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- It would be good to provide a link to BX24.placement.getInterface, but it is not documented
- Check permissions and scope

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute methods and subscribe to events: `any user`

The placement `CALL_CARD` is designed for working with the call card in the CRM:

The interface is returned by calling `BX24.placement.getInterface`.

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [getStatus](./get-status.md) | Returns information about the current call ||
|| [disableAutoClose](./disable-auto-close.md) | Disables automatic closing of the card for 60 seconds after the call ends ||
|| [enableAutoClose](./enable-auto-close.md) | Enables automatic closing of the card after the call ends ||
|#

## Overview of Events

#|
|| **Event** | **Triggered** ||
|| [CallCard::EntityChanged](./call-card-entity-changed.md) | When the current client changes in dial mode ||
|| [CallCard::BeforeClose](./call-card-before-close.md) | Before closing the call card ||
|| [CallCard::CallStateChanged](./call-card-call-state-changed.md) | When the state of the current call changes ||
|#