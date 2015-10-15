# titanium_module_alipay_ios
* 支付宝的titanium module for ios
* 作者刘明星

本模块实现了支付宝手机支付sdk的封装，避免了wap支付接口每次支付还需要输入支付宝账号的麻烦。用本地sdk支付仅需要输入6位数字密码即可，其方便程度与微信支付一样。调用方式：
```javascript
var alipay = require("com.mamashai.alipay");
alipay.alipay({
	partner: "2088311250002935",
	seller: "pay@mamashai.com",
	private_key: "MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAPKQNq01zk8X8KvEvPcU9fQM/OABzz2Q1576f303DGzL5Jy2ihNHzMbXOt6o707fMCvtA+jb98FycSuOoZcGe4miLitJeiyrfALQ7axs+TNpWJVfGOBjClyTDx3s1DOvBqbknAz7VqiwttWqkg5XBLlRAPQ0oO19EqRCYA6kr96lAgMBAAECgYEA76aIPs3ATejLQgoY4M12y27hkLh49szaHBpGR4JR5lP0RNkcxjvUGEihw0eJWJWuVFfR2wkpWZkmMvCyujIPblnBg2WOoq2a23ljnkddLTLRZsdDbIucAMDHeK8ZHumeJ/tkD/ypqHBVi4kYmnICBLpJV5lDnk/PoaIM4/fO9IECQQD60568OKtH6AZCieZEzcdswNX+fCOTvEmLWmCV+oCGCH3l/eDwj84sD06j1zZPBMLMlmt0gfzDg2VS9CilrV01AkEA95D2d2BLsHaC+t289PCtDmHECkNqwlnAhZkK1jj3poqQes1ptWMI8TLOym2PM8frtvGZjx9IMrM+AucwhR9ZsQJAIGTUS1rGRDMjG9TTeG9bIiCFgqhlr97RYL37W2NO1gCiweFX+7mW1vnjHiXdTbc/sUx79EAVdOqzW1NNLJiHQQJAX9myc1nHNFVONQ7w/+zHNBBKNKcRiJnzXkZ42aRIziRL+B/b06y6Y5iGU/3DOgsnijdUewNjkq2vTrRwJrqSoQJAEYf93Tyu8DGhWnsx0cCOgcWPmVBMnSDaN0CxIJ+x7dKShvk3Ejl0NWM7kQ1ZaYJp/dms6MYwQkTGletdcQk1WQ==",
	public_key: "mw0mqdblunyhqb72l6xit24jhmvx73xx",
	id: win.id,
	subject: "付费商品",
	desc: "商品详细说明",
	price: "10.28",
	notify_url: "http://www.mamashai.com/api/gou/pay_url",
	schema: "wxc4e544191aa9121a"
})
function ios_resume(e){
	if (!win.add_ios_resumed){
		var url = decodeURI(e.url);
		if (e.source == "com.alipay.iphoneclient"){
			var json = JSON.parse(url.substr(url.indexOf("?")+1));
			if (json.memo.ResultStatus == "9000"){
				Ti.App.fireEvent("pay_success", {trade_no: win.json.id})
			}
			else{
				show_notice(json.memo.memo)
			}
		}
	}
					
	win.add_ios_resumed = true;
}
Ti.App.addEventListener("ios_resumed", ios_resume)
win.addEventListener("close", function(e){
	Ti.App.removeEventListener("ios_resumed", ios_resume)
})
Ti.App.addEventListener("resumed", function(e){
	var args = Ti.App.getArguments();
	Ti.App.fireEvent("ios_resumed", {url: args.url, source: args.source});
});				
```

private_key和public_key的取得方法可以参考支付宝手机支付sdk文档。要获得schema的值请用xcode打开titanium自动生成的项目文件查看。

