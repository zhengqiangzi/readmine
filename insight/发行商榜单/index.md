# performad说明文档 发行商榜单
### [发行商榜单]( http://performad.qa.onemad.com/platform/atd/main#/atd/insight/publishlist/list "发行商榜单" )
### 页面引用路径 
> dev\js\atd\adInsight\publishlist\index.vue

#### 一个路由空壳 子页面包含 三个子页面 分别为列表页面、产品页面、国家导入页面

 *{path:"list",component: () => import('atd/adInsight/publishlist/main')},
 *{path:"product",component: () => import('atd/adInsight/publishlist/product')},
 *{path:"ci",component: () => import('atd/adInsight/publishlist/countryimport'),}



 ## 列表页面( {path:"list",component: () => import('atd/adInsight/publishlist/main')} )
 #### 拉取列表数据
 ```
         computed: {
            table_data:function(){
                mock_table_data.bind(this)(this.selected_country,this.date_type,this.date2).then(x=>{
                    this.table_source=x;
                })
            }
        },
 ```


 ## 产品页面 ({path:"product",component: () => import('atd/adInsight/publishlist/product')})
 #### 拉取页面中的数据。这里的数据返回后需要处理一下，发行商榜单点击详情选择默认国家为美国，如果美国地区没有应用投放，则依次选择日本，印度尼西亚，  新加坡，马来西亚，泰国，中国香港，中国台湾，韩国，法国
 ```
 computed: {
        table_data_d:function(){
            this.filter_date
            var id = this.$route.query.id 
            mock_table_data.bind(this)(this.filter_date,id).then(x=>{
                this.table_source=x;
                if(this.$refs.tab){
                /*发行商榜单点击详情选择默认国家为美国，如果美国地区没有应用投放，则依次选择日本，印度尼西亚，
                新加坡，马来西亚，泰国，中国香港，中国台湾，韩国，法国。*/
                   var _a=_.uniq(x.appList.map(x=>{
                        return x.country
                    }))
                   // _a=["us","jp"]
                    if(_.indexOf(_a,"us")>=0){
                        this.$refs.tab.query.columns.country="us"

                    }else if( _.indexOf(_a,"jp")>=0){
                        this.$refs.tab.query.columns.country="jp"

                    }else if( _.indexOf(_a,"id")>=0){

                        this.$refs.tab.query.columns.country="id"

                    }else if( _.indexOf(_a,"sg")>=0){

                        this.$refs.tab.query.columns.country="sg"

                    }else if( _.indexOf(_a,"my")>=0){

                        this.$refs.tab.query.columns.country="my"

                    }else if( _.indexOf(_a,"th")>=0){

                        this.$refs.tab.query.columns.country="th"

                    }else if( _.indexOf(_a,"hk")>=0){

                        this.$refs.tab.query.columns.country="hk"

                    }else if( _.indexOf(_a,"tw")>=0){

                        this.$refs.tab.query.columns.country="tw"

                    }else if( _.indexOf(_a,"kr")>=0){

                        this.$refs.tab.query.columns.country="kr"

                    }else if( _.indexOf(_a,"fr")>=0){

                        this.$refs.tab.query.columns.country="fr"
                    }


                }  
            })

        },
 ```

## 国家导入页面( *{path:"ci",component: () => import('atd/adInsight/publishlist/countryimport'),})
#### fans-in这个组件是共用的，所以这里的urlHandler数据返回后，需要处理成业务逻辑中所需要的格式 
 ```
        <fans-in :urls="prop.row"  btn_name="添加公司" @urlin="urlHandler"></fans-in>
        
         urlHandler:function({old,newlinks}){
             old.page_url=[newlinks.join("")];
             old.displayFlag=1
             old.status=true
            modifyFn.bind(this)({companyId:old.companyId,publisherName:old.page_url.join("")})
        },
 ```