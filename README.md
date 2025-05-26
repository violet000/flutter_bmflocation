# tms_mobile

#### 1、项目介绍
天津银行手持PDA(目前已经集成了红外线扫码以及AGPS定位)
- 插件集成：集成C71手持机厂商的DeviceAPI_ver20250209_release.aar包
- 应用框架：Flutter 3.4.0 + Dart + Android SDK


#### 2、软件架构
### 2.1 Java环境
```
java --version
java 11 2018-09-25
Java(TM) SE Runtime Environment 18.9 (build 11+28)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11+28, mixed mode)
```
### 2.2 Gradle环境
```
Gradle 7.3.3
Build time:   2021-12-22 12:37:54 UTC
Revision:     6f556c80f945dc54b50e0be633da6c62dbe8dc71
Kotlin:       1.5.31
Groovy:       3.0.9
Ant:          Apache Ant(TM) version 1.10.11
JVM:          11 (Oracle Corporation 11+28)
OS:           Windows 10 10.0 amd64
```
### 2.3 Flutter环境
```
Flutter (Channel dev, 3.4.0-17.2.pre)
• Flutter version 3.4.0-17.2.pre on channel dev
• Framework revision d6260f127f
• Engine revision 3950c6140a
• Dart version 2.19.0
• DevTools version 2.16.0
```
### 2.4 Android环境
```
Android toolchain - develop for Android devices (Android SDK version 35.0.1)
• Android SDK at C:\Users\15590\Documents\AndroidSDKManager
• Platform android-35, build-tools 35.0.1
```


#### 3、flutter的AGPS定位插件集成描述
## 1.0.2
  1、解决了与地图Flutter插件编译冲突的问题；
  2、解决了iOS端输出位置对应坐标系不准确的问题；
  3、解决了对外输出POI列表某些情况下为空的问题。
## 1.0.3
  解决了一些崩溃问题
## 2.0.0-nullsafety.0
  适配null-safe
## 3.0.0
###新增： 
    1、定位结果返回Poi数据； 
    2、定位结果返回作弊概率； 
    3、新增地理围栏功能，支持圆形、多边形地理围栏； 
    4、新增移动热点识别功能； 
    5、新增隐私政策接口； 
###修复： 
    1、适配Flutter2.8.1； 
    2、整体架构优化； 
    3、修复遗留闪退问题；
## 3.1.0
    1、新增设备朝向API；
    2、修复Android释放listener问题；
    3、定位结果新增speed参数，iOS新增垂直精度和水平精度参数；
## 3.1.0+1
    1、新增城市编码，行政区划编码
    2、修复locationDetail字段赋值问题
## 3.2.0
    1、iOS端新增address字段对齐Android；
    2、Android端退出应用后重新进入后定位不回调BUG修复；
    3、优化Android端错误码；
    4、适配Flutter3.0；
    

#### 4、子项目tj_tms_mobile环境配置
### 4.1 Android配置修改
<!-- 修改 android/build.gradle，将 repositories 配置修改为： -->
```gradle
repositories {
    maven { url 'https://maven.aliyun.com/repository/google' }
    maven { url 'https://maven.aliyun.com/repository/public' }
    maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
    maven { url 'https://mirrors.tuna.tsinghua.edu.cn/flutter/download.flutter.io' }
    google()
    mavenCentral()
}
```
<!-- 将 dependencies 配置修改为： -->
dependencies {
    classpath 'com.android.tools.build:gradle:7.3.0'
    classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
}
<!-- 修改 android/gradle.properties -->
```properties
org.gradle.java.home=C:\\Users\\15590\\Documents\\JDK11
org.gradle.jvmargs=-Xmx1536M
android.useAndroidX=true
android.enableJetifier=true
systemProp.http.ssl.allowAll=true
systemProp.https.ssl.allowAll=true
org.gradle.welcome=never
org.gradle.unsafe.configuration=true
org.gradle.internal.http.connectionTimeout=120000
org.gradle.internal.http.socketTimeout=120000
org.gradle.internal.http.ssl.allowAll=true
org.gradle.internal.http.ssl.verify=false
```
<!-- 修改 gradle/wrapper/gradle-wrapper.properties -->
```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://mirrors.aliyun.com/macports/distfiles/gradle/gradle-7.4-all.zip
```

#### 5、APK打包配置
<!-- 签名配置 -->
<!-- 创建 key.properties 文件 -->
```properties
storePassword=itms_chengdu
keyPassword=itms_chengdu
keyAlias=key
storeFile=./key.jks
```
<!-- 修改 android/app/build.gradle -->
<!-- 在 android { 之前添加： -->
```gradle
def keystorePropertiesFile = rootProject.file("key.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
```
<!-- 在 android { 中添加： -->
```gradle
signingConfigs {
    release {
        keyAlias keystoreProperties['keyAlias']
        keyPassword keystoreProperties['keyPassword']
        storeFile file(keystoreProperties['storeFile'])
        storePassword keystoreProperties['storePassword']
    }
}

buildTypes {
    debug {
        signingConfig signingConfigs.release
        minifyEnabled false
        shrinkResources false
    }

    release {
        signingConfig signingConfigs.release
        minifyEnabled true
        shrinkResources true
        zipAlignEnabled true
        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
    }
}
```
#### 6、打包步骤
<!-- 
1. 将 key.jks 文件放置在 android/app 目录下
2. 执行打包命令： 
-->
```
flutter build apk --release
```

生成的APK文件将位于：`build/app/outputs/flutter-apk/app-release.apk`

## 注意事项

1. 确保所有环境变量正确配置
2. 签名文件（key.jks）请妥善保管
3. 不要将签名相关文件提交到版本控制系统
4. 建议在 .gitignore 中添加：
```
**/android/key.properties
**/android/app/key.jks
```