# performad说明文档 应用洞察
### [应用洞察]( http://performad.qa.onemad.com/platform/atd/main#/atd/insight/app/day "应用洞察" )
### 页面引用路径 
> dev\js\atd\adInsight\appInsight\list\index.vue
#### 当前页面是分路由的。根据地址栏参数不一样，显示的页面也不相同
```
   function getOrderType(){
        this.order_type= this.$route.path=='/atd/insight/app/week'?'week': this.$route.path=='/atd/insight/app/day'?'day':'day'
    }
```

#### show_table_data 这个方法是获取 day和week 表格中的数据的
```
        computed: {
            show_table_data:async function(){
                var params = [
                    this.condition,
                ]
                var c  =  await this.$load("Atd_Performad_Material").getAppRangeList(params);
                this.table_source = c.map((xx,yy)=>{
                    xx.local_index=yy+1;
                    xx.income_order2=xx.income_order
                    xx.loading=false;//此项数据是否正在提交或取消关注操作
                    return xx;
                });
                return []
            }
        },
```

#### 生命周期 *mounted*拉取过滤数据

```
      mounted:async function() {
             this.filter =  makeCate(await this.$load("Atd_Performad_Material").getAppRangeCategoryList())
        },
```


