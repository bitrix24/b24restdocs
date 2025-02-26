# Time Tracking: Overview of Methods

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

## Overview of Methods 

> Scope: [`timeman`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

### Workday {#all-methods}

#| 
|| **Method** | **Description** ||
|| [timeman.open](./base/timeman-open.md) | Start a new workday or resume a closed one ||
|| [timeman.pause](./base/timeman-pause.md) | Pause the workday ||
|| [timeman.close](./base/timeman-close.md) | Close the workday ||
|| [timeman.status](./base/timeman-status.md) | Get information about the user's current workday ||
|| [timeman.settings](./base/timeman-settings.md) | Get the user's work time settings ||
|#

### Office Networks

#| 
|| **Method** | **Description** ||
|| [timeman.networkrange.get](./networkrange/timeman-networkrange-get.md) | Retrieves the ranges of network addresses that are part of the office network ||
|| [timeman.networkrange.set](./networkrange/timeman-networkrange-set.md) | Sets the ranges of network addresses that are part of the office network ||
|| [timeman.networkrange.check](./networkrange/timeman-networkrange-check.md) | Checks if an IP address is within the ranges of the office network ||
|#

### Time Control

#| 
|| **Method** | **Description** ||
|| [timeman.timecontrol.report.add](./timecontrol/timeman-timecontrol-report-add.md) | Sends a report of identified absences ||
|| [timeman.timecontrol.reports.get](./timecontrol/timeman-timecontrol-reports-get.md) | Retrieves a report of identified absences ||
|| [timeman.timecontrol.settings.get](./timecontrol/timeman-timecontrol-settings-get.md) | Gets the settings for the time control tool ||
|| [timeman.timecontrol.settings.set](./timecontrol/timeman-timecontrol-settings-set.md) | Sets the settings for the time control tool ||
|| [timeman.timecontrol.reports.settings.get](./timecontrol/timeman-timecontrol-reports-settings-get.md) | Gets user settings for building the interface of the time control tool's reports ||
|| [timeman.timecontrol.reports.users.get](./timecontrol/timeman-timecontrol-reports-users-get.md) | Retrieves a list of users in the specified department ||
|#

### Work Schedule

#| 
|| **Method** | **Description** ||
|| [timeman.schedule.get](./schedule/timeman-schedule-get.md) | Retrieves the work schedule by ID ||
|#

## Several Examples

Examples of operation on the old core.

Confirming a work report with a positive rating on the page `/timeman/work_report.php`:

```php
CModule::IncludeModule('timeman');

$ID = 8;
$arFields = array(
    "MARK" => "G", // "G" - positive, "B" - negative, "N" - no rating, "X" - no confirmation
);

if ($arFields["MARK"] != "X")
{
    $arFields["APPROVER"] = $USER->GetID();
    $arFields["APPROVE"] = "Y";
    $arFields["APPROVE_DATE"] = ConvertTimeStamp(time(), "FULL");
}
else
{
    $arFields["APPROVE"] = "N";
    $arFields["APPROVER"] = 0;
    $arFields["APPROVE_DATE"] = "";
}

$result = CTimeManReportFull::Update($ID, $arFields);
var_dump($result);
```

Working with an employee's workday

```php
CModule::IncludeModule('timeman');
$USER_ID = 1;
$report = "";
$obUser = new CTimeManUser($USER_ID);

$obUser->OpenDay($timestamp, $report); // open workday
$obUser->CloseDay($timestamp, $report); // close workday
$state = $oTimeManUser->State(); // get the status of the workday for employee $USER_ID
$arInfo = $oTimeManUser->GetCurrentInfo(); // information about the workday for employee $USER_ID

// Data about the active task
CModule::IncludeModule('tasks');
$oTaskTimer = CTaskTimerManager::getInstance($USER_ID);
$rs = $oTaskTimer->getLastTimer();
```