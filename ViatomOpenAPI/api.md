
### Common API url
> COMMON

**Host（Dev）:** https://testai.viatomtech.com


**Host（Online）:** 

(US server address): https://ai.viatomtech.com

(EU server address): https://eu-cloud.viatomtech.com



### Query device data

> BASIC

**Path:** /viatom-platform/third/queryDeviceData



**Method:** POST

> REQUEST

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| Content-Type | application/json | YES |  |
| secret-key | **Secret** | YES | Secret |
| Access-token | **Access-Token** | YES | Access-Token |

**Request Body:**

| name | type | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| current | integer | NO | current page |
| size | integer | NO | current page item size |
| userIds | array | NO | user id collection |
| &ensp;&ensp;&#124;─ | string |  ||
| dataType | integer | YES | data type<br>1 :Temperature<br>2 :SpO2<br>3 :Blood Pressure<br>4 :Blood Gucose<br>5 :ECG<br>6 :Heart Rate<br>7 :Sleep<br>9 :Cholesterol<br>10 :Weight<br>15 :Fetal Heart Rate |
| startTime | integer | NO | start timestamp |
| endTime | integer | NO | end timestamp |

**Request Demo:**
```json
{
    "current": 1,
    "size": 10,
    "userIds": [
        "MzIwMDA="
    ],
    "dataType": 1,
    "startTime": 2675490530000,
    "endTime": 2675490540001
}
```



> RESPONSE

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| content-type | application/json;charset=UTF-8 | NO |  |

**Body:**

| name | type | desc |
| ------------ | ------------ | ------------ |
| code | integer | code |
| msg | string | message |
| data | object | data |

**Response Demo:**

```json
// when dataType = 1
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1, // record type
                "account_id": "MzIwMDA=", // account id
                "member_id": "bnVsbA==", // member id
                "device_sn": "0000000000", // device sn
                "data_tag": "VEVNUEVSQVRVUkVAMzE3Ng==", // item unique
                "data": {
                    "data_type": 1, // data type
                    "measure_time": "2675490540000", // measure timestamp
                    "measure_result": {
                        "type": 0, // measure type 0:defalut/undefined 1:adult 2:child 3:ear 4:object
                        "temperature": {
                            "value": 36.94, // temp value
                            "unit": "Cel" // unit
                        }
                    }
                },
                "create_time": "1665619050000", // item create timestamp
                "update_time": "1694676339000" // item update timestamp
            }
        ],
        "total": "53", // total item count
        "size": "1", // current page item count
        "current": "1", // current page
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "53" // total pages
    }
}
```

```json
// when dataType = 2
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "21092F3633",
                "data_tag": "QkxPT0RfT1hZR0VOQDE2MTI3",
                "data": {
                    "data_type": 2,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "type": 2, // measure type. 0: undefined;1:spot-check; 2:Continuous measurement
                        "duration": 3928, // measure duration
                        "spo2": { // spo2
                            "value": 98,
                            "unit": "%"
                        },
                        "pr": { // pulse rate
                            "value": 85,
                            "unit": "bpm"
                        },
                        "90_duration": { // <90% duration
                            "value": 50,
                            "unit": "s"
                        },
                        "drops": { // 4% drops
                            "value": 0,
                            "unit": ""
                        },
                        "spo2_min": { // min spo2
                            "value": 98,
                            "unit": "%"
                        },
                        "o2_score": { // O2 score
                            "value": 10.00,
                            "unit": ""
                        },
                        "file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/2023/0912/758547cb-1d22-420f-a501-22af6ee4d233.dat" // spo2 file url
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "16127",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "16127"
    }
}
```

```json
// when dataType = 3
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "2106374387",
                "data_tag": "QkxPT0RfUFJFU1NVUkVAMjI4NzY=",
                "data": {
                    "data_type": 3,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "sbp": { // SBP
                            "value": 0,
                            "unit": "mmHg"
                        },
                        "dia": { // DIA
                            "value": 0,
                            "unit": "mmHg"
                        },
                        "pr": { // pulse rate
                            "value": 0,
                            "unit": "bpm"
                        },
                        "pulse_pressure": { // pulse pressure
                            "value": 0,
                            "unit": "mmHg"
                        },
                        "map": { // MAP
                            "value": 0,
                            "unit": "mmHg"
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "36673",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "36673"
    }
}
```

