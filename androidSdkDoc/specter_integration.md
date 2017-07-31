# Specter SDK(V1.0)使用手册

## 配置SDK

 1.引入SDK包：specter.jar

* ![](/assets/pic1.png)

* ![](/assets/pic2.png)
* 请在入口activity或者application添加以下代码

```java
public class MainActivity extends AppCompatActivity {
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	super.onCreate(savedInstanceState);
        	setContentView(R.layout.activity_main);
        	SPLog.setLevel(SPLog.VERBOSE);//设置log显示等级 默认不显示log
               //初始化sdk
        	SpecterAPI.getInstance(this, "你的appkey");
                //或者
            SpecterAPI.getInstance(this, "你的appkey"，"渠道编号");
            //也可以直接使用这个方法打开log
            SpecterAPI.getInstance(this, "你的appkey"，"渠道编号",boolean openDebugLog);
            //disableBindingUI 选择true的情况下 摇一摇功能将被禁用
            SpecterAPI.getInstance(this, "你的appkey"，"渠道编号",boolean openDebugLog，boolean  disableBindingUI);


    	}
}
```
2.配置权限

```xml

<?xml version="1.0" encoding="utf-8"?>
    <uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
	<uses-permission android:name="android.permission.BLUETOOTH" />
```
3.配置渠道
* 渠道配置提供了两种方 代码配置 和 Manifest.xml配置
* Manifest配置如下
```xml
<?xml version="1.0" encoding="utf-8"?>
<meta-data
    	android:name="SPECTER_CHANNEL"
    	android:value="您的渠道名称" />
```
* 代码埋点可在sdk初始化的时候配置
```java
SpecterAPI.getInstance(this, "appkey","您的渠道名称");
```

4.自定义事件(手动埋点)示例代码，提供了3个方法。

```java
    SpecterAPI specterApi = SpecterAPI.getInstance(this, "appkey","您的渠道名称");
    specterApi.track("事件名称");//直接传字符串 最终调用specterApi.track(JSONObject)
    specterApi.track(Map<String,Object>);//传map 最终调用specterApi.track(JSONObject)
    specterApi.track(JSONObject);//传json串
```
* 完整的事件上报字段，请注意带有'$'的字段为默认属性，请不要在设置自定义属性的时候使用带有'$'开始的属性名称

```json
 {
    "event": "事件名称",
    "properties": {
        "$sp_lib": "android",
        "$os": "android",
        "$os_version": "6.0",
        "$manufacturer": "HUAWEI",
        "$brand": "HUAWEI",
        "$model": "HUAWEI NXT-AL10",
        "$screen_dpi": 440,
        "$screen_height": 1920,
        "$screen_width": 1080,
        "$app_version": "5.1.0",
        "$app_version_string": "5.1.0",
        "$app_release": 21510,
        "$app_build_number": 21510,
        "$has_nfc": true,
        "$has_telephone": true,
        "$wifi": true,
        "$bluetooth_enabled": true,
        "$bluetooth_version": "ble",
        "$fpid": "",
        "$channel": "渠道",
        "$capacity": "24.61 GB",
        "$memory": "2.73 GB",
        "$cpu_count": 8,
        "$cpu_serial": "",
        "$imei": "861918038151750",
        "$imsi": "null",
        "$cpu_speed": "480.0 - 1805.0MHZ",
        "$cpu_abi": "arm64-v8a,armeabi-v7a,armeabi",
        "$cpu_type": "Hisilicon Kirin 950",
        "$network_type": "WiFi",
        "$language": "zh",
        "$operator": "null",
        "$mac": "dc:d9:16:af:d0:04",
        "$dns": "192.168.0.43",
        "$ip": "10.15.235.95",
        "$proxy_ip": "null",
        "$wifi_name": "niwodai",
        "$platform": "android",
        "$event_time": 1499741554674,
        "$sdk_version": "1.0.0",
        "$latitude": 31.220296,
        "$longitude": 121.530789,
        "$current_url": "com.chinaideal.bkclient.tabmain.homepage.HomeMainAc",
        "$token": "f4dab211bed00312e5a7be6193c76a7a",
        "$appkey": "f4dab211bed00312e5a7be6193c76a7a",
        "super1": "1",//用户自定义的属性
        "super2": "2",
        "$time": 1499741554,
        "$distinct_id": "测试",
        "$session_id": "1499741554438",
        "$el_text": "精选",//触发事件的view的文本或者内部所有子控件的文本
        "$from_binding": true,
        "$position_x": 45,
        "$position_y": 1683,
        "$scroll_width": 330,
        "$scroll_height": 138,
        "$event_type": "click",
        "$path": [//触发事件的view的路径
            {
                "prefix": "shortest",
                "index": 0,
                "id": 16908290
            },
            {
                "view_class": "com.bricks.widgets.base.BaseContextView",
                "index": 0
            },
            {
                "sp_id_name": "dl_filter",
                "index": 0
            },
            {
                "view_class": "android.widget.LinearLayout",
                "index": 0
            },
            {
                "sp_id_name": "bottom_navigation_bar",
                "index": 0
            },
            {
                "sp_id_name": "bottom_navigation_bar_container",
                "index": 0
            },
            {
                "sp_id_name": "bottom_navigation_bar_item_container",
                "index": 0
            },
            {
                "view_class": "com.nwd.bottomnavigationbar.NwdShiftingBottomNavigationTab",
                "index": 0
            }
        ]
    }
}
```

