
# 1.Rest API
## 访问地址：http://api.goodvalue.one


## 1-1 行情API
[1-1.1 GET /data/v2/ticker] 行情
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|currency|平台可用交易对|

currency：

###### 代码示例
```
//# Request
    GET http://api.goodvalue.one/data/v2/ticker?currency=etc_btc
    //# Response
    {
        "ticker": {
            "high": "0.033153",
            "low": "0.030352",
            "buy": "0.030728",
            "sell": "0.030751",
            "last": "0.030729",
            "vol": "838.98"
        }
    }
```
###### 返回说明

```
    high : 最高价
    low : 最低价
    buy : 买一价
    sell : 卖一价
    last : 最新成交价
    vol : 成交量(最近的24小时)                                    
```

[1-1.2 GET /data/v2/depth] 深度
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|currency平台可用交易对|
|size|档位1-50，如果有合并深度，只能返回5档深度|
|merge|合并深度|

###### 代码示例
```
//# Request
    GET http://api.goodvalue.one/data/v2/depth?currency=btm_cnyt&size=3&merge=0.1
    //# Response
    {
        "asks": [
            [
                83.28,
                11.8
            ]...
        ],
        "bids": [
            [
                81.91,
                3.65
            ]...
        ],
        "timestamp" : 时间戳
    }
                       
```
###### 返回说明
```
asks : 卖方深度
    bids : 买方深度
    timestamp : 此次深度的产生时间戳
```
[1-1.3 GET /data/v2/trades]() 历史成交
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|currency|平台可用交易对|
###### 代码示例
```
//# Request
    GET http://api.goodvalue.one/data/v2/trades?currency=btm_cnyt
    //# Response
    [
        {
            "amount": 0.541,
            "date": 1472711925,
            "price": 81.87,
            "tid": 16497097,
            "trade_type": "ask",
            "type": "sell"
        }...
    ]
```
###### 返回说明
```
date : 交易时间(时间戳)
    price : 交易价格
    amount : 交易数量
    tid : 交易生成ID
    type : 交易类型，buy(买)/sell(卖)
    trade_type : 委托类型，ask(卖)/bid(买)
```

[1-1.4 GET /data/v2/kline]() k线
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|currency|平台可用交易对|
|size|返回数据的条数限制(默认为1000，如果返回数据多于1000条，那么只返回1000条)|
|type|描述如下（type）|
type:
```
1 : 1分钟
3 : 3分钟
5 : 5分钟
15 : 15分钟
30 : 30分钟
1440 : 1日
10080 : 1周
60 : 1小时
120 : 2小时
240 : 4小时
360 : 6小时
720 : 12小时
```
###### 代码示例
```
//# Request
    GET http://api.goodvalue.one/data/v2/kline?currency=btc_cnyt
    //# Response
    {
    "data": [
        [
            1472107500000,
            3840.46,
            3843.56,
            3839.58,
            3843.3,
            492.456
        ]...
    ]
    }
```
###### 返回说明
```
data : K线内容
    data : 内容说明
    [
    1417536000000, 时间戳
    2370.16, 开
    2380, 高
    2352, 低
    2367.37, 收
    17259.83 交易量
    ]
```
## 1-2 交易API
**请求参数说明**(加密签名请根据这个顺序签名,sign和reqTime不用加入待签名字符串)

[1-2.1 GET /api/v2/order] 委托下单
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|method|直接赋值order|
|accesskey|accesskey|
|price|价格，保留小数位根据币种的不同而不同|
|amount|交易数量|
|tradeType|交易类型0/1[buy/sell]|
|currency|平台可用交易对|
|sign|请求加密签名串|
|reqTime|当前时间毫秒数|
###### 代码示例
```
//# Request
    GET  http://api.goodvalue.one/api/v2/order?method=order
        &accesskey=accesskey&price=1024&amount=1.5&tradeType=1&currency=btc_cnyt
        &sign=请求加密签名串&reqTime=当前时间毫秒数
    //# Response
    {
        "code": "0000",
        "message": "操作成功。",
        "id": "20131228361867"
    }
```
###### 返回说明
```
code : 返回代码
    message : 提示信息
    id : 委托挂单号
```
[1-2.2 GET /api/v2/cancel]() 取消委托
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|method|直接赋值order|
|accesskey|accesskey|
|id|委托挂单号|
|currency|平台可用交易对|
|sign|请求加密签名串|
|reqTime|当前时间毫秒数|

###### 代码示例
```
//# Request
    GET  http://api.goodvalue.one/api/v2/cancel?method=cancel
        &accesskey=your_access_key&id=123456789&currency=btc_cnyt
        &sign=请求加密签名串&reqTime=当前时间毫秒数
    //# Response
    {
        "code": "0000",
        "message": "操作成功。"
    }
```
###### 返回说明
```
code : 返回代码
    message : 提示信息
```
[1-2.3 GET /api/v2/getOrder]() 获取委托买单或卖单
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|method|直接赋值getOrder|
|accesskey|accesskey|
|id|委托挂单号|
|currency|平台可用交易对|
|sign|请求加密签名串|
|reqTime|当前时间毫秒数|