```json
// when dataType = 4
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "",
                "data_tag": "QkxPT0RfU1VHQVJAMjQ1Nw==",
                "data": {
                    "data_type": 4,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "type": 0, // measure state 0:empty stomach，1:pre- meal，2:after a meal，3:undefined
                        "blood_gucose": { // blood gucose
                            "value": 0.00,
                            "unit": "mmol/l"
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "3311",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "3311"
    }
}
```

```json
// when dataType = 5
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "2302170805",
                "data_tag": "RUNHQDM4OTA2",
                "data": {
                    "data_type": 5,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "duration": 30, // measure duration, unit: second
                        "file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/analyze/2023/0320/f894c6c2-96f0-4f2f-82c1-7a36fb5990a1.txt", // ecg file url
                        "hr": { // heart rate
                            "value": 0,
                            "unit": "bpm"
                        },
                        "ai_diagnose": { // AI result
                            "code": 0, // result code
                            "diagnose_info": "窦性心律", // result description
                            "report_file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/2023/0414/e0dc518e-a2b1-4bc4-9cc5-bfd7847db2b8.pdf" // pdf report url
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "41038",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "41038"
    }
}
```


```json
// when dataType = 6
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "2107441785",
                "data_tag": "SEVBUlRfUkFURUAxMTA0",
                "data": {
                    "data_type": 6,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "type": 0, // measure type, reserved. 0:default.
                        "hr": { // HR
                            "value": 59,
                            "unit": "bpm"
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "1115",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "1115"
    }
}
```

```json
// when dataType = 7
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "NDIyNQ==",
                "device_sn": "2208620079",
                "data_tag": "U0xFRVBAMTE2",
                "data": {
                    "data_type": 7,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "duration": 36008, // sleep duration
                        "sleep": [ // sleep detail
                            {
                                "sleep_type": 1, // sleep stage type. 0:deep, 1:light, 2:REM, 3:awake
                                "duration": 424 // sleep stage duration, unit:second
                            },
                            {
                                "sleep_type": 0,
                                "duration": 32820
                            },
                            {
                                "sleep_type": 1,
                                "duration": 328
                            }
                        ]
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "7",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "7"
    }
}
```

```json
// when dataType = 9
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "",
                "data_tag": "QkxPT0RfRkFUQDQ3NzE=",
                "data": {
                    "data_type": 9,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "chol": { // total cholesterol
                            "value": 1.00,
                            "unit": "mmol/L"
                        },
                        "trig": { // triglyceride
                            "value": null,
                            "unit": "mmol/L"
                        },
                        "hdl": { // high density lipoprotein (HDL)
                            "value": null,
                            "unit": "mmol/L"
                        },
                        "ldl": { // low density lipoprotein (LDL)
                            "value": null,
                            "unit": "mmol/L"
                        },
                        "chol_hdl": { // total cholesterol/high density
                            "value": null,
                            "unit": ""
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "4772",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "4772"
    }
}
```

```json
// when dataType = 10
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "Lescale Lite D0:4D:00:0E:01:FF",
                "data_tag": "Qk9EWV9GQVRANzI=",
                "data": {
                    "data_type": 10,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "device_type": 1,
                        "weight": { // weight
                            "value": 60.0,
                            "unit": "kg"
                        },
                        "bfp": { // body fat percentage
                            "value": 0.00,
                            "unit": "%"
                        },
                        "bmi": { // BMI
                            "value": null,
                            "unit": ""
                        }
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "21594",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "21594"
    }
}
```

