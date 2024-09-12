# Event Queue Change onOfflineEvent

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Who can subscribe: any user

This event notifies about the occurrence of new offline events at certain intervals.

The application can subscribe to two types of events:

- Regular: the event triggers an external URL and performs an action defined by that address.
- Offline: instead of calling an external URL, events are locally saved on the account, from where they can later be retrieved using the event.offline.* methods.

For the onOfflineEvent, the necessity of sending a notification is determined based on the local saving, and then it is sent as a regular event to the external URL.

More about offline events.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **minTimeout** | Timeout in seconds. Default is 1 second. If the parameter value is:

- If equal to 0, regardless of the number of events added to the offline queue, only 1 event will be sent to the handler address within a single hit;
- If greater than 0, it sends one event upon the first trigger. Then, a pause of at least the timeout duration is made before sending the next event. | ||
|#

## Example

```
CRest::call(
    'event.bind',
    [
        'event' => 'ONOFFLINEEVENT',
        'handler' => 'https://example.com/handler.php',
        'options' => [
            'minTimeout' => 30,
        ]
    ]
);
```

offline_event - the application is not always in a state to receive events. It may be hidden behind firewalls, operate within an internal network, etc. In this case, the offline event mechanism is used, where the application subscribes to events but does not specify a handler URL.

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)