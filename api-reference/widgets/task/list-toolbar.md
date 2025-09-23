# Item of the main dropdown menu TASK_USER_LIST_TOOLBAR, TASK_GROUP_LIST_TOOLBAR

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your item to the main dropdown menu in user and group tasks.

The code for the specific widget placement is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the widget is embedded

#|
|| **Widget Code** | **Location** ||
|| `TASK_USER_LIST_TOOLBAR` | Item in the main dropdown menu in user tasks ||
|| `TASK_GROUP_LIST_TOOLBAR` | Item in the main dropdown menu in group tasks ||
|#

## What the handler receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- TASK_USER_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 4617fa96af5d1f523fc2e2b72bd54f11
        [AUTH_ID] => 5253ba6600705a0700005a4b00000001f0f1076fef51e6d3d3c1616a9fd92a714ca452
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 42d2e16600705a0700005a4b00000001f0f107cf69d8060249da353587f8ec862be702
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => TASK_USER_LIST_TOOLBAR
        [PLACEMENT_OPTIONS] => {"USER_ID":"1"}
    )

    ```

- TASK_GROUP_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 9f3b397a4bc09ad1ee9b7a5db991a603
        [AUTH_ID] => cc53ba6600705a0700005a4b00000001f0f107e316cd1ed3be4be6856b7077e180656c
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => bcd2e16600705a0700005a4b00000001f0f1075b826a128425efbda11902d7f5d78062
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => TASK_GROUP_LIST_TOOLBAR
        [PLACEMENT_OPTIONS] => {"GROUP_ID":"10"}
    )

    ```

{% endlist %}

{% include [Footnote about required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Footnote about required parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **USER_ID***
[`string`](../../data-types.md) | The identifier of the user whose task list the widget was opened over.

Can be used to retrieve additional information using the [user.get](../../user/user-get.md) method.

||
|| **GROUP_ID***
[`string`](../../data-types.md) | The identifier of the workgroup/project whose task list the widget was opened over.

Can be used to retrieve additional information using the [sonet.group.get](../../sonet-group/sonet-group-get.md) method.

||
|#

## Continue your exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)