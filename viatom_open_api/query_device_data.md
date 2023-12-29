### Common API url

> COMMON

**Host（Dev）:** https://testai.viatomtech.com

**Host（Online）:**

(US server address): https://ai.viatomtech.com

(EU server address): https://eu-cloud.viatomtech.com

### Query device data

> BASIC

**Path:** /viatom-platform/v1/third/queryDeviceData

**Method:** POST

> REQUEST

**Headers:**

| name           | value              | required | desc           |
|----------------|--------------------|----------|----------------|
| Content-Type   | application/json   | YES      |                |
| secret-key     | **Secret**         | YES      | Secret         |
| Access-token   | **Access-Token**   | YES      | Access-Token   |
| application-id | **Application-ID** | NO       | Application-ID |

**Request Body:**

| name       | type    | required | desc                                                                                                                                                                          |
|------------|---------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| current    | integer | NO       | current page                                                                                                                                                                  |
| size       | integer | NO       | current page item size                                                                                                                                                        |
| data_type  | integer | YES      | data type<br>1 :Temperature<br>2 :SpO2<br>3 :Blood Pressure<br>4 :Blood Gucose<br>5 :ECG<br>6 :Heart Rate<br>7 :Sleep<br>9 :Cholesterol<br>10 :Weight<br>15 :Fetal Heart Rate |
| start_time | integer | NO       | start timestamp                                                                                                                                                               |
| end_time   | integer | NO       | end timestamp                                                                                                                                                                 |

**Request Demo:**

```json
{
  "current": 1,
  "size": 10,
  "data_type": 1,
  "start_time": 2675490530000,
  "end_time": 2675490540001
}
```

> RESPONSE

**Headers:**

| name         | value                          | required | desc |
|--------------|--------------------------------|----------|------|
| content-type | application/json;charset=UTF-8 | NO       |      |

**Body:**

| name | type    | desc    |
|------|---------|---------|
| code | integer | code    |
| msg  | string  | message |
| data | object  | data    |

