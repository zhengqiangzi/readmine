# performad说明文档 facebook/step2/create
### [facebook step2 create](http://performad.qa.onemad.com/platform/atd/main#/atd/facebook/step2?accountId=712344008863677&aName=Dragonsmeet%20Inc.-Madhouse&from=step1 "facebook step1" )
### 页面引用路径 
> dev\js\atd\facebook\step2\index.vue
#### 创建facebook广告第二步,从第一步创建跑转过来，页面部分数据需要从第一步数据中获取 ，所以单独打开些页面，页面会报错。

#### beforeRouteLeave方法  如果路由跳转的路径为 step3或step3 需要验证成完成后，存储状态数据，以便恢复状态,否则就弹出警告
```
  beforeRouteLeave: function(to, from, next) {
  	if(to.path=='/atd/facebook/step1' || to.path=='/atd/facebook/step3'){
	   
      if (this.formstate.$valid) {

	      DB.setItem("step2", _.cloneDeep(this.result));

        next();

	    }

  	}else{

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
  	}
  },
```

#### 生命周期 beforeRouteEnter 进入页面获取第一步数据，如果没有则需要api请求第一步数据
```
  beforeRouteEnter: function(to, from, next) {
    step1_data = DB.getItem("step1");
    step2_data = DB.getItem("step2");
    
    if (!step1_data) {
      //第一步数据没有，需要异步请求加载 第一步数据
      getStep1Data().then(_data => {
        DB.setItem("step1", _data);
        step1_data = DB.getItem("step1");
        next();
      }).catch((e)=>{
      	next();
      })
    } else {
      next();
	  }
  },
```


#### 生命周期  beforeMount 进入页面时，要把第一步数据拉到，第二步也可以拉的到，如果有的话;

```
  
  beforeMount: function() {
    step1_data = DB.getItem("step1");
    step2_data = DB.getItem("step2");

```