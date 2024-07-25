# 高值耗材柜 JRI-CB-C1 SDK使用说明

### [📖中文文档](README.md) | 📖English Document

This document is the standard integration guide document for the SDK of the Smart RFID Cabinet JRI-CB-C1 (hereinafter referred to as the cabinet), used to explain the usage of the cabinet SDK. The default readers are already familiar with the basic usage of Android Studio, familiar with the basic syntax of Kotlin, and have a certain foundation in Android programming.

The cabinet SDK contains two `aar` packages, namely `jri-manager-core-*.aar` located in [aar/core](/aar/core/) and `jri-manager-cb-c1-*.aar` located in [aar/c1](/aar/c1/). Please import both latest versions of the `aar` package into your project.

## Update Log

### 1.0.4 📅`2024.03.04`

[Click to view more update records](CHANGE-LOG.md)

## Import SDK

### Copy File

1. Download the latest version of `jri-manager-core-*.aar` file from the `core` folder.
2. Download the latest version of `jri-manager-cb-c1-*.aar` file from the `c1` folder.
3. Copy the two files to the `libs` folder in your project.

### Add Dependency

```
dependencies {
    implementation files('libs/jri-manager-core-*-release.aar')
    implementation files('libs/jri-manager-cb-c1-*-release.aar')
}
```

### Import Package

```
import com.jrfid.manager.cabinet.c1.JRIDevicesManager
```

## Function Introduction

### Init Connect Device

Init connect device is a long-running task,recommend using kotlin coroutines.

```
lifecycleScope.launch(Dispatchers.IO) {
    JRIDevicesManager.instance.initDevices()
}
```

### Device Control

#### IC/ID Module

##### Listening IC/ID card data

```
JRIDevicesManager.instance.addOnReceivedICCardDataCallback(object : ReceivedICCardDataCallback {
    override fun onReceived(model: ICCardPacketData) {

            
        }

    })
```

`ICCardPacketData`class description

```
class ICCardPacketData{

    val icCardNo: ByteArray

    val icCardNoText: String

}
```

| field        | description                  |
|--------------|------------------------------|
| icCardNo     | IC/ID card number byte array |
| icCardNoText | IC/ID card number string     |

#### Barcode Module

##### Listening barcode scanning data

```
JRIDevicesManager.instance.addOnReceivedQrCodeDataCallback(object : ReceivedQrCodeDataCallback {

            override fun onReceived(model: QrCodePacketData) {


            }

        })
```

`QrCodePacketData`class description

```
class QrCodePacketData{

    val dataText: String

}
```

| field    | description  |
|----------|--------------|
| dataText | barcode data |

#### Cabinet Door Control

##### Open the cabinet door

```
JRIDevicesManager.instance.openTheDoor()
```

##### Listening door state

```
JRIDevicesManager.instance.addOnReceivedBasicDataCallback(object : ReceivedBasicDataCallback {

            override fun onDoorOpen() {


            }


            override fun onDoorClose() {


            }

        })
```

`ReceivedBasicDataCallback`interface description

| method            | description                  |
|-------------------|------------------------------|
| fun onDoorOpen()  | callback when door is opened |
| fun onDoorClose() | callback when door is closed |

#### UHF Control

##### Start inventory

```
JRIDevicesManager.instance.startUhfInventory()
```

##### Listening inventory result

```
JRIDevicesManager.instance.addOnReceivedUhfInventoryDataCallback(object:ReceivedUhfInventoryDataCallback{

            override fun onReceivedTag(tag: UHFTagInfoModel) {

                

            }


            override fun onInventoryEnd() {

                

            }

    })
```

`ReceivedUhfInventoryDataCallback`interface description

| method                                            | description                   |
|---------------------------------------------------|-------------------------------|
| fun onReceivedTag(tag: UHFTagInfoModel)           | callback when inventory tag   |
| fun onInventoryEnd()                              | callback when inventory end.  |

`UHFTagInfoModel`class description

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

| field/method              | description                                                                                 |
|---------------------------|---------------------------------------------------------------------------------------------|
| freq                      | working frequency byte data,detail information viewing the follow "Working frequency table" |
| pc                        | tag pc byte data                                                                            |
| epc                       | tag epc byte data                                                                           |
| rssi                      | tag rssi                                                                                    |
| fun getFreqText(): String | working frequency string,contain unit:MHz.                                                  |
| fun getEpcText(): String  | tag epc string                                                                              |

##### Working frequency table

