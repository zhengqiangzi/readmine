# performad说明文档 竞品关注
### [竞品关注]( http://performad.dev.onemad.com/platform/atd/main#/atd/insight/focuslist/sys/android "竞品关注" )
### 页面引用路径 
> dev\js\atd\adInsight\focusList\index.vue

#### 当前页面是分路由的。根据地址栏参数不一样，显示的页面也不相同
```
        watch: {
            "$route":function(){
                this.type=this.$route.params.type
            }
        },
        mounted() {
            this.type=this.$route.params.type
        },
```


#### table_data获取列表中的数据分为android和 ios 二种 类型数据
```
        computed: {
            table_data:function(){
                this.sdate && getData.bind(this)(this.$route.params.type,this.sdate).then(_g=>{

                    this.list=_g.map((xxx,index)=>{
                       // console.log(xxx)
                       // xxx.downloadFlag=0;
                       // xxx.downloadFloating=11
                        xxx.id=index + 1;
                        return xxx;
                    });
                })
                return [];
            }
        },
```