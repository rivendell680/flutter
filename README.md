# flutter

## Android

~/.gradle/gradle.properties

```
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=7890
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=7890
```

vi /Users/aden/Downloads/backup/mainland/helloworld/android/settings.gradle.kts

```
pluginManagement {
    val flutterSdkPath =
        run {
            val properties = java.util.Properties()
            file("local.properties").inputStream().use { properties.load(it) }
            val flutterSdkPath = properties.getProperty("flutter.sdk")
            require(flutterSdkPath != null) { "flutter.sdk not set in local.properties" }
            flutterSdkPath
        }

    includeBuild("$flutterSdkPath/packages/flutter_tools/gradle")

    repositories {
        // 直接在这里写镜像，确保能下载到 flutter-plugin-loader
        maven { url = uri("https://maven.aliyun.com/repository/google") }
        maven { url = uri("https://maven.aliyun.com/repository/public") }
        maven { url = uri("https://maven.aliyun.com/repository/gradle-plugin") }
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

plugins {
    id("dev.flutter.flutter-plugin-loader") version "1.0.0"
    id("com.android.application") version "8.11.1" apply false
    id("org.jetbrains.kotlin.android") version "2.2.20" apply false
}

dependencyResolutionManagement {
    // 【核心修改】改为 PREFER_PROJECT，这样就不会再报 "repository maven was added" 的错误了
    repositoriesMode.set(RepositoriesMode.PREFER_PROJECT)
    repositories {
        maven { url = uri("https://maven.aliyun.com/repository/google") }
        maven { url = uri("https://maven.aliyun.com/repository/public") }
        google()
        mavenCentral()
    }
}

include(":app")
```

### 1. 删除本地缓存
```
rm -rf .gradle/
rm -rf ~/.gradle/caches/8.14/
```

### 2. 运行编译（直接在 android 目录下）
```
./gradlew help -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=7890 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=7890
```

## iOS

### 选择二：在真机 iPhone 运行（需要签名）

如果你必须在真机上测试，请按照报错提示的这几步操作（仅需设置一次）：

1. **打开 Xcode 项目**： 在终端输入：`open ios/Runner.xcworkspace`
2. **配置签名 (Signing)**：
   - 在左侧菜单点击最顶部的蓝色图标 **Runner**。
   - 在右侧选择 **Signing & Capabilities** 选项卡。
   - 点击 **Add Account...** 登录你的 Apple ID（免费的即可）。
   - 在 **Team** 下拉框选择你的名字。
3. **设置唯一的 Bundle ID**：
   - 将 `com.example.helloworld` 改成一个唯一的名称（比如 `com.aden.mytestapp`），否则会报 ID 被占用的错误。
4. **信任设备**：
   - 运行后，手机会提示“不受信任的开发者”。
   - 去手机的 **设置 -> 通用 -> 设备管理 (或 VPN 与设备管理) -> 点击你的 Apple ID -> 点击信任**。
