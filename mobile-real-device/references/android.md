# Android 真机接入（Windows 优先）

## 接入链路

Windows → Android SDK/ADB → Appium UiAutomator2 → USB 真机。当前设备和电脑上已验证的版本、路径、包名只在工作区 `mobile-test/devices.md` 保存；skill 不携带任何设备默认值。

## 新电脑或新手机的顺序

1. 备好 Python、Node.js/npm、JDK、Android SDK Platform Tools、Appium、UiAutomator2 驱动和 Appium Python Client。按当前 [Appium/驱动兼容表](https://appium.github.io/appium-uiautomator2-driver/latest/getting-started/)选版本，不直接安装“最新”而忽略 Node 兼容性。
2. 手机开启开发者选项和 USB 调试，确认电脑授权；Windows 上必要时装厂商 ADB 驱动。运行 `adb devices -l`，状态须为 `device`。
3. 从实际 APK 确认包名、版本和启动 Activity；在设备上确认安装状态。对已有钱包数据的 App，不自动执行卸载、清数据或覆盖安装。
4. 设置当前会话的 `ANDROID_HOME`、`JAVA_HOME`、`APPIUM_HOME`；启动 Appium。用实际序列号设 `udid`，保留登录状态建立单机冒烟会话。
5. 记录可用定位符和启动页到 [设备档案](device-memory.md)。多台手机并行时，每个会话指定唯一 `udid`、`systemPort`；WebView 用例还需核对 Chromedriver 版本与端口。参考 [UiAutomator2 能力参数](https://appium.github.io/appium-uiautomator2-driver/latest/reference/capabilities/)。

常用发现命令（PowerShell，先把实际 SDK 的 `platform-tools` 加入 `Path`）：

```powershell
adb devices -l
adb -s <设备序列号> shell getprop ro.build.version.release
adb -s <设备序列号> shell pm path <被测包名>
appium --version
appium driver list --installed
```

若 Appium 安装在项目 `tools` 而不在 `Path`，使用该目录下的 `node_modules/.bin/appium.cmd`。发现真实缺陷时，在失败动作之后、继续操作之前调用 Appium 客户端的 `driver.save_screenshot(<工作区本次case的证据路径>)`，并按报告模板记录前置状态与复现步骤。

`appium driver doctor uiautomator2` 可用于诊断，但它可能因为缺少模拟器报必需项失败；**USB 真机是否可测，以真实会话和冒烟断言为准**。bundletool、ffmpeg、GStreamer 分别对应 App Bundle、录屏、视频流，用到时再装。

对于 Flutter 页面，优先读取无障碍语义并验证输入后页面状态；不要只因输入框显示值就认定业务状态已更新。若遇到人工验证，暂停并交给用户。旧 ADB 脚本可能输出敏感页面或日志，也可能覆盖安装 App；迁移时先审查其行为，不直接当作通用入口。
