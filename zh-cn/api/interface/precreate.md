# 预下单

## 入口

        {api_domain}/upay/v2/precreate
## 签名验证
  请见[签名机制文档](https://wosai.gitbooks.io/hemayun-doc/content/zh-cn/api/sign.html)
## 请求参数说明

参数 | 参数名称 | 类型 | 必填 | 描述 | 范例
--------- | ------ | ----- | ------- | --- | ----
terminal_sn | 合码云终端ID| String(32)| Y	 | 合码云终端ID | "23420593829"
client_sn | 商户系统订单号 | String(32)| Y | 必须在商户系统内唯一；且长度不超过32字节 | "18348290098298292838"
total_amount | 交易总金额 | String(10) | Y | 以分为单位,不超过10位纯数字字符串,超过1亿元的收款请使用银行转账 | "1000"
payway	|支付方式|	String|	Y	|内容为数字的字符串 |1:支付宝<br/>3:微信<br/>4:百度钱包<br/>5:京东钱包<br/>6:qq钱包
sub_payway	|二级支付方式|	String|N|内容为数字的字符串，<font color="red">如果要使用WAP支付，则必须传 "3"</font> | "3"
payer\_uid|	付款人id|	String(64)|	N|消费者在支付通道的唯一id,**微信WAP支付必须传open_id**| "okSzXt_KIZVhGZe538aOKIMswUiI"
subject|	交易简介|	String(64)|	Y|本次交易的概述| "pizza"
operator	|门店操作员|	String(32)|	Y	|发起本次交易的操作员|	"Obama"
description	|商品详情	|String(256)|N|对商品或本次交易的描述|
longitude|	经度	|String|	N	|经纬度必须同时出现|
latitude	|纬度	|String	|N|经纬度必须同时出现|
extended	|扩展参数集合	|String(256)|	N|	合码云与特定第三方单独约定的参数集合,json格式，最多支持24个字段，每个字段key长度不超过64字节，value长度不超过256字节 | { "goods_tag": "beijing"}
reflect|	反射参数|	String(64)|	N|任何调用者希望原样返回的信息 | { "tips" : "100"}
notify_url|回调|String(128)|N| 支付回调的地址|例如：www.baidu.com 如果支付成功通知时间间隔为1s,5s,30s,600s

**商户系统订单号必须在商户系统内唯一，支付失败订单的二次预下单请求，请创建新的商户订单号**

## 同步返回参数说明

参数 | 参数名称 | 类型 | 必填|描述 |范例
--------- | ------ | ----- | -------|---|----
sn|	合码云唯一订单号|	String(16)|	Y|	合码云系统内部唯一订单号	|7892259488292938
client_sn|	商户订单号|	String(64)|	Y	|商户系统订单号|	7654321132
trade_no	|支付服务商订单号|	String(64)|	Y	|支付通道交易凭证号|	2013112011001004330000121536
status	|流水状态	|String(32)|	Y	|本次操作产生的流水的状态|CREATED
order_status	|订单状态	|String(32)|	Y	|当前订单状态	|CREATED
payway	|支付方式	|String(2)|	Y	|一级支付方式，取值见附录《支付方式列表》|1
sub_payway|	二级支付方式|	String(2)|	Y|	二级支付方式，取值见附录《二级支付方式列表》|3
qr_code	|二维码内容	|String(128)|		 预下单成功后生成的二维码|	https://qr.alipay.com/bax00069h45nvvfc3tu9803a
total_amount	|交易总额	|String(10)|	Y|	本次交易总金额|	10000
net_amount|	实收金额	|String(10)|	Y	|如果没有退款，这个字段等于total_amount。否则等于total_amount减去退款金额|0
subject	|交易概述|	String(64)|	Y|本次交易概述|Pizza
operator|	操作员|	String(32)|Y|门店操作员|张三丰
reflect|	反射参数|	String(64)|	N|透传参数
wap_pay_request|支付通道返回的调用WAP支付需要传递的信息|String(1024)|N|WAP支付一定会返回；

## 支付成功回调地址参数说明：

参数 | 参数名称 | 类型 | 必填|描述 |范例
--------- | ------ | ----- | -------|---|----
terminal_sn	|终端号|	String(32)|Y|合码云终端ID|"01939202039923029"
sn|	收钱吧唯一订单号|	String(16)|Y|合码云系统内部唯一订单号|"7892259488292938"
client_sn|商户订单号|	String(32)|Y|商户系统订单号|"7654321132"
trade_no|支付服务商订单号|String(64)|Y|支付通道交易凭证号|"2013112011001004330000121536"
status|流水状态|String(32)|	Y|本次操作产生的流水的状态| "SUCCESS"
order_status	|订单状态	|String(32)|Y|当前订单状态|"PAID"
payway|	支付方式|	String(2)|Y|一级支付方式，取值见附录《支付方式列表》|"1"
sub_payway|二级支付方式|	String(2)|Y|二级支付方式，取值见附录《二级支付方式列表》|"1"
payer_uid|	付款人ID|	String(64)|N|支付平台（微信，支付宝）上的付款人ID|"2801003920293239230239"
payer_login|付款人账号|String(128)|N|支付平台上(微信，支付宝)的付款人账号|"134****3920"
total_amount|交易总额	|String(10)|Y|本次交易总金额|"10000"
net\_amount|实收金额|String(10)|Y|如果没有退款，这个字段等于total\_amount。否则等于 total_amount减去退款金额|"0"
subject|	交易概述|	String(64)|	Y|本次交易概述|"Pizza"
finish_time	|付款动作在合码云的完成时间|String(13)|Y|时间戳|"1449646835244"
channel\_finish_time|付款动作在支付服务商的完成时间|String(13)|Y|时间戳|"1449646835244"
operator	|操作员	|String(32)	|Y	|门店操作员	|"张三丰"
reflect	|反射参数|	String(64)	|N|	透传参数	| {"tips": "200"}

收到成功的回调需要响应<font color="red">**success**给服务器端

返回的状态码请参考<font color="red">**附录**</font>

## 预下单返回示例
 预下单成功

    ```json
    {
        "result_code": "200",
        "biz_response": {
            "result_code": "PRECREATE_SUCCESS",
            "data": {
                "sn": "7894259244096169",
                "client_sn": "765432112",
                "status": "IN_PROG",
                "order_status": "CREATED",
                "total_amount": "1",
                "net_amount": "1",
                "operator ": "张三丰",
                "subject ": "coca cola",
                "qr_code": "https://qr.alipay.com/bax8z75ihyoqpgkv5f"
            }
        }
    }
    ```


## 预下单接口接入常见问题
   
### 1.调用预下单接口成功后需要主动轮询获取订单状态
      precreate接口何时发起轮询：
      在得到预下单成功的结果后，即可向合码云服务器发起轮询请求。
	    预下单成功:biz_response.result_code="PRECREATE_SUCCESS" or biz_response.data.order_status="CREATED")
		  合码云目前所有预下单的订单有效支付时长约为4分钟，若超时仍未支付，合码云会自动取消该订单；因此请控制好轮询时间。
		  轮询的间隔建议为前30秒内2秒一次，之后5秒一次。
### 2.轮询如何发起？
      需要主动调用查询接口，按照问题1里的规则进行轮询。
      
### 3.有回调地址了为什么还有轮询？
     目前回调地址的通知间隔是1s, 5s, 30s, 600s，为了确保稳定，建议以主动轮询的结果为主，回调地址的结果为辅。
