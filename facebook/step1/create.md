# performad说明文档 facebook/step1/create
### [facebook step1 create]( http://performad.dev.onemad.com/platform/atd/main#/atd/facebook/step1?accountId=776104019238092&aName=Shanghai-Dacheng-Network-0320-91707-05&from=step1 "facebook step1" )
### 页面引用路径 
> dev\js\atd\facebook\step1\index.vue

#### 创建facebook广告组的第一步,此页面有数据记录功能，即hash地址变化后，再次返回，之前填写的数据还有状态恢复
#### beforeRouteLeave方法  如果路由跳转的路径为 step2 需要验证成完成后，存储状态数据，以便恢复状态,否则就弹出警告
```
    if(to.path=='/atd/facebook/step2'){
        if(!this.formstate.$invalid)
        {
            let copy=_.cloneDeep(this.result);
            copy.accountId=this.$route.query.accountId
            copy.app_themes=getAppByfbAppId(this.result.app);
            DB.setItem("step1",copy);
            next();
        }
    }else{
            let confirm = window.confirm("系统可能不会保存您所做的更改。")
                if(confirm){
                    DB.setItem("step1",null)
                    DB.setItem("step2",null)
                    DB.setItem("step3",null)
                    clearProject()
                    this.isBatchName=false;
                    next();
                }else{
                    next(false);
                }

    }
}
```

#### beforeMount 方法 页面加载后是判断是否是返回或第一次进入
```
		beforeMount:function(){
			//加载老的数据
			let old_data=DB.getItem("step1");
			if(old_data){
				this.result=Object.assign({},this.result,old_data)
			}
			this.isBatchName=getProjectId()?true:false;
			if(this.isBatchName){
				var pp=getProjectData();
				this.initData=getProjectData()
				this.initData.compaign=this.initData.campaign
			}
			this.$nextTick(()=>{
				this.$load('Atd_Conn').appContainAccountAppList({channel:1,accountId:this.$route.query.accountId}).then((data)=>{
					this.AppList=AppListFn(data);
				})
				this.$extendBreadcrumbs([
					{name:this.$route.query.aName,link:"#/atd/campaign/admin"}
				])
				//获取推荐关键词
				getRecommandKeys.bind(this)().then((_data)=>{
					this.recommendKeys=_data
				})
				//获取已选择方案列表
				getBatchList.bind(this)().then((_data)=>{
					this.projects=_data
				})
			})

		}
```

#### 设置批量命名,具体组件使用的组件为 *fe-input-group*
```
           <el-dialog title="批量命名" :visible.sync="showGname" width="70%">
                <fe-input-group
                :init-data="initData"
                :recommend-keys="recommendKeys"
                :projects="projects"
                @result="resultHandler"
                ref="fgroup"
                @delproject="delProjectHandler">
                </fe-input-group>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="showGname = false">取 消</el-button>
                    <el-button type="primary" @click="saveProjectsHandler(0)" :disabled="!((allSelectedKeys.campaign.length>=1) && (allSelectedKeys.adsets.length>=1) && (allSelectedKeys.ad.length>=1)) ">保存</el-button>
                    <el-button type="primary" @click="saveProjectsHandler(1)" :disabled="!((allSelectedKeys.campaign.length>=1) && (allSelectedKeys.adsets.length>=1) && (allSelectedKeys.ad.length>=1)) ">应用</el-button>
                </div>
            </el-dialog>
```