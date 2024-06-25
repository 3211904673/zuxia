<p align="center">
  < img id="header" src="./images/logo_docker-android.png" />
</p >

[![Paypal Donate](https://img.shields.io/badge/paypal-donate-blue.svg)](http://paypal.me/budtmo) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) [![codecov](https://codecov.io/gh/budtmo/docker-android/branch/master/graph/badge.svg)](https://codecov.io/gh/budtmo/docker-android) [![Join the chat at https://gitter.im/budtmo/docker-android](https://badges.gitter.im/budtmo/docker-android.svg)](https://gitter.im/budtmo/docker-android?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![GitHub release](https://img.shields.io/github/release/budtmo/docker-android.svg)](https://github.com/budtmo/docker-android/releases)

Docker-Android 是一个用于 Android 相关开发的 Docker 镜像。可用于应用程序开发和测试（原生应用、网页应用和混合应用）。

使用此项目的优势
-----------------
1. 拥有不同设备配置文件和外观的模拟器，如三星 Galaxy S6、LG Nexus 4、HTC Nexus One 等。
2. 支持 VNC，可以查看 Docker 容器内部发生的情况。
3. 支持日志共享功能，所有日志可从 web-UI 访问。
4. 可以通过 adb connect 从容器外控制模拟器。
5. 与其他云解决方案集成，例如 [Genymotion Cloud](https://www.genymotion.com/cloud/)。
6. 可用于构建 Android 项目。
7. 可用于运行单元测试和 UI 测试，支持不同的测试框架，如 Appium、Espresso 等。

Docker-Images 列表
-----------------
| Android | API | 最新版本镜像 | 指定版本镜像 |
|:--------|:----|:--------------|:-------------|
| 9.0     | 28  | budtmo/docker-android:emulator_9.0 | budtmo/docker-android:emulator_9.0_<release_version> |
| 10.0    | 29  | budtmo/docker-android:emulator_10.0 | budtmo/docker-android:emulator_10.0_<release_version> |
| 11.0    | 30  | budtmo/docker-android:emulator_11.0 | budtmo/docker-android:emulator_11.0_<release_version> |
| 12.0    | 32  | budtmo/docker-android:emulator_12.0 | budtmo/docker-android:emulator_12.0_<release_version> |
| 13.0    | 33  | budtmo/docker-android:emulator_13.0 | budtmo/docker-android:emulator_13.0_<release_version> |
| 14.0    | 34  | budtmo/docker-android:emulator_14.0 | budtmo/docker-android:emulator_14.0_<release_version> |
| -       | -   | budtmo/docker-android:genymotion | budtmo/docker-android:genymotion_<release_version> |

设备列表
-------
| 类型   | 设备名称 |
|:------|:---------|
| Phone | Samsung Galaxy S10 |
| Phone | Samsung Galaxy S9 |
| Phone | Samsung Galaxy S8 |
| Phone | Samsung Galaxy S7 Edge |
| Phone | Samsung Galaxy S7 |
| Phone | Samsung Galaxy S6 |
| Phone | Nexus 4 |
| Phone | Nexus 5 |
| Phone | Nexus One |
| Phone | Nexus S |
| Tablet | Nexus 7 |

要求
----
1. 系统上已安装 Docker。

快速开始
-------
1. 如果你的主机系统是 ***Ubuntu OS***，可以跳过这一步。对于 ***OSX*** 和 ***Windows OS*** 用户，你需要使用支持虚拟化的虚拟机运行 Ubuntu OS，因为镜像只能在 ***Ubuntu OS*** 下运行。

2. 你的机器应支持虚拟化。检查虚拟化是否启用的方法：
    ```
    sudo apt install cpu-checker
    kvm-ok
    ```

3. 运行 Docker-Android 容器
    ```
    docker run -d -p 6080:6080 -e EMULATOR_DEVICE="Samsung Galaxy S10" -e WEB_VNC=true --device /dev/kvm --name android-container budtmo/docker-android:emulator_11.0
    ```

4. 打开 ***http://localhost:6080*** 查看运行中的容器。

5. 检查模拟器状态
    ```
    docker exec -it android-container cat device_status
    ```

数据持久化
-----------
默认情况下，模拟设备在容器重启时会被销毁。要持久化数据，需要在 `/home/androidusr` 挂载一个卷：
    ```
    docker run -v data:/home/androidusr budtmo/docker-android:emulator_11.0
    ```

WSL2 硬件加速（仅限 Windows 11）
-----------
感谢 [Guillaume - The Parallel Interface blog](https://www.paralint.com/2022/11/find-new-modified-and-unversioned-subversion-files-on-windows)

[Microsoft - WSL 高级设置配置](https://learn.microsoft.com/en-us/windows/wsl/wsl-config)

1. 将自己添加到 `kvm` 用户组。
    ```
    sudo usermod -a -G kvm ${USER}
    ```

2. 在 `/etc/wsl2.conf` 文件的相应部分添加必要的标志。
    ```
    [boot]
    command = /bin/bash -c 'chown -v root:kvm /dev/kvm && chmod 660 /dev/kvm'

    [wsl2]
    nestedVirtualization=true
    ```
3. 通过 CMD 或 Powershell 重启 WSL2
    ```
    wsl --shutdown
    ```

`command = /bin/bash -c 'chown -v root:kvm /dev/kvm && chmod 660 /dev/kvm'` 设置 `/dev/kvm` 为 `kvm` 用户组，而不是 WSL2 启动时的默认 `root` 用户组。

`nestedVirtualization` 标志仅在 Windows 11 上可用。

用例
-----
1. [构建 Android 项目](./documentations/USE_CASE_BUILD_ANDROID_PROJECT.md)
2. [使用 Appium 进行 UI 测试](./documentations/USE_CASE_APPIUM.md)
3. [在主机上控制 Android 模拟器](./documentations/USE_CASE_CONTROL_EMULATOR.md)
4. [SMS 模拟](./documentations/USE_CASE_SMS.md)
5. [Jenkins](./documentations/USE_CASE_JENKINS.md)
6. [部署到云端（Azure, AWS, GCP）](./documentations/USE_CASE_CLOUD.md)

自定义配置
-----------
这个[文档](./documentations/CUSTOM_CONFIGURATIONS.md)包含有关配置的信息，这些配置可用于启用某些功能，例如日志共享等。

Genymotion
----------
<p align="center">
  < img id="geny" src="./images/logo_genymotion_and_dockerandroid.png" />
</p >

对于那些没有资源维护模拟器或购买机器，或者需要不同设备配置文件的人，可以尝试使用 [Genymotion SAAS](https://cloud.geny.io/)。Docker-Android 已与 [Genymotion](https://www.genymotion.com/blog/partner_tag/docker/) 集成，支持不同的云服务，例如 Genymotion SAAS、AWS、GCP、阿里云。请参考[此文档](./documentations/THIRD_PARTY_GENYMOTION.md)获取更多详细信息。

模拟器皮肤
----------
模拟器皮肤取自 [Android Studio IDE](https://developer.android.com/studio) 和 [Samsung Developer 网站](https://developer.samsung.com/)

用户
----
<a href=" ">
  <p align="center">
    < img src="./images/docker-android_users.png" alt="docker-android-users" width="800" height="600">
  </p >
</a >

PRO 版本
--------
由于大量请求帮助和能够积极维护项目，创建者决定创建 docker-android-pro。Docker-Android-Pro 是一个基于赞助的项目，这意味着 pro 版本的 docker 镜像只能由[活跃赞助者](https://github.com/sponsors/budtmo)拉取。

普通版本与 Pro 版本的区别如下：

| 功能                  | 普通版 | Pro 版 | 备注 |
|:---------------------|:------|:------|:-----|
| 用户行为分析          | 是    | 否    | -    |
| 代理                 | 否    | 是    | 在 Android 模拟器上动态设置公司代理 |
| 语言设置             | 否    | 是    | 在 Android 模拟器上动态设置语言 |
| root 权限            | 否    | 是    | 能够以安全权限运行命令 |
| 无头模式             | 否    | 是    | 通过使用无头模式节省资源 |
| Selenium 4.x 集成    | 否    | 是    | 通过一个 (Selenium Hub) 端点运行 Android 和 iOS 模拟器/设备的 Appium UI 测试 |
| 多个 Android 模拟器  | 否    | 是（即将推出） | 在一个 Docker 容器中节省资源，运行多个 Android 模拟器 |
| Google Play 商店    | 否    | 是（即将推出） | -    |
| 视频录制             | 否    | 是（即将推出） | 有助于调试 |

该[文档](./documentations/DOCKER-ANDROID-PRO.md)包含关于如何使用 docker-android-pro 的详细信息。

许可证
-------
参见 [License](LICENSE.md)