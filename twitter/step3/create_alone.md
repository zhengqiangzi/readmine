# performad说明文档 twitter/step3/create_alone
### 页面引用路径 
> dev\js\atd\twitter\create\step3\single.vue
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


#### 页面继承第三步创建，置空 *beforeRouteEnter */ *beforeRouteLeave*生命周期方法，并且重新写入逻辑
```

    let createStep3_clone = _.cloneDeep(createStep3)
        createStep3_clone.beforeRouteEnter=null;
        createStep3_clone.beforeRouteLeave=null;
```


#### 生命周期 *beforeRouteLeave*跳转页面并且清除弹出窗口状态 
```
        beforeRouteLeave:function(to, from, next){
                if(this.mustGo){
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
                        this.mustGo=true;
                        this.$router.push(to)
                    }).catch(function(){
                     })
                },10)
        }
```