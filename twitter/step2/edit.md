# performad说明文档 twitter/step2/edit
### 页面引用路径 
> dev\js\atd\twitter\edit\step2\index.vue
#### mixin *mixins:[tip_mixins]*这个mixin主要是解决页面弹出弹出错误提示时，在未消除前，页面发生跳转，提示框还在的问题。这个功能未全部页面应用。
```
        leaveDone:function(){
            this.tips.map(xx => {
                xx.close();
            })
            this.tips = [];
        },
        createTips:function(fn){
            setTimeout(x=>{
                this.tips.push(fn());

            },10)
        }

```

#### Ajv是验证JSON数据的格式的。为了让后端数据传输正确，特地在这里加上这个验证，属于局部测试应用验证。未大面积应用 
```
   var ajv = new Ajv();
    var step1_data_validate=ajv.compile(schema)
```

#### 页面继承了创建
```
 export default {
        extends:createStep2_clone,
        ...
```

#### 生命周期 beforeRouteEnter 拉取第二步数据和第一步数据，并且进行json格式验证，如果错误 则弹出提示，否则赋值给页面
```
        beforeRouteEnter:function(to, from, next){
            next(async x=>{

                let _r = await getEditStep2DataFn.bind(x)();
                var valid = validate(_r);
               if(valid){
                    x.result= _r;
                    x.step1_data=_r.step1_data;
               }else{
                   x.canclick=false;
                   x.createTips(cc=>x.$message({
                        type: 'error',
                        message: validate.errors.map(x=>x.dataPath+":"+x.message).join(","),
                        duration: 0,
                        showClose:false
                    }));
               }
            })
        },
```


#### 生命周期  beforeRouteLeave 清除弹出窗口并且弹出确认退出提示
```
        beforeRouteLeave:function(to, from, next){
               if(this.mustGo){
                  this.leaveDone();
                   next();
                   return;
               }
               setTimeout(x=>{
                    this.$confirm('离开此页面,输入的数据将被清除,是否继续', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        type: 'warning'
                    }).then(x=>{
                        //  clear_step_data(this.$route.query.accountId);
                        this.mustGo=true;
                        this.$router.push(to)
                    }).catch(function(){

                    })
                },10)
                next(false);
        },
```