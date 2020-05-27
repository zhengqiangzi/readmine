# performad说明文档 google/step3/create
### 页面引用路径 
> dev\js\atd\google\step3\index.vue

#### 生命周期 beforeRouteLeave 页面前页面跳转的目标地址如果是step2或是编辑状态下的step2 ，则保存数据，其它地址则清除数据
```

		var p = to.path

		if( p == '/atd/google/step2' || p == '/atd/google/estep2' ){

			setStepData("step3",this.result)

		}else{
			setStepData("step3",{})
		}
		next();
```
#### 生命周期 beforeRouteEnter 获取第三步数据，并赋值给页面变量
```
		let temp = getStepData("step3");
		next((vm)=>{
			vm.result=_.assign(vm.result,temp)
		});

```
#### 生命周期 beforeMount 获取accountID,
`this.accountId=this.$route.query.accountId;`

#### 生命周期 mounted 获取设置面包屑的数据

```
		let br=[]
		this.$nextTick(()=>{

			this.$load("Atd_Ads_Google").getReportAccountName(this.$route.query.accountId).then((data)=>{

				br.push({name:data,link:"#/atd/campaign/admin"})
				if(this.edit){

					br.push({name:getStepData("step2").adName,link:null})
				}
 				this.$extendBreadcrumbs(br) 

```