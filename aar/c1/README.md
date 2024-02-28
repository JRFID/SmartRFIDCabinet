# 高值耗材柜 JRI-CB-C1 SDK使用说明

本文是高值耗材柜 JRI-CB-C1（以下简称：耗材柜）SDK的标准的集成指南文档，用以说明耗材柜SDK的使用方法。默认读者已经熟悉Android Studio的基本使用方法，熟悉kotlin的基本语法，并且具有一定的Android编程基础。

## 引入SDK

### 复制aar

1. 下载`core`文件夹中的最新版本`jri-manager-core-*.aar`文件。
2. 下载`c1`文件夹中的最新版本`jri-manager-cb-c1-*.aar`文件。
3. 将上面下载的两个文件复制到你的项目中`libs`文件夹中。

### 添加依赖

```
dependencies {
    implementation files('libs/jri-manager-core-1.0.2-release.aar')
    implementation files('libs/jri-manager-cb-c1-1.0.4-release.aar')
}
```

### 导入包

````
import com.jrfid.manager.cabinet.c1.JRIDevicesManager
````

## 功能使用

### 初始化连接设备

初始化连接设备是一个耗时的超过，推荐使用协程来处理。

```
lifecycleScope.launch(Dispatchers.IO) {
    JRIDevicesManager.instance.initDevices()
}
```

### 监听设备数据

#### IC/ID模块

监听IC/ID卡数据

```
JRIDevicesManager.instance.addOnReceivedICCardDataCallback(object : ReceivedICCardDataCallback {
    override fun onReceived(model: ICCardPacketData) {

            
        }

    })
```

`ICCardPacketData`类说明

```
class ICCardPacketData{

    val icCardNo: ByteArray

    val icCardNoText: String

}
```

| 属性          |     说明          |
| ------------ | ----------------- |
| icCardNo     | IC/ID的卡号字节数组 |
| icCardNoText | IC/ID的卡号字符串   |












