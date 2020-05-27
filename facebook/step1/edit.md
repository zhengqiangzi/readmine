# performad说明文档 facebook/step1/estep1
### [facebook step1 edit](http://performad.dev.onemad.com/platform/atd/main#/atd/facebook/estep1?accountId=776104019238092&offerId=9626"facebook "step1" )
### 页面引用路径 
> dev\js\atd\facebook\step1\edit.vue

#### 编辑facebook广告组的第一步，从后端摘取数据，并且填入相应的表单中,此页面继承创建第一步，部分地方稍做修改

```
    import Index from "./index";
    import { getStep1Data } from "./option";
    var Index_clone=_.cloneDeep(Index);
    Index_clone.beforeRouteLeave=null
```

#### 生命周期mounted 拉取第一步需要编辑的数据
```
    mounted() {
       getStep1Data.bind(this)().then(x=>{
          this.result = x 
          /*{ "accountId": "712344008863677", 
          "ad_group_name": "FtoP测ad报错0701", "marketing": 21, "optimize_budget": true, "bidding": "1", "budget": { "type": "1", "value": 12 }, "throw_type": "1" };
            */
       });
    },
```
#### saveAndGoOn方法，点击确定按钮 直接提交到后端
```
        saveAndGoOn:function(){
            this.formstate.$submitted=true;
            if(!this.formstate.$valid){
                this.positionErrorMsg();
            } else{
                this.$load("atd.facebook").ajaxEditCampaign(_.cloneDeep(this.result)).then(xx=>{
                    this.$router.push({
                        path:"/atd/facebook/admin/campaign",
                        query:{
                            accountId:this.$route.query.accountId,
                        }
                     })
                })
            }
        }
```
