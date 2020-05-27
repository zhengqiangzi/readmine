# performad说明文档 google/step2/create
### 页面引用路径 
> dev\js\atd\google\step2\index.vue
#### 生命周期  beforeRouteLeave 页面发生跳转时，如果目标是 第三步、第一步或 是第三步编辑状态时，则把当前页面的数据做一个本地化保存，如果上面是其它页面，则清空数据
```
	beforeRouteLeave:function(to,from,next){
		/*
		/atd/google/step3、/atd/google/step1
		*/

		var p = to.path
		if( p == '/atd/google/step3' || p == '/atd/google/estep3' || p== '/atd/google/step1' ){
			setStepData("step2",this.result)
		}else{

			setStepData("step2",{})
		}

		next();
	},
```

### 方法 watch中 result.budget 因为页面的特殊要求，在这里要监听budget ，并手动设置验 证的状态

```
		"result.budget":{
			handler:function(){
				this.formstate["budget"] && (this.formstate["budget"].$touched=true,this.formstate["budget"].$dirty=true)
			},
			immediate:true
		},
	
```

### 方法 watch中 result.price 因为页面的特殊要求，在这里要监听price ，并手动设置验 证的状态

```
		"result.price":{
			handler:function(){
				this.formstate["price"] && (this.formstate["price"].$touched=true,this.formstate["price"].$dirty=true)
			},
			immediate:true
		}
	
```