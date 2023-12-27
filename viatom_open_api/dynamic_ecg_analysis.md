### Common API url

> COMMON

**Host（Dev）:** https://testai.viatomtech.com

**Host（Online）:**

(US server address): https://ai.viatomtech.com

(EU server address): https://eu-cloud.viatomtech.com

### Dynamic ECG analysis

> BASIC

**Path:** /viatom-platform/v1/third/dynamicEcgAnalysis

**Method:** POST

> REQUEST

**Headers:**

| name         | value               | required | desc         |
|--------------|---------------------|----------|--------------|
| Content-Type | multipart/form-data | YES      |              |
| secret-key   | **Secret**          | YES      | Secret       |
| Access-token | **Access-Token**    | YES      | Access-Token |

**Form:**

| name         | value                                                                                                      | required | type   | desc                                    |
|--------------|------------------------------------------------------------------------------------------------------------|----------|--------|-----------------------------------------|
| file         | [sample file](https://github.com/viatom-develop/Lepu-AI/blob/main/ecg_demo_file/R20210525181552.dat)       | YES      | file   | The ECG file in dat, xml or txt format. |
| user_info    | {"patient_name":"The patient's name","gender":1,"birth":775238400000,"language":"zh-CN"}                   | YES      | string | JSON string of user's infomation        |
| measure_info | {"device_type":88,"device_sn":"2208820499","measure_time":1701826514000,"duration":2860,"time_zone":28800} | YES      | string | JSON string of measurement information  |

**Request Demo:**

```json
{
  "file": "The ECG file",
  "user_info": {
    "patient_name": "The patient's name", // Name of the current patient
    "gender": 1, // Gender of the current patient: 1-Male, 2-Female, 0-Unknown
    "birth": 775238400000, // Timestamp (in milliseconds) of the current patient's date of birth, accurate to the year, month, and day
    "language": "zh-CN"// see Language
  },
  "measure_info": {
    "device_type": 88, // see DeviceType
    "device_sn": "2208820499", // Device serial number
    "measure_time": 1701826514000, // The start timestamp of this measurement
    "duration": 2860, // The duration of this measurement session, in seconds
    "time_zone": 28800// Timezone offset in seconds. Default is UTC. For example: UTC=0, GMT+8=28800
  }
}
```

> RESPONSE

**Headers:**

| name         | value               | required | desc |
|--------------|---------------------|----------|------|
| content-type | multipart/form-data | NO       |      |

**Body:**

| name | type    | desc    |
|------|---------|---------|
| code | integer | code    |
| msg  | string  | message |
| data | object  | data    |

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

**Path:** /viatom-platform/v1/third/queryEcgResult

**Method:** POST

> REQUEST

**Headers:**

| name         | value            | required | desc |
|--------------|------------------|----------|------|
| Content-Type | application/json | YES      |      |

**Request Body:**

| name | type    | desc                   |
|------|---------|------------------------|
|      | array   | Collection of ECG IDs. |
|      | integer |                        |

**Request Demo:**

```json
{
  "ecg_ids": [ // Collection of ECG IDs
    1739911395169832961,
    1739912092507414529,
    1739913374319337473
  ]
}
```

> RESPONSE

**Headers:**

| name         | value                          | required | desc |
|--------------|--------------------------------|----------|------|
| content-type | application/json;charset=UTF-8 | NO       |      |

**Body:**

| name                                        | type    | desc                                               |
|---------------------------------------------|---------|----------------------------------------------------|
| code                                        | integer | code                                               |
| msg                                         | string  | message                                            |
| data                                        | object  | data                                               |
| &ensp;&ensp;&#124;─                         | object  |                                                    |
| &ensp;&ensp;&ensp;&ensp;&#124;─id           | integer | The ECG ID                                         |
| &ensp;&ensp;&ensp;&ensp;&#124;─file_path     | string  | Cloud path of the original ECG file.               |
| &ensp;&ensp;&ensp;&ensp;&#124;─result_path   | string  | Text path of the ECG analysis result.              |
| &ensp;&ensp;&ensp;&ensp;&#124;─report_path   | string  | Path to the PDF report of the ECG analysis result. |
| &ensp;&ensp;&ensp;&ensp;&#124;─complete_time | string  | Time of completion for the ECG analysis.           |

**Response Demo:**

```json
// ps: Only return the analyzed ECG data.
{
  "code": 0,
  "msg": "",
  "data": [
    {
      "id": "1737737228878090241", // The ECG ID
      "file_path": "http://", // File path of the original ECG file
      "result_path": "http://", // File path of the text result
      "report_path": "http://", // File path of the PDF report of the ECG analysis result
      "complete_time": 1703210815000 // Time of completion for the ECG analysis
    },
    {
      "id": "1737738147003883522",
      "file_path": "http://",
      "result_path": "http://",
      "report_path": "http://",
      "complete_time": 1703210815000
    }
  ]
}
```

### Language

| language | description        |
|----------|--------------------|
| en-US    | English            |
| zh-CN    | Simplified Chinese |
| de-DE    | German             |
| it-IT    | Italian            |
| es-ES    | Spanish            |
| fr-FR    | French             |

### DeviceType

| device type | device name                   |
|-------------|-------------------------------|
| 1           | BP2 WiFi                      |
| 2           | BP2                           |
| 3           | BP2A                          |
| 4           | BP2T                          |
| 5           | Spot-Check Monitor            |
| 6           | B02T                          |
| 7           | B02                           |
| 8           | TH3                           |
| 9           | TH12                          |
| 10          | Checkme O2                    |
| 11          | Checkme O2 Max                |
| 12          | Checkme O2 Lite               |
| 13          | SleepO2                       |
| 14          | SleepO2 Lite                  |
| 15          | SnoreO2                       |
| 16          | WearO2                        |
| 17          | SleepU                        |
| 18          | Oxylink                       |
| 19          | BabyO2 S2                     |
| 20          | O2RING                        |
| 21          | BabyO2                        |
| 22          | KidsO2                        |
| 23          | Checkme Pod                   |
| 24          | Oxyfit                        |
| 25          | PC60FW                        |
| 26          | FS20F                         |
| 27          | OxySmart                      |
| 28          | POD-2W                        |
| 29          | Oxylink-Remote                |
| 30          | O2Ring-Remote                 |
| 31          | ER1                           |
| 32          | ER2                           |
| 33          | DouEK                         |
| 34          | VisualBeat                    |
| 35          | Pulsebit EX                   |
| 36          | Pulsebit                      |
| 37          | baby Tone (AD51)              |
| 38          | baby Tone(FD-510G/VCOMIN)     |
| 39          | baby Tone(P600L)              |
| 40          | AOJ-20A(Infrared Thermometer) |
| 41          | F1                            |
| 42          | LEM1                          |
| 43          | Air Force                     |
| 44          | F4                            |
| 45          | F5                            |
| 46          | F6                            |
| 47          | PC 60NW 1                     |
| 48          | POD-1W                        |
| 49          | AP-20                         |
| 50          | PC-66B                        |
| 51          | P1                            |
| 52          | SP-20                         |
| 53          | PC-80B                        |
| 54          | PF-10                         |
| 55          | PF-20                         |
| 56          | PC 60NW                       |
| 57          | BBSM S1                       |
| 58          | BBSM S2                       |
| 59          | OxyRing                       |
| 60          | OxyU                          |
| 61          | S5W                           |
| 62          | HHM1                          |
| 63          | HHM2                          |
| 64          | HHM3                          |
| 65          | HHM4                          |
| 66          | S6W                           |
| 67          | S7W                           |
| 68          | PC-300SNT                     |
| 69          | LPM311                        |
| 70          | PoctorM3102                   |
| 71          | PC-68B                        |
| 72          | S6W1                          |
| 73          | S7BW                          |
| 74          | S5                            |
| 75          | PC200                         |
| 76          | ER3                           |
| 77          | Checkme Pro                   |
| 78          | Checkme Lite                  |
| 79          | P3                            |
| 80          | Lescale Pro                   |
| 81          | O2Ring S                      |
| 82          | PF-10BW                       |
| 83          | SA-10AW                       |
| 84          | PO6B                          |
| 85          | ER1 LW                        |
| 86          | ER1-LB                        |
| 87          | ER2 S                         |
| 88          | Lepod PRO                     |
| 89          | M12                           |

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
