# HarmonyOSQuickPack
鸿蒙系统，内部应用打包工具，一键打包，彻底解放您的双手！告别提交应用商店审核限制！

## 内部测试Profile申请及应用签名配置流程

内部测试包，必须使用内部测试Profile文件进行签名，方可进行安装下载。

### 内部测试Profile申请

申请内部测试Profile步骤和申请正式的Profile步骤是一样的。

1、登录[AppGallery Connect](https://developer.huawei.com/consumer/cn/service/josp/agc/index.html#/)，选择“证书、APP ID和Profile”。

![profile_001.jpg](profile_001.jpg)

2、在左侧导航栏选择“证书、APP ID和Profile > Profile”，进入“Profile”页面，点击右上角“添加”。

![profile_002.jpg](profile_002.jpg)

3、这里选择内部测试，其他的信息和你之前申请发布类型一样。

![profile_003.jpg](profile_003.jpg)


| 参数       | 说明 |
|----------| ------------------------------------------------------------------------------------------------------------------------------------------ |
| 应用名称     | 选择需要申请内部测试Profile的应用名称。如尚未新建应用，请先[前往“APP ID”菜单创建应用](https://developer.huawei.com/consumer/cn/doc/app/agc-help-create-app-0000002247955506)。 |
| 包名       | 选择应用名称后自动填充。                                                                                                                               |
| Profile名称 | 不超过100个字符。                                                                                                                                 |
| 类型       | 选择“内部测试”。                                                                                                                                  |
| 选择证书     | 点击“选择”，选择一个[发布证书](https://developer.huawei.com/consumer/cn/doc/app/agc-help-release-cert-0000002283336729)。                                |
| 选择设备     | 点击“选择”，选择一个或多个[测试设备](https://developer.huawei.com/consumer/cn/doc/app/agc-help-add-device-0000002283189937)。最多可选择100个设备，已删除的设备无法选择。


#### 注意事项

选择证书时，请选择发布证书，测试设备一定要选择，否则未添加至设备的手机是无法进行安装应用的。

内部测试Profile生成之后，下载保存！

### 应用签名配置流程

正式签名是如何配置的，那么内部测试仿照即可。

首先配置内部测试签名信息，仿照正式签名信息复制一份即可，名字自定义，需要注意，profile字段，设置成你创建的内部测试Profile，其他信息和正式签名保持一致。

```json
"signingConfigs": [
      //……其他签名信息
      {
        "name": "internalTesting",
        "type": "HarmonyOS",
        "material": {
          "storeFile": "你的秘钥.p12文件",
          "storePassword": "你的秘钥密码",
          "keyAlias": "你的别名",
          "keyPassword": "你的别名密码",
          "signAlg": "SHA256withECDSA",
          "profile": "你的内部测试Profile文件也就是.p7b文件",
          "certpath": "你的发布证书.cer文件"
        }
      }
    ]
```

配置品类信息，复制其他品类信息，增加内部测试品类信息，名字自定义，signingConfig选择内部测试签名信息。

```json
"products": [
     //……其他品类
      {
        "name": "internalTesting",
        "signingConfig": "internalTesting",
        "compatibleSdkVersion": "5.0.0(12)",
        "targetSdkVersion": "5.0.0(12)",
        "runtimeOS": "HarmonyOS",
        "buildOption": {
          "strictMode": {
            "useNormalizedOHMUrl": true
          }
        }
      }
    ]
```

配置modules信息，记住，modules下，你有几个模块，就要配置几个模块，并且每个模块下的applyToProducts配置要保持一致，
并把内部测试品类信息配置上。

```json
"modules": [
    {
      "name": "entry",
      "srcPath": "./entry",
      "targets": [
        {
          "name": "default",
          "applyToProducts": [
            "default",
            "internalTesting"
          ]
        }
      ]
    }
  ]
```

### 相关总结

以上信息配置完成之后，就可以在插件中，选择你的内部测试品类，进行一键打包更新了，当然了，插件中的配置也不要忘记哦~

