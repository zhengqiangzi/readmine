# performad说明文档 twitter/edit/step1/index.vue
### 页面引用路径 
> dev\js\atd\twitter\edit\step1\index.vue

## 页面继承创建第一步 并置空 beforeRouteLeave 和 beforeRouteEnter生命周期方法 ,在编辑状态下面再重新写入 beforeRouteLeave/beforeRouteEnter 生命周期逻辑状态
```
    let createIndex_clone=_.cloneDeep(createIndex)
    createIndex_clone.beforeRouteLeave=null;
    createIndex_clone.beforeRouteEnter=null;
```

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

#### 生命周期 beforeRouteEnter 拉入本来需要操作的数据
```
        beforeRouteEnter:function(to, from, next){
           next(async function(t){
               let _r = await (getStep1Data.bind(t))();
                var valid = validate(_r);
                if(valid){
                    t.result =_r;
                }else{
                    t.canclick=false;
                    t.createTips(x=>t.$message({
                        type:"error",
                        message: validate.errors.map(x=>x.dataPath+":"+x.message).join(","),
                        duration: 0,
                        showClose:false
                    }));
                }
            }) 
        },
```

#### 生命周期 beforeRouteLeave 判断跳转页面的是并给出提示 *this.leaveDone*这个方法是mixin里面定义的，目的是在页面跳转时把页面提示框去掉，防止带入目标页面中
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
                next(false)


        },
```