# 代码示例

```
		VfinanceClient client = new DefaultVfinanceClient("http://func105.vfinance.cn/test/open-gw/gateway.do","appKey","your private_key");
		VfinanceCommonRequest request=new VfinanceCommonRequest();
		request.setMethod("com.netfinworks.ma.queryBalance");
		request.setBizContent("{\"account_type\":\"101\",\"identity_no\":\"288888888888\",\"identity_type\":\"MEMBER_ID\",\"service\":\"query_balance\"}");
		try {
			VfinanceResponse result=client.execute(request);
			if(result.isSuccess()){
			    System.out.println("调用成功！");
			}else{
			    System.out.println("调用失败！");
			}
		} catch (VfinanceApiException e) {
			System.out.println("调用失败！");
		}
```