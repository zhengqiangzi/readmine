# performad说明文档 facebook/step3/create
### [facebook step3 create](http://performad.qa.onemad.com/platform/atd/main#/atd/facebook/step3?accountId=712344008863677&aName=Dragonsmeet%20Inc.-Madhouse&from=step1 "facebook step1" )
### 页面引用路径 
> dev\js\atd\facebook\step3\index.vue
#### 生命周期beforeMount 中在发址栏中判断是否是单独创建还是从上一步来的,并且合并基础数据

```
    beforeMount:function(){

			this.isAlongCreate=this.$route.query.from=='step3'?true:false;
			this.result=Object.assign({},this.result,DB.getItem("step3"));
			this.$api("Atd_Conn").facebookTargetConnectionPages().then((data)=>{
				this.fansList=data;
			})

			var hasPatchName=getProjectId();//页面存储数据中是否有标识当前名称是群编辑
			if(hasPatchName){
				var pdata=getProjectData()//页面batch名称 数据
				this.isBatchName=true
				this.result.adName=pdata.ad.map((item)=>{
					return item.label
				}).join("－")
			}
		},

```

#### 生命周期 beforeRouteLeave 第三步在返回第二时本地要记录下当前页面的数据。如果跳转到其它页面则清空数据并提示

```
		beforeRouteLeave:function(to,from,next){
			//if(this.formstate.$valid){
				if(to.path=='/atd/facebook/step2'){
					DB.setItem("step3",_.cloneDeep(this.result));
					next();
				}else{
					if(!this.isSubmit){
						this.isSubmit=false;
						let confirm = window.confirm("系统可能不会保存您所做的更改。")
						if(confirm){
							DB.setItem("step1",null)
							DB.setItem("step2",null)
							DB.setItem("step3",null)
							clearProject()
							next();
						}else{
							  next(false);
						}					
					}else{
						DB.setItem("step1",null)
						DB.setItem("step2",null)
						DB.setItem("step3",null)
						clearProject()
						next();
					}
				}
		},
```



#### computed 方法 media_rules 此方法是根据页面中的素材类型，返回具体素材的验证规则
```
        media_rules:function(){

            return this.result.xinshi==1 ? rule.carousel_rule :this.result.xinshi==0 ? rule.single_rule : rule.video_rule;
        },
```