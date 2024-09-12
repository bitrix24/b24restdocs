# Error Codes

To handle errors that occur during request execution, it is necessary to analyze the HTTP response status or the presence of a special JSON structure in the response:

```json
{
    "error": "ERROR_HANDLER_ALREADY_EXIST",
    "error_description": "Handler already exists!"
}
```

{% include notitle [error handling](_includes/error-info.md) %}

A list of specific error codes that occur when calling a particular REST API method is described for each method separately. There is also a list of system errors that are independent of the logic of a specific REST API request.

## Statuses and Codes of System Errors

{% include notitle [system errors](_includes/system-errors.md) %}