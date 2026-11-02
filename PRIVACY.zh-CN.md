# CallTrail 隐私政策

**生效日期：** 2026 年 11 月 2 日

**适用范围：** 通过 JetBrains Marketplace 和官方 [GitHub Releases](https://github.com/0xZZSheep/CallTrail-intellij-plugin/releases) 分发的 CallTrail JetBrains IDE 插件。

CallTrail 由 Kai Li（“我们”“开发者”）开发。本政策说明插件如何处理你的数据。它只覆盖插件本身；JetBrains Marketplace 和你配置的 AI 服务有各自的隐私政策。

## 摘要

- CallTrail 不需要账号，不包含分析、广告或跟踪。
- CallTrail 在本地读取源码来构建调用图，开发者不会收到你的代码（你在崩溃报告中主动填写的内容除外）。
- AI 摘要**默认关闭**。开启后，CallTrail 会把选中节点的名称、位置和源码发送到你配置的 OpenAI 兼容端点。使用 Ollama、LM Studio 等本地端点时，数据留在你的机器上。
- 你的设置（包括 AI API key）保存在本地 IDE 配置中，不会发送给开发者。
- 唯一可能到达开发者的诊断数据，是你在 IDE 错误对话框里主动选择发送的崩溃报告。

## CallTrail 在本地处理什么

CallTrail 使用 IntelliJ 平台的调用层次，为你选中的元素构建调用者/被调用者图。图在 IDE 内根据 IDE 已经提供的源码和索引计算，插件不会上传你的项目。

CallTrail 把设置保存在本地 IDE 配置中，包括图深度、节点上限、库节点和过滤设置、弹窗行为以及 AI 设置。应用级设置文件是 `callHierarchyGraph.xml`；项目级过滤规则保存在项目的 IDE 配置中。这些设置不会发送给开发者，并会一直留在你的机器上，直到你修改它们或删除相应的 IDE 配置。

## AI 摘要

AI 功能**默认关闭**（`aiEnabled = false`），需要你主动开启并配置一个 OpenAI 兼容端点。

开启后，CallTrail 会向你配置的端点发送：

- 选中节点的名称和位置（例如方法签名和文件、行号）；
- 选中节点的源码；请求文件级摘要时，发送整个文件的源码；
- 要求模型生成摘要的提示词文本。

端点可以是 OpenAI、Azure OpenAI、OpenRouter、Ollama、LM Studio 或其他兼容服务。

- 开发者不会收到你的源码、提示词或 API key。
- 你选择的 AI 服务商可能按其隐私政策和服务条款记录或保留请求；开启前请先阅读。
- 使用 Ollama、LM Studio 等本地端点时，请求留在你的机器或本地网络（取决于你的配置）。
- AI 功能关闭时，CallTrail 不会自动发出 AI 请求。设置里的 **Verify** 按钮只会在你点击时发送一次最小测试请求。
- API key 保存在本地 `callHierarchyGraph.xml`，只会作为 API 请求的一部分发送给你配置的端点，不会写入插件日志。请保护好你的 IDE 配置目录。

## 崩溃与错误报告

CallTrail 注册了 IntelliJ 平台的 Marketplace 错误报告处理器。如果 IDE 弹出错误对话框并且你选择发送报告，报告会提交到 JetBrains Marketplace，开发者可能看到它。报告可能包含异常堆栈、IDE 和插件版本、操作系统，以及你在对话框里填写的内容。只有你选择发送时才会发送。

如果你不发送报告，就不会传输任何报告。JetBrains 按其[隐私政策](https://www.jetbrains.com/legal/docs/privacy/privacy.html)处理 Marketplace 数据。

## CallTrail 不会做什么

- 不需要账号或登录。
- 不做分析、广告或跟踪。
- 不使用 cookie 或网络信标。
- 开发者不会出售或分享你的数据。
- 不会在后台上传你的项目或源码。
- 不会访问 IDE 为当前项目及其依赖提供的代码和索引之外的文件。

## 第三方

- **JetBrains** 提供 IDE、JetBrains Marketplace、插件分发和更新检查，以及可选的错误报告处理器。JetBrains 按其隐私政策处理数据。
- **你选择的 AI 服务商**按自己的隐私政策和服务条款处理 AI 请求。

CallTrail 不集成其他第三方服务。

## 数据保留

开发者不维护插件用户数据库，也不保留插件产生的个人数据。崩溃报告由 JetBrains 保留，AI 请求由你选择的服务商按其政策保留。设置会留在你的 IDE 配置中，直到你修改或删除它们。

## 你的权利

由于开发者不通过插件收集个人数据，开发者手中没有可供访问、更正或删除的个人数据。如果你对本政策有疑问，或认为插件以不同方式处理了数据，请通过下面的地址联系我们。对于 JetBrains 或 AI 服务商持有的数据，请联系相应服务商。

## 变更

如果插件的数据处理方式发生变化，我们可能更新本政策。顶部的生效日期会更新，重大变更会在插件的更新说明中注明。当前版本始终可在此 URL 获取。

## 联系

Kai Li
邮箱：likaiqiang.frontend@gmail.com
