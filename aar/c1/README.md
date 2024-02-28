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
    implementation files('libs/jri-manager-core-*-release.aar')
    implementation files('libs/jri-manager-cb-c1-*-release.aar')
}
```

### 导入包

```
import com.jrfid.manager.cabinet.c1.JRIDevicesManager
```

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

#### 条码模块

监听条码扫描数据

```
JRIDevicesManager.instance.addOnReceivedQrCodeDataCallback(object : ReceivedQrCodeDataCallback {

            override fun onReceived(model: QrCodePacketData) {


            }

        })
```

`QrCodePacketData`类说明

```
class QrCodePacketData{

    val dataText: String

}
```

| 属性     | 说明             |
| -------- | ------------ |
| dataText | 扫描到的条码数据 |

#### 柜门控制

打开柜门

```
JRIDevicesManager.instance.openTheDoor()
```

柜门状态监听

```
JRIDevicesManager.instance.addOnReceivedBasicDataCallback(object : ReceivedBasicDataCallback {

            override fun onDoorOpen() {


            }


            override fun onDoorClose() {


            }

        })
```

`ReceivedBasicDataCallback`回调方法说明

| 方法              | 说明             |
| ------------------- | ------------------ |
| fun onDoorOpen()  | 柜门打开时的回调 |
| fun onDoorClose() | 柜门关闭时的回调 |

#### 超高频模块控制

开始盘存

```
JRIDevicesManager.instance.startUhfInventory()
```

获取盘存结果

```
JRIDevicesManager.instance.addOnReceivedUhfInventoryDataCallback(object:ReceivedUhfInventoryDataCallback{

            override fun onReceivedTag(tag: UHFTagInfoModel) {

                

            }


            override fun onInventoryEnd() {

                

            }

    })
```

`ReceivedUhfInventoryDataCallback`回调方法说明

| 方法                               | 说明                                         |
| --------------------------------- |--------------------------------------------|
| fun onReceivedTag(UHFTagInfoModel) | 盘存到标签回调，标签详细信息见下方`UHFTagInfoModel`类说明。 |
| fun onInventoryEnd()               | 本次盘存结束回调                                   |

`UHFTagInfoModel`类说明

```
class UHFTagInfoModel {

    val freq: Int

    val pc: ByteArray

    val epc: ByteArray

    val rssi: Int

    fun getFreqText(): String

    fun getEpcText(): String

}
```

| 属性/方法                      | 说明                                                             |
| --------------------------- | ------------------------------------------------------------------ |
| freq                      | 盘存到标签的工作频率参数，对应的频率见下方《工作频率参数对应表》 |
| pc                        | 标签的pc字节数据                                                 |
| epc                       | 标签epc字节数据                                                  |
| rssi                      | 盘存到标签的rssi值                                               |
| fun getFreqText(): String | 盘存到标签的工作频率字符串，包含单位：MHz。 |
| fun getEpcText(): String  | epc字符串 |


#### 指静脉模块控制

#### 空气质量模块

监听空气质量数据

```
JRIDevicesManager.instance.addOnReceivedAirQualityDataCallback(

    object : ReceivedAirQualityDataCallback {

        override fun onReceived(model: AirQualityPacketData) {


            }

        })
```

`AirQualityPacketData`类说明

```
class AirQualityPacketData(val byteArray: ByteArray) {

    val temperatureNumber: Int

    val humidityNumber: Int

}
```

| 属性              | 说明                            |
| ------------------- | --------------------------------- |
| temperatureNumber | 温度值，单位为摄氏度（℃）。    |
| humidityNumber    | 空气湿度值，单位为百分比（%）。 |

#### 消毒模块控制

开启消毒

* 协程方式调用，直接返回`Boolean`类型结果，返回`true`代表开启成功，其它结果表示开启失败。

```
lifecycleScope.launch(Dispatchers.IO){

    val result = JRIDevicesManager.instance.openUVSterilization()

}
```

* 回调方式调用，开启成功回调`onSuccess`，其它结果回调`onFailed`。

```
JRIDevicesManager.instance.openUVSterilization(
 
   onSuccess = {
 
 
 
   }, onFailed = {
 
 
 
   })
```

关闭消毒

* 协程方式调用，直接返回`Boolean`类型结果，返回`true`代表关闭成功，其它结果表示关闭失败。

```
lifecycleScope.launch(Dispatchers.IO) {

  val result = JRIDevicesManager.instance.closeUVSterilization()

}
```

* 回调方式调用，关闭成功回调onSuccess，其它结果回调onFailed。

```
JRIDevicesManager.instance.closeUVSterilization(
  
    onSuccess = {
  
  
  
    }, onFailed = {
  
  
  
   })
```

