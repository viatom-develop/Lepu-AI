# AI开放平台接口文档

**本文档仅提供给服务对接方使用，未经允许不得对外发布。**

## 版本修订记录
| 时间           | 版本 | 修改人 | 修改说明 |
| -------------- | ---- | ------ | -------- |
| 2021年12月15日 | V0.1 | 王江   | 初次发布 |
| 2022年04月26日 | v0.2 | 王江   | 更新AI分析与签字报告返回JSON结构体 |
| 2022年04月27日 | v0.3 | 王登普   | 更新部分字段 |
| 2022年05月18日 | v0.5 | 王登普   | 更新部分字段描述 |


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
   | 参数名  | 是否必须 | 类型（长度） | 说明                                 |
   | ------- | -------- | ------------ | ------------------------------------ |
   | code    | 是       | int          | 响应码。0：成功，其他：异常          |
   | message | 是       | string       | 响应描述（常用于出错后的提示）       |
   | reason  | 是       | string       | 错误详情（主要为出错后的堆栈信息）   |
   | data    | 是       | 无限制       | 响应内容（请参考具体业务接口的出参） |

## 2. 接口信息
### 2.1 请求AI分析
 **接口地址**
`${host}/api/v1/ecg/analysis/request`

 **请求方式**
`POST`

 **请求数据类型（必须）**
`multipart/form-data`

 **请求体**
 | 参数名       | 是否必须 | 类型（长度） | 说明                                                                       |
 | ------------ | -------- | ------------ | -------------------------------------------------------------------------- |
 | ecg_file     | 否       | .txt/.dat    | 原始文件，可不传，参考AI分析接口ECG文件协议                                |
 | analyse_file | 是       | .txt         | 乐普标准单导ECG文件，优先使用此文件作为分析文件，参考AI分析接口ECG文件协议 |
 | ecg_info     | 是       | string       | JSON字符串，见附录 ecgInfo                                                 |

 **响应参数**
| 参数名     | 是否必须 | 类型（长度） | 说明                     |
| ---------- | -------- | ------------ | ------------------------ |
| analysis_id | 是       | string       | 分析ID，用于请求分析结果 |

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
 | 参数名     | 是否必须 | 类型（长度） | 说明                     |
 | ---------- | -------- | ------------ | ------------------------ |
 | analysis_id | 是       | string       | 分析ID，JSON编码字符串。 |

 **响应参数**
 | 参数名          | 是否必须 | 类型（长度） | 说明                         |
 | --------------- | -------- | ------------ | ---------------------------- |
 | analysis_id     | 是       | string       | 分析ID，用于请求分析结果      |
 | analysis_status | 是       | string       | 分析状态                   |
 | analysis_result | 是       | string       | AI分析结果，具体字段见附录    |
 | report_url      | 是       | string       | 分析报告，比AI分析结果有延迟  |

