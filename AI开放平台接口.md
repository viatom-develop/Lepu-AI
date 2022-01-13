# AI开放平台接口文档

**本文档仅提供给服务对接方使用，未经允许不得对外发布。**

## 版本修订记录
| 时间 | 版本 | 修改人 | 修改说明 |
| ---- | ---- | ---- | ---- |
| 2021年12月15日 | V0.1 | 王江 | 初次发布 |


## 1. 请求信息
1. 接口地址
   测试Host: https://open.viatomtech.com.cn

2. 接口基本定义
   本文档中的业务接口的入（出）参均是指业务参数，调用接口时Headers必须添加上公共请求头，解析响应时，必须按照公共出参解析

3. 字符编码
   UTF-8

4. 公共请求头 - Header
   | 参数名 | 是否必须 | 类型（长度）| 说明 |
   | ---- | ---- |---- | ---- |
   | secret | 是 | string | 乐普给出 |
   | access-token | 是 | string | 乐普给出, 不同业务使用不同的access_token |
   | language | 是 | string | zh-CN  / en-US |

5. 公共出参
   | 参数名 | 是否必须 | 类型（长度）| 说明 |
   | ---- | ---- |---- | ---- |
   | code | 是 | int | 响应码。1：成功，其他：异常 |
   | message | 是 | string | 响应描述（常用于出错后的提示）|
   | reason | 是 | string | 错误详情（主要为出错后的堆栈信息）|
   | data | 是 | 无限制 | 响应内容（请参考具体业务接口的出参）|

## 2. 接口信息
### 2.1 请求AI分析
 **接口地址**
`${host}/api/v1/ecg/analysis/request`

 **请求方式**
`POST`

 **请求数据类型（必须）**
`multipart/form-data`

 **请求体**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | ecg_file | 否 | .txt/.dat | 原始文件，可不传，参考AI分析接口ECG文件协议 |
 | analyse_file | 是 | .txt | 乐普标准单导ECG文件，优先使用此文件作为分析文件，参考AI分析接口ECG文件协议 |
 | ecg_info | 是 | string | JSON字符串，见附录 ecgInfo |

 **响应参数**
| 参数名 | 是否必须 | 类型（长度）| 说明 |
| ---- | ---- |---- | ---- |
| analysisId | 是 | string | 分析ID，用于请求分析结果|
例：
```json
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": {
        "analysis_id": "Mg==|OTA="
    }
}
```


### 2.2 获取AI分析结果
 **接口地址**
`${host}/api/v1/ecg/analysis/getResults`

 **请求方式**
`POST`

 **请求数据类型（必须）**
`application/json`

 **请求体**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | analysisId | 是 | string | 分析ID，JSON编码字符串。 |

 **响应参数**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | analysisId | 是 | string | 分析ID，用于请求分析结果|
 | analysis_result | 是 | string | AI分析结果，具体字段见附录 |
 | report_url | 是 | string | 分析报告，比AI分析结果有延迟 |

```json
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": {
        "analysis_id": "Mg==|ODU=",
        "analysis_status": "10010391",
        "analysis_result": {
            "dataStartTime": "2021-11-19 10:41:24",
            "dataEndTime": "2021-11-19 10:44:35",
            "reportTime": "2021-12-27 17:26:42",
            "totalDuration": 191,
            "validDuration": 136,
            "beatCount": "159",
            "abnormalBeatCount": "0",
            "abnormalBeatPercent": 0,
            "afBeatPercent": 0,
            "maxHeartRate": 73,
            "maxHeartRateTime": "2021-11-19 10:42:18",
            "minHeartRate": 57,
            "minHeartRateTime": "2021-11-19 10:42:36",
            "averageHeartRate": 66,
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
            "diagnoseList": [
                {
                    "code": "101",
                    "diagnoseInfo": "窦性心律"
                }
            ],
            "eventList": [
                {
                    "eventCode": "13000",
                    "eventName": "最大心率",
                    "startPos": "13201",
                    "endPos": "15201",
                    "eventTime": "2021-11-19 10:42:16"
                },
                {
                    "eventCode": "13001",
                    "eventName": "最小心率",
                    "startPos": "17625",
                    "endPos": "19625",
                    "eventTime": "2021-11-19 10:42:34"
                }
            ],
            "longRRPeriodCount": 0
        },
        "report_url": ""
    }
}
```



### 2.3 批量查询AI分析状态
 **接口地址**
`${host}/api/v1/ecg/analysis/batchGetStatus`

 **请求方式**
`POST`

 **请求数据类型（必须）**
`application/json`

 **请求体**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | analysisId | 是 | string | 分析ID，JSON编码字符串 |

 **响应参数**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 |  | 是 | list | 分析状态列表 |
例：
```json
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": [
        {
            "analysis_id": "Mg==|NjY=",
            "analysis_status": "10010391"
        },
        {
            "analysis_id": "Mg==|Njg=",
            "analysis_status": null
        },
        {
            "analysis_id": "Mg==|OTA=",
            "analysis_status": "10010391"
        }
    ]
}
```

