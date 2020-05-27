# performad说明文档 Audience
### [audience create ]( http://performad.dev.onemad.com/platform/atd/main#/atd/audience/create/1 )
### 页面引用路径 
>E:\workspace\workspace\platform_performad\dev\js\atd\audience\create\index.vue

#### audience创建分为二种。，即下面那个组件。地址中参数1代表的是第一种 。2是代表的是第二种 创建 ，模板中有明确的表标
```
	 	   
    <look-like-diy-create v-if="type==1" ref="audience"></look-like-diy-create>
    <look-like-create  v-if="type==2" ref="audience"></look-like-create>
			
	import LookLikeDiyCreate from '../../facebook/looklike/diy/create.along.vue'
	import LookLikeCreate from '../../facebook/looklike/looklike/create.along.vue'
```

#### 编辑audience也是相同的写法