- AI 分析结果
```json
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
                "startTime": "2022-04-20 15:20:27",
                "endTime": "2022-04-20 15:21:06"
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
            "eventList": [{
                "eventCode": "13000",
                "eventName": "最大心率",
                "startPos": "5805",
                "endPos": "7805",
                "eventTime": "2022-04-20 15:20:50",
                "hr": 87,
                "leadNameList": [
                    "II"
                ]
            }, {
                "eventCode": "13001",
                "eventName": "最小心率",
                "startPos": "0",
                "endPos": "2000",
                "eventTime": "2022-04-20 15:20:27",
                "hr": 82,
                "leadNameList": [
                    "II"
                ]
            }],
            "hrvList": null,
            "imgList": null,
            "imgSummary": null,
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
- 签字报告结果
```json
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": {
        "analysis_id": "Mg==|ODU=",
        "analysis_status": "10010391",
        "analysis_result": {
            "dataStartTime": "2022-04-20 15:22:07",
            "dataEndTime": "2022-04-20 15:22:44",
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


### 2.3 批量查询AI分析状态
 **接口地址**
`${host}/api/v1/ecg/analysis/batchGetStatus`

 **请求方式**
`POST`

 **请求数据类型（必须）**
`application/json`

 **请求体**
 | 参数名     | 是否必须 | 类型（长度） | 说明                   |
 | ---------- | -------- | ------------ | ---------------------- |
 | analysis_ids | 是       | array       | 分析ID数组 |

例：
```json
{
	"analysis_ids": [
		"OA==|MTU3",
		"Ng==|OTk5"
	]
}
```
 **响应参数**
 | 参数名 | 是否必须 | 类型（长度） | 说明         |
 | ------ | -------- | ------------ | ------------ |
 |        | 是       | array         | 分析状态列表 |

例：
```json
{
    "code": 0,
    "message": "请求成功",
    "reason": "",
    "data": [
        {
            "analysis_id": "Mg==|NjY=",
            "analysis_status": "80001"
        },
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

## 附录
1. ecginfo
   ```jsonc
   {
        "user": {
            "name": "Xiao Ming",    // 必填
	    "phone": "18888555000", // 必填
            "gender": "0",          // 性别（0：默认；1：男；2：女），对于签字报告必填
            "birthday": "1991-08-13", // 生日（yyyy-MM-dd），对于签字报告必填
            "id_number": "44301xxx",  // 身份证号码，对于签字报告必填
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
            "duration": "30", //分析数据时长，单位s, 不少于30，>=900为长程，否则为短程
            "sample_rate": "500",
            "lead": "II" // I导联/II导联
        }
    }

   ```
2. 设备列表
   | band | model | 备注             |
   | ---- | ----- | ---------------- |
   | Lepu | er1   | 心安宝ER1        |
   | Lepu | er2   | 心小秘心电记录仪 |
   | Lepu | s1    | 心电体脂秤       |
   | Lepu | bp2   | 心电血压计       |
   | Lepu | w1    | 乐普手表w1       |
   | Lepu | pc700 | PC700一体机      |
3. AI分析状态
   | 码值  | 状态             |
   | ----- | ---------------- |
   | 80000 | ai分析未返回状态 |
   | 80001 | 分析完成         |
   | 80002 | 分析中           |
   | 80003 | 分析异常         |
   | 80004 | 等待上传分析文件 |
4. AI分析结果详情

| |  key |	中文说明 |	类型 |	备注 |	
| ---- | ---- |	---- |	---- | ---- |	
| 报告返回对象  |	  |	 |
| baseInfo|  |	|  | 基本信息 |
| |	testId |	检查id |	string |	用于消费者端截取片段数据|
| |	sample |	采样率 |	int |	用于消费者端截取片段数据|
| |	analysisTime |	分析时间 |	string |
| |	analysisDuration |	分析时长 |	int |	单位秒|
| |	name |	姓名 |	string |	这些基本信息，以消费者端的病人信息为准|
| |	sex |	性别 |	string |
| |	age |	年龄 |	int |
| |	createTime |	记录日期 |	string |
| |	startTime |	数据开始时间 |	string |
| |	endTime |	数据结束时间 |	string |
| hrInfo  |	  |	心率统计|
| |	beatCount |	心博总数 |	int |
| |	averageHeartRate |	平均心率 |	int |
| |	maxHeartRate |	最快（大）心率 |	int |
| |	minHeartRate |	最慢（小）心率 |	int |
| |	longRRPeriodAt |	最长RR间期发生时间 |	string |
| |	longRRPeriodCount |	长RR（间期）数量 |	int |
| |	maxLongRRPeriod |	最长RR间期时长 |	int |
| |	asystoleRRPeriodCount |	停搏数量 |	int |
| |	abnormalBeatCount |	异常心博数 |	int |	签字报告|
| |	ventricularBeatCount |	室性心博 |	int |	签字报告|
| |	atrialBeatCount |	室上性心博 |	int |	签字报告|
| hrvInfo  |  |  |	| 心率变异统计|
| 心率变异 |	sdnn |	SDNN |	double |
| |	sdnnIndex |	SDNN Index |	double |
| |	rmssd |	RMSSD |	double |
| |	pnn50 |	PNN50 |	double |
| |	triangularIndex |	三角指数 |	double |
| |	hf |	HF |	double |
| |	lf |	LF |	double |
| |	vlf |	VLF |	double |
| afInfo  |  |  |	| 房颤房扑事件|
| |	afBeatPercent |	（房扑）房颤占比 |	float |
| ventricularInfo  |  |  |	| 室性节律|
| 室性 |	ventricularBeatCount |	室性心博总数 |	int |
| |	ventricularPermatureBeatCount |	室性早搏数量（室早单发） |	int |
| |	coupleVentricularPermatureCount |	室性早搏成对（成对室早） |	int |
| |	ventricularBigeminyCount |	室性二联律数量 |	int |
| |	ventricularTrigeminyCount |	室性三联律数量 |	int |
| |	ventricularTachycardiaCount |	室性心动过速数量（室速出现的阵数） |	int |
| |	longestVentricularTachycardiaDuration |	最长室性心动过速持续时间 |	int |
| |	longestVentricularTachycardiaOccurTime |	最长室性心动过速发生时间 |	string |
| |	longestVentricularTachycardiaCount |	最长室速心博数 |	int |	签字报告|
| |	maxVentricularPermatureHeartRate |	室速最高心率 |	int |	签字报告|
| supraventricularInfo  |  |  |	| 室上性节律|
| 室上性 |	atrialBeatCount |	室上性心博总数 |	int |
| |	atrialPermatureBeatCount |	室上性早搏数量（室上早单发） |	int |
| |	coupleAtrialPermatureCount |	室上性早搏成对（成对室上早） |	int |
| |	atrialBigeminyCount |	室上性二联律数量 |	int |
| |	atrialTrigeminyCount |	室上性三联律数量 |	int |
| |	atrialTachycardiaCount |	室上性心动过速数量（室上速出现的阵数） |	int |
| |	longestAtrialTachycardiaDuration |	最长室上性心动过速持续时间 |	int |
| |	longestAtrialTachycardiaOccurTime |	最长室上性心动过速发生时间 |	string |
| |	longestAtrialTachycardiaCount |	最长室上速心博数 |	int |	签字报告|
| |	maxAtrialPermatureHeartRate |	室上速最高心率 |	int |	签字报告|
| diagnose  |  |  |	| 诊断结果|
| 结论 |	description |	描述结论 |	string |
| |	diagnoseInfo |	诊断性结论 |	string |
| ecgHourEventList  |  |  |	| （签字报告）小时统计列表，单个对象属性如下：|
| 小时事件更表 |	hourName |	时间 |	string |
| |	beatCount |	心博数 |	int |
| |	minHeartRate |	最小 |	int |
| |	averageHeartRate |	平均 |	int |
| |	maxHeartRate |	最大 |	int |
| |	asystoleCount |	停博 |	int |
| |	ventricularBeatCount |	室性室早 |	int |
| |	coupleVentricularPermatureCount |	室性成对 |	int |
| |	ventricularTachycardiaCount |	室性室速 |	int |
| |	atrialBeatCount |	室上早 |	int |
| |	coupleAtrialPermatureCount |	室上性成对 |	int |
| |	atrialTachycardiaCount |	室上速 |	int |
| eventList  |  |  |	| 事件片段列表，单个对象属性如下：|
| 事件列表 |	eventCode |	事件编码 |	string |
| |	eventName |	事件描述 |	string |
| |	startPos |	开始位置 |	long |
| |	endPos |	结束位置 |	long |
| |	eventTime |	事件时间 |	string |
| |	hr |	心率  |	int |
| |	leadNameList |	要打印展示的导联列表 |	List\<String\> |	空的话是打印所有导联|
| hrvList  |	  |	（签字报告）心率变异小时统计列表，单个对象属性如下：|
| 心率变异列表 |	tBeat |	T.Beat |	int |
| |	qBeat |	Q.Beat |	int |
| |	hr |	HR |	int |
| |	rr |	RR |	int |
| |	sdnn |	SDNN |	int |
| |	sdann |	SDANN |	int |
| |	sdnnIndex |	SDNN* |	int |
| |	rmssd |	rMSSD |	int |
| |	pnn50 |	PNN50 |	int |
| |	ulf |	ULF |	int |
| |	vlf |	VLF |	int |
| |	lf |	LF |	int |
| |	hf |	HF |	int |
| imgList |  | | | （签字报告）直方图、散点图数据列表|
| 直方图 |	imgCode |	图片编码 |	string | "1-NN间期直方图, 2-NN间期差值直方图, 3-NN间期散点图, 4-RR间期散点图" |
| |	imgData |	图片base64编码值 |	sting |
| imgSummary |  | | | （签字报告）直方图、散点图汇总数据|
| 图片描述 |	maxRR |	Max RR |	double |
| |	minRR |	Min RR |	double |
| |	meanRR |	Mean RR |	double |
| |	sdnn |	SDNN |	double |
| |	triangularIndex |	Triangular Index |	double |
| |	nn50 |	NN50 |	double |
| |	pnn50 |	PNN50 |	double |
| |	sdann |	SDANN |	double |
| |	sdnnIndex |	SDNN Index |	double |
| |	time |	Time |	string |
| |	totalNumber |	Total Number |	int |
| |	qualifiedNumber |	Qualified Number |	double |
| |	qualified |	Qualified |	double |
| stList |  | | | ST(mv)数据列表，单个对象的属性如下：|
| |	leadName |	导联名称 |	string |
| |	leadData |	st数据 |	List\<Float\> |
| diagnoseList |  | | list\<Object\> | ai诊断内容列表（人工诊断内容可能与上述diagnose诊断结果不同）|
| |	code |	诊断code |	string |
| |	diagnoseInfo |	诊断信息 |	string |
