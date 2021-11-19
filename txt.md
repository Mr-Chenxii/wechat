1.js.data里的数据会一直保存着吗？
          用户关闭小程序，重新进入页面，页面数据的data是固定值？
                 通过this.setData修改js.data里的数据能否修改视图层的数据？

2.我这样子修改

```js
data:{
    data:[{
        position:'双子塔',
        status:0,
    },{
        position:'15栋'
        status:0
    }]
}


this.setData({
    [data.data[0].status]:1
})
```

修改之后数组[0]里面的position就不见了，数组[1]里的所有数据也不见了

就只剩数组[0].status数据了，是什么地方有错误吗

