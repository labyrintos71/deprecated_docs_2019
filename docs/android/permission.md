---
layout: default
title: Permission
parent: Android
nav_order: 1
---
# Permission
안드로이드 시스템 권한은 정상 권한 및 위험한 권한으로 분류 되어있다.  
안드로이드 M(API 23) 미만 버전 에서는 권한 분류 없이 설치할 때 권한을 부여 하도록 요청 했지만  
안드로이드 M(API 23) 이상 버전 부터는 위험한 권한일 경우 런타임 상에서 권한을 요청하도록 해야한다.  

아래는 위험한 권한 그룹이다.  

| 권한그룹      | 권한                    | 
|:-------------|:------------------------|
| CALENDAR     | READ_CALENDAR           |
|              | WRITE_CALENDAR          |
| CAMERA       | CAMERA                  |
| CONTACTS     | READ_CONTACTS           |
|              | WRITE_CONTACTS          |
|              | GET_ACCOUNTS            |
| LOCATION     | ACCESS_FINE_LOCATION    |
|              | ACCESS_COARSE_LOCATION  |
| MICROPHONE   | RECORD_AUDIO            |
| PHONE        | READ_PHONE_STATE        |
|              | CALL_PHONE              |
|              | READ_CALL_LOG           |
|              | WRITE_CALL_LOG          |
|              | ADD_VOICEMAIL           |
|              | USE_SIP                 |
|              | PROCESS_OUTGONING_CALLS |
| SENSORS      | BODY_SENSORS            |
| SMS          | SEND_SMS                |
|              | READ_SMS                |
|              | RECEIVE_SMS             |
|              | RECEIVE_WARP_PUSH       |
|              | RECEIVE_MMS             |
| STORAGE      | READ_EXTERNAL_STORAGE   |
|              | WRITE_EXTERNAL_STOAGE   |