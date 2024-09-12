# List of Methods for Working with Working Time

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [timeman.settings](./timeman-settings.md) | Retrieve user working time settings ||
|| [timeman.status](./timeman-status.md) | Get information about the user's current working day ||
|| [timeman.open](./timeman-open.md) | Start a new working day or resume a closed or paused one ||
|| [timeman.close](./timeman-close.md) | Close the working day ||
|| [timeman.pause](./timeman-pause.md) | Pause the working day ||
|#

By default, the methods operate with the working day of the owner of the authorization token or webhook. If the owner has permission to write to other users' working days, they can work with the working days of any user.

Similar to other REST API methods, all time parameters are accepted in ISO-8601 (ATOM) format. The time zone specified in the transmitted data is taken into account and is considered the user's time zone. The date of opening the working day must correspond to the current day in the user's time zone, and the closing date must match the opening date.