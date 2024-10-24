# Get Chat and Dialogue Messages imopenlines.session.history.get

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

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the session history.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** ||
|| **CHAT_ID***
[`unknown`](../../../data-types.md) | 2020 | Chat identifier ||
|| **SESSION_ID***
[`unknown`](../../../data-types.md) | 494 | Session identifier ||
|#

If the user is not a participant in the chat, an error will be returned when calling with **only the parameter** `CHAT_ID`:

```json
{error: 'ACCESS_DENIED', error_description: 'You cannot open this conversation because you do not have sufficient rights.', ex: s}
error
:
"ACCESS_DENIED"
error_description
:
"You cannot open this conversation because you do not have sufficient rights."
```

You should use the `SESSION_ID` parameter; with it, no error is returned.

## Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX24.callMethod(
        'imopenlines.session.history.get',
        {
            CHAT_ID: 2024
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    // example for PHP

{% endlist %}

## Successful Response

```json
{
    "result": {
        "chatId": 1982,
        "canJoin": "Y",
        "canVoteHead": "Y",
        "sessionId": 469,
        "sessionVoteHead": 0,
        "sessionCommentHead": null,
        "userId": "chat1982",
        "message": {
            "19009": {
                "id": "19009",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-30T07:45:34+02:00",
                "text": "Dialogue completed.",
                "textlegacy": "Dialogue completed.",
                "params": {
                    "class": "bx-messenger-content-item-ol-end",
                    "componentId": "bx-imopenlines-message",
                    "imolVoteHead": 0,
                    "imolVoteSid": 469,
                    "imolVoteUser": 0,
                    "type": "lines"
                }
            },
            "19008": {
                "id": "19008",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-30T07:45:33+02:00",
                "text": "[USER=1 REPLACE]Vladimir Loginov",
                "textlegacy": "[USER=1 REPLACE]Vladimir Loginov",
                "params": []
            },
            "18148": {
                "id": "18148",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:52:03+02:00",
                "text": "NOT ANSWERED! ALARM! WARNING! ATTENTION! To the",
                "textlegacy": "NOT ANSWERED! ALARM! WARNING! ATTENTION! To the",
                "params": {
                    "class": "bx-messenger-content-item-ol-output",
                    "componentId": "bx-imopenlines-message",
                    "connectorMid": [
                        "18149"
                    ],
                    "imolForm": "offline",
                    "type": "lines"
                }
            },
            "18113": {
                "id": "18113",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "Start a new dialogue # [U",
                "textlegacy": "Start a new dialogue",
                "params": {
                    "class": "bx-messenger-content-item-ol-start"
                }
            },
            "18114": {
                "id": "18114",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "Forwarded",
                "textlegacy": "<B>Forwarded",
                "params": {
                    "attach": [
                        {
                            "id": 1684478426,
                            "blocks": [
                                {
                                    "message": "Page site"
                                }
                            ],
                            "description": "",
                            "color": "#df532d"
                        }
                    ],
                    "class": "bx-messenger-content-item-system"
                }
            },
            "18115": {
                "id": "18115",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "Address directed",
                "textlegacy": "Address directed",
                "params": []
            },
            "18116": {
                "id": "18116",
                "chatid": "1982",
                "senderid": "450",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "sdfsdfsdfsdfdff",
                "textlegacy": "sdfsdfsdfsdfdff",
                "params": {
                    "connectorMid": [
                        "18112"
                    ]
                }
            },
            "18117": {
                "id": "18117",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "Welcome to the",
                "textlegacy": "Welcome to the",
                "params": {
                    "class": "bx-messenger-content-item-ol-output",
                    "connectorMid": [
                        "18118"
                    ]
                }
            },
            "18119": {
                "id": "18119",
                "chatid": "1982",
                "senderid": "0",
                "recipientid": "chat1982",
                "date": "2023-05-19T01:40:26+02:00",
                "text": "[B]Sent form \"F",
                "textlegacy": "<B>Sent form",
                "params": {
                    "attach": [
                        {
                            "id": 1684478426,
                            "blocks": [
                                {
                                    "link": [
                                        {
                                            "name": "http://b24-3b4hq4.bitrix24.site/crm_form_v0fcl/",
                                            "link": "http://b24-3b4hq4.bitrix24.site/crm_form_v0fcl/"
                                        }
                                    ]
                                }
                            ],
                            "description": ""
                        }
                    ],
                    "componentId": "bx-imopenlines-form",
                    "connectorMid": [
                        "18120"
                    ],
                    "crmFormId": "3",
                    "crmFormSec": "huqwyy"
                }
            }
        },
        "usersMessage": {
            "chat1982": [
                "19009",
                "19008",
                "18148",
                "18113",
                "18114",
                "18115",
                "18116",
                "18117",
                "18119"
            ]
        },
        "users": {
            "450": {
                "id": "450",
                "name": "Guest",
                "active": true,
                "firstName": "Guest",
                "lastName": "",
                "workPosition": null,
                "color": "#ab7761",
                "avatar": "/bitrix/js/im/images/blank.gif",
                "avatarId": null,
                "birthday": false,
                "gender": "M",
                "phoneDevice": false,
                "phones": false,
                "extranet": true,
                "tzOffset": 0,
                "network": false,
                "bot": false,
                "connector": true,
                "profile": "/company/personal/user/450/",
                "externalAuthId": "imconnector",
                "status": null,
                "idle": false,
                "lastActivityDate": "2023-05-19T15:40:26+02:00",
                "mobileLastDate": false,
                "desktopLastDate": false,
                "departments": [],
                "absent": false,
                "services": null
            }
        },
        "openlines": {
            "canvoteashead": {
                "58": true
            }
        },
        "userInGroup": [],
        "woUserInGroup": [],
        "chat": {
            "1982": {
                "id": "1982",
                "name": "Guest Room #33 - A1",
                "owner": "0",
                "color": "#ab7761",
                "extranet": false,
                "avatar": "/bitrix/js/im/images/blank.gif",
                "call": "0",
                "callNumber": "",
                "entityType": "LINES",
                "entityId": "livechat|58|1981|450",
                "entityData1": "N|NONE|0|N|N|0|1684478426|0|0|0",
                "entityData2": "",
                "entityData3": "N",
                "public": "",
                "muteList": {
                    "450": false
                },
                "managerList": null,
                "dateCreate": "2023-05-19T08:40:26+02:00",
                "type": "lines",
                "messageType": "L"
            }
        },
        "userBlockChat": {
            "1982": {
                "450": false
            }
        },
        "userInChat": {
            "1982": [
                450
            ]
        },
        "files": []
    },
    "time": {
        "start": 1685712569.097672,
        "finish": 1685712569.247064,
        "duration": 0.14939212799072266,
        "processing": 0.0682840347290039,
        "date_start": "2023-06-02T15:29:29+02:00",
        "date_finish": "2023-06-02T15:29:29+02:00"
    }
}
```

## Error Response

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access to the specified chat ||
|| **CHAT_TYPE** | The specified chat is not an open line ||
|| **CHAT_ID** | An incorrect chat identifier was provided ||
|#