# zhifudashi_php_demo
支付大师php_demo版本,是真正的个人免签约第三方h5支付通道聚合支付接口，云端免挂机监控，即时到账，支持小程序、网站二维码扫码收款。只需个人微信支付宝账号即可自动化收款，超稳定不漏单
# 支付大师 https://www.zhifudashi.com
## 是真正的个人免签约第三方h5支付通道聚合支付接口，云端免挂机监控，即时到账，支持小程序、网站二维码扫码收款。只需个人微信支付宝账号即可自动化收款，超稳定不漏单

## 接口文档  https://docs.zhifudashi.com

### 对接简单，异步回调，安全高效，不限行业


``` php
<?php
	# php json方式接收接口返回 
	#开发手册：
    $order_price = $_GET['order_price']; # 获取充值金额
    $order_id = '20220729023137U97661109';       # 自己创建的本地订单号
    $merchantNum = 'xxxxxxxxxxxxxxx';  # 商户号,  商户后台的用户中心页面查看
	$secret = 'xxxxxxxxxxxxxxxx';     # 商户密钥, 商户后台的用户中心页面查看
    $api_url = 'http://127.0.0.1:8080/ApiOrders/createorder';   # 接口地址， 商户后台的用户中心页面查看
    $payType = $_GET['payType'];    # 查看支付接口文档说明payType的取值
    $notifyUrl = 'https:///xxxx.com/zhifu_callback_demo.php';   # 修改为您自己用来接收支付成功的公网地址
    $returnUrl = ''; //'http://xxxx.com/return_url.php';  # 支付成功您想让页面跳转的地址
	$returnType = "json"; //接口返回方式 page为直接跳转到支付页面，不传返回json
	$extension='';// 扩展字段
	$order_name='测试商品';
// sign 签名是order_id+order_price MD5加密之后 再次+secretKey 然后再次MD5
    $sign = sign($order_id,$order_price,$secret);

    $native = array(
		"uid" => $merchantNum,
		"order_type" => $payType,
		"order_price" => $amount,
		"order_id" => $order_id,
		"redirect_url" => $notifyUrl,
		"returnUrl" => $returnUrl,
		"extension" => $extension,
		"order_name" => $order_name,
		"sign" => $sign
		);
	
	$param = http_build_query($native);
	$return = http_request($api_url, $param, 'application/x-www-form-urlencoded;charset=utf-8');
	if(strpos($return,'{') === 0){
		$return = json_decode($return, true);
		if($return['code']==200){
			// 返回json 
			echo $return['data'];//
		}else{
			 echo $return['msg'];
		}
	}else{
		echo "请求异常";
	}
	exit;
	
// sign 签名是order_id+order_price MD5加密之后 再次+secretKey 然后再次MD5
function sign($order_id,$order_price,$secret) {
    return md5(md5($order_id.$order_price).$secret);
};	

// 发送请求
function http_request($url, $post_data=array(), $header='Content-Type: application/json'){
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	// 返回最后的Location
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
	curl_setopt($ch, CURLOPT_POST, 1);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
	curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 60);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
	curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array(
        $header,
        'Content-Length: ' . strlen($post_data)
    ));
	$contents = curl_exec($ch);
	curl_close($ch);
	return $contents;
}
	

?>

```

### 交流群：QQ：385468484

### 更多demo
java:    https://github.com/mailes/zhifudashi_java_demo  
ios: https://github.com/mailes/zhifudashi_ios_demo  
android:    