```json
// when dataType = 15
{
    "code": 200,
    "msg": "success",
    "data": {
        "records": [
            {
                "type": 1,
                "account_id": "MzIwMDA=",
                "member_id": "bnVsbA==",
                "device_sn": "",
                "data_tag": "RkVUQUxfSEVBUlRAODU0",
                "data": {
                    "data_type": 15,
                    "measure_time": "1694676339000",
                    "measure_result": {
                        "minHr": { // min HR
                            "value": 68,
                            "unit": "bpm"
                        },
                        "avgHr": { // avg HR
                            "value": 92,
                            "unit": "bpm"
                        },
                        "maxHr": { // max HR
                            "value": 137,
                            "unit": "bpm"
                        },
                        "times": 0,
                        "duration": 27
                    }
                },
                "create_time": "1665619050000",
                "update_time": "1665619050000"
            }
        ],
        "total": "854",
        "size": "1",
        "current": "1",
        "orders": [],
        "optimizeCountSql": true,
        "hitCount": false,
        "countId": null,
        "maxLimit": null,
        "searchCount": true,
        "pages": "854"
    }
}

```

### Dynamic ECG analysis

> BASIC

**Path:** /third/dynamicEcgAnalysis

**Method:** POST

> REQUEST

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| Content-Type | multipart/form-data | YES |  |
| secret-key | **Secret** | YES | Secret |
| Access-token | **Access-Token** | YES | Access-Token |

