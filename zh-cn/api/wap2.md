# 微信公众号支付 wap 收银台

对于具有一定开发能力的商户，可以通过对接Upay支付网关来支持WAP支付，让顾客可以直接在商户的网页上进行移动支付，从而拓展商户的线上业务渠道。

## 1. 业务场景

顾客在支付客户端（如微信）中访问商户的商城页面，选择商品后结账，触发支付控件（如点击支付按钮），支付客户端会弹出支付控件提示用户输入支付密码，用户输入密码后完成支付，商户页面提示交易进行中，在商户获取到支付结果后，在页面上展示支付结果和订单信息。

## 2. WAP接入须知

在对接WAP接口前，请联系收钱吧销售人员填写服务商入网申请资料和测试商户入网资料。入网资料中填写的微信相关参数必须与开发者使用的微信相关参数保持一致。

**激活接口用于通过终端激活码（code）来获取终端号（terminal\_sn）和终端密钥（terminal\_key），以用于调用wap支付接口时的签名。激活接口对于同一台终端，只需要调用一次，调用wap支付之前需要先进行激活获取terminal\_sn和terminal\_key。激活接口调用方法就是web api对接接口里的激活接口。**

## 网关地址

https://qr.hemayun.com/wapapi

## 请求方式

GET

### 请求参数

| 参数 | 参数名称 | 类型 | 是否必填 | 描述 | 范例 |
| --- | --- | --- | --- | --- | --- |
| terminal\_sn | 合码云终端ID | String\(32\) | Y | 合码云终端ID | "23420593829" |
| client\_sn | 商户系统订单号 | String\(32\) | Y | 必须在商户系统内唯一；且长度不超过32字节 | "18348290098298292838" |
| total\_amount | 交易总金额 | String\(10\) | Y | 以分为单位,不超过10位纯数字字符串,超过1亿元的收款请使用银行转账 | "1000" |
| subject | 交易概述 | String\(64\) | Y | 本次交易的概述 | "pizza" |
| payway | 支付方式 | String | N | 支付方式，目前支持的支付方式参照附录 《支付方式》。不传默认为微信 | 3 |
| operator | 门店操作员 | String\(32\) | Y | 发起本次交易的操作员 | "Obama" |
|  | description | 商品详情 | String\(256\) | N | 对商品或本次交易的描述 |
|  | longitude | 经度 | String | N | 经纬度必须同时出现 |
|  | latitude | 纬度 | String | N | 经纬度必须同时出现 |
| extended | 扩展参数集合 | String\(256\) | N | 合码云与特定第三方单独约定的参数集合,json格式，最多支持24个字段，每个字段key长度不超过64字节，value长度不超过256字节 | { "goods\_tag": "beijing"} |
| reflect | 反射参数 | String\(64\) | N | 任何调用者希望原样返回的信息 | { "tips" : "100"} |
| notify\_url | 服务器异步回调 url | String\(128\) | N | 支付回调的地址 | 例如：www.shouqianba.com 如果支付成功通知时间间隔为1s,5s,30s,600s |
| return\_url | 页面跳转同步通知页面路径 | String\(128\) | Y | 处理完请求后，当前页面自动跳转到商户网站里指定页面的http路径 | "[https://www.shouqianba.com](https://www.shouqianba.com)" |
|  | sign | 签名 | String\(32\) | Y | 签名，规则请参考附录 《签名规则》 |

### 页面同步跳转参数说明

商户的请求数据处理完成后，会将处理的结果通知给商户网站。这些处理结果数据就是页面跳转同步通知参数\(当消费者付完款之后，会从付款界面跳转回return\_url地址下，然后处理的数据也相应的同步到这个地址下\)。

| 参数 | 参数名称 | 类型 | 必填 | 描述 | 范例 |
| --- | --- | --- | --- | --- | --- |
| is\_success | 成功标识 | String\(1\) | Y | 表示接口调用是否成功，并不表明业务处理结果。 | T |
| error\_code | 错误码 | String | N | 仅 is\_success 为 F 时出现 | 见附录 《错误码》 |
| error\_message | 错误描述 | String | N | 仅 is\_success 为 F 时出现 | 见附录 《错误码》 |
| terminal\_sn | 合码云终端ID | String\(32\) | Y | 合码云终端ID | "23420593829" |
| sn | 合码云唯一订单号 | String\(16\) | Y | 合码云系统内部唯一订单号 | 7892259488292938 |
| trade\_no | 支付服务商订单号 | String\(64\) | Y | 支付通道交易凭证号 | 2013112011001004330000121536 |
| client\_sn | 商户系统订单号 | String\(32\) | Y | 必须在商户系统内唯一；且长度不超过32字节 | "18348290098298292838" |
| status | 支付状态 | String | N | 标志支付是否成功 | "SUCCESS" |
| result\_code | 业务错误码 | String\(64\) | N | status 为 FAIL 时出现 | 见附录 《错误码》 |
| result\_message | 业务错误描述 | String | N | status 为 FAIL 时出现 | 见附录 《错误码》 |
| total\_amount | 交易总金额 | String\(10\) | Y | 以分为单位,不超过10位纯数字字符串,超过1亿元的收款请使用银行转账 | "1000" |
| subject | 交易概述 | String\(64\) | Y | 本次交易的概述 | "pizza" |
| operator | 门店操作员 | String\(32\) | Y | s发起本次交易的操作员 | "Obama" |
| reflect | 反射参数 | String\(64\) | N | 任何调用者希望原样返回的信息 | { "tips" : "100"} |
|  | sign | 签名 | String\(32\) | Y | 签名，规则请参考附录 《签名规则》 |

