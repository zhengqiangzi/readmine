# performad说明文档 twitter/step3/create
### 页面引用路径 
> dev\js\atd\twitter\create\step3\index.vue

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
  
    let step1_ajv=new Ajv();
    let step1_validate = step1_ajv.compile(schema);

    let step2_ajv=new Ajv();
    let step2_validate = step2_ajv.compile(step2_schema);

```


#### 生命周期 *beforeRouteLeave *判断页面跳转目标路径 如果是 step2a或是step1则当前页面中的操作数据保存起来，否则弹出窗口确认是否跳出，确认则跳出，否则继续留在当前页面中
```
  beforeRouteLeave:function(to, from, next){
            if(to.path=="/atd/twitter/create/step2"||to.path=="/atd/twitter/create/step1"){
                step3_data.set(_.cloneDeep(this.result))
                this.leaveDone();
                next();

            }else{
                if(this.mustGo){
                    clear_step_data(this.$route.query.accountId);
                     this.leaveDone();
                    next();
                    return;
                }
                next(false);
                setTimeout(()=>{
                    this.$confirm('离开此页面,输入的数据将被清除,是否继续', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        type: 'warning'
                    }).then(x=>{
                       // clear_step_data(this.$route.query.accountId);
                        this.mustGo=true;
                        this.$router.push(to)
                    }).catch(function(){

                    })
                },10)
            }
        },
```

#### 生命周期 *beforeRouteEnter *在页面进入后，要验证数据是否正确，如果数据不证确的话，弹出窗口提示。这里要验 证第一步数据和第二步数据是否都是正确。
```
        beforeRouteEnter:function(to, from, next){
            next(function(t){

                //验证广告系列和广告组 & 定向数据是否完整
                let error_list=[]
                var _v=step1_validate(step1_data.get())
                !_v&& console.error("广告系列数据不完整",step1_validate.errors)
                !_v&&(error_list.push("广告系列数据不完整"))
                
               !_v && t.createTips(x=>t.$notify({
                    title: '警告',
                    message:"广告系列数据不完整",
                    type: 'warning',
                    duration:0,
                    dangerouslyUseHTMLString:true
                }))
                

                var _v2 = step2_validate(step2_data.get())
                !_v2 && console.error("广告组定向数据不完整",step2_validate.errors)
                !_v2&&(error_list.push("广告组定向数据不完整"))
                !_v2 && t.createTips(x=>t.$notify({
                        title: '警告',
                        message:"广告组定向数据不完整",
                        type: 'warning',
                        duration:0,
                        dangerouslyUseHTMLString:true
                    }))
                var p = step3_data.get();
                t.mustGo=false;
                t.result = _.cloneDeep(p);
            })
        },
```