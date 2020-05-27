# performad说明文档 twitter/step1/create
### 页面引用路径 
> dev\js\atd\twitter\create\step1\index.vue

#### mixin *mixins:[tip_mixins]* 这个mixin主要是解决页面弹出弹出错误提示时，在未消除前，页面发生跳转，提示框还在的问题。这个功能未全部页面应用。
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


#### 生命周期 beforeRouteLeave 页面跳转目标页面如果是step2则，保存当前数据，如果不是则判断 this.mustGo是否为true,如果为真就清除数据，否则弹出窗口提示 *mustGo*是弹出窗口中点击确认按钮所设置的变量
```
        beforeRouteLeave:function(to, from, next){
            
            if(to.path=="/atd/twitter/create/step2"){
                step1_data.set(_.cloneDeep(this.result))
                next();
            }else{
                if(this.mustGo){
                    clear_step_data(this.$route.query.accountId);
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
                    }).catch(function(e){
                        console.error(e)
                    })
                },10)
            }
        },
```

#### 生命周期 beforeRouteEnter 页面进入渲染手，把第一步数据拿出来（数据有可能是在本地，也有可能需要加载，取决于是否从上一步返回或第一次加载）
```
        beforeRouteEnter:function(to, from, next){
            next(function(t){
                var p = step1_data.get();
                t.mustGo=false;
                t.result = _.cloneDeep(p);
            })
        }
```
