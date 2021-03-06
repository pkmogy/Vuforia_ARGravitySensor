Introduction
============
使用手機上的重力感測器，將現實的重力數據傳入到Unity AR遊戲中，讓物件受到重力時會顯得更真實。
* 本專案使用 Unity 2018.1.6f1
* 影像辨識使用 Vuforia 7.2.24
  - 專案有使用 Vuforia UserDefinedTargetBuilder Function，需匯入Vuforia Core Samples。
  https://assetstore.unity.com/packages/templates/packs/vuforia-core-samples-99026
  
* 專案測試時一定要在手機上測試，手機上才有重力感測器

<img src="https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Image/Screenshot.jpg" height="315" width="200" /> <img src="https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Image/Screenshot02.jpg" height="315" width="200" /> <img src="https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Image/Screenshot03.jpg" height="315" width="200" /> <img src="https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Image/Screenshot04.jpg" height="315" width="200" /> 


About Gravity Sensor
============

重力感測器可以提供重力大小的三維向量，可以參考Android Developer 的介紹。

https://developer.android.com/guide/topics/sensors/sensors_motion#sensors-motion-grav

<img src="https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Image/Gravity%20Sensor.png" height="418" width="962" />

Unity API 中的 Input.gyro.gravity 可以取得重力感測器。

https://docs.unity3d.com/ScriptReference/Gyroscope-gravity.html


Vuforia + Gravity Sensor
============
事前AR Unity 環境設定 - 切換手機平台、世界中心為裝置攝影機、攝影機X軸旋轉90度
* **Platform = Android**
* World Center Mode = DEVICE
* ARCamera Rotation x = 90

首先要將感測器啟動，並取得重力向量。由於感測器的方向軸與Unity不相同，要自己轉換方向。
轉換方式很簡單，當手機螢幕朝上時上方為Z軸，對應到Unity物件上方為Y軸，所以對物件施加重力時轉換方式

為：Vector3(GravitySensor.x, GravitySensor.z, GravitySensor.y)

最後再將轉換的向量給物件的Rigidbody velocity就完成了！

```C#

// Enable gravity by gyroscope

Input.gyro.enabled = true;


// Use a gravity sensor and convert vector 

m_gravitySensor = Input.gyro.gravity;
Vector3 TransGravity = new Vector3(m_gravitySensor.x, m_gravitySensor.z, m_gravitySensor.y);
m_rigidbody.velocity = TransGravity;
  
```

* 詳細請至 GravitySensor.cs 中查看。
 - https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Project/Assets/Scripts/GravitySensor.cs

* 輸出的APK測試檔案
 - https://github.com/Yan-Jun/Vuforia_ARGravitySensor/blob/master/Project/Output/ARGravity.apk


Other Scripts Information
============
攝影機對焦模式設定(Camera Focus Mode)、閃光燈設定(Flash Torch)、重啟相機設定(Restart Camera)。
* Vuforia Core Samples 套件內的 Common > Scripts > CameraSettings.cs
https://library.vuforia.com/articles/Solution/Working-with-the-Camera#Camera-Focus-Modes

攝影機擴展辨識(Extended Tracking)。
* Vuforia Core Samples 套件內的 Common > Scripts > TrackableSettings.cs
https://library.vuforia.com/articles/Training/Extended-Tracking.html
