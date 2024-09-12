# REST Methods for the Time Tracking Module

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Scope: [`timeman`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

## List of Methods

#|
|| **Method** | **Description** | **Version** ||
|| **Basic Methods** | | ||
|| [timeman.settings](./base/timeman-settings.md) | Retrieve user time tracking settings | 17.0.2 ||
|| [timeman.status](./base/timeman-status.md) | Get information about the user's current workday | 17.0.2 ||
|| [timeman.open](./base/timeman-open.md) | Start a new workday or resume a closed or paused one | 17.0.2 ||
|| [timeman.close](./base/timeman-close.md) | Close the workday | 17.0.2 ||
|| [timeman.pause](./base/timeman-pause.md) | Pause the workday | 17.0.2 ||
|| **Office Networks** | | ||
|| [timeman.networkrange.check](./networkrange/timeman-networkrange-check.md) | Method to check if an IP address falls within the office network address ranges. | 18.5.0 ||
|| [timeman.networkrange.get](./networkrange/timeman-networkrange-get.md) | Method to retrieve the address ranges that belong to the office network. | 18.5.0 ||
|| [timeman.networkrange.set](./networkrange/timeman-networkrange-set.md) | Method to set the address ranges that belong to the office network. | 18.5.0 ||
|| **Time Control** | | ||
|| [timeman.timecontrol.report.add](./timecontrol/timeman-timecontrol-report-add.md) | Method to submit a report of identified absences. | 18.5.0 ||
|| [timeman.timecontrol.reports.get](./timecontrol/timeman-timecontrol-reports-get.md) | Method to retrieve a report of identified absences. | 18.5.0 ||
|| [timeman.timecontrol.reports.settings.get](./timecontrol/timeman-timecontrol-reports-settings-get.md) | Method to get user settings for building the time control report interface. | 18.5.0 ||
|| [timeman.timecontrol.reports.users.get](./timecontrol/timeman-timecontrol-reports-users-get.md) | Method to get a list of users belonging to the specified department. | 18.5.0 ||
|| [timeman.timecontrol.settings.get](./timecontrol/timeman-timecontrol-settings-get.md) | Method to retrieve settings for the time control tool. | 18.5.0 ||
|| [timeman.timecontrol.settings.set](./timecontrol/timeman-timecontrol-settings-set.md) | Method to set settings for the time control tool. | 18.5.0 ||
|| **Work Schedule** | | ||
|| [timeman.schedule.get](./schedule/timeman-schedule-get.md) | Method to retrieve the work schedule by its identifier. | ||
|#

## Several Examples

Examples of operation on the old core.

Confirming a work report with a positive rating on the page `/timeman/work_report.php`:

```php
CModule::IncludeModule('timeman');

$ID = 8;
$arFields = array(
    "MARK" => "G", // "G" - positive, "B" - negative, "N" - no rating, "X" - without confirmation
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

$obUser->OpenDay($timestamp, $report); // open the workday
$obUser->CloseDay($timestamp, $report); // close the workday
$state = $oTimeManUser->State(); // get the status of the workday for employee $USER_ID
$arInfo = $oTimeManUser->GetCurrentInfo(); // information about the workday for employee $USER_ID

// Data about the active task
CModule::IncludeModule('tasks');
$oTaskTimer = CTaskTimerManager::getInstance($USER_ID);
$rs = $oTaskTimer->getLastTimer();
```