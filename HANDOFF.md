# ZCode Handoff: Federation Communication Lite

版本：`v1.0`
用途：在独立沙盒中继续调试一个轻量、跨平台的 Agent 通讯 Skill。

## 包内文件

- `FEDERATION_COMMUNICATION_LITE_SKILL_V1.md`
  - 正式 Skill 单文件
  - 原生任务通讯优先
  - 公共 Markdown 作为跨平台兜底
  - 最小状态消息、隐私边界和停止重复敲门规则
- `federation-communication-lite-logo.html`
  - Canvas Logo 样例
  - 可在浏览器中预览
  - 页面内的 `Download PNG` 按钮可导出 Logo
- `federation-communication-lite-sample.html`
  - GitHub 风格介绍页样例
  - 已采用办公灰背景、灰白面板、墨黑边界、绿色主路径和蓝色备用路径

## 设计逻辑

- 绿色路径：平台原生直连，是默认通信方式。
- 蓝色路径：共享 Markdown 公共板，是无法直连时的人工兜底。
- 中央节点：协调汇合点，不代表所有 Agent 永久在线。
- 外框：权限、任务范围和隐私边界。

## ZCode 使用规则

1. 先读取 `FEDERATION_COMMUNICATION_LITE_SKILL_V1.md`。
2. 只把它作为通信协议使用，不自动创建常驻进程、心跳或轮询。
3. 有原生对话通信时，优先使用原生通信，不重复写公共板。
4. 没有原生通信时，使用用户指定的公共 Markdown 文件人工交接。
5. ACK 只表示收到，不表示完成；RESULT 必须附证据和风险。
6. 对未响应会话只做一次边界提醒，然后报告无响应，不持续敲门。
7. 不修改业务代码，不上传外部平台，不执行账号操作，除非用户另行授权。

## 安全边界

本包不包含也不需要：

- GitHub 用户名、密码、登录 Cookie 或浏览器配置；
- 任何 API Key、Secret、Bearer Token 或完整授权值；
- 抖音、小红书或其他网站的登录状态；
- 完整浏览记录、私人收藏内容或完整请求 payload；
- 本机凭据和系统设置。

如果 ZCode 要求上述内容才能工作，应停止并向用户报告，不要索取或复制。

## 调试目标

- 检查 Skill 是否能在沙盒中被正确识别和触发。
- 检查 Markdown 公共板消息是否能被正确追加和读取。
- 检查 Logo 页面 Canvas 是否正常显示并能导出 PNG。
- 检查介绍页在桌面和移动宽度下是否保持清晰，不出现文字溢出。

## 预期回报格式

```text
status: ACK | QUESTION | BLOCKED | RESULT
files_checked: <文件名>
evidence: <测试或页面证据>
issues: <没有则写 none>
next_owner: coordinator
```