###### 代码示例
```
//# Request
    GET  http://api.goodvalue.one/api/v2/getOrder?method=getOrder
        &accesskey=your_access_key&id=123456789&currency=btc_cnyt
        &sign=请求加密签名串&reqTime=当前时间毫秒数
    //# Response
    {
        "currency": "btc",
        "id": "20150928158614292",
        "price": 1560,
        "status": 3,
        "total_amount": 0.1,
        "trade_amount": 0,
        "trade_price" : 6000,
        "trade_date": 1443410396717,
        "trade_money": 0,
        "type": 0,
    }
```
###### 返回说明
```
    currency : 交易类型
    id : 委托挂单号
    price : 单价
    status : 挂单状态(1：待成交,2：待成交未交易部份,3：交易完成,5：已取消)
    total_amount : 挂单总数量
    trade_amount : 已成交数量
    trade_date : 委托时间
    trade_money : 已成交总金额
    trade_price : 成交均价
    type : 挂单类型 0/1[buy/sell]
```
[1-2.4.GET /api/v2/getOrders]() API获取多个委托买或卖单，每次请求返回PageSize小于100条记录
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|method|直接赋值getOrders|
|accesskey|accesskey|
|tradeType|交易类型0/1[buy/sell]|
|currency|平台可用交易对|
|pageIndex|当前页数|
|pageSize|每页数量|
|sign|请求加密签名串|
|reqTime|当前时间毫秒数|

###### 代码示例
```
//# Request
    GET  http://api.goodvalue.one/api/v2/getOrders?method=getOrders
        &accesskey=your_access_key&tradeType=1&currency=btc_cnyt&pageIndex=1
        &pageSize=20&sign=请求加密签名串&reqTime=当前时间毫秒数
    //# Response
    [
        {
            "currency": "btc",
            "id": "20150928158614292",
            "price": 1560,
            "status": 3,
            "total_amount": 0.1,
            "trade_amount": 0,
            "trade_price" : 6000,
            "trade_date": 1443410396717,
            "trade_money": 0,
            "type": 0
        }...
    ]
```
###### 返回说明
```
currency : 交易类型
    id : 委托挂单号
    price : 单价
    status : 挂单状态(1：待成交,2：待成交未交易部份,3：交易完成,5：已取消)
    total_amount : 挂单总数量
    trade_amount : 已成交数量
    trade_date : 委托时间
    trade_money : 已成交总金额
    trade_price : 成交均价
    type : 挂单类型 0/1[buy/sell]
```
[1-2.5 GET /api/v2/getAccountInfo]() 获取用户信息
###### 请求说明
| 参数名 | 描述 |   
| ------------ | :-----: | 
|method|getAccountInfo|
|accesskey|accesskey|
|sign|请求加密签名串|
|reqTime|当前时间毫秒数|

###### 代码示例
```
//# Request
    GET  http://api.goodvalue.one/api/v2/getUserAddress?method=getUserAddress
        &accesskey=your_access_key&currency=btc
        &sign=请求加密签名串&reqTime=当前时间毫秒数
    //# Response
    {
        "key": "0x0af7f36b8f09410f3df62c81e5846da673d4d9a9"
    }
```
###### 返回说明
```
key : 地址
```

## 1-3 错误代码

所有API方法调用在请求失败或遇到未知错误时会返回JSON错误对象。

| 参数名 | 描述 |   
| ---- |----| 
|0000|调用成功|
|1000|系统异常|
|2000|非法访问|
|2001|accessKey不能为空|
|2002|accessKey非法|
|2011|timestamp格式不正确|
|2012|timestamp与服务器时间相差太多|
|2021|sign不能为空|
|2022|签名不一致|
|2023|签名失败|
|2031|业务参数不能为空|
|3000|用户状态异常|
|3001|钱包余额不足|
|3100|不支持的交易|
|3101|当前时间段停止交易|
|3102|交易价格不在合理范围内|
|3103|交易金额不在合理范围内|
|3104|委单失败|
|3105|撤销委单失败|
|3106|撤销所有委单失败|
|3107|获取委单信息失败|
|3108|不支持的币种|
|3109|超过每日提现次数|
|3110|超过每日提现最大数量|
|3111|需绑定手机或者邮箱|
|3112|需要实名认证|
|3113|账号异常被冻结，如有疑问请联系客服|
|3114|用户禁止提现，如有疑问请联系客服|
|3115|平仓中|
|3116|提现小于最小值|
|3117|需要设置交易密码|
|3118|交易密码错误超过当天最大次数|
|3119|交易密码错误|
|3120|用户余额不足|
|3121|提现地址未验证|
|3122|请求频率太高,请稍后|
|3123|无对应地址|

## 1-4 示例代码
目前支持JAVA 版本。其他语言版本会相继支持。如果在使用过程中有任何问题请联系我们API技术QQ群： ，我们将在第一时间帮您解决技术问题。

签名方式： 先用sha加密secretkey，然后根据加密过的secretkey把请求的参数签名，请求参数按照上述接口参数列表排序加密，通过md5填充16位加密

```
//......
public void order() throws Exception {
    HashMap<String, Object> paramMap = new LinkedHashMap();
    paramMap.put("method", "order");
    paramMap.put("accesskey", accessKey);
    paramMap.put("price", "0.00005418");
    paramMap.put("amount", "1");
    paramMap.put("tradeType", "0");
    paramMap.put("currency", "btm_btc");
    signUp(paramMap);
    String result = restTemplate.getForObject(domainUrl + "order?method={method}" +
            "&accesskey={accesskey}&price={price}&amount={amount}&tradeType={tradeType}¤cy={currency}" +
            "&sign={sign}&reqTime={reqTime}", String.class, paramMap);
    System.out.println(result);
}
//......
           
```

## 1-5 文档下载
[下载DEMO以及开发文档](https://github.com/goodValueApi/exchange-api)

## 1-6 常见问题
### 访问限制
1.单个用户限制每1秒秒只能请求一次数据。
