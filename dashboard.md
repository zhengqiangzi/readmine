# performad说明文档 dashboard
### [dashboard]( http://performad.dev.onemad.com/platform/atd/main#/atd/dashboard "dashboard" )
### 页面引用路径 
> dev\js\atd\dashboard
#### 页面根据帐户分组 日期 获取某个产品的详情 详情 包含 ‘花费趋势-点击/安装’、‘广告系列消耗Top10’、‘转化流程渠道占比’、‘消耗环比’、‘转化流程漏斗图’
#### 花费趋势-点击/安装 绑定的数据源为 *putInData*
```<div slot="header" class="clearfix">
             <span v-show="charts_status==1">花费趋势</span>
              <span v-show="charts_status==2">点击/安装</span>
              <div style="float: right">
                  <el-button type="primary" size="mini" :disabled="charts_status==1" @click="charts_status=1">花费趋势</el-button>
                  <el-button type="primary" size="mini" :disabled="charts_status!=1" @click="charts_status=2">点击/安装</el-button>
              </div>
        </div>
         <chart :options="putInData" ref="bar" autoresize style="width:100%"></chart>
    </el-card>
```
#### 广告系列消耗Top10 绑定的数据源为 *top10Data_data*
```<el-row :gutter="20">
    <el-col :span="12" >
        <el-card class="box-card">
            <div slot="header" class="clearfix">
                <el-row>
                    <el-col :span="10">广告系列消耗Top10</el-col>
                    <el-col :span="14">
                        <el-row type="flex" justify="end">
                            <el-col :span="12">
                                <el-select v-model="top10_date" size="small">
                                    <el-option v-for="item in top10_date_list" :key="item.value" :label="item.title"
                                        :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-col>
                            <el-col :span="1">&nbsp;</el-col>
                            <el-col :span="11">
                                <el-select v-model="top10_channel" size="small">
                                    <el-option v-for="item in top10_channel_list" :key="item.value" :label="item.title"
                                        :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-col>
                        </el-row>
                    </el-col>
                </el-row>
            </div>
            <mad-panel class="panel-has-select">
                <div slot="header">
                        &nbsp;
                </div>
                <div slot="body">
                    <el-scrollbar style="height:254px;" wrap-class="el-select-dropdown__wrap el-select-dropdown__wrap_c"
                        view-class="el-select-dropdown__list">
                        <el-table :data="top10Data_data" :g="top10">
                            <el-table-column prop="order" label="No" width="60px">
                            </el-table-column>
                            <el-table-column prop="campaignName" label="活动名称">
                                <template slot-scope="scope">
                                    <a :title="scope.row.campaignName" href="javascript:void(0)"
                                        @click="top10ClickHandler(scope)">{{scope.row.campaignName}}</a>
                                </template>
                            </el-table-column>
                            <el-table-column prop="cost2" label="消耗" width="150px">
                            </el-table-column>
                            <el-table-column prop="channel" label="投放渠道" width="100px">
                                <template slot-scope="scope">
                                    {{ toFirstUpperCase(scope.row.platform) }}
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-scrollbar>
                </div>
            </mad-panel>
        </el-card>
    </el-col>
```
#### 转化流程渠道占比 绑定数据*opt1*
```<el-col :span="12">
        <el-card class="box-card">
            <div slot="header" class="clearfix">转化流程渠道占比</div>
            <mad-panel>
                <div slot="body">
                    <div style="width:100%;" :style="{height:opt1.height+'px'}">
                        <chart :init-options="opt1" autoresize :options="transform_channel_chars_data" ref="bar2"
                            style="width: 100%;">
                        </chart>
                    </div>
                </div>
            </mad-panel>
        </el-card>
    </el-col>
```
#### 消耗环比 绑定数据为 *costChartData*

```<el-col :span="12">
        <el-card class="box-card">
            <div slot="header" class="clearfix">
                <el-row>
                    <el-col :span="8">消耗环比</el-col>
                    <el-col :span="8">&nbsp;</el-col>
                    <el-col :span="8">
                        <el-select v-model="consume_data" size="small">
                            <el-option v-for="item in consume_channel_data" :key="item.value" :label="item.title" :value="item.value">
                            </el-option>
                        </el-select>
                    </el-col>
                </el-row>
            </div>
            <mad-panel class="panel-has-select">
                <div slot="header">
                    &nbsp;
                </div>
                <div slot="body">
                    <div style="height:418px;width:100%">
                        <chart :options="costChartData" ref="bar3" autoresize style="width:100%"></chart>
                    </div>
                </div>
            </mad-panel>
        </el-card>
    </el-col>
```
#### 转化流程漏斗图 绑定数据为 *transform_mchars_data*
>因为echars没有提供原型上面的样式，些组件为自定义组件
```
    <el-col :span="12"  >
        <el-card class="box-card">
            <div slot="header" class="clearfix">
                <el-row>
                    <el-col :span="8">转化流程漏斗图</el-col>
                    <el-col :span="8">&nbsp;</el-col>
                    <el-col :span="8">
                        <el-select v-model="transform_data" size="small">
                            <el-option v-for="item in transform_channel_data" :key="item.value" :label="item.title" :value="item.value">
                            </el-option>
                        </el-select>
                    </el-col>
                </el-row>
            </div>
                <mad-panel class="panel-has-select" style="height:443px;">
                    <div slot="header">
                       &nbsp;
                    </div>
                    <div slot="body" style="width: 100%;overflow:hidden;">
                        <mad-charts :data="transform_mchars_data"></mad-charts>
                    </div>
                </mad-panel>
        </el-card>
    </el-col>
```

#### 页面顶部五个模块显示的数据 为 *block_datas*
#### 页面右上角的通知组件为 *dev\js\atd\dashboard\notifyMessage*