| parameter | value      | parameter    | value      |
|-----------|------------|--------------|------------|
| 0(0x00)   | 865.00 MHz | 30(0x1E)     | 913.50 MHz |
| 1(0x01)   | 865.50 MHz | 31(0x1F)     | 914.00 MHz |
| 2(0x02)   | 866.00 MHz | 32(0x20)     | 914.50 MHz |
| 3(0x03)   | 866.50 MHz | 33(0x21)     | 915.00 MHz |
| 4(0x04)   | 867.00 MHz | 34(0x22)     | 915.50 MHz |
| 5(0x05)   | 867.50 MHz | 35(0x23)     | 916.00 MHz |
| 6(0x06)   | 868.00 MHz | 36(0x24)     | 916.50 MHz |
| 7(0x07)   | 902.00 MHz | 37(0x25)     | 917.00 MHz |
| 8(0x08)   | 902.50 MHz | 38(0x26)     | 917.50 MHz |
| 9(0x09)   | 903.00 MHz | 39(0x27)     | 918.00 MHz |
| 10(0x0A)  | 903.50 MHz | 40(0x28)     | 918.50 MHz |
| 11(0x0B)  | 904.00 MHz | 41(0x29)     | 919.00 MHz |
| 12(0x0C)  | 904.50 MHz | 42(0x2A)     | 919.50 MHz |
| 13(0x0D)  | 905.00 MHz | 43(0x2B)     | 920.00 MHz |
| 14(0x0E)  | 905.50 MHz | 44(0x2C)     | 920.50 MHz |
| 15(0x0F)  | 906.00 MHz | 45(0x2D)     | 921.00 MHz |
| 16(0x10)  | 906.50 MHz | 46(0x2E)     | 921.50 MHz |
| 17(0x11)  | 907.00 MHz | 47(0x2F)     | 922.00 MHz |
| 18(0x12)  | 907.50 MHz | 48(0x30)     | 922.50 MHz |
| 19(0x13)  | 908.00 MHz | 49(0x31)     | 923.00 MHz |
| 20(0x14)  | 908.50 MHz | 50(0x32)     | 923.50 MHz |
| 21(0x15)  | 909.00 MHz | 51(0x33)     | 924.00 MHz |
| 22(0x16)  | 909.50 MHz | 52(0x34)     | 924.50 MHz |
| 23(0x17)  | 910.00 MHz | 53(0x35)     | 925.00 MHz |
| 24(0x18)  | 910.50 MHz | 54(0x36)     | 925.50 MHz |
| 25(0x19)  | 911.00 MHz | 55(0x37)     | 926.00 MHz |
| 26(0x1A)  | 911.50 MHz | 56(0x38)     | 926.50 MHz |
| 27(0x1B)  | 912.00 MHz | 57(0x39)     | 927.00 MHz |
| 28(0x1C)  | 912.50 MHz | 58(0x3A)     | 927.50 MHz |
| 29(0x1D)  | 913.00 MHz | 59(0x3B)     | 928.00 MHz |

#### Finger Vein Control

##### Recommended usage process

###### Get finger vein

get finger vein characteristic value in three stages -> composite finger vein template -> save finger vein template

###### Check finger vein

import finger vein template into the search library -> save the returned id and bind it to the user -> get finger vein characteristic value -> check if the finger vein characteristic value exists in the search library

##### Whether to place finger

After calling this method, it will return a result of type `Boolean`,`true` is finger placed on the module,otherwise `false`.

```
val result:Boolean = JRIDevicesManager.instance.checkFingerIn()
```

##### Whether to remove finger

After calling this method, it will return a result of type `Boolean`,`true` is finger removed from the module,otherwise `false`.

```
val result:Boolean = JRIDevicesManager.instance.checkFingerOut()
```

##### Waiting for finger placement

This operation is a blocking operation that needs to be placed in the coroutine for calling. After calling, the program will be blocked all the time. When it is detected that the finger is placed on the finger vein module, it will be released.

```
lifecycleScope.launch {
    JRIDevicesManager.instance.waitingFingerIn()
 }
```

##### Waiting for finger removed

This operation is a blocking operation that needs to be placed in the coroutine for calling. After calling, the program will be blocked all the time. When it is detected that the finger is removed from the finger vein module, it will be released.

```
lifecycleScope.launch {
    JRIDevicesManager.instance.waitingFingerOut()
}
```

##### Get finger vein characteristic value

Real time acquisition, only when the finger is correctly placed on the finger vein module and called, will immediately return the `String` type finger vein characteristic value, and in other cases, return `null`.

```
val result:String? = JRIDevicesManager.instance.getFingerVeinChara()
```

Blocking acquisition,this needs to be placed in the coroutine for calling. After calling, the program will be blocked until the finger vein characteristic value is obtained. If the finger vein characteristic value is not obtained within the set timeout, it will return `null`.
```
lifecycleScope.launch {
    val result: String? = JRIDevicesManager.instance.getFingerVeinChara(timeoutMillis = 1000)
}
```

##### Get finger vein template