## 附录
1. ecginfo
   ```jsonc
   {
    "user": { // 用户信息，目前全部为可选
        "name": "Xiao Ming", 
        "gender": "0", // 性别（0：默认；1：男；2：女）
        "birthday": "1991-08-13", // 生日（yyyy-MM-dd）
        "id_number": "44301xxx", // 身份证号码,非必填。对于签字报告必填
        "phone": "18888555000" // 非必填
        },
    "analysis_type": "1", // 1短程，2长程
    "service_ability": "1", // 1 AI分析， 2 医生签字报告
    "access_token": "shaxxxx", // 对应服务能力的AccessToken
    "application_id": "com.lepu.lepucare", // 应用id
    "device": {
        "model": "er1",
        "band": "Lepu",
        "sn": "20001001200"
        },
    "ecg": {
        "measure_time": "2021-09-07T10:25:41Z",
        "duration": "30", //单位s
        "sample_rate": "500",
        "lead": "II" // I导联/II导联
        }
    }

   ```
2. 设备列表
   | band | model | 备注 |
   | ---- | ---- | ---- |
   | Lepu | er1 | 心安宝ER1 |
   | Lepu | er2 | 心小秘心电记录仪 |
   | Lepu | s1 | 心电体脂秤 |
   | Lepu | bp2 | 心电血压计 |
   | Lepu | w1 | 乐普手表w1 |
   | Lepu | pc700 | PC700一体机 |
3. AI分析状态
   | 码值 | 状态 |
   | ---- | ---- |
   | 80000 | ai分析未返回状态 |
   | 80001 | 分析完成 |
   | 80002 | 分析中 |
   | 80003 | 分析异常 |
   | 80004 | 等待上传分析文件 |
4. AI分析结果详情
   | 参数名 | 必须 | 类型（长度）| 说明 |
   | ---- | ---- | ---- | ---- |
   | dataStartTime * | 是 | string($date-time) | 数据开始时间 |
   | dataEndTime * | 是 | string($date-time) | 数据结束时间 |
   | reportTime * | 是 | string($date-time) | 报告时间 |
   | totalDuration * | 是 | integer($int32) | 总时长 单位秒 |
   | validDuration * | 是 | integer($int32) | 有效时长 单位秒 |
   | beatCount* | 是 | integer($int32) | 心博总数 |
   | abnormalBeatPercent* | 是 | integer($int32) | 异常心博占比 百分比 |
   | afBeatPercent* | 是 | integer($int32) | 房颤房颤占比  |
   | maxHeartRate * | 是 | integer($int32) | 最大心率 |
   | maxHeartRateTime * | 是 | string($date-time) | 最大心率发生时间 |
   | minHeartRate * | 是 | integer($int32) | 最小心率 |
   | minHeartRateTime * | 是 | string($date-time) | 最小心率发生时间 |
   | averageHeartRate * | 是 | integer($int32) | 平均心率 |
   | maxLongRRPeriod * | 是 | number($float) | 最长RR间期时长 |
   | longRRPeriodCount * | 是 | integer($int32) | 长RR间期数量 |
   | maxLongAsystoleTime * | 是 | string($date-time) | 最长停搏发生时间 |
   | asystoleRRPeriodCount | 是 | integer($int32) | 停搏数量 |
   | atrialBeatCount | 是 | integer($int32) | 室上性心博数量 |
   | atrialPermatureBeatCount | 是 | integer($int32) | 室上性早搏数量 |
   | coupleAtrialPermatureCount* | 是 | integer($int32) | 室上性早搏成对 |
   | atrialBigeminyCount | 是 | integer($int32) | 室上性二联律 |
   | atrialTrigeminyCount | 是 | integer($int32) | 室上性三联律 |
   | atrialTachycardiaCount * | 是 | integer($int32) | 室上性心动过速数量 |
   | longestAtrialTachycardiaDuration * | 是 | number($float) | 最长室上性心动过速持续时间 |
   | longestAtrialTachycardiaOccurTime * | 是 | string($date-time) | 最长室上性心动过速发生时间 |
   | ventricularBeatCount * | 是 | integer($int32) | 室性心博总数 |
   | ventricularPermatureBeatCount* | 是 | integer($int32) | 室性早搏数量 |
   | coupleVentricularPermatureCount * | 是 | integer($int32) | 室性早搏成对 |
   | ventricularBigeminyCount * | 是 | integer($int32) | 室性二联律数量 |
   | ventricularTrigeminyCount * | 是 | integer($int32) | 室性三联律数量 |
   | ventricularTachycardiaCount * | 是 | integer($int32) | 室性心动过速数量 |
   | longestVentricularTachycardiaDuration* | 是 | integer($int32) | 最长室性心动过速持续时间 |
   | longestVentricularTachycardiaOccurTime |  是 | integer($int32)  | 最长室性心动过速发生时间 |
   | diagnoseList * | 是 | [...] | 诊断内容列表 |
   | | code* | string | 诊断 code |
   | | diagnoseInfo* | string | 诊断信息 |
   | eventList* | 是 | [...] | 事件片段 |
   | | endPos* | integer($int64) | 结束位置 |
   | | eventCode* | string | 片段Code |
   | | eventName* | string | 描述 |
   | | startPos* | integer($int64) | 开始位置 |
   | | eventTime* | String | 事件时间 |
   | allEventList | 是 | [...] | 事件列表|
   | | endPos* | integer($int64) | 结束位置|
   | | eventCode* | string | 片段Code|
   | | eventName* | string | 描述|
   | | startPos* | integer($int64) | 开始位置|
   | | eventTime* | String | 事件时间|