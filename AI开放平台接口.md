# AI开放平台接口文档

**本文档仅提供给服务对接方使用，未经允许不得对外发布。**

## 版本修订记录
| 时间 | 版本 | 修改人 | 修改说明 |
| ---- | ---- | ---- | ---- |
| 2021年12月15日 | V0.1 | 王江 | 初次发布 |


## 1. 请求信息
1. 接口地址
   测试Host：118.25.61.2 .36   Secret：

2. 接口基本定义
   本文档中的业务接口的入（出）参均是指业务参数，调用接口时必须添加上公共请求头，解析响应时，必须按照公共出参解析

3. 字符编码
   UTF-8

4. 公共请求头
   | 参数名 | 是否必须 | 类型（长度）| 说明 |
   | ---- | ---- |---- | ---- |

5. 公共出参
   | 参数名 | 是否必须 | 类型（长度）| 说明 |
   | ---- | ---- |---- | ---- |
   | code | 是 | int | 响应码。1：成功，其他：异常 |
   | message | 是 | string | 响应描述（常用于出错后的提示）|
   | reason | 是 | string | 错误详情（主要为出错后的堆栈信息）|
   | data | 是 | 无限制 | 响应内容（请参考具体业务接口的出参）|

   
6. 签名算法
## 2. 接口信息
### 2.1 请求AI分析
 **接口地址**
`${host}/api/v1/ecg/analysis/request`

 **请求方式**
`POST`

 **请求数据类型**
`multipart/form-data`

 **请求体**
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | ecgFile | 否 | .txt/.dat | 原始文件，可不传，参考AI分析接口ECG文件协议 |
 | analyseFile | 是 | .txt/.dat | 原始文件，可不传，参考AI分析接口ECG文件协议 |
 | ecgInfo | 是 | string | JSON字符串，见附录 ecgInfo |

 **响应参数**
| 参数名 | 是否必须 | 类型（长度）| 说明 |
| ---- | ---- |---- | ---- |
| analysisId | 是 | string | 分析ID，用于请求分析结果|


### 2.2 获取AI分析结果
 接口地址
`${host}/api/v1/ecg/analysis/getResults`

 请求方式
`POST`

 请求数据类型
`json`

 请求体
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | analysisId | 是 | string | 分析ID|

 响应参数



### 2.3 获取AI分析报告
 接口地址
`${host}/api/v1/ecg/analysis/getReport`

 请求方式
`json`

 请求数据类型
`multipart/form-data`

 请求体
 | 参数名 | 是否必须 | 类型（长度）| 说明 |
 | ---- | ---- |---- | ---- |
 | analysisId | 是 | string | 分析ID|

 响应参数


## 附录
1. ecginfo
   ```jsonc
   {
    "user": { // 用户信息，目前全部为可选
        "name": "Xiao Ming", 
        "gender": "0", // 性别（0：默认；1：男；2：女）
        "birthday": "1991-08-13", // 生日（yyyy-MM-dd）
        "idNumber": "44301xxx", // 身份证号码,非必填
        "phone": "18888555000" // 非必填
        },
    "analysisType": "1", // 1：AI分析；2：医生签字报告
    "accessToken": "shaxxxx", // 对应服务能力的AccessToken
    "applicationId": "com.lepu.lepucare", // 应用id
    "device": {
        "model": "er1",
        "band": "Lepu",
        "sn": "20001001200"
        },
    "ecg": {
        "measureTime": "2021-09-07T10:25:41Z",
        "duration": "30", //单位s
        "sampleRate": "500",
        "lead": "I/II"
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
3. 错误码
   | 响应码 | 范围 | 说明 |
   | ---- | ---- | ---- |
   | 1 | 通用 | 请求成功 |