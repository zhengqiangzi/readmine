# performad说明文档 facebook/estep2
### [facebook step2 edit](http://performad.qa.onemad.com/platform/atd/main#/atd/facebook/estep2?accountId=712344008863677&stepId=53737&accountName=Dragonsmeet%20Inc.-Madhouse "facebook step2" )
### 页面引用路径 
> dev\js\atd\facebook\step2\edit.vue
#### 编辑facebook第二步数据

#### 生命周期 设置面包屑、并且通过地址中的数据获取*stepID*,拉取数据
```
  beforeRouteEnter: (to, from, next) => {
    breadcrumbsOptions=[]
    let vm =new Vue();
    let sid=getLocationParams("stepId");
    vm.$api("Atd_Ads_Fb").getAdsetInfo(sid).then((data)=>{
      tdata= transformDataToStep2Data(data.step2_data);
      breadcrumbsOptions.push({name:getLocationParams("accountName"),link:'#/atd/campaign/admin'})
      breadcrumbsOptions.push({name:data.step2_data.ggzmc,link:null})
      step2_data = tdata.data;
      breadcrumbsOptions.splice(1,0,{name:data.step1_data.ad_group_name,link:null});
      step1_data={
              accountId: data.step1_data.accountId,
              marketing:data.step1_data.marketing,
              app_themes:{
                id:data.step1_data.app_themes.id,
                os:data.step1_data.app_themes.os,
                storeUrl:data.step1_data.app_themes.storeUrl,
                abbr:data.step1_data.app_themes.abbr
              },
              ad_group_name:data.step1_data.ad_group_name,
              bidding: data.step1_data.bidding,
              budget: data.step1_data.budget,
              optimize_budget: data.step1_data.optimize_budget,
              throw_type: data.step1_data.throw_type
          }
           next();
    }).catch((e)=>{
      console.log(e)
    })
  },
```


#### 生命周期  beforeMount 拉取数据 并填入变量 result中
`this.result =Object.assign({}, this.result, step2_data); //合并老的数据`

