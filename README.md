
[蚂蚁金服开放平台](https://openhome.alipay.com/platform/home.htm)`Node.js` SDK。

---

## 安装

``` javascript

npm i alipay-libs -S

```

## 基本使用

``` javascript
const fs = require('fs')
const Alipay = require('alipay-libs').default

const readFile = filename => {
  return fs.readFileSync(path.resolve(__dirname, filename))
}
const options = {
  app_id: '20180xxxxxxxxxxxx', // 应用ID
  appPrivKeyFile: readFile('./pem/app_private_key.pem'), // 应用私钥
  alipayPubKeyFile: readFile('./pem/alipay_public_key.pem'), // 支付宝公钥
  notifyUrl: '', // 异步通知url（选填）
  gatewayUrl: '', // 应用网关url（选填）
}

const service = new Alipay(options)
const data = {
  subject: 'QQ充值中心',
  out_trade_no: '1232423',
  total_amount: '100'
}
const result = service.createOrder(data)

```


## 接口返回格式

``` javascript
{
  code: '' // 错误码,
  message: '', // 错误信息,
  data: '' // 蚂蚁金服返回的原始数据（可能为空对象）
}
```


## 功能列表

- [x] 创建订单
- [x] 取消订单
- [x] 订单查询
- [x] 验证支付状态
- [x] 订单状态异步推送
- [x] 预创建订单
- [x] 申请退款
- [x] 退款查询
- [x] 交易结算
- [x] 关闭交易
- [x] 账单下载地址查询
- [x] 单笔转账到支付宝账户

---

## API 说明

### 创建订单`createOrder`

[当面付官方文档](https://opendocs.alipay.com/apis/028ln0)

>生成二维码后，展示给用户，由用户扫描二维码完成订单支付

```javascript
const service = new Alipay(options)
const data = {
  subject: 'QQ充值中心',
  out_trade_no: '1232423',
  total_amount: '100'
}
const result = service.createNativeOrderURL(data)
```

---

### 创建网页订单`createWebOrderURL`

[手机网页支付官方文档](https://docs.open.alipay.com/203/107090/)

>该接口用于支付宝手机网页支付，服务端调用该接口生成一个`URL`返回给客户端, 客户端拿到该`URL`之后跳转到该URL发起支付请求。支付结束支付宝会跳转到客户端填写的`return_url`。

``` javascript
const service = new Alipay(options)
const data = {
  subject: '辣条',
  out_trade_no: '1232423',
  total_amount: '100'
}
const basicParams = {
  return_url: 'http://xxx.com'
}
const result = service.createWebOrderURL(data, basicParams)
```

---

### 创建pc端订单`createPageOrderURL`
[创建pc端订单官方文档](https://docs.open.alipay.com/270/105899/)

``` javascript
const service = new Alipay(options)
const data = {
  subject: 'QQ充值中心',
  out_trade_no: '1232423',
  total_amount: '100'
}
const basicParams = {
  return_url: 'http://xxx.com'
}
const result = service.createPageOrderURL(data, basicParams)
```

---

[APP支付官方文档](https://docs.open.alipay.com/204/105465/)

>用于返回给APP,传递给支付宝端发起交易申请 

```javascript
const service = new Alipay(options)
const data = {
  subject: 'QQ充值中心',
  out_trade_no: '1232423',
  total_amount: '100'
}
const result = service.createOrder(data)
```

---

### 订单查询`queryOrder`

[订单查询官方文档](https://docs.open.alipay.com/api_1/alipay.trade.query)

``` javascript
const outTradeNo = '1232423'
return service.queryOrder({ out_trade_no: outTradeNo }).then(result => {\
  // 结果
  console.log(result)
})
```

---

### 取消订单`cancelOrder`

[取消订单官方文档](https://docs.open.alipay.com/api_1/alipay.trade.cancel)

``` javascript
const outTradeNo = 'foobar'
return service.cancelOrder({ out_trade_no: outTradeNo }).then(result => {
  // 结果
  console.log(result)
})
```

---

### 验证支付结果`verifyPayment`

[App支付同步通知参数校验](https://docs.open.alipay.com/204/105302)

```javascript
const params = {
  memo: "xxxx",
  result: "xxxx",
  resultStatus: "xxx"
}
return utils.verifyPayment(params)

```

---

### 异步通知校验`makeNotifyResponse`

[异步通知官方文档](https://docs.open.alipay.com/204/105301/)

```javascript
const params = {
  sign: 'xxxxxxxx',
  sign_type: 'xxxxx',
  ...
}

return service.makeNotifyResponse(params)

```

### 交易关闭`tradeClose`

[关闭交易官方文档](https://docs.open.alipay.com/api_1/alipay.trade.close/)

```javascript
const params = {
  out_trade_no: 'xxxxx'
}
return service.tradeClose(params)

```

---

### 交易退款`tradeRefund`

[交易退款官方文档](https://docs.open.alipay.com/api_1/alipay.trade.refund/)

```javascript
const params = {
  out_trade_no: 'xxxxx'
}
return service.tradeRefund(params)
```

---

### 交易退款查询`tradeRefundQuery`

[交易退款查询官方文档](https://docs.open.alipay.com/api_1/alipay.trade.fastpay.refund.query/)

```javascript
const params = {
  out_trade_no: 'xxxxx'
}
return service.tradeRefundQuery(params)
```

---

### 查询账单下载地址`billDownloadQuery`

[查询账单下载地址文档](https://docs.open.alipay.com/api_15/alipay.data.dataservice.bill.downloadurl.query)

```javascript
const params = {
  bill_type: 'trade',
  bill_date: '2017-05-06'
}
return service.billDownloadQuery(params)

```

---

### 交易预创建`tradePrecreate`

[交易预创建官方文档](https://docs.open.alipay.com/api_1/alipay.trade.create/)

```javascript
const params = {
  out_trade_no: 'xxx',
  seller_id: 'asad',
  total_amount: '231wawsda',
  subject: '面包'
}
return service.tradePrecreate(params)

```

---

### 交易结算`tradeSettle`

[交易结算官方文档](https://docs.open.alipay.com/api_1/alipay.trade.order.settle/)

```javascript
const params = {
  out_request_no: 'xxx'
}
return service.tradeSettle(params)
```

---

### 单笔转账到支付宝账户接口`toaccountTransfer`

[接口文档](https://opendocs.alipay.com/open/309/alipay.fund.trans.toaccount.transfer)

```javascript
const params = {
  out_biz_no: "1234",
  payee_type: 'ALIPAY_LOGONID',
  payee_account: "user666",
  amount: "100"
}

return service.toaccountTransfer(params)

```

---

## LICENSE

  [MIT](https://mit-license.org/)