Merge the finger vein characteristic value obtained in three stages into a finger vein template. Success return true, otherwise return `null`.

```
val result: String? = JRIDevicesManager.instance.createFingerVeinTemp("", "", "")
```


##### Continuously get finger vein characteristic value:boom:

By continuously obtaining the finger vein characteristic values in the form of `flow` in `kotlin`, users can take corresponding actions based on the returned `MODEL_TYPE` feedback.

```
lifecycleScope.launch {
    JRIDevicesManager.instance.getFingerVeinCharaFlow()
        .flowOn(Dispatchers.IO)
        .collect {
            if (it.type == FingerVeinDataModel.MODEL_TYPE_FINGER_IN) {
                Log.d("Please place finger")
            } else if (it.type == FingerVeinDataModel.MODEL_TYPE_FINGER_OUT) {
                Log.d("Please remove finger")
            } else if (it.type == FingerVeinDataModel.MODEL_TYPE_CHARA_DATA) {
                Log.d("finger vein characteristic value：${it.data}")
            }
        }
}
```

##### Continuously get finger vein template:boom:

By continuously obtaining the finger vein template in the form of `flow` in `kotlin`, users can take corresponding actions based on the returned `MODEL_TYPE` feedback.

```
lifecycleScope.launch {
    JRIDevicesManager.instance.getFingerVeinTempFlow()
        .flowOn(Dispatchers.IO)
        .collect {
            if (it.type == FingerVeinDataModel.MODEL_TYPE_FINGER_IN) {
                 Log.d("Please place finger")
            } else if (it.type == FingerVeinDataModel.MODEL_TYPE_FINGER_OUT) {
                 Log.d("Please remove finger")
            } else if (it.type == FingerVeinDataModel.MODEL_TYPE_TEMP_DATA) {
                 Log.d("finger vein template：${it.data}")
            }
         }
}
```
##### Add finger vein template into search library

```
val id:Long = JRIDevicesManager.instance.addFingerVeinTempIntoLib(temp:String)
```

| param         | description                           |
|---------------|---------------------------------------|
| temp:String   | create/get finger vein template value |

| return      | description                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| id:Long     | id>0 added successfully,and its value is the id value of the finger vein template in the search library.id<0 added failed,its value is error code. |


##### Remove finger vein template from search library

```
JRIDevicesManager.instance.removeFingerVeinTempFromLib(id:Long)
```
| param          | description                                             |
|----------------|---------------------------------------------------------|
| id:Long        | The id value returned when adding to the search library |

##### Remove all finger vein template from search library

```
JRIDevicesManager.instance.removeAllFingerVeinTempFromLib()
```

##### Check if finger vein characteristic value exist in the search library

```
val id:Long = JRIDevicesManager.instance.checkCharaIsExist(chara:String)
```
| param         | description                           |
|---------------|---------------------------------------|
| chara:String  | got finger vein characteristic value  |

| return      | description                                                                                                               |
|-------------|---------------------------------------------------------------------------------------------------------------------------|
| id:Long     | id>0 exist, the id value is the value returned by adding the finger vein template to the search library, id<=0 otherwise. |

#### Air Quality Module

##### Listening air quality data

```
JRIDevicesManager.instance.addOnReceivedAirQualityDataCallback(

    object : ReceivedAirQualityDataCallback {

        override fun onReceived(model: AirQualityPacketData) {


            }

        })
```

`AirQualityPacketData`class description

```
class AirQualityPacketData(val byteArray: ByteArray) {

    val temperatureNumber: Int

    val humidityNumber: Int

}
```

| field             | description                               |
|-------------------|-------------------------------------------|
| temperatureNumber | temperature value in degrees Celsius (℃). |
| humidityNumber    | air humidity value in percentage (%).     |

#### Disinfection Module

##### open the disinfection

* kotlin coroutine mode, after calling this method, it will return a result of type `Boolean`,`true` is open success,otherwise `false`.

```
lifecycleScope.launch(Dispatchers.IO){

    val result = JRIDevicesManager.instance.openUVSterilization()

}
```

* callback mode, open the successful callback `onSuccess`, otherwise callback `onFailed`.

```
JRIDevicesManager.instance.openUVSterilization(
 
   onSuccess = {
 
 
 
   }, onFailed = {
 
 
 
   })
```

##### close the disinfection

* kotlin coroutine mode, after calling this method, it will return a result of type `Boolean`,`true` is close success,otherwise `false`.

```
lifecycleScope.launch(Dispatchers.IO) {

  val result = JRIDevicesManager.instance.closeUVSterilization()

}
```

* callback mode, close the successful callback `onSuccess`, otherwise callback `onFailed`.

```
JRIDevicesManager.instance.closeUVSterilization(
  
    onSuccess = {
  
  
  
    }, onFailed = {
  
  
  
   })
```

