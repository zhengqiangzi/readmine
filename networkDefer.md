#### 为了解决多次点击产生的ajax请求，而导致数前后不一致的问题，特此使用些方法

```
    this.spliceNumFn = new NetWorkDefer({
      callBack: data => {//完成后数据回调
        this.activeNumber = data || 1;
      },
      networkCallBack: data => {//网络状态回调
        //	console.log(data)
      }
    });
    //添加请求方法
    this.spliceNumFn.add(
        this.$api("Atd_Ads_Fb").getReachNum,//方法名
        this.getResultData()//请求参数
    )

export default class NetWorkDefer {

    constructor({ callBack, networkCallBack }) {

        this.list = [];
        this.status = false;
        this.timer = null;
        this.networkCallBack = networkCallBack;
        this.callBack = callBack;

        this.start()
    }
    start() {

        this.timer = setInterval(() => {

            if (this.status == true || this.list.length <= 0) return;
            this.status = true;
            let promise = this.list.pop();
            this.list = []
            this.networkCallBack({ type: true });
            this.status = true;
            promise.fn.call(null, promise.params).then((data) => {

                this.status = false;
                this.networkCallBack({ type: false });
                this.callBack(data);

            }).catch((e) => {
                this.status = false;
                this.networkCallBack({ type: false });
                this.callBack(e)

            });

        }, 200)

    }
    add(_fn, _params) {

        this.list.push({ fn: _fn, params: _params });
    }
}
```