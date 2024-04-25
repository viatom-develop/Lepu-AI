## ViHealth AI-ECG API

### Host URL
- **Development Environment:** `https://testai.viatomtech.com`
- **Production Environment:**
  - US Server: `https://ai.viatomtech.com`
  - EU Server: `https://eu-cloud.viatomtech.com`

### 1. Request ECG Analysis

#### Endpoint
- **Path:** `/viatom-platform/v1/third/dynamicEcgAnalysis`
- **Method:** POST

#### Request

##### Headers
| Name            | Value             | Required | Description  |
|-----------------|-------------------|----------|--------------|
| Content-Type    | multipart/form-data  | Yes      |              |
| secret-key      | *Secret*          | Yes      | API secret   |
| Access-token    | *Access-Token*    | Yes      | API token    |
| application-id  | *Application-ID*  | No       | App ID       |

##### Body: form-data
| Name        | Type   | Required | Description                                      |
|-------------|--------|----------|--------------------------------------------------|
| file        | file   | Yes      | ECG file in .dat, .xml, or .txt format.          |
| ecg_info    | string | Yes      | JSON string containing user and measurement info.|

###### file
ECG file is required to request AI-ECG analysis. You can use the ECG file download from our device or use our standard 1-lead .txt ECG format(English doc TBD).
[sample ECG files](https://github.com/viatom-develop/Lepu-AI/tree/main/ecg_demo_file)

###### ecg_info
```json
{
      "user_info": {
        "patient_name": "The patient's name", // patient name
        "gender": 1, // gender: 1-Male, 2-Female, 0-Unknown
        "birth": 775238400000, // date of birth, timestamp of year/month/day
        "language": "zh-CN"// see Language
      },
      "measure_info": {
        "device_type": 88, // see DeviceType
        "device_sn": "2208820499", // Device serial number
        "measure_time": 1701826514000, // The start timestamp of this measurement
        "duration": 2860, // The duration of this measurement, unit: second
        "time_zone": 28800 // Timezone offset in seconds. Default is UTC. For example: UTC=0, GMT+8=28800
      }
  }
```


#### Response

**Body:**

| Name | Type | Description |
| ---- | ---- | ----------- |
| code | integer | Response code |
| msg | string | Message |
| data | object | Response data including ECG ID |

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ecg_id": "MTc0MDU3NDU5MDk2OTU0MDYxMA==" // The ECG ID.
  }
}
```

#### Example
![image](./image/ecg_analysis.png)

### 2. Query ECG Analysis status

#### Endpoint
- **Path:** `/viatom-platform/v1/third/queryEcgStatus`
- **Method:** POST

#### Request

##### Headers

| Name | Value | Required | Description |
| ------------ | ------------ | ------------ | ------------ |
| Content-Type | application/json | YES |  |
| secret-key      | *Secret*          | Yes      | API secret   |
| Access-token    | *Access-Token*    | Yes      | API token    |
| application-id  | *Application-ID*  | No       | App ID       |

**Request Body:**

| Name | Value | Required | Description |
| ------------ | ------------ | ------------ | ------------ |
| ecg_ids | array | Yes | Collection of ECG IDs |

###### ecg_ids
```json
{
  "ecg_ids": ["ECG_ID_1", "ECG_ID_2", ...]
}
```

#### Response

**Body:**

| Name | Type | Description |
| ---- | ---- | ---- |
| code | integer | Response code |
| msg | string | Message |
| data | object | Response data including ECG status |

##### status_code
| status   | description                                        |
|--------------|----------------------------------------------------|
| 00000000 | ECG data does not exist.               |
| 00000001 | attempt to analysis has failed, try again.  |
| 00000002 | ECG analysing.            |
| 10010391 | ECG analysis success.       |
| 10010202 | ECG analysis failed.          |
| 02000106 | ECG analysis abnormal, or the service is not available now.|
| 02000106 | valid ecg data is not enough. |

```json
{
  "code": 200,
  "msg": "success",
  "data": [
    {"ecgId": "ECG_ID_1", "status": "status_code"},
    {"ecgId": "ECG_ID_2", "status": "status_code"},
    ...
  ]
}
```

#### Example
![image](./image/query_ecg_status.png)


### 3. Query ECG result

#### Endpoint
- **Path:** `/viatom-platform/v1/third/queryEcgResult`
- **Method:** POST

#### Request

##### Headers


| name         | value            | required | desc |
|--------------|------------------|----------|------|
| Content-Type | application/json | YES      |      |
| secret-key      | *Secret*          | Yes      | API secret   |
| Access-token    | *Access-Token*    | Yes      | API token    |
| application-id  | *Application-ID*  | No       | App ID       |

**Request Body:**

| name     | type   | desc                   |
|----------|--------|------------------------|
| ecg_ids  | array  | Collection of ECG IDs. |
|          | string |                        |

###### ecg_ids
```json
{
  "ecg_ids": [ // Collection of ECG IDs
    "MTc0MDU3NDUyMzIzNTcyNTMxMw=="
  ]
}
```

#### Response

**Body:**

| name  | type    | desc                                               |
|------|---------|----------------------------------------------------|
| code   | integer | code                                               |
| msg  | string  | message                                            |
| data   | object  | Collection of ECG analysis result   |
| - ecg_id  | integer | ECG analysis ID  |
| - diagnosis_result | string  | ECG analysis diagnostic result |
| - result_path | string  | full ECG analysis result  |
| - report_path | string  | ECG analysis PDF report url |
| - complete_time| string  | time of ECG analysis complete. |

##### ECG analysis result
```json
{
  "code": 0,
  "msg": "",
  "data": [
    {
      "ecg_id": "MTc0MDU3NDUyMzIzNTcyNTMxMw==", // The ECG ID
       "diagnosis_result": {
          "version": 2,
          "baseInfo": { // Basic Information
              "testId": "VNmnaZGY---pNaFOXSPReHnS", // Check ID
              "sample": "250", // Sampling Rate
              "analysisTime": "2023-12-29 11:24:56", // Analyze Time 
              "analysisDuration": 2860, // Analysis Duration
              "name": "Zhou", // Name
              "sex": "00160001", // Gender
              "age": "{\"d\": 2, \"m\": 5, \"w\": 0, \"y\": 29}", // Age
              "createTime": "2023-12-29 11:24:55", // Record Date   
              "startTime": "2023-12-06 09:35:14", // Start Time of Data   
              "endTime": "2023-12-06 10:22:54" // End Time of Data  
          },
          "afInfo": {
              "afBeatPercent": 0
          },
          "hrInfo": {
              "beatCount": 4290, // Total Heartbeats 
              "maxHeartRate": 91, // Average Heart Rate
              "minHeartRate": 89, // Maximum Heart Rate  
              "longRRPeriodAt": "", // Minimum Heart Rate  
              "atrialBeatCount": 0, // Time of Longest RR Interval   
              "maxLongRRPeriod": 0, // Number of long RR
              "averageHeartRate": 89, // Maximum RR interval duration
              "abnormalBeatCount": 0, // Number of arrests  
              "longRRPeriodCount": 0, // Abnormal heart count 
              "ventricularBeatCount": 0, // Ventricular beat  
              "asystoleRRPeriodCount": 0 // Supraventricular beats 
          },
          "hrvInfo": {
              "sdnn": 6.29, // SDNN
              "sdnnIndex": null, // SDNN Index  
              "rmssd": 9.05, // RMSSD
              "pnn50": 0.28, // PNN50
              "triangularIndex": 3.39, // Triangular index   
              "hf": 22.84, // HF
              "lf": 1.5, // LF
              "vlf": 0.5 // VLF
          },
          "ventricularInfo": {
              "ventricularBeatCount": 0, // Total number of ventricular cardiac beats
              "ventricularPermatureBeatCount": 0, // Number of premature ventricular contractions  
              "coupleVentricularPermatureCount": 0, // Premature ventricular contractions are paired
              "ventricularBigeminyCount": 0, // Number of ventricular dyads
              "ventricularTrigeminyCount": 0, // Number of ventricular triads
              "ventricularTachycardiaCount": 0, // Number of ventricular tachycardias
              "longestVentricularTachycardiaDuration": 0, // Longest duration of ventricular tachycardia
              "longestVentricularTachycardiaOccurTime": "", // Time to onset of longest ventricular tachycardia 
              "longestVentricularTachycardiaCount": null, // Longest ventricular tachycardia count
              "maxVentricularPermatureHeartRate": null // Ventricular velocity, maximum heart rate 
          },
          "supraventricularInfo": {
              "atrialBeatCount": 0, // Total supraventricular beats  
              "atrialPermatureBeatCount": 0, // Number of supraventricular premature contractions
              "coupleAtrialPermatureCount": 0, // Supraventricular premature contractions are paired
              "atrialBigeminyCount": 0, // Number of supraventricular dyads
              "atrialTrigeminyCount": 0, // Number of supraventricular triplets
              "atrialTachycardiaCount": 0, // Number of supraventricular tachycardias
              "longestAtrialTachycardiaDuration": 0, // Duration of longest supraventricular tachycardia
              "longestAtrialTachycardiaOccurTime": "", // Time to onset of longest supraventricular tachycardia
              "longestAtrialTachycardiaCount": null, // Longest supraventricular tachycardia count
              "maxAtrialPermatureHeartRate": null // Maximum heart rate at supraventricular velocity
          },
          "stList": null,
          "hrvList": null,
          "imgList": null,
          "imgSummary": null
      },
      "result_path": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/platform-test/2023/12/29/txt/d915e7bd-2ad8-46db-909a-83a7bbb04ff6.txt", // File path of the text result
      "report_path": "https://elasticbeanstalk-us-west-2-697648770036.s3.us-west-2.amazonaws.com/platform-test/2023/12/29/pdf/13dc6858-5832-405a-b5eb-f31b9e3c4730.pdf", // File path of the PDF report of the ECG analysis result
      "complete_time": "1703820332000" // Time of completion for the ECG analysis
    }
  ]
}
```

#### Example
![image](./image/query_ecg_result.png)

### 4. Appendix
#### Language

| language | description        |
|----------|--------------------|
| en-US    | English            |
| zh-CN    | Simplified Chinese |
| de-DE    | German             |
| it-IT    | Italian            |
| es-ES    | Spanish            |
| fr-FR    | French             |

#### Device type

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
| 999         | other single lead             |
| 1000        | other multi-lead              |

#### Error code

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