**商户务必以查单或者服务器异步通知的订单结果为主作出正确的处理。return\_url的结果仅供参考。查询订单请使用轮询方式获取为主，具体的轮询方式请使用web api接口里查询接口，轮询的时间控制在100-120s之间，轮询的间隔建议为前30秒内2秒一次，之后5秒一次**

# 附录

## 支付方式

| 取值 | 含义 |
| --- | --- |
| 3 | 微信 |

## 错误码

| error\_code | error\_message |
| --- | --- |
| INVALID\_PARAMS | 参数错误 |
| INVALID\_TERMINAL | 终端错误 |
| ILLEGAL\_SIGN | 签名错误 |
| UNKNOWN\_SYSTEM\_ERROR | 系统错误 |
| INVALID\_BARCODE | 条码错误 |
| INSUFFICIENT\_FUND | 账户金额不足 |
| EXPIRED\_BARCODE | 过期的支付条码 |
| BUYER\_OVER\_DAILY\_LIMIT | 付款人当日付款金额超过上限 |
| BUYER\_OVER\_TRANSACTION\_LIMIT | 付款人单笔付款金额超过上限 |
| SELLER\_OVER\_DAILY\_LIMIT | 收款账户当日收款金额超过上限 |
| TRADE\_NOT\_EXIST | 交易不存在 |
| TRADE\_HAS\_SUCCESS | 交易已被支付 |
| SELLER\_BALANCE\_NOT\_ENOUGH | 卖家余额不足 |
| REFUND\_AMT\_NOT\_EQUAL\_TOTAL | 退款金额无效 |
| TRADE\_FAILED | 交易失败 |
| UNEXPECTED\_PROVIDER\_ERROR | 不认识的支付通道 |
| TRADE\_TIMEOUT | 交易超时自动撤单 |
| ACCOUNT\_BALANCE\_NOT\_ENOUGH | 商户余额不足 |
| CLIENT\_SN\_CONFLICT | client\_sn在系统中已存在 |
| UPAY\_ORDER\_NOT\_EXISTS | 订单不存在 |
| REFUNDABLE\_AMOUNT\_NOT\_ENOUGH | 订单可退金额不足 |
| UPAY\_TERMINAL\_NOT\_EXISTS | 终端号在交易系统中不存在 |
| UPAY\_TERMINAL\_STATUS\_ABNORMAL | 终端未激活 |
| UPAY\_CANCEL\_ORDER\_NOOP | 无效操作，订单已经是撤单状态了 |
| UPAY\_CANCEL\_INVALID\_ORDER\_STATE | 当前订单状态不可撤销 |
| UPAY\_REFUND\_ORDER\_NOOP | 无效操作，本次退款退款已经完成了 |
| UPAY\_REFUND\_INVALID\_ORDER\_STATE | 当前订单状态不可退款 |
| UPAY\_STORE\_OVER\_DAILY\_LIMIT | 商户日收款额超过上限 |
| UPAY\_TCP\_ORDER\_NOT\_REFUNDABLE | 订单参与了活动并且无法撤销 |

| result\_code | result\_message | 说明 |
| --- | --- | --- |
| get\_brand\_wcpay\_request:ok | get\_brand\_wcpay\_request:ok | 支付成功 |
| get\_brand\_wcpay\_request:fail | get\_brand\_wcpay\_request:fail | 支付失败 |
| get\_brand\_wcpay\_request:cancel | get\_brand\_wcpay\_request:cancel | 用户取消支付 |

**商户务必以查单或者服务器异步通知的订单结果为主作出正确的处理。return\_url的结果仅供参考。查询订单请使用轮询方式获取为主，具体的轮询方式请使用web api接口里查询接口，轮询的时间控制在100-120s之间，轮询的间隔建议为前30秒内2秒一次，之后5秒一次**

说明：一般除 get\_brand\_wcpay\_request:ok 外皆认为支付失败，无需细分处理。

## 签名规则

1. 筛选 获取所有请求参数，不包括字节类型参数，如文件、字节流，剔除sign与sign\_type参数。
2. 排序 将筛选的参数按照第一个字符的键值ASCII码递增排序（字母升序排序），如果遇到相同字符则按照第二个字符的键值ASCII码递增排序，以此类推。
3. 拼接 将排序后的参数与其对应值，组合成“参数=参数值”的格式，并且把这些参数用&字符连接起来，此时生成的字符串为待签名字符串。将key的值拼接在字符串后面，调用MD5算法生成sign。将sign转换成大写。

例：

```
     传入参数如下
     terminal_sn: "123"
     client_sn:"123"
     total_amount:"1"

     拼接参数字符串
     stringA="client_sn=123&terminal_sn=123&total_amount=1"
     拼接密钥
     stringSignTemp = "stringA&key=19b820737ace6937a7808c"
     md5生成sign
     sign = md5(stringSignTemp).toUpperCase()
```

## 注：

需要使用微信浏览器，使用302跳转的方式访问https://qr.hemayun.com/qr/gateway  
  示例：

```
  <?php
    $paramsStr = "client_sn=test&operator=TEST&return_url=test&subject=TEST&terminal_sn=test&total_amount=3";
    $sign = strtoupper(md5($paramsStr.'&key=test'));
    $paramsStr = $paramsStr."&sign=".$sign;

    header("Location:https://qr.hemayun.com/qr/gateway?".$paramsStr);
  ?>
```

## wap支付接入常见问题




