# Overview of Events When Working with Price Rounding Rules

Events allow applications to instantly respond to changes. Applications receive notifications about the creation, update, or deletion of price rounding rules.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to rounding rule events through the [application](./../../../app-installation/index.md) and the [event.bind](./../../../events/event-bind.md) method.

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.ROUNDING.ON.ADD](catalog-rounding-on-add.md) | When a rounding rule is added manually or via the [catalog.roundingRule.add](../catalog-rounding-rule-add.md) method ||
|| [CATALOG.ROUNDING.ON.UPDATE](catalog-rounding-on-update.md) | When a rounding rule is updated manually or via the [catalog.roundingRule.update](../catalog-rounding-rule-update.md) method ||
|| [CATALOG.ROUNDING.ON.DELETE](catalog-rounding-on-delete.md) | When a rounding rule is deleted manually or via the [catalog.roundingRule.delete](../catalog-rounding-rule-delete.md) method ||
|#