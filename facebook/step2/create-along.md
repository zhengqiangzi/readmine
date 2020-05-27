# performad说明文档 facebook/astep2
### [facebook step2 create](http://performad.qa.onemad.com/platform/atd/main#/atd/facebook/astep2?accountId=712344008863677&from=step2&offerId=6572&appId=8 "facebook step2" )
### 页面引用路径 
> dev\js\atd\facebook\step2\index2.vue
#### 创建facebook广告第二步,从列表中点击创建按钮，单独创建第二步数据

#### 此页面继承 index.vue并置空 beforeRouteEnter生命周期操作
```

    var createIndex_clone= _.cloneDeep(createIndex);
        createIndex_clone.beforeRouteEnter=null;
```

#### 生命周期 beforeRouteEnter 
```
      beforeRouteEnter: function(to, from, next) {
            step1_data = DB.getItem("step1");
            step2_data = DB.getItem("step2");

            if (!step1_data) {
            //第一步数据没有，需要异步请求加载 第一步数据
            getAStep1Data().then(_data => {
                DB.setItem("step1", _data);
                step1_data = DB.getItem("step1");
             //   console.log(step1_data)
                next();
            }).catch((e)=>{
                next();
            })
            } else {
                next();
            }
        }
```
