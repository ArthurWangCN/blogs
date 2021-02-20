---
title: Vue项目中的支付
date: 2020-04-25 22:04:39
tags: Vue
categories: [前端, Vue]
---



> 商城项目必不可少的一个环节就是最后的支付，目前最常用的是支付宝支付和微信支付。

 下面介绍一下Vue中H5页面如何使用支付宝支付。其实很简单的，只不过是调自己后台的一个接口而已（后台根据支付宝文档，写好支付接口）。 

## 支付宝支付

 **后台会返回来一个form，然后提交form自动跳转到支付宝支付页面**。 

> 触发支付宝支付调用后台接口，后台会返回支付宝提供的form表单，我们只要在vue里面创建新节点，将返回的form表单append进去，并提交就可以唤起支付宝支付。另在此说一下这个*returnUrl*, 它是支付后支付宝回调的页面。具体可以根据自身业务，后台写死或者由前端控制。 

实际项目中我选择通过一个中间组件 `OrderAlipay.vue` 来做请求。

通过 query 的形式把订单号传给 OrderAlipay.vue ：

```javascript
//OrderPay.vue
paySubmit (payType) {
	//考虑封装支付宝和微信到一个方法里
	//通过payType判断支付类型
	if (payType === 1) {
	window.open('/#/order/alipay?orderNo=' + this.orderId, '_blank')
    }
}
```

通过 `this.$route.query.orderId` 拿到订单号，发起请求：

```javascript
//OrderAlipay.vue
alipaySubmit () {
    this.$http.post('/pay', {
    	orderId: this.orderId,
    	orderName: '商城订单',
    	amount: 0.01,	//订单价格
    	payType: 1	//前后台约定，1为支付宝方式
    }).then(res => {
    	this.content = res.content
    	setTimeout(() => {
    		document.forms[0].submit()
    	})
    })
}
```



## 微信支付

微信支付跟支付宝支付的不同之处在于需要自己根据后台返回的url生成二维码页面，其中用到一个npm插件是 [qrcode](https://www.npmjs.com/package/qrcode)：

安装： ` npm install --save qrcode `

把请求返回的 url 传给 `QRCode.toDataURL`，同时记得控制二维码图片的显示和隐藏：

```javascript
this.$http.post('/pay', {
	orderId: this.orderNo,
	orderName: '米米商城订单',
	amount: 0.01,
	payType: 2
}).then(res => {
	QRCode.toDataURL(res.content)
	.then(url => {
		this.showPay = true
		this.payImg = url
		this.loopOrderState()
	})
	.catch(err => {
		this.$message.error(err)
	})
})
```

## 微信支付状态轮询

二维码生成后开始通过定时器进行轮询，具体间隔根据业务需要自行设定：

```javascript
loopOrderState () {
	this.T = setInterval(() => {
		this.$http.get(`/orders/${this.orderNo}`).then(res => {
            // 状态码20表示已支付，清除定时器并跳转到列表页
			if (res.status === 20) {
				clearInterval(this.T)
				this.$router.push('/order/list')
			}
		})
	}, 1000)
}
```





## 参考

+ [Vue的H5页面唤起支付宝支付](https://segmentfault.com/a/1190000018900988)
+ [vue项目 微信支付 和 支付宝支付](https://blog.csdn.net/zyg1515330502/article/details/94737044)