**Example:**
![image](./image/query_device_data.png)

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
        "user_id": "MzIwMDA=|bnVsbA==", // user's ID
        "device_sn": "0000000000", // device sn
        "data_tag": "VEVNUEVSQVRVUkVAMzE3Ng==", // item unique
        "data": {
          "data_type": 1, // data type
          "measure_time": "2675490540000", // measure timestamp
          "measure_result": {
            "type": 0, // measure type 0:defalut/undefined 1:adult 2:child 3:ear 4:object
            "temperature": {
              "value": 36.94, // temp value
              "unit": "Cel"// unit
            }
          }
        },
        "create_time": "1665619050000", // item create timestamp
        "update_time": "1694676339000"// item update timestamp
      }
    ],
    "total": "53", // total item count
    "size": "1", // current page item count
    "current": "1", // current page
    "pages": "53"// total pages
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "21092F3633",
        "data_tag": "QkxPT0RfT1hZR0VOQDE2MTI3",
        "data": {
          "data_type": 2,
          "measure_time": "1694676339000",
          "measure_result": {
            "type": 2, // measure type. 0: undefined;1:spot-check; 2:Continuous measurement
            "duration": 3928, // measure duration
            "spo2": {// spo2
              "value": 98,
              "unit": "%"
            },
            "pr": {// pulse rate
              "value": 85,
              "unit": "bpm"
            },
            "90_duration": {// <90% duration
              "value": 50,
              "unit": "s"
            },
            "drops": {// 4% drops
              "value": 0,
              "unit": ""
            },
            "spo2_min": {// min spo2
              "value": 98,
              "unit": "%"
            },
            "o2_score": {// O2 score
              "value": 10.00,
              "unit": ""
            },
            "file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/2023/0912/758547cb-1d22-420f-a501-22af6ee4d233.dat"// spo2 file url
          }
        },
        "create_time": "1665619050000",
        "update_time": "1665619050000"
      }
    ],
    "total": "16127",
    "size": "1",
    "current": "1",
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "2106374387",
        "data_tag": "QkxPT0RfUFJFU1NVUkVAMjI4NzY=",
        "data": {
          "data_type": 3,
          "measure_time": "1694676339000",
          "measure_result": {
            "sbp": {// SBP
              "value": 0,
              "unit": "mmHg"
            },
            "dia": {// DIA
              "value": 0,
              "unit": "mmHg"
            },
            "pr": {// pulse rate
              "value": 0,
              "unit": "bpm"
            },
            "pulse_pressure": {// pulse pressure
              "value": 0,
              "unit": "mmHg"
            },
            "map": {// MAP
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "",
        "data_tag": "QkxPT0RfU1VHQVJAMjQ1Nw==",
        "data": {
          "data_type": 4,
          "measure_time": "1694676339000",
          "measure_result": {
            "type": 0, // measure state 0:empty stomach，1:pre- meal，2:after a meal，3:undefined
            "blood_gucose": {// blood gucose
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "2302170805",
        "data_tag": "RUNHQDM4OTA2",
        "data": {
          "data_type": 5,
          "measure_time": "1694676339000",
          "measure_result": {
            "duration": 30, // measure duration, unit: second
            "file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/analyze/2023/0320/f894c6c2-96f0-4f2f-82c1-7a36fb5990a1.txt", // ecg file url
            "hr": {// heart rate
              "value": 0,
              "unit": "bpm"
            },
            "ai_diagnose": {// AI result
              "code": 0, // result code
              "diagnose_info": "窦性心律", // result description
              "report_file_url": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/viatomstoragetest/data/ecg/2023/0414/e0dc518e-a2b1-4bc4-9cc5-bfd7847db2b8.pdf"// pdf report url
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "2107441785",
        "data_tag": "SEVBUlRfUkFURUAxMTA0",
        "data": {
          "data_type": 6,
          "measure_time": "1694676339000",
          "measure_result": {
            "type": 0, // measure type, reserved. 0:default.
            "hr": {// HR
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "2208620079",
        "data_tag": "U0xFRVBAMTE2",
        "data": {
          "data_type": 7,
          "measure_time": "1694676339000",
          "measure_result": {
            "duration": 36008, // sleep duration
            "sleep": [// sleep detail
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "",
        "data_tag": "QkxPT0RfRkFUQDQ3NzE=",
        "data": {
          "data_type": 9,
          "measure_time": "1694676339000",
          "measure_result": {
            "chol": {// total cholesterol
              "value": 1.00,
              "unit": "mmol/L"
            },
            "trig": {// triglyceride
              "value": null,
              "unit": "mmol/L"
            },
            "hdl": {// high density lipoprotein (HDL)
              "value": null,
              "unit": "mmol/L"
            },
            "ldl": {// low density lipoprotein (LDL)
              "value": null,
              "unit": "mmol/L"
            },
            "chol_hdl": {// total cholesterol/high density
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "Lescale Lite D0:4D:00:0E:01:FF",
        "data_tag": "Qk9EWV9GQVRANzI=",
        "data": {
          "data_type": 10,
          "measure_time": "1694676339000",
          "measure_result": {
            "device_type": 1,
            "weight": {// weight
              "value": 60.0,
              "unit": "kg"
            },
            "bfp": {// body fat percentage
              "value": 0.00,
              "unit": "%"
            },
            "bmi": {// BMI
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
        "user_id": "MzIwMDA=|bnVsbA==",
        "device_sn": "",
        "data_tag": "RkVUQUxfSEVBUlRAODU0",
        "data": {
          "data_type": 15,
          "measure_time": "1694676339000",
          "measure_result": {
            "minHr": {// min HR
              "value": 68,
              "unit": "bpm"
            },
            "avgHr": {// avg HR
              "value": 92,
              "unit": "bpm"
            },
            "maxHr": {// max HR
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
    "pages": "854"
  }
}

```

### Error code

| code   | message                       | desc                          |
|--------|-------------------------------|-------------------------------|
| 200    | success                       | request success               |
| 500    | failure                       | request failed                |
| 20015  | Parameter error               | Parameter error               |
| 100003 | Account does not exist        | account does not exist        |
| 100004 | Account disabled.             | account is forbiden           |
| 100005 | Application service disabled. | service is forbiden           |
| 100006 | Invalid application service.  | invalid service               |
| 101009 | No authorized user exists.    | unauthorized user found       |
| 101017 | ECG analysis failed.          | ECG file upload failed.       |
| 101018 | Your balance is insufficient. | Your balance is insufficient. |
| 109993 | Access token is invalid.      | API invalid Access-token      |
| 109994 | The request was not granted.  | API unauthorized Access-token |
| 109996 | Access token error.           | API Access-token not matched  |
| 109997 | Secret key error.             | API secret error              |
| 109997 | Authentication failure.       | invalid auth                  |
| 109999 | System busy, try again later! | too much requests             |
