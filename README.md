# RepoShep · 牧仓犬

**Keep an eye on all your repositories.** — **替你看好每一个仓库。**

RepoShep（牧仓犬）是一款 macOS 原生的**多仓库健康监控 App**：它不替你操作 Git，而是替你盯着所有仓库——哪个仓库需要你、为什么需要、这种状态已经持续多久。

> RepoDeck 帮你操作仓库，RepoShep 告诉你哪个仓库需要你。
> *RepoDeck helps you work with repositories. RepoShep tells you which repositories need you.*

[隐私政策（Privacy Policy）](privacy-policy.html) · Mac App Store

---

## 产品定位

RepoShep 的核心模型是 **Health + Time + Attention**（健康 + 时间 + 注意力）。它回答五个问题：

1. 我的仓库们最近发生了什么？
2. 哪个仓库需要我处理？
3. 为什么需要我？
4. 这个状态已经持续多久？
5. 最近改动了什么？

交互目标：**0 次点击知道哪里有问题，1 次点击知道为什么，2 次点击看到改动。**

## 功能

- **仓库发现** —— 授权文件夹后递归发现 Git 仓库；识别普通仓库、worktree、子模块、嵌套仓库与裸仓库
- **实时监控** —— FSEvents 事件驱动增量刷新；睡眠唤醒、外置卷挂载后自动对账
- **仓库状态** —— 七状态模型：Clean / Changed / Sync Needed / Diverged / Conflict / Unavailable / Unknown
- **Diff 查看** —— 已改动文件与差异文本；大仓库保护（超时、字节上限、截断提示、二进制检测）
- **健康分（Health Score）** —— 0–100 的可解释运营健康分，每一处扣分都可展开「为什么是这个分」
- **状态老化（State Aging）** —— 记录每种状态被观察到多久：`Changed for 18 min`、`Pending Push for 2 days`
- **需要关注（Needs Attention）** —— 按严重度与持续时间给仓库排序；仓库问题与环境问题分开呈现
- **最近活动（Recent Activity）** —— 语义化的状态流转记录，事件去重合并，不刷屏
- **仪表盘** —— 总体健康一览 + 需要关注的仓库队列
- **远端刷新** —— 对 HTTPS 远端执行 `git fetch` 刷新超前/落后（默认关闭，双确认后开启）
- **AI 助手** —— 可选、自带端点：状态解读 / 命令助手 / 改动总结（详见[隐私政策](privacy-policy.html)）
- **环境与诊断** —— Git 可执行选择（自动/系统/Homebrew/自定义）、逐项环境体检、诊断报告导出
- **双语** —— 简体中文 / English；Dark Mode；无障碍（语义色 + 图标 + 文字三重信号）

**不做什么：** RepoShep 刻意不做 Stage/Commit/Push/Pull/Stash/Merge/Rebase，也不执行 AI 建议的命令。需要动手时，它把你送到正确的地方：View Changes、Open in Xcode、Open in Terminal，或你配置的 Git 客户端。

## 隐私

**Local First —— 你的代码留在你的 Mac 上。**

- 核心监控（发现 / 状态 / Diff / 健康 / 老化 / 活动）**完全本地运行，零网络**；无遥测、无分析、无账号、无广告
- 应用运行于 **App Sandbox**，只能访问你通过系统文件选择器明确授权的文件夹（安全作用域书签）
- 仅存在**两条可选出站路径，均默认关闭**，需你显式开启：
  1. **远端刷新**：仅对 HTTPS 远端执行 `git fetch`；传输内容仅为 Git 传输协议所需，不含源码、diff 或健康数据
  2. **AI 功能**：直连你自行配置的 OpenAI 兼容端点；API Key 只存 macOS 钥匙串
- 诊断报告与日志不含源码、diff 与凭据；远程 URL 中的凭据先脱敏再进入界面/日志

详见[隐私政策](privacy-policy.html)。

## 系统要求

- macOS 15.6 或更高版本
- 本机已安装 Git（应用不捆绑、不下载 Git 组件；首次启动提供 `xcode-select --install` 引导，也支持 Homebrew 或自定义路径的 Git）

## 构建与测试

核心逻辑为 SwiftPM 多模块包（`RepoShepCore` / `RepoShepGitCore` / `RepoShepSandboxAdapter`）：

```bash
swift build
swift test
```

macOS 应用工程：

```bash
open App/RepoShep/RepoShep.xcodeproj
```

## 项目文档

| 文档 | 内容 |
| --- | --- |
| [V1.9 完整开发设计](RepoShep-V1.9-Complete-Development-Design.md) | 架构冻结件（含修订标记） |
| [V1.9 修订记录](RepoShep-V1.9-Amendment-Record.md) | 冻结后的逐条决策与未验证项台账 |
| [Phase 7 Release Gates](Phase7-Release-Gates.md) | 发行门槛与 App Store 提审清单 |
| [Phase 0–6](Phase-0-1-Acceptance-Plan.md) | 从许可证审计、沙盒 Spike 到加固的分阶段验收 |
| [远端刷新设计](docs/superpowers/specs/2026-10-10-remote-fetch-design.md) | 应用内 `git fetch` 的规范性边界 |

## 许可证

- 本仓库源码以 [MIT 许可](LICENSE)发布；应用品牌、图标与 App Store 分发版本归作者所有
- 包含自 [RepoDeck](ThirdPartyNotices/RepoDeck-MIT.txt)（MIT）选定并改编的组件，声明原文在应用内「设置 → About → Acknowledgements」展示
- Git 托管图标来自 [Simple Icons](ThirdPartyNotices/SimpleIcons-CC0.txt)（CC0 1.0）
- RepoShep 不捆绑、不分发 Git 本身（Git 为 GPLv2）
