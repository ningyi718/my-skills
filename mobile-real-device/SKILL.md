---
name: mobile-real-device
description: 在 Android 或 iOS 真机上快速接入、执行移动 App 用例、维护设备记忆并记录可复现缺陷；适用于换电脑、换设备和多机测试。
---

# 真机测试

先读 [共用流程](references/workflow.md)、[设备档案规则](references/device-memory.md)和当前工作区的 `mobile-test/README.md`、`mobile-test/devices.md`。执行已有 case 前读 [用例记忆规则](references/case-memory.md)及工作区 `mobile-test/cases/` 中的对应记录。Android 任务再读 [Android 接入](references/android.md)，iOS 任务再读 [iOS 接入](references/ios.md)。发现真实缺陷时使用 [报告模板](assets/bug-report.md)。

## 执行要点

- skill 目录只读。所有新记忆、case、自动化脚本、截图、结果、bug 报告写入**当前打开工作区**的 `mobile-test/`；若目录不存在，先在工作区创建，不写进 `.codex/skills`。
- 每次先发现当前设备和 App 状态，再复用工作区档案；档案中的历史值只作线索，不作事实。
- 首次探索成功后，立即保存每一步的稳定定位符、入口状态和结果校验点。再次运行时先回放，禁止重新从全屏截图或页面源码漫无目的地找按钮；只在校验失败的步骤做局部修复。
- 一台设备同一时刻只分配一个会话。用例与设备、端口、账号、数据一一对应，避免并发串扰。
- 测试前明确步骤与预期；每步操作后确认界面状态。结果区分通过、不通过、环境阻塞和未执行。
- 密码、GA 码、私钥、助记词和 token 不写入命令行、报告、页面源码输出或日志。仅使用经授权的测试数据；人工验证关卡交给用户完成。
- 不因换机而自动卸载 App、清除数据、重置钱包或修改签名。真实 bug 立即截屏并记录复现步骤；缺陷证据先检查敏感信息再分享。

本 skill 只保存方法和模板，不保存凭据或运行记录。项目级规则、用户当次指令及业务预期以当前会话为准。