**Query:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| patientId | 0001 | YES | Patient's unique identification number |
| patientName | your name | YES | Name of the current patient |
| gender | 1 | YES | Gender of the current patient: 1-Male, 2-Female, 0-Unknown |
| birth | 775324800000 | YES | Timestamp (in milliseconds) of the current patient's date of birth, accurate to the year, month, and day |
| measureTime | 1703112038000 | YES | The start timestamp of this measurement |
| duration | 70 | YES | The duration of this measurement session, in seconds |
| lead | 1 | YES | The number of leads |
| deviceType | 33 | NO | [Device type](#DeviceType) |
| deviceSn | 23034F0241 | NO | Device serial number |
| timeZone | 28800 | NO | Timezone offset in seconds. Default is UTC. For example: UTC=0, GMT+8=28800 |
| language | zh-CN | NO | [Language](#Language) |

**Form:**

| name | value | required | type | desc |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| file | [Sample file](https://github.com/viatom-develop/Lepu-AI/blob/main/ViatomOpenAPI/ecg-analysis.dat) | YES | file | The ECG file in dat, xml or txt format. |

> RESPONSE

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| content-type | multipart/form-data | NO |  |

**Body:**

| name | type | desc |
| ------------ | ------------ | ------------ |
| code | integer | code |
| msg | string | message |
| data | object | data |

**Response Demo:**

```json
{
    "code": 200,
    "msg": "success",
    "data": "1737750783631364098" // The ECG ID.
}
```

### Query ECG result

> BASIC

**Path:** /third/queryEcgResult

**Method:** POST

> REQUEST

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| Content-Type | application/json | YES |  |

**Request Body:**

| name | type | desc |
| ------------ | ------------ | ------------ |
|  | array | Collection of ECG IDs. |
|  | integer |  |

**Request Demo:**

```json
[
  1737737228878090241,
  1737738147003883522
]
```

> RESPONSE

**Headers:**

| name | value | required | desc |
| ------------ | ------------ | ------------ | ------------ |
| content-type | application/json;charset=UTF-8 | NO |  |

**Body:**

| name | type | desc |
| ------------ | ------------ | ------------ |
| code | integer | code |
| msg | string | message |
| data | object | data |
| &ensp;&ensp;&#124;─ | object |  |
| &ensp;&ensp;&ensp;&ensp;&#124;─id | integer | The ECG ID |
| &ensp;&ensp;&ensp;&ensp;&#124;─filePath | string | Cloud path of the original ECG file. |
| &ensp;&ensp;&ensp;&ensp;&#124;─resultPath | string | Text path of the ECG analysis result. |
| &ensp;&ensp;&ensp;&ensp;&#124;─reportPath | string | Path to the PDF report of the ECG analysis result. |
| &ensp;&ensp;&ensp;&ensp;&#124;─completeTime | string | Time of completion for the ECG analysis. |

**Response Demo:**

```json
// ps: Only return the analyzed ECG data.
{
  "code": 0,
  "msg": "",
  "data": [
    {
      "id": "1737737228878090241", // The ECG ID
      "filePath": "http://", // File path of the original ECG file
      "resultPath": "http://", // File path of the text result
      "reportPath": "http://", // File path of the PDF report of the ECG analysis result
      "completeTime": 1703210815000 // Time of completion for the ECG analysis
    },
    {
      "id": "1737738147003883522",
      "filePath": "http://",
      "resultPath": "http://",
      "reportPath": "http://",
      "completeTime": 1703210815000
    }
  ]
}
```

### Language
| language | descrpition |
| ------------ | ------------ |
|en-US |English|
|zh-CN |Simplified Chinese|
|de-DE |German|
|it-IT |Italian|
|es-ES |Spanish|
|fr-FR |French|

### DeviceType

| device type | device name |
| ------------ | ------------ |
|1|BP2_WIFI    |
|2|BP2 |
|3|BP2A    |
|4|BP2T    |
|5|SPOT_CHECK_MONITOR  |
|6|B02T    |
|7|B02 |
|8|TH3 |
|9|TH12    |
|10|CHECKME_O2 |
|11|CHECKME_O2_MAX |
|12|CHECKME_O2_LITE    |
|13|SLEEPO2    |
|14|SLEEPO2_LITE   |
|15|SNOREO2    |
|16|WEARO2 |
|17|SLEEPU |
|18|OXYLINK    |
|19|BABYO2_S2  |
|20|O2RING |
|21|BabyO2 |
|22|KIDSO2 |
|23|CHECKME_POD    |
|24|OXYFIT |
|25|PC60FW |
|26|FS20F  |
|27|OXYSMART   |
|28|POD_2W |
|29|OXYLINK_REMOTE |
|30|O2RING_REMOTE  |
|31|ER1    |
|32|ER2    |
|33|DOUEK  |
|34|VISUALBEAT |
|35|PULSEBIT_EX    |
|36|PULSEBIT   |
|37|AD51   |
|38|FD_510G    |
|39|P600L  |
|40|AOJ_20A    |
|41|F1 |
|42|LEM1   |
|43|AIR_FORCE  |
|44|F4 |
|45|F5 |
|46|F6 |
|47|PC_60NW_1  |
|48|POD_1W |
|49|AP_20  |
|50|PC_66B |
|51|P1 |
|52|SP_20  |
|53|PC_80B |
|54|PF_10  |
|55|PF_20  |
|56|PC_60NW    |
|57|BBSM_S1    |
|58|BBSM_S2    |
|59|OxyRing    |
|60|OxyU   |
|61|S5W    |
|62|HHM1   |
|63|HHM2   |
|64|HHM3   |
|65|HHM4   |
|66|S6W    |
|67|S7W    |
|68|PC_300SNT  |
|69|LPM311 |
|70|POCTORM3102    |
|71|PC_68B |
|72|S6W1   |
|73|S7BW   |
|74|S5 |
|75|PC200  |
|76|ER3    |
|77|CHECKME_PRO    |
|78|CHECKME_LITE   |
|79|P3 |
|80|F4_Pro |
|81|O2RING_S   |
|82|PF_10BW    |
|83|SA_10AW    |
|84|PO6B   |
|85|ER1_LW |
|86|ER1_LB |
|87|ER2_S  |
|88|LEPOD_PRO  |
|89|M12    |

### Error code

| code | message | desc |
| ------------ | ------------ | ------------ |
| 200 | success | request success |
| 500 | failure | request failed |
| 100003 | Account does not exist | account does not exist |
| 100004 | Account disabled. | account is forbiden |
| 100005 | Application service disabled. | service is forbiden |
| 100006 | Invalid application service. | invalid service |
| 101009 | No authorized user exists. | unauthorized user found |
| 109993 | Access token is invalid. | API invalid Access-token |
| 109994 | The request was not granted. | API unauthorized Access-token |
| 109996 | Access token error. | API Access-token not matched |
| 109997 | Secret key error. | API secret error |
| 109997 | Authentication failure. | invalid auth |
| 109999 | System busy, try again later! | too much requests |