5.数据发送机制，目前只要有数据缓存在本地1分钟后将会自动发送数据，specter也提供立即发送数据的功能

```java
    SpecterAPI specterApi = SpecterAPI.getInstance(this, "appkey","您的渠道名称");
    specterApi.flush();//立即发送数据
```
6.记录事件时间 specter 提供了记录时间的功能。

```java
     specterApi.timeEvent("事件名称");//开始记录 填写需要记录的事件名称
     specterApi.track("事件名称");//结束 记录的事件名称将被记录时长
     specterApi.track("事件名称",JSONObject);//同上
     specterApi.track("事件名称",Map<String,Object>);//同上
```

7.混淆配置 如果您的项目需要混淆请在proguard-project.text（具体文件根据开发者自己配置来确定）文件中添加以下配置

```json
    -dontwarn com.specter.codeless**
    -keep class com.specter.codeless**{*;}
```
8.超级属性 如果您有一些属性需要每次都要记录您不需要每次track的时候才写进去可以通过如下代码进行设置

```java
    //示例代码
    Map<String, Object> superProperties = new LinkedHashMap<>();
    superProperties.put("super1", "1");
    superProperties.put("super2", "2");
    specterAPI.registerSuperPropertiesMap(superProperties);//绑定超级属性
    //或者使用
    specterAPI.registerSuperProperties(JSONObject);
    //带Once与不带Once的区别在于 Once不会替换原有的属性 如果已经存在属性就不会替换
    specterAPI.registerSuperPropertiesOnce(JSONObject);
    specterAPI.registerSuperPropertiesOnceMap(Map<String,Object>);

```
* 配置之后所有上报的事件中就会有您自定义的字段了
* ```json
{
    "event": "$page_duration",
    "properties": {
        ......
        "super1": "1",
        "super2": "2",
        ......
    }
}
```
* 移除超级属性可以使用如下代码
```java
    specterAPI.unregisterSuperProperty("事件名称");
    //清除所有超级属性
    specterAPI.clearSuperProperties();
```



9.替换默认匿名id,默认情况系统会自动生成一个id，如果需要特殊指定可以使用以下方法用来区分用户

```java
  specterAPI.identify("自定义匿名id");
```
```json
{
  "event": "$page_duration",
  "properties": {
      ......
      "super1": "1",
      "super2": "2",
      "$distinct_id":"自定义匿名id",//identify设置之后会将值存入$distinct_id发送到后台
      ......
  }
}
```



9.查看log信息，很多时候开发者需要知道自己定义的事件或者其他信息有没有被成功的记录，我们也提供了一些日志信息供参考，希望可以帮到开发者
	   * 使用SPlog.setLevel(SPlog.常量);即可在logcat中查看log
	   * 常用的Regex	
	
```java
    SpecterAPI.Messages 查看所有用track相关的log
    SpecterAPI.EditorCnctn查看websocket相关log
    SpecterAPI.ViewCrawler查看控件绑定相关log
    SpecterAPI.DChecker查看部署的事件信息log
    如需查看更多log，请直接使用SpecterAPI即可。

```	

