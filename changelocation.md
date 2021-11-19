```js
wx.getLocation({
      type: 'gcj02',
      isHighAccuracy: true,
    }).then(res => {
      console.log(res)
      //持续更新用户地理位置
      const _locationChange = function (e) {
        console.log('locaiton change', e)
      }
      wx.authorize({
        scope: 'scope.userLocationBackground',
        success: function (res) {
          wx.startLocationUpdateBackground({
            success: (res) => {
              console.log(res);
              wx.onLocationChange(_locationChange)
            },
          })
        },
        fail: function (res) {
          console.log(res);
          wx.showModal({
            content: '获取定位权限失败，请前往设置打开“使用小程序和离开小程序后”',
            confirm: '设置',
            success(res) {
              if (res.confirm) {
                wx.openSetting()
              } else if (res.cancel) {
                wx.onLocationChange(_locationChange)
              }
            }
          })
        },
      })
      let dic = position
      var a = []
      for (var key in position) {
        a = that.distance(res.latitude, position[key][0], res.longitude, position[key][1])
        console.log(a)
        dic[key] = a
      }
      var result = Object.keys(dic).sort(function (a, b) {
        return dic[a] - dic[b]
      })
      let POI = result[0]
      console.log(result)
      console.log(POI)
      console.log(dic)
      if (dic[POI] < 100) {
        app.globalData.POI = POI
        wx.hideLoading({
          success: (res) => {
            wx.navigateTo({
              url: '/pages/AR/index?id=' + useropenid + '&position=' + POI,
            })
          },
        })
      } else {
        app.globalData.POI = ''
        wx.hideLoading({
          success: (res) => {
            wx.navigateTo({
              url: '/pages/AR/index?id=' + useropenid + '&position=' + '',
            })
          },
        })
      }
    }).catch(err => {
      wx.hideLoading({
        success: (res) => {
          wx.getSetting({
            success(res) {
              that.setData({
                disabled: false
              })
              console.log(res.authSetting["scope.userLocation"])
              if (res.authSetting["scope.userLocation"] == false) {
                wx.showModal({
                  title: '获取定位权限',
                  content: '需要授权定位',
                  showCancel: true,
                  cancelText: '取消',
                  confirmText: '确定',
                  success: res => {
                    if (res.confirm) {
                      wx.openSetting({
                        success(res) {
                          console.log(res)
                        }
                      })
                    } else {
                      wx.showToast({
                        title: '需要授权定位',
                        duration: 1000,
                        icon: 'none'
                      })
                    }
                  }
                })
              } else {
                wx.showToast({
                  title: '请打开定位',
                  duration: 2000,
                  icon: 'none',
                  success() {
                    that.setData({
                      disabled: false
                    })
                  }
                })
              }
            }
          })
        },
      })

    })
    console.log(useropenid)
    // wx.navigateTo({
    //   url: '../AR/index?id='+openid,
    // })
  },
```

