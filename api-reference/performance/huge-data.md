# Retrieve Large Volumes of Data

Often, there is a need to import certain entities from the account via REST. However, when dealing with a large number of entities, the direct approach of setting a filter and passing `start = start + 50` in each subsequent request is not optimal. This is because using `start >= 0` requires an additional request to count the number of elements that meet the filter criteria for each request. This can be slow when there are many elements or when the filtering is complex.

Therefore, if you do not need the count of elements (for example, if you only need the last 10 records), or if you are importing all records based on a filter, you should pass `start = -1`.

This parameter will disable the count request and significantly speed up the data retrieval.

To perform the import, you will need to sort the records by `ID` and add a condition to the filter: `ID > value of the last element`. With each step, you will increase this value. The value of the last element should be taken from the last value of the received result.

The condition for stopping the import will be an empty response or when the number of elements in the response is less than 50.

Below is an example of code and a comparison of execution time with the classic approach. With 2,387,743 leads, using the same permissions and filter, the execution time decreased from 49.9 seconds to 0.097 seconds.

## Example

```php
$tokenID = 'XXXXXXXXXXXXXXXXXXXXX';
$host = 'XXXX.bitrix24.com';
$user = 1;
/**
* Start from zero or from some previous step
*/
$leadID = 0;
$finish = false;
while (!$finish)
{
    /**
    * Continue until all data is retrieved; in this case, do not forget about the delay between hits.
    * Alternatively, select only 50 each time, starting from the element where the last iteration stopped.
    */
    $http = new \Bitrix\Main\Web\HttpClient();
    $http->setTimeout(5);
    $http->setStreamTimeout(50);
    $json = $http->post(
        'https://'.$host.'/rest/'.$user.'/'.$tokenID.'/crm.lead.list/',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['>ID' => $leadID],
            'select' => ['ID', 'TITLE', 'DATE_CREATE'],
            'start' => -1
        ]
    );
    $result = \Bitrix\Main\Web\Json::decode($json);
    if (count($result['result']) > 0)
    {
        foreach ($result['result'] as $lead)
        {
            $leadID = $lead['ID'];
        }
        // Do something
    }
    else
    {
        $finish = true;
    }
}
/*
//Results of the REST request using count.
Array
(
    [result] => Array
    (
        [0] => Array()
        [1] => Array()
        .....
        [49] => Array()
    )
    [next] => 50
    [total] => 2387743
    [time] => Array
    (
        [start] => 1581607213.4833
        [finish] => 1581607263.3997
        [duration] => 49.916450023651
        [processing] => 49.899916887283
        [date_start] => 2020-02-13T18:20:13+03:00
        [date_finish] => 2020-02-13T18:21:03+03:00
    )
)
//Results of the REST request without using count.
Array
(
    [result] => Array
    (
        [0] => Array()
        [1] => Array()
        .....
        [1] => Array()
    )
    [total] => 0
    [time] => Array
    (
        [start] => 1581609136.3857
        [finish] => 1581609136.4835
        [duration] => 0.097883939743042
        [processing] => 0.068500995635986
        [date_start] => 2020-02-13T18:52:16+03:00
        [date_finish] => 2020-02-13T18:52:16+03:00
    )
)
```