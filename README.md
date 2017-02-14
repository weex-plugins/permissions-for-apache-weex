# weex-permissions

## 背景

在Android 6.0 (API 23)及以上版本的系统中引入了动态权限申请机制，对于级别为Dangerous的权限，除了需要在AndroidManifest.xml中声明以外，在使用到相关权限的代码中还需要对权限做动态申请，否则默认不给予该权限。为了对API 23及以上版本系统进行适配，因此提供weex-permission插件允许用户在js中对特定权限的授予情况进行检查和动态申请敏感权限。

## 项目地址
[weex-permissions](https://github.com/weex-plugins/weex-permissions)

## API
### `checkPermission(permission, callback)`
检查当前是否拥有指定权限
#### 参数
- `permission {string}`: 权限ID，详情见[Mainfest.permission](https://developer.android.com/reference/android/Manifest.permission.html)，注意，某些厂商的设备可能会包厂商自定义的权限。
- `callback {function}`: 回调函数
 - `granted {boolean}` : 是否拥有该权限
 - `permission {string}` : 请求权限的id
 - `state {string}` : 该次请求是否成功，可能值为success/cancel/failed
 - `message {string}` : 在`state`为cancel/failed时附带的描述信息    
     
     
 
### `requestPermission(permission, rationale, callback)`
请求权限
#### 参数
 - `permission {string}`: 权限ID
 - `rationale {object}`: 对于为何需要请求该权限的描述，如果应用曾经请求过该权限并且被用户拒绝了，那么在再次请求权限的时候会通过展示该信息来向用户说明应用为何需要该权限，该参数是可选的，如果你不想在请求权限前向用户解释为何需要该权限，那么可以在此处传入一个空对象
  - `title {string}` : 描述的标题
  - `message {string}`: 描述的内容，应该清晰明了    
 
 - `callback {function}`: 回调函数
  - `granted {boolean}` : 是否拥有该权限
  - `permission {string}` : 请求权限的id
  - `state {string}` : 该次请求是否成功，可能值为success/cancel/failed
  - `message {string}` : 在`state`为cancel/failed时附带的描述信息  
 
 ## 注意
 - **动态请求的权限必须事先在`AndroidManifest.xml`中进行声明**
 - Android需依赖**0.9.5**及以上版本的SDK
 
 ## 示例
 ```vue
<template>
    <div>
        <text style="font-size: 60px" onclick="{{doCheck}}">Check Permission</text>
        <text>{{check}}</text>
        <text style="font-size: 60px; margin-top: 60px" onclick="{{doRequest}}">Request Permission</text>
        <text>{{request}}</text>
    </div>
</template>

<script>
    var permissions = require('@weex-module/permissions');
    module.exports = {
        data: {
            check: "-",
            request: '-',
        },
        methods: {
            doCheck: function () {
                var self = this;
                permissions.checkPermission("android.permission.ACCESS_NETWORK_STATE", function (e) {
                    self.check = e;
                });
            },
            doRequest: function () {
                var self = this;
                permissions.requestPermission("android.permission.ACCESS_FINE_LOCATION", {"title":"PLEASE","message":"A cute message"}, function (e) {
                    self.request = e;
                });
            }
        }
    }
</script>
 ```
 
