# iOS 真机接入（Mac 或云真机）

## 路由

- 自有 iPhone：由 Mac 安装 Xcode、签名并运行 WebDriverAgent（WDA）；Windows 可作为 Python 用例客户端，通过受控网络连接 Mac 上的 Appium。
- 只有 Windows：优先用托管 iOS 真机服务。Appium 的 [Windows 直连有限支持](https://appium.github.io/appium-xcuitest-driver/latest/guides/non-macos-hosts/)仅适用于符合其系统版本要求且已预装/外部运行 WDA 的设备，不作为新机默认路线。

## Mac + iPhone 的顺序

1. 用 [XCUITest 兼容要求](https://appium.github.io/appium-xcuitest-driver/latest/getting-started/system-requirements/)核对 iOS、macOS、Xcode、Appium 与驱动版本。
2. iPhone 连接 Mac，用户完成“信任此电脑”、开发者模式、开发者证书信任等手机交互。WDA 需要匹配设备的有效签名；免费签名可能短期过期，换机或过期后重新验证。
3. 用 Xcode/Appium 在 **一台** 真机启动 WDA，确认状态可用，再启动被测 App 并完成冒烟断言。不要把旧机器的 WDA URL、UDID、Team ID 或 Bundle ID 直接带到新机器。
4. 并行时每台 iPhone 指定唯一 `udid`、`wdaLocalPort`、`derivedDataPath`；需要视频流时还要唯一 `mjpegServerPort`。参考 [XCUITest 并行指南](https://appium.github.io/appium-xcuitest-driver/latest/guides/parallel-tests/)。

Mac 上先用 `xcrun devicectl list devices` 确认手机被识别，再用 `appium driver doctor xcuitest` 检查驱动前置项。WDA 可用后才建立 Appium 会话；失败当时用 Appium 客户端 `driver.save_screenshot(<本次case的证据路径>)` 留证，并记录对应设备、iOS/Xcode 与签名状态。

迁移旧 Mac/iPhone 的 WDA 脚本时，设备 ID、签名、系统版本和 URL 均须重新核对。若脚本把输入值作为命令行参数，敏感凭据不得通过这种方式传入。切换到新设备时先更新工作区档案，再验证链路。

## 云真机

云真机提供 iPhone 和托管 Mac。Windows 上运行测试客户端，连接服务商给出的 Appium 地址；多机并行受设备会话/额度限制。上传测试包前确认数据范围、签名行为与测试账号。参考 [AWS Device Farm 客户端接入](https://docs.aws.amazon.com/devicefarm/latest/developerguide/appium-endpoint-getting-started.html)。
