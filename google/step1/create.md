# performad说明文档 google/step1/create
### 页面引用路径 
> dev\js\atd\google\step1\index.vue
#### 生命周期 mounted 获取applist数据和当前accountName

```
		this.$load("Atd_Ads_App_product").appProductMediaTypeList(2,{ad_account:this.$route.query.accountId}).then((data)=>{
			this.appList=data.map(x=>{
				if(x.osType == 0){
					x.name=x.name +"-Android";
				} else if(x.osType == 1){
					x.name=x.name +"-iOS";
				}
				if (x.validated){

					x.name=x.name;
				}else{

					x.name = x.name +"(该账户无此应用权限)";
				}
				return x;
			})
        });

		this.$nextTick(()=>{
 				
			this.$load("Atd_Ads_Google").getReportAccountName(this.$route.query.accountId).then((data)=>{
				this.$extendBreadcrumbs([
					{name:data,link:"#/atd/campaign/admin"}
				])

			})
		})
```

#### 生命周期 beforeRouteLeave 如果当前页面是要跳转到 step2，则把数据保存在本地中，否测清空数据
```

	beforeRouteLeave:function(to,from,next){

		if(to.path=='/atd/google/step2'){

			setStepData("step1",this.result)
		}else{
			setStepData("step1",{})
		}
		next();
	},

```
#### 生命周期beforeRouteEnter获取step1数据（数据有可能是本地的）并合并数据到 result中
```

	beforeRouteEnter:function(to,from,next){
		let temp = getStepData("step1");
		next((vm)=>{
			vm.result=Object.assign({},vm.result,temp)
		});
	},
```