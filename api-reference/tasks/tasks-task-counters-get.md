# Get User Counters tasks.task.counters.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for standard writing
- parameter types not specified
- parameter requirements not indicated
- examples missing (should include three examples - curl, js, php)
- no error response provided
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.counters.get` retrieves user counters.

You can filter by:
- userId
- groupId
- type

#|
|| **Parameter** / **Type** | **Description** ||
|| **userId**
[`unknown`](../data-types.md) | User identifier (namespace). If `userId` is not specified, the current user is used. ||
|| **groupId**
[`unknown`](../data-types.md) | User group identifier. ||
|| **type**
[`unknown`](../data-types.md) | Counter roles: 
- **view_all** - all roles; 
- **view_role_responsible** - "Responsible" role; 
- **view_role_accomplice** - "Accomplice" role; 
- **view_role_auditor** - "Auditor" role; 
- **view_role_originator** - "Originator" role. 
||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('tasks.task.counters.get', {userId:1, groupId:0, type:'view_all'}, (res)=>{console.log(res.answer.result);});
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}

## Success Response

> 200 OK

```json
{
    "result": {
        "wo_deadline": {
            "counter": 0,
            "code": 10485760
        },
        "expired": {
            "counter": 1,
            "code": 6291456
        },
        "expired_soon": {
            "counter": 0,
            "code": 9437184
        },
        "not_viewed": {
            "counter": 0,
            "code": 1048576
        },
        "wait_ctrl": {
            "counter": 0,
            "code": 8388608
        }
    },
    "total": 1,
    "time": {
        "start": 1552383141.526606,
        "finish": 1552383141.576861,
        "duration": 0.05025482177734375,
        "processing": 0.002279996871948242,
        "date_start": "2019-03-12T11:32:21+02:00",
        "date_finish": "2019-03-12T11:32:21+02:00"
    }
}
```