---
name: wechat-style-publisher
description: 训练并发布用户自己的公众号风格。用于：从用户提供的 5-10 篇历史公众号文章中学习风格并生成 style-a/style-b 等风格文件；引导用户完成微信开发者平台 AppID/Secret/IP 白名单配置；引导用户接入 Tuzi API Key 获得文生图能力；最后用指定 style 对新素材或链接生成测试文章、配图并保存到微信公众号草稿箱，供用户查看并迭代优化。
---

# WeChat Style Publisher

这是一个“总 skill”，负责把 **风格学习 + 微信接入 + Tuzi 接入 + 测试发稿** 串成完整流程。

## 固定总流程

1. 先读取 `references/style-learning-workflow.md`
2. 如需学习新号风格，读取 `references/style-schema.md`
3. 如需接微信，读取 `references/wechat-setup-guide.md`
4. 如需接 Tuzi，读取 `references/tuzi-setup-guide.md`
5. 如需测试发布，读取 `references/draft-test-workflow.md`
6. 按需调用 `scripts/` 中的脚本完成分析、渲染、上传与草稿保存

## MVP 口径（重要）

第一版只负责跑通“学习 style → 接微信/Tuzi → 测试保存草稿”的闭环。

默认不强绑：
- 二维码
- 私有 CTA
- 私有结尾模板
- 某个账号专属引流组件

这些都应作为 style 里的可选项，而不是总 skill 的默认行为。

## 工作模式

### 模式 1：风格学习
当用户提供 5-10 篇历史文章并要求“学习这个公众号风格 / 生成 style-a / 提炼风格”时：
- 分析文章样本
- 提取标题、摘要、正文、重点样式、图片策略、结尾引流、二维码规则
- 生成一个 style 文件（如 `style-a.md`）
- 更新 `references/styles/account-style-map.json`

### 模式 2：平台接入
当用户要求“接微信 / 接 Tuzi / 配发布环境”时：
- 引导完成 AppID、AppSecret、IP 白名单
- 引导提供 `TUZI_API_KEY`
- 用最小测试验证 token 与文生图链路

### 模式 3：测试发布
当用户提供素材、链接或主题并要求“按 style-a 试一篇 / 存草稿测试”时：
- 按 style 渲染标题、摘要、正文
- 按 style 生成封面图和正文插图
- 上传到微信并保存草稿
- 回执草稿 `media_id`

## 设计原则

- 风格学习与发布执行分层，不混成一个大脚本
- 先学习 style，再接发布链路，再做测试稿
- style 文件优先描述“规则”，脚本优先执行“动作”
- 默认先做 MVP：学习一个号 → 接微信/Tuzi → 跑一篇测试稿
- 如用户反馈“不像”，优先更新 style spec，而不是盲目改脚本

## 最小输出要求

每次测试发布完成后，至少告诉用户：
- 使用的 `style_id`
- 是否接通微信 API
- 是否接通 Tuzi API
- 封面图 / 正文图是否生成成功
- 微信草稿 `media_id`
- 建议用户去草稿箱查看并反馈“哪里像 / 哪里不像”
