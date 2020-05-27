# performad说明文档 facebook/step3/edit
### [facebook step3 create](http://performad.qa.onemad.com/platform/atd/main#/atd/facebook/estep3?accountId=712344008863677&stepId=111977&accountName=Dragonsmeet%20Inc.-Madhouse "facebook step3" )
### 页面引用路径 
> dev\js\atd\facebook\step3\index.vue

#### 生命周期 mounted从地址栏中获取*stepId*通过接口获取第三步数据

```
		mounted:function(){
			//获取第三步信息
			this.$load("Atd_Ads_Fb").getAdInfo(this.$route.query.stepId).then((_data)=>{

				let breadcrumbsOptions = [
					{name:this.$route.query.accountName,link:`#/atd/campaign/admin`},
					{name:_data.campaignName,link:null},
					{name:_data.adsetName,link:null},
					{name:_data.name,link:null},				
				 ]

				this.$extendBreadcrumbs(breadcrumbsOptions)
				this.result=Object.assign({},this.result,_data);
			})
			this.$load("Atd_Conn").facebookTargetConnectionPages().then((data)=>{
				this.fansList=data;
			})
		},
```


#### 生命周期 beforeMount 通过地址栏获取accountId
```
		beforeMount:function(){

			this.accountId=this.$route.query.accountId;
		},

```
