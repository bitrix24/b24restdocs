# How to Contact Support

For questions regarding the REST API of Bitrix24, you can reach out to technical support. The AI support agent answers first in the chat. If the agent cannot find an answer, it transfers the chat to a support specialist, who clarifies the details, explains how a method works, or registers a request.

{% note warning "" %}

Chat with the [AI support agent](https://helpdesk.bitrix24.com/open/24410660/) is available on any plan. Only commercial plans allow the agent to transfer the chat to a support specialist.
If Partner Support is enabled in Bitrix24, the Bitrix24 Support chat is not available.

{% endnote %}

## How Support Can Help

- Assist with documentation, for example, finding a method based on its description.
- Accept requests for new functionality.
- Explain how a method or event works.
- Accept reports about a non-functioning method or event.

## Where to Ask Other Questions

Support answers questions about the REST API. Other topics are handled on separate resources.

| Question | Where to go |
| --- | --- |
| Partner status and publishing a solution in Bitrix24 Market | [Bitrix24 Market Partner Status](./market/technology-partnership.md) |
| Error or suggestion about the documentation | [Feedback](./feedback.md) |
| Discussing a scenario with other developers | [Support and Community for Developers](./support.md) |

## Where to Find the Support Chat

The support chat opens from the Bitrix24 interface. On a computer, the _Helpdesk_ button is in the upper right corner, and the path from there depends on the interface generation.

- **Classic interface.** Click _Helpdesk > Start chat > Still need help > Contact support_ and ask your question.
- **New interface.** The _Helpdesk_ button opens a panel with a link to support, where you will find the Bitrix24 status, the list of your requests, and the chat.

The classic interface currently runs on most Bitrix24 accounts, and the new one is rolling out gradually.

In the mobile app, tap _Menu > Support_.

In one chat, a support specialist addresses only one issue. If you have multiple questions, create [parallel dialogues](https://helpdesk.bitrix24.com/open/15510780/).

## What to Send to Support

The more complete the initial data, the faster support can look into your question.

{% note warning "" %}

Remove confidential data from the examples: the secret code of an inbound webhook, the access token, and the `auth` parameter. Replace their values with asterisks.

{% endnote %}

### Question About Method Functionality

1. Describe the method you are having trouble with. You can attach a link to the documentation page.

2. Send the request code.

3. Send the response to the request: JSON or an error message.

4. Describe what you expect from the method.

{% cut "Example Request Code" %}

```javascript
BX24.callMethod(
    'crm.item.productrow.add', {
        fields: {
            ownerId: 13142,
            ownerType: 'D',
            productId: 9621,
            price: 80000.000000,
            quantity: 2,
            discountTypeId: 2,
            discountRate: 20,
            taxRate: 20,
            taxIncluded: 'Y',
            measureCode: 796,
            sort: 10,
        },
    },
    function(result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.log(result.data());
        }
    }
);
```

{% endcut %}

{% cut "Example Response to Request" %}

```json
{
    "result": {
        "productRow": {
            "id": 17647,
            "ownerId": 13142,
            "ownerType": "D",
            "productId": 9621,
            "price": 80000,
            "quantity": 2,
            "discountTypeId": 2,
            "discountRate": 20,
            "taxRate": 20,
            "taxIncluded": "Y",
            "measureCode": 796,
            "sort": 10,
            "type": 4,
            "productName": "iphone 14",
            "priceAccount": 80000,
            "priceExclusive": 66666.67,
            "priceNetto": 83333.34,
            "priceBrutto": 100000.01,
            "discountSum": 16666.67,
            "customized": "Y",
            "measureName": "pcs",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1716887721.77879,
        "finish": 1716887723.259695,
        "duration": 1.4809050559997559,
        "processing": 1.2986550331115723,
        "date_start": "2024-05-28T12:15:21+02:00",
        "date_finish": "2024-05-28T12:15:23+02:00"
    }
}
```

{% endcut %}

{% cut "Example Error Message" %}

```json
{
    "error": "OWNER_NOT_FOUND",
    "error_description": "Owner was not found"
}
```

{% endcut %}

### Question About Event Functionality

1. Describe the event you are having trouble with. You can attach a link to the documentation page.

2. Provide the URL of the handler that is subscribed to the event:
	- URL from the `handler` field of the [event.bind](./api-reference/events/event-bind.md) method,
	- URL from the **URL of your handler** field of the [outgoing webhook](./local-integrations/local-webhooks.md).

3. Indicate the date and time when the event last failed to trigger.

## Continue Learning

- [{#T}](./support.md)
- [{#T}](./feedback.md)
- [{#T}](./error-codes.md)
- [{#T}](./limits.md)
