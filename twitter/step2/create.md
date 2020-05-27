# performad说明文档 twitter/step2/create
### 页面引用路径 
> dev\js\atd\twitter\create\step2\index.vue

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

#### 生命周期 beforeRouteLeave 页面跳转目标页面如果是step3或是step1 则，保存当前数据，如果不是则判断 this.mustGo是否为true,如果为真就清除数据，否则弹出窗口提示 *mustGo *是弹出窗口中点击确认按钮所设置的变量

```

        beforeRouteLeave:function(to, from, next){
            
          //  console.log('leave',JSON.stringify(this.result))

            this.fs.$submitted=true;

            if(to.path=="/atd/twitter/create/step3"||to.path=="/atd/twitter/create/step1"){
                step2_data.set(_.cloneDeep(this.result))
                next();
                this.leaveDone();

            }else{
                if(this.mustGo){
                    clear_step_data(this.$route.query.accountId);
                    next();
                    this.leaveDone();
                    return;
                }
                next(false);
                setTimeout(()=>{
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
            }
        },
```
 
#### 生命周期 beforeRouteEnter 拉取第一步数据并且验证第一步数据是否完整 否则提出警告。如果成功 则把值赋给页面
```
        beforeRouteEnter:function(to, from, next){
            next(function(t){
                t.step1_data = step1_data.get()
                var p = step2_data.get();
                /**
                    验证第一步数据是否完整 
                 */
                var _v = step1_data_validate(step1_data.get());
                !_v&& (console.error("广告系列数据不完整",step1_data_validate.errors));
                !_v&&(
                    t.createTips(x=>t.$notify({
                        title: '警告',
                        message: '广告系列数据不完整',
                        type: 'warning',
                        duration:0,
                    }))
                )
                t.result = _.cloneDeep(p);
                t.mustGo=false;
            })
        },
```