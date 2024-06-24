# AI开放平台接口文档

> **本文档仅提供给服务对接方使用，未经允许不得对外发布**

<!-- TOC -->

- [AI开放平台接口文档](#ai开放平台接口文档)
    - [版本修订记录](#版本修订记录)
    - [1. 请求信息](#1-请求信息)
    - [2. 接口信息](#2-接口信息)
        - [2.1 请求动态心电AI分析](#21-请求动态心电ai分析)
        - [2.2 请求静息心电AI分析](#22-请求静息心电ai分析)
        - [2.3 分析状态批量查询](#23-分析状态批量查询)
        - [2.4 获取分析结果](#24-获取分析结果)
            - [2.4.1 动态心电分析结果](#241-动态心电分析结果)
            - [2.4.2 静息心电分析结果](#242-静息心电分析结果)
    - [附录](#附录)
        - [动态心电](#动态心电)
        - [静态心电](#静态心电)
        - [分析状态](#分析状态)

<!-- /TOC -->

## 版本修订记录

| 时间           | 版本 | 修改人 | 修改说明 |
| -------------- | ---- | ------ | -------- |
| 2021年12月15日 | V0.1 | 王江   | 初次发布 |
| 2022年04月26日 | v0.2 | 王江   | 更新AI分析与签字报告返回JSON结构体 |
| 2022年04月27日 | v0.3 | kevin   | 更新部分字段 |
| 2022年05月18日 | v0.4 | kevin   | 更新部分字段描述 |
| 2022年09月23日 | v0.5 | kevin   | 新增静息心电分析接口，完善部分文档内容 |
| 2023年05月12日 | v0.5 | kevin   | 完善文档描述 |


## 1. 请求信息

1. 接口地址

   测试Host: `https://open.viatomtech.com.cn`

2. 接口基本定义
   本文档中的业务接口的入（出）参均是指业务参数，调用接口时Headers必须添加上公共请求头，解析响应时，必须按照公共出参解析

3. 字符编码
   UTF-8

4. 公共请求头 - Header

   | 参数名       | 是否必须 | 类型（长度） | 说明                                     |
   | ------------ | -------- | ------------ | ---------------------------------------- |
   | secret       | 是       | string       | 乐普给出                                 |
   | access-token | 是       | string       | 乐普给出, 不同业务使用不同的access_token |
   | language     | 是       | string       | zh-CN  / en-US                           |

5. 公共出参

   | 参数名  | 是否必须  |  类型（长度） | 说明                                 |
   | -------- | --------- | ------ | ------------------------------------- |
   | code    | 是       | int          | 响应码。0：成功，其他：异常          |
   | message | 是       | string       | 响应描述（常用于出错后的提示）       |
   | reason  | 是       | string       | 错误详情（主要为出错后的堆栈信息）   |
   | data    | 是       | object \| array | 响应内容（请参考具体业务接口的出参）, 默认为object,请求列表数据时为array  |

## 2. 接口信息

### 2.1 请求动态心电AI分析

&emsp;&emsp;接口地址：POST&emsp;`${host}/api/v1/ecg/analysis/request` \
&emsp;&emsp;Content-Type：`multipart/form-data`

 **请求体**

 | 参数名        | 是否必须 | 类型（长度） | 说明     |
 | ------------ | -------- | ------------ | ----------------------------------------------------------------|
 | ecg_file     | 否       | .txt/.dat    | 原始文件，可不传，参考AI分析接口ECG文件协议                            |
 | analyse_file | 是       | .txt         | 乐普标准单导ECG文件，优先使用此文件作为分析文件，参考AI分析接口ECG文件协议 |
 | ecg_info     | 是       | string       | JSON字符串，见附录 ecgInfo                                        |

 **响应参数**

 | 参数名       | 是否必须  | 类型（长度）  | 说明                   |
 | ----------- | -------- | ------------| --------------------- |
 | analysis_id | 是       | string      | 分析ID，用于请求分析结果  |

例：

```jsonc
{
 "code": 0,
 "data": {
  "analysis_id": "Mg==|OTA="
 },
 "message": "请求成功",
 "reason": ""
}
```

### 2.2 请求静息心电AI分析

&emsp;&emsp;接口地址：POST&emsp;`${host}/api/v1/resting_ecg/analysis/request` \
&emsp;&emsp;Content-Type：`multipart/form-data`

 **请求体**

 | 参数名       | 是否必须 | 类型（长度） | 说明                         |
 | ------------| -------- | ------------ | ------------------------ |
 | ecg_file    | 是       | .xml    | HL7格式的心电文件                        |
 | ecg_info    | 是       | string  | JSON字符串，见附录静态心电ecgInfo |

 **响应参数**

 | 参数名        | 是否必须  | 类型（长度）   | 说明                       |
 | ----------   | -------- | ------------ | ------------------------  |
 | analysis_id  | 是       | string       | 分析ID，用于请求分析结果      |

例：

```jsonc
{
 "code": 0,
 "data": {
  "analysis_id": "OA==|MzUyMg=="
 },
 "message": "请求成功",
 "reason": ""
}
```

### 2.3 分析状态批量查询

- 该接口适用于动态心电、静息心电。对应的状态值描述请在附录中查看。
- 调用查询分析结果接口前，先调用该接口获取状态，分析成功后再去获取结果。

&emsp;&emsp;接口地址：POST&emsp;`${host}/api/v1/ecg/analysis/batch_status/query` \
&emsp;&emsp;Content-Type：`application/json` \

 **请求体**

 | 参数名     | 是否必须 | 类型（长度） | 说明                   |
 | ---------- | -------- | ------------ | ---------------------- |
 | analysis_ids | 是       | array       | 分析ID数组 |

例：

```jsonc
{
 "analysis_ids": [
  "Mg==|Njg=",
  "Mg==|OTA="
 ]
}
```

 **响应参数**

 | 参数名 | 是否必须 | 类型（长度） | 说明         |
 | ------ | -------- | ------------ | ------------ |
 |        | 是       | array         | 分析状态列表 |

例：

```jsonc
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": [
        {
            "analysis_id": "Mg==|Njg=",
            "analysis_status": "80001"
        },
        {
            "analysis_id": "Mg==|OTA=",
            "analysis_status": "80001"
        }
    ]
}
```

### 2.4 获取分析结果
>请求该接口前，请先获取分析状态，分析完成后再请求

&emsp;&emsp;接口地址：POST&emsp;`${host}/api/v1/ecg/analysis/result/query` \
&emsp;&emsp;Content-Type：`application/json`

 **请求体**

 | 参数名     | 是否必须 | 类型（长度） | 说明                     |
 | ---------- | -------- | ------------ | ------------------------ |
 | analysis_id | 是       | string       | 分析ID，JSON编码字符串。 |

**动态心电响应参数**

 | 参数名          | 是否必须 | 类型（长度） | 说明                         |
 | --------------- | -------- | ------------ | ---------------------------- |
 | analysis_id     | 是       | string       | 分析ID，用于请求分析结果      |
 | analysis_status | 是       | string       | 分析状态                   |
 | analysis_result | 是       | string       | AI分析结果，具体字段见附录    |
 | report_url      | 是       | string       | 分析报告，比AI分析结果有延迟  |

#### 2.4.1 动态心电分析结果

- AI 分析结果
    <details>
    <summary>展开/折叠</summary>

    ```jsonc
    {
        "code": 0,
        "message": "请求成功",
        "reason": "",
        "data": {
            "analysis_id": "Mg==|ODU=",
            "analysis_status": "10010391",
            "analysis_result": {
                "baseInfo": {
                    "testId": "VN0EElDZ--0kZ0GbNkCA6jae",
                    "sample": 250,
                    "analysisTime": "2022-04-22 11:09:38",
                    "analysisDuration": 39,
                    "name": "用户名",
                    "sex": "00160002",
                    "age": "{\"d\": 0, \"m\": 0, \"w\": 0, \"y\": 26}",
                    "createTime": "2022-04-22 11:09:37",
                    "startTime": "2022-04-20 15:20:27", //会受measure_time格式影响
                    "endTime": "2022-04-20 15:21:06" //会受measure_time格式影响
                },
                "hrInfo": {
                    "beatCount": 56,
                    "averageHeartRate": 84,
                    "maxHeartRate": 87,
                    "minHeartRate": 82,
                    "longRRPeriodAt": "",
                    "longRRPeriodCount": 0,
                    "maxLongRRPeriod": 0,
                    "asystoleRRPeriodCount": 0,
                    "abnormalBeatCount": 0,
                    "ventricularBeatCount": 0,
                    "atrialBeatCount": 0
                },
                "hrvInfo": {
                    "sdnn": 23.85,
                    "sdnnIndex": null,
                    "rmssd": 18.43,
                    "pnn50": 0.0,
                    "triangularIndex": 4.82,
                    "hf": 164.85,
                    "lf": 250.85,
                    "vlf": 137.87
                },
                "afInfo": {
                    "afBeatPercent": 0.0
                },
                "ventricularInfo": {
                    "ventricularBeatCount": 0,
                    "ventricularPermatureBeatCount": 0,
                    "coupleVentricularPermatureCount": 0,
                    "ventricularBigeminyCount": 0,
                    "ventricularTrigeminyCount": 0,
                    "ventricularTachycardiaCount": 0,
                    "longestVentricularTachycardiaDuration": 0,
                    "longestVentricularTachycardiaOccurTime": "",
                    "longestVentricularTachycardiaCount": null,
                    "maxVentricularPermatureHeartRate": null
                },
                "supraventricularInfo": {
                    "atrialBeatCount": 0,
                    "atrialPermatureBeatCount": 0,
                    "coupleAtrialPermatureCount": 0,
                    "atrialBigeminyCount": 0,
                    "atrialTrigeminyCount": 0,
                    "atrialTachycardiaCount": 0,
                    "longestAtrialTachycardiaDuration": 0,
                    "longestAtrialTachycardiaOccurTime": "",
                    "longestAtrialTachycardiaCount": null,
                    "maxAtrialPermatureHeartRate": null
                },
                "diagnose": {
                    "description": "",
                    "diagnoseInfo": "窦性心律"
                },
                "ecgHourEventList": null,
                "eventList": [
                     {
                        "eventCode": "13000",
                        "eventName": "最大心率",
                        "startPos": "5805",
                        "endPos": "7805",
                        "eventTime": "2022-04-20 15:20:50", //会受measure_time格式影响
                        "hr": 87,
                        "leadNameList": ["II"]
                    }
                ],
                "hrvList": null,
                "imgList": null,
                "imgSummary": null,
                "allEventList": [
                    {
                        "endPos": "15410",
                        "startPos": "13410",
                        "eventCode": "13000",
                        "eventName": "最大心率",
                        "eventTime": "2023-05-12 15:00:53" //会受measure_time格式影响
                    }       
                ],
                "diagnoseList": [
                    {
                        "code": "101",
                        "diagnoseInfo": "窦性心律"
                    }
                ],
                "stList": null,
                "diagnoseList": [{
                    "code": "101",
                    "diagnoseInfo": "窦性心律"
                }]
            },
            "report_url": ""
        }
    }
    ```

    </details>

- 签字报告结果

    <details>
    <summary>展开/折叠</summary>

    ```jsonc
    {
        "code": 0,
        "message": "请求成功",
        "reason": "",
        "data": {
            "analysis_id": "Mg==|ODU=",
            "analysis_status": "10010391",
            "analysis_result": {
                "dataStartTime": "2022-04-20 15:22:07", //会受measure_time格式影响
                "dataEndTime": "2022-04-20 15:22:44", //会受measure_time格式影响
                "reportTime": "", 
                "totalDuration": 0,
                "validDuration": 0,
                "beatCount": "50",
                "abnormalBeatCount": "0",
                "abnormalBeatPercent": 0,
                "afBeatPercent": 0,
                "maxHeartRate": 87,
                "maxHeartRateTime": "",
                "minHeartRate": 84,
                "minHeartRateTime": "",
                "averageHeartRate": 85,
                "maxLongRRPeriod": 0.0,
                "maxLongAsystoleTime": "",
                "asystoleRRPeriodCount": 0,
                "atrialBeatCount": 0,
                "atrialPermatureBeatCount": 0,
                "coupleAtrialPermatureCount": 0,
                "atrialBigeminyCount": 0,
                "atrialTrigeminyCount": 0,
                "atrialTachycardiaCount": 0,
                "longestAtrialTachycardiaDuration": 0.0,
                "longestAtrialTachycardiaOccurTime": "",
                "ventricularBeatCount": 0,
                "ventricularPermatureBeatCount": 0,
                "coupleVentricularPermatureCount": 0,
                "ventricularBigeminyCount": 0,
                "ventricularTrigeminyCount": 0,
                "ventricularTachycardiaCount": 0,
                "longestVentricularTachycardiaDuration": 0.0,
                "longestVentricularTachycardiaOccurTime": "",
                "diagnoseList": [{
                    "code": "0",
                    "diagnoseInfo": "1.窦性心律"
                }],
                "eventList": [{
                    "eventCode": "13000",
                    "eventName": "最大心率",
                    "startPos": "87032",
                    "endPos": "89032",
                    "eventTime": "2021-09-07 10:31:29"
                }],
                "longRRPeriodCount": 0
            },
            "report_url": ""
        }
    }
    ```

    </details>
    
#### 2.4.2 静息心电分析结果

- AI分析/签字报告结果

    <details>
    <summary>展开/折叠</summary>

    ```jsonc
    {
        "code": 0,
        "data": {
            "analysis_id": "NQ==|NTE=",
            "analysis_status": "80001",
            "analysis_result": {
                "age": "{\"d\": 2, \"m\": 2, \"w\": 2, \"y\": 49}",
                "pid": "26",
                "gender": "00160002",
                "checkNo": "220708000006",
                "testTime": "2022-07-08T02:10:19.000Z", //utc时间，需要加本地时区得到本地时间
                "warnList": [],
                "itemCount": null,
                "patientId": "VN6Mr7ch--0Emwe56F7sNoLS",
                "genderText": "",
                "diagnosedBy": "",
                "featureData": {
                    "scale": 2.5199999809265137,
                    "filter": null,
                    "sample": 1000,
                    "dataLen": 1200,
                    "leadNum": 12,
                    "testTime": null,
                    "leadDatas": [
                        {
                            "leadName": "I",
                            "waveData": "-15 -15 -15 -15 -14 -14 -14 -14 -14 -14 -13 -13 -13 -13 -13 -13 -13 -13 -14 -14 -14 -14 -14 -14 -14 -14 -14 -14 -15 -15 -15 -16 -16 -17 -18 -18 -19 -19 -20 -20 -20 -20 -19 -19 -19 -18 -18 -17 -17 -16 -16 -15 -15 -15 -15 -15 -15 "
                        },
                        {
                            "leadName": "II",
                            "waveData": "-21 -21 -22 -22 -22 -21 -21 -20 -21 -20 -20 -20 -19 -19 -19 -20 -20 -20 -20 -20 -20 -21 -21 -21 -21 -21 -21 -22 -22 -23 -23 -24 -25 -26 -27 -28 -29 -29 -30 -30 -30 -30 -29 -29 -29 -28 -27 -26 -25 -25 -24 -24 -23 -23 -23 -23 -22 "
                        },
                        {
                            "leadName": "III",
                            "waveData": "-6 -6 -7 -7 -8 -7 -7 -6 -7 -6 -7 -7 -6 -6 -6 -7 -7 -7 -6 -6 -6 -7 -7 -7 -7 -7 -7 -8 -7 -8 -8 -8 -9 -9 -9 -10 -10 -10 -10 -10 -10 -10 -10 -10 -10 -10 -9 -9 -8 -9 -8 -9 -8 -8 -8 -8 -7 -9 -8 -8 -8 -8 -8 -8 -8 -8 -8 -7 -8 -7 -7 -7 -7 "
                        },
                        {
                            "leadName": "aVR",
                            "waveData": "18 18 18 18 18 17 17 17 17 17 16 16 16 16 16 16 16 16 17 17 17 17 17 17 17 17 17 18 18 19 19 20 20 21 22 23 24 24 25 25 25 25 24 24 24 23 22 21 21 20 20 19 19 19 19 19 18 18 18 18 18 18 18 18 18 18 18 17 18 17 16 16 15 15 14 13 13 "
                        },
                        {
                            "leadName": "aVL",
                            "waveData": "-5 -5 -4 -4 -3 -4 -4 -4 -4 -4 -3 -3 -4 -4 -4 -3 -3 -3 -4 -4 -4 -4 -4 -4 -4 -4 -4 -3 -4 -4 -4 -4 -4 -4 -5 -4 -5 -5 -5 -5 -5 -5 -5 -5 -5 -4 -5 -4 -5 -4 -4 -3 -4 -4 -4 -4 -4 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 -4 -3 -4 -3 -3 -3 -3 -3 -3 -2 "
                        },
                        {
                            "leadName": "aVF",
                            "waveData": "-14 -14 -15 -15 -15 -14 -14 -13 -14 -13 -14 -14 -13 -13 -13 -14 -14 -14 -13 -13 -13 -14 -14 -14 -14 -14 -14 -15 -15 -16 -16 -16 -17 -18 -18 -19 -20 -20 -20 -20 -20 -20 -20 -20 -20 -19 -18 -18 -17 -17 -16 -17 -16 -16 -16 -16 -15 "
                        },
                        {
                            "leadName": "V1",
                            "waveData": "-16 -16 -16 -16 -16 -16 -16 -15 -15 -15 -14 -14 -14 -14 -14 -15 -15 -15 -15 -15 -15 -15 -15 -16 -16 -16 -15 -16 -16 -17 -17 -18 -18 -19 -20 -20 -21 -22 -22 -22 -22 -22 -22 -21 -21 -20 -19 -18 -18 -17 -17 -16 -16 -16 -16 -16 -16 "
                        },
                        {
                            "leadName": "V2",
                            "waveData": "-16 -16 -16 -16 -16 -16 -15 -15 -15 -14 -14 -14 -14 -14 -14 -14 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -16 -16 -17 -17 -18 -19 -19 -20 -21 -22 -22 -22 -22 -22 -22 -21 -21 -20 -19 -18 -17 -17 -16 -16 -15 -15 -16 -16 -16 "
                        },
                        {
                            "leadName": "V3",
                            "waveData": "-16 -16 -16 -16 -16 -16 -16 -15 -15 -15 -15 -14 -14 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -16 -16 -15 -16 -16 -16 -17 -18 -18 -19 -19 -20 -21 -22 -22 -22 -22 -22 -22 -21 -21 -20 -19 -19 -18 -17 -17 -16 -16 -16 -16 -16 -16 "
                        },
                        {
                            "leadName": "V4",
                            "waveData": "-16 -17 -16 -16 -16 -16 -16 -16 -15 -15 -14 -14 -14 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -16 -15 -16 -16 -16 -17 -17 -18 -18 -19 -20 -20 -21 -22 -22 -22 -22 -22 -22 -21 -21 -20 -19 -18 -18 -17 -17 -16 -16 -16 -16 -16 -16 "
                        },
                        {
                            "leadName": "V5",
                            "waveData": "-16 -16 -16 -16 -16 -16 -16 -16 -15 -15 -15 -14 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -16 -15 -15 -16 -16 -16 -17 -17 -18 -19 -20 -20 -21 -21 -22 -22 -22 -22 -22 -21 -21 -20 -19 -18 -18 -17 -16 -16 -16 -16 -16 -16 -16 "
                        },
                        {
                            "leadName": "V6",
                            "waveData": "-16 -16 -16 -16 -16 -16 -16 -15 -15 -15 -14 -14 -14 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -15 -16 -16 -16 -16 -17 -18 -18 -19 -20 -20 -21 -22 -22 -22 -22 -22 -22 -21 -21 -20 -19 -18 -18 -17 -16 -16 -16 -16 -16 -16 -16 "
                        }
                    ]
                },
                "patientName": "Li Desong",
                "measurements": {
                    "hr": 81,
                    "pd": 145,
                    "pr": 178,
                    "qt": 392,
                    "rr": 749,
                    "td": 192,
                    "qrs": 93,
                    "qtc": 452,
                    "rv1": 337,
                    "rv5": 337,
                    "rv6": 337,
                    "sv1": 0,
                    "sv2": 0,
                    "sv5": 0,
                    "maxHR": 110,
                    "minHR": 63,
                    "axis_P": 47,
                    "axis_T": -129,
                    "axis_QRS": 47
                },
                "featurePoints": {
                    "pb": 262,
                    "pe": 407,
                    "tb": 632,
                    "te": 831,
                    "qrsb": 440,
                    "qrse": 530
                },
                "diagnosticList": [
                    {
                        "code": "821",
                        "content": "窦性心律不齐",
                        "leadInvolved": ""
                    }                
                ],
                "testOfficeName": "",
                "diagnoseContent": "",
                "diagnoseEndTime": "2022-07-08T02:36:27.000Z",//utc时间，需要加本地时区得到本地时间
                "diagnosedByName": "resting_pd",
                "leadMeasurements": [
                    {
                        "pd": 147,
                        "pr": 177,
                        "qa": 0,
                        "qd": 0,
                        "qt": 396,
                        "td": 180,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 75,
                        "pa2": 0,
                        "qrs": 97,
                        "ra1": 311,
                        "ra2": 0,
                        "rd1": 97,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": 0,
                        "st3": -30,
                        "stj": 29,
                        "ta1": -103,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -5,
                        "st40": -5,
                        "st60": -10,
                        "st80": -8,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "I"
                    },
                    {
                        "pd": 0,
                        "pr": 0,
                        "qa": 0,
                        "qd": 0,
                        "qt": 395,
                        "td": 181,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 0,
                        "pa2": 0,
                        "qrs": 78,
                        "ra1": 345,
                        "ra2": 0,
                        "rd1": 78,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -108,
                        "st2": -130,
                        "st3": -128,
                        "stj": 35,
                        "ta1": -256,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -88,
                        "st40": -113,
                        "st60": -115,
                        "st80": -115,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "II"
                    },
                    {
                        "pd": 152,
                        "pr": 189,
                        "qa": 0,
                        "qd": 0,
                        "qt": 389,
                        "td": 205,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 34,
                        "pa2": 0,
                        "qrs": 72,
                        "ra1": 136,
                        "ra2": 0,
                        "rd1": 72,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": -12,
                        "st3": -12,
                        "stj": 42,
                        "ta1": -47,
                        "ta2": 0,
                        "vat": 0,
                        "st20": 5,
                        "st40": -7,
                        "st60": -7,
                        "st80": -7,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "III"
                    },
                    {
                        "pd": 144,
                        "pr": 175,
                        "qa": -380,
                        "qd": 96,
                        "qt": 395,
                        "td": 177,
                        "ua": 0,
                        "ud": 0,
                        "pa1": -98,
                        "pa2": 0,
                        "qrs": 96,
                        "ra1": 0,
                        "ra2": 0,
                        "rd1": 0,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": 5,
                        "st2": 0,
                        "st3": 37,
                        "stj": -37,
                        "ta1": 128,
                        "ta2": 0,
                        "vat": 0,
                        "st20": 5,
                        "st40": 5,
                        "st60": 10,
                        "st80": 10,
                        "delta": 1,
                        "nRNotch": 1,
                        "leadType": "aVR"
                    },
                    {
                        "pd": 0,
                        "pr": 0,
                        "qa": 0,
                        "qd": 0,
                        "qt": 389,
                        "td": 192,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 0,
                        "pa2": 0,
                        "qrs": 72,
                        "ra1": 85,
                        "ra2": 0,
                        "rd1": 72,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -2,
                        "st2": -5,
                        "st3": -5,
                        "stj": 22,
                        "ta1": -27,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -2,
                        "st40": -2,
                        "st60": -5,
                        "st80": -5,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "aVL"
                    },
                    {
                        "pd": 147,
                        "pr": 181,
                        "qa": 0,
                        "qd": 0,
                        "qt": 393,
                        "td": 216,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 73,
                        "pa2": 0,
                        "qrs": 94,
                        "ra1": 294,
                        "ra2": 0,
                        "rd1": 94,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": -3,
                        "st3": -30,
                        "stj": 32,
                        "ta1": -98,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -8,
                        "st40": -8,
                        "st60": -10,
                        "st80": -10,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "aVF"
                    },
                    {
                        "pd": 147,
                        "pr": 180,
                        "qa": 0,
                        "qd": 0,
                        "qt": 393,
                        "td": 196,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 80,
                        "pa2": 0,
                        "qrs": 93,
                        "ra1": 337,
                        "ra2": 0,
                        "rd1": 93,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -8,
                        "st2": -3,
                        "st3": -33,
                        "stj": 34,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -8,
                        "st40": -3,
                        "st60": -10,
                        "st80": -8,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V1"
                    },
                    {
                        "pd": 144,
                        "pr": 175,
                        "qa": 0,
                        "qd": 0,
                        "qt": 395,
                        "td": 188,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 82,
                        "pa2": 0,
                        "qrs": 87,
                        "ra1": 337,
                        "ra2": 0,
                        "rd1": 87,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": -8,
                        "st3": -18,
                        "stj": 77,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -3,
                        "st40": -8,
                        "st60": -18,
                        "st80": -3,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V2"
                    },
                    {
                        "pd": 145,
                        "pr": 182,
                        "qa": 0,
                        "qd": 0,
                        "qt": 389,
                        "td": 200,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 85,
                        "pa2": 0,
                        "qrs": 80,
                        "ra1": 339,
                        "ra2": 0,
                        "rd1": 80,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": -10,
                        "st3": -15,
                        "stj": 82,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": 0,
                        "st40": -8,
                        "st60": -18,
                        "st80": -3,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V3"
                    },
                    {
                        "pd": 145,
                        "pr": 178,
                        "qa": 0,
                        "qd": 0,
                        "qt": 393,
                        "td": 184,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 85,
                        "pa2": 0,
                        "qrs": 85,
                        "ra1": 339,
                        "ra2": 0,
                        "rd1": 85,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -5,
                        "st2": -8,
                        "st3": -18,
                        "stj": 77,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -3,
                        "st40": -8,
                        "st60": -18,
                        "st80": -3,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V4"
                    },
                    {
                        "pd": 144,
                        "pr": 177,
                        "qa": 0,
                        "qd": 0,
                        "qt": 393,
                        "td": 198,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 85,
                        "pa2": 0,
                        "qrs": 93,
                        "ra1": 337,
                        "ra2": 0,
                        "rd1": 93,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -8,
                        "st2": -5,
                        "st3": -33,
                        "stj": 34,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -5,
                        "st40": -3,
                        "st60": -10,
                        "st80": -8,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V5"
                    },
                    {
                        "pd": 144,
                        "pr": 177,
                        "qa": 0,
                        "qd": 0,
                        "qt": 393,
                        "td": 193,
                        "ua": 0,
                        "ud": 0,
                        "pa1": 85,
                        "pa2": 0,
                        "qrs": 94,
                        "ra1": 337,
                        "ra2": 0,
                        "rd1": 94,
                        "rd2": 0,
                        "sa1": 0,
                        "sa2": 0,
                        "sd1": 0,
                        "sd2": 0,
                        "st1": -8,
                        "st2": -3,
                        "st3": -33,
                        "stj": 29,
                        "ta1": -116,
                        "ta2": 0,
                        "vat": 0,
                        "st20": -5,
                        "st40": -3,
                        "st60": -10,
                        "st80": -10,
                        "delta": 0,
                        "nRNotch": 0,
                        "leadType": "V6"
                    }
                ],
                "diagnoseBeginTime": "2022-07-08T02:30:27.000Z", //utc时间，需要加本地时区得到本地时间
                "diagnosedSignature": "",
                "diagnoseFeatureDesc": ""
            },
            "report_url": "https://********/report/20220708/e8b1f29d74554b7aaa6e4f29bed5d962.pdf"
        },
        "message": "请求成功",
        "reason": ""
    }
    ```

    </details>

## 附录

### 动态心电

1. ecgInfo

```jsonc
   {
        "user": {
            "name": "",    // 姓名，必填
            "phone": "", // 手机号，必填
            "gender": "1",          // 性别（1：男；2：女）
            "birthday": "1991-08-13", // 生日（yyyy-MM-dd）
            "id_number": "",  // 身份证号码，对于签字报告必填
            "height":170, // 身高cm，非必填
            "weight":80 // 体重kg，非必填
            //gender、birthday会影响分析结果，请填写真实数据
        },
        "analysis_type": "1", // 1短程，2长程
        "service_ability": 1, // 1 AI分析， 2 医生签字报告
        "access_token": "", // 对应服务能力的AccessToken
        "application_id": "xxx.xxx.xx", //应用id(应用包名,注意格式)
        "device": {
            "model": "ER1", //型号，从下列设备列表中查找对应的model(区分大小写)
            "band": "Lepu", //品牌
            "sn": "" //设备sn
        },
        "ecg": {
            "measure_time": "2022-08-01 10:25:00", //开始测量时间，支持UTC格式(即0时区时间,如2022-10-01T10:30:00Z,注意与本地当前时间的正确换算)和本地当前时间格式(如2022-08-01 10:25:00),
            //注意事项：如使用UTC格式，则需要加上本地对应的时区得到本地时间，使用后者格式则不需要。measure_time字段的不同格式会影响到报告和分析结果里的数据、事件等时间，
            //请根据实际需求正确使用
            "duration": 30, //分析数据时长，单位s, 不少于30，>=900为长程，否则为短程，要与analysis_type字段的值匹配
            "sample_rate": 125, //设备采样率，如500
            "lead": "II" // I导联/II导联
        }
    }
```

2. 设备列表

   | band | model | 采样率 | 导联| 备注             |
   | ---- | ----- | ------|----|----------- |
   | Lepu | ER1   | 125hz | II |单导联动态心电记录仪|
   | Lepu | ER2   | 125hz | II |单导联心电记录仪 |
   | Lepu | ER2-S | 125hz | II |单导联心电记录仪 |
   | Lepu | 心电体脂秤S1 | 125hz |  II |心电体脂秤S1|
   | Lepu | BP2    | 存储数据125hz/实时数据250hz | II | 心电血压计        |
   | Lepu | BP2-Pro| 存储数据125hz/实时数据250hz | II |心电血压计wifi版   |
   | Lepu | 手表W1  |                          | II | 乐普手表W1        |
   |      | 通用设备类型 | 如果您使用其他品牌的设备，<br>请使用“通用设备类型”，<br>或者与我们联系 | | |

   注：ecgInfo中的device相关字段必须与该列表提供的保持一致，否则可能导致分析失败



4. 分析结果描述

    |      |  key | 中文说明 | 类型 | 备注 |
    | ---- | ---- | ------- | ---- | ---- |
    | 报告返回对象  |   |  |||
    | baseInfo|  | |  | 基本信息 |
    | | testId | 检查id | string | 用于消费者端截取片段数据|
    | | sample | 采样率 | int | 用于消费者端截取片段数据|
    | | analysisTime | 分析时间 | string ||
    | | analysisDuration | 分析时长 | int | 单位秒|
    | | name | 姓名 | string | 这些基本信息，以消费者端的病人信息为准|
    | | sex | 性别 | string ||
    | | age | 年龄 | int    ||
    | | createTime | 记录日期 | string ||
    | | startTime | 数据开始时间| string |会受measure_time格式影响|
    | | endTime | 数据结束时间| string |会受measure_time格式影响|
    | hrInfo  |   | 心率统计|||
    | | beatCount | 心博总数 | int ||
    | | averageHeartRate | 平均心率 | int ||
    | | maxHeartRate | 最快（大）心率 | int ||
    | | minHeartRate | 最慢（小）心率 | int ||
    | | longRRPeriodAt | 最长RR间期发生时间 | string ||
    | | longRRPeriodCount | 长RR（间期）数量 | int ||
    | | maxLongRRPeriod | 最长RR间期时长 | int ||
    | | asystoleRRPeriodCount | 停搏数量 | int ||
    | | abnormalBeatCount | 异常心博数 | int | 签字报告|
    | | ventricularBeatCount | 室性心博 | int | 签字报告|
    | | atrialBeatCount | 室上性心博 | int | 签字报告|
    | hrvInfo  |  |  | | 心率变异统计|
    | 心率变异 | sdnn | SDNN | double ||
    | | sdnnIndex | SDNN Index | double ||
    | | rmssd | RMSSD | double ||
    | | pnn50 | PNN50 | double ||
    | | triangularIndex | 三角指数 | double||
    | | hf | HF | double||
    | | lf | LF | double ||
    | | vlf | VLF | double ||
    | afInfo  |  |  | | 房颤房扑事件|
    | | afBeatPercent | （房扑）房颤占比 | float ||
    | ventricularInfo  |  |  | | 室性节律|
    | 室性 | ventricularBeatCount | 室性心博总数 | int ||
    | | ventricularPermatureBeatCount | 室性早搏数量（室早单发） | int ||
    | | coupleVentricularPermatureCount | 室性早搏成对（成对室早） | int ||
    | | ventricularBigeminyCount | 室性二联律数量 | int ||
    | | ventricularTrigeminyCount | 室性三联律数量 | int ||
    | | ventricularTachycardiaCount | 室性心动过速数量（室速出现的阵数） | int ||
    | | longestVentricularTachycardiaDuration | 最长室性心动过速持续时间 | int ||
    | | longestVentricularTachycardiaOccurTime | 最长室性心动过速发生时间 | string ||
    | | longestVentricularTachycardiaCount | 最长室速心博数 | int | 签字报告|
    | | maxVentricularPermatureHeartRate | 室速最高心率 | int | 签字报告|
    | supraventricularInfo  |  |  | | 室上性节律|
    | 室上性 | atrialBeatCount | 室上性心博总数 | int ||
    | | atrialPermatureBeatCount | 室上性早搏数量（室上早单发） | int ||
    | | coupleAtrialPermatureCount | 室上性早搏成对（成对室上早） | int ||
    | | atrialBigeminyCount | 室上性二联律数量 | int ||
    | | atrialTrigeminyCount | 室上性三联律数量 | int ||
    | | atrialTachycardiaCount | 室上性心动过速数量（室上速出现的阵数） | int ||
    | | longestAtrialTachycardiaDuration | 最长室上性心动过速持续时间 | int ||
    | | longestAtrialTachycardiaOccurTime | 最长室上性心动过速发生时间 | string||
    | | longestAtrialTachycardiaCount | 最长室上速心博数 | int | 签字报告|
    | | maxAtrialPermatureHeartRate | 室上速最高心率 | int | 签字报告|
    | diagnose  |  |  | | 诊断结果|
    | 结论 | description | 描述结论 | string ||
    | | diagnoseInfo | 诊断性结论 | string ||
    | ecgHourEventList  |  |  | | （签字报告）小时统计列表，单个对象属性如下：|
    | 小时事件更表 | hourName | 时间 | string ||
    | | beatCount | 心博数 | int||
    | | minHeartRate | 最小 | int ||
    | | averageHeartRate | 平均 | int ||
    | | maxHeartRate | 最大 | int ||
    | | asystoleCount | 停博 | int ||
    | | ventricularBeatCount | 室性室早 | int ||
    | | coupleVentricularPermatureCount | 室性成对 | int ||
    | | ventricularTachycardiaCount | 室性室速 | int ||
    | | atrialBeatCount | 室上早 | int ||
    | | coupleAtrialPermatureCount | 室上性成对 | int ||
    | | atrialTachycardiaCount | 室上速 | int ||
    | eventList  |  |  | | 事件片段列表，单个对象属性如下：|
    | 事件列表 | eventCode | 事件编码 | string ||
    | | eventName | 事件描述 | string ||
    | | startPos | 开始位置 | long ||
    | | endPos | 结束位置 | long ||
    | | eventTime | 事件时间| string |会受measure_time格式影响|
    | | hr | 心率  | int ||
    | | leadNameList | 要打印展示的导联列表 | List\<String\> | 空的话是打印所有导联|
    | hrvList  |   | （签字报告）心率变异小时统计列表，单个对象属性如下：|||
    | 心率变异列表 | tBeat | T.Beat | int ||
    | | qBeat | Q.Beat | int ||
    | | hr | HR | int ||
    | | rr | RR | int ||
    | | sdnn | SDNN | int ||
    | | sdann | SDANN | int ||
    | | sdnnIndex | SDNN* | int ||
    | | rmssd | rMSSD | int ||
    | | pnn50 | PNN50 | int ||
    | | ulf | ULF | int ||
    | | vlf | VLF | int ||
    | | lf | LF | int ||
    | | hf | HF | int ||
    | imgList |  | | | （签字报告）直方图、散点图数据列表|
    | 直方图 | imgCode | 图片编码 | string | "1-NN间期直方图, 2-NN间期差值直方图, 3-NN间期散点图, 4-RR间期散点图" |
    | | imgData | 图片base64编码值 | sting ||
    | imgSummary |  | | | （签字报告）直方图、散点图汇总数据|
    | 图片描述 | maxRR | Max RR | double ||
    | | minRR | Min RR | double ||
    | | meanRR | Mean RR | double ||
    | | sdnn | SDNN | double ||
    | | triangularIndex | Triangular Index | double ||
    | | nn50 | NN50 | double ||
    | | pnn50 | PNN50 | double ||
    | | sdann | SDANN | double ||
    | | sdnnIndex | SDNN Index | double ||
    | | time | Time | string ||
    | | totalNumber | Total Number | int ||
    | | qualifiedNumber | Qualified Number | double ||
    | | qualified | Qualified | double ||
    | stList |  | | | ST(mv)数据列表，单个对象的属性如下：|
    | | leadName | 导联名称 | string ||
    | | leadData | st数据 | List\<Float\> ||
    | diagnoseList |  | | List\<Object\> | ai诊断内容列表（人工诊断内容可能与上述diagnose诊断结果不同）|
    | | code | 诊断code | string ||
    | | diagnoseInfo | 诊断信息 | string ||

### 静态心电

1. ecgInfo

```jsonc
   {
    "ecg": {
        "duration": 370, //分析时长，单位s
        "sample_rate": 125, //采样率，如125
        "measure_time": "2022-02-03 13:29:55" //测量时间(YYYY-MM-DD HH:mm:ss格式，如2022-02-04 13:29:55),
    },
    "user": {
        "name": "test1", //姓名，必填
        "phone": "", //手机号，必填
        "gender": "1", //性别（1：男，2：女）,对于签字报告必填
        "birthday": "1991-08-13",//出生日期（yyyy-mm-dd）,对于签字报告必填
        "id_number": "" //身份证号，对于签字报告必填
    },
    "device": {
        "sn": "236xxxx", //设备sn
        "band": "Lepu", //品牌
        "model": "PC-700" //型号，从下列设备列表中查找对应正确的model(区分大小写)
    },
    "access_token": "", //对应服务能力的AccessToken
    "analysis_type": "1-12", //分析类型，“-”前表示1-短程，“-”后表示导联类型,12-十二导
    "application_id": "xxx.xxx.xx", //应用id(应用包名,注意格式)
    "service_ability": 1 // 1 AI分析， 2 医生签字报告
   }
```

2. 设备列表

   | band | model    |采样率| 备注               |
   | ---- | ---------|-----| ----------------   |  
   | Lepu | PC-700   |  1000hz   | PC700一体机,12导        |
   | Lepu | PCECG-500|  1000hz   | PCECG-500一体机,12导 |


4. 分析结果描述

   |                     |     |中文说明   | 类型 |备注  |
   | --------------------|---- |----------|-----|-----|
   | age                 | |年龄 |string||
   | pid                 | |病人编号        |string||
   | gender              | |性别编码        |string||
   | genderText          | | 性别名称 |string ||
   | checkNo             | | 检查号        |string||
   | testTime            | | 检查时间|string|utc时间，需要加本地时区得到本地时间|
   | warnList            | | 危急值列表 |object []||
   | itemCount           | |  |int||
   | patientId           | | 病人id |string||
   | diagnosedBy         | |                |string||
   | featureData         | | 特征波形数据 |object||
   |                     |scale     |增益 |float||
   |                     |filter    |滤波参数 |int||
   |                     |sample    |采样率 |int||
   |                     |dataLen   |数据长度 |int||
   |                     |leadNum   |导联个数 |int||
   |                     |testTime  |采集时间 |string||
   |                     |leadDatas |导联数据 |object []||
   | patientName         | |患者姓名 |string||
   | measurements        | | 平均测量值|object||
   | featurePoints       | |特征值 |object||
   | diagnosticList      | |诊断列表 |object [] ||
   | testOfficeName      | |检查科室 |string||
   | diagnoseContent     | |诊断结果 |string||
   | diagnoseBeginTime   | | 诊断开始时间|string|utc时间，需要加本地时区得到本地时间|
   | diagnoseEndTime     | | 诊断结束时间|string|utc时间，需要加本地时区得到本地时间|
   | diagnosedByName     | | 诊断人名称 |string||
   | leadMeasurements    | | 各个导联的测量值 |object []||
   | diagnosedSignature  | |诊断签名 |string||
   | diagnoseFeatureDesc | |诊断特征描述 |string||

### 分析状态

 80002状态是一个中间过渡状态，其他的皆为最终状态。  

   | 码值  | 状态             |
   | ----- | ---------------- |
   | 80000 | ai分析未返回状态 |
   | 80001 | 分析完成         |
   | 80002 | 分析中           |
   | 80003 | 分析异常         |
   | 80004 | 等待上传分析文件 |
   | 80006 | 其他异常        |
