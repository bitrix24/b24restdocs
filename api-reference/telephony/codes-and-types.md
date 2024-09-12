# Code and Type Reference

## Call Failure Code Table (CALL_FAILED_CODE)

#|
|| **Code** | **Description** ||
|| **$MESS["VI_STATUS_200"]**
| Successful call. ||
|| **$MESS["VI_STATUS_304"]**
| Missed call. ||
|| **$MESS["VI_STATUS_603"]**
| Declined. ||
|| **$MESS["VI_STATUS_603-S"]**
| Call canceled. ||
|| **$MESS["VI_STATUS_403"]**
| Forbidden. ||
|| **$MESS["VI_STATUS_404"]**
| Invalid number. ||
|| **$MESS["VI_STATUS_486"]**
| Busy. ||
|| **$MESS["VI_STATUS_484"]**
| This direction is not available. ||
|| **$MESS["VI_STATUS_503"]**
| This direction is not available. ||
|| **$MESS["VI_STATUS_480"]**
| Temporarily unavailable. ||
|| **$MESS["VI_STATUS_402"]**
| Insufficient funds in the account. ||
|| **$MESS["VI_STATUS_423"]**
| Blocked. ||
|| **$MESS["VI_STATUS_OTHER"]**
| Undefined. ||
|#

## Call Type Table (CALL_TYPE)

#|
|| **Type** | **Description** ||
|| **1**
| Outgoing. ||
|| **2**
| Incoming. ||
|| **3**
| Incoming with forwarding (to mobile or landline). ||
|| **4**
| Callback. ||
|#

## PBX Type Table

#|
|| **Type** | **Description** ||
|| **cloud**
| Cloud PBX. ||
|| **office**
| Office PBX. ||
|#

## SIP Registration Status Table

#|
|| **Status** | **Description** ||
|| **success**
| Successful registration. ||
|| **error**
| Failed registration. ||
|| **in_progress**
| Registration in progress. ||
|| **wait**
| Waiting to start registration. ||
|#

## See also

- [Complete list of allowed statuses](https://en.wikipedia.org/wiki/List_of_SIP_response_codes)