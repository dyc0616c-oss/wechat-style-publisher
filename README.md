# wechat-style-publisher

一个给 **微信公众号内容团队 / 个人号主** 用的总 skill。

它不是单纯“帮你写一篇文章”的小工具，
而是想做成一条完整路径：

- 学习你的公众号风格
- 接微信开发者平台
- 接 Tuzi 文生图
- 拿素材做一篇测试稿
- 自动保存到公众号草稿箱

---

## 这个 skill 现在可以干什么？

目前这版是 **MVP / 可试机版**，重点先把路径跑通。

它现在主要能做：

1. **学习公众号风格**
   - 用户提供 5-10 篇自己写过的公众号文章
   - skill 会分析这些文章，提炼一个 style 文件（例如 `style-a`）

2. **生成 style 文件**
   - 把标题长度、摘要长度、正文节奏、重点句样式、结尾结构等规则整理成可执行 spec

3. **接微信公众号开发者平台**
   - 支持通过 `AppID + AppSecret` 获取 token
   - 支持微信接口测试
   - 支持创建公众号草稿

4. **接 Tuzi 文生图**
   - 用户提供 `TUZI_API_KEY`
   - 可用于封面图 / 正文插图测试

5. **跑测试稿闭环**
   - 用户给一个素材、主题或链接
   - skill 生成一篇测试稿
   - 保存到微信公众号草稿箱
   - 用户去后台看效果，再决定要不要继续优化 style

---

## 这个 skill 适合谁？

适合这些人：

- 自己运营公众号，想把“自己的号味”固化下来
- 有多个公众号，想把不同号做成 `style-a / style-b / style-c`
- 想把“写稿 + 生图 + 草稿测试”流程串起来
- 不想每次都手工重复配微信接口

不适合这些场景：

- 只想随手写一篇，不关心风格沉淀
- 不打算接微信开发者平台
- 不打算做图文测试闭环

---

## 使用前，用户需要准备什么？

### 一、学习风格时需要提供
用户至少要提供：

- **公众号名称**
- **5-10 篇历史文章**

最好是：
- 同一个公众号最近一段时间的代表性文章
- 能体现这个号的标题、语气、排版、结尾习惯

---

### 二、接微信公众号时需要提供
用户需要去 **微信公众平台开发者设置** 里准备：

- `AppID`
- `AppSecret`
- 把当前运行机器的公网 IP 加入 **API 白名单**

如果没加白名单，常见报错会是：
- `invalid ip not in whitelist`

---

### 三、接 Tuzi 文生图时需要提供
用户需要提供：

- `TUZI_API_KEY`

用途：
- 生成封面图
- 生成正文插图

---

## 这个 skill 的基本使用流程

### 第 1 步：先学习一个公众号风格
用户提供 5-10 篇历史文章后，先跑风格学习。

当前脚本：

```bash
python3 scripts/learn_style.py \
  --account-name "你的公众号名" \
  --style-id style-a \
  --inputs article1.md article2.md article3.md \
  --write-map
```

这一步会做什么：
- 分析文章样本
- 生成 `style-a.md`
- 更新 `account-style-map.json`

---

### 第 2 步：接微信开发者平台
需要先准备环境变量：

```bash
export WECHAT_APP_ID="你的AppID"
export WECHAT_APP_SECRET="你的AppSecret"
```

如果你用的是旧变量名，也兼容：

```bash
export DASHU_MP_APP_ID="你的AppID"
export DASHU_MP_APP_SECRET="你的AppSecret"
```

---

### 第 3 步：接 Tuzi 文生图
准备：

```bash
export TUZI_API_KEY="你的TUZI_API_KEY"
```

旧变量名也兼容：

```bash
export AI_GATEWAY_API_KEY="你的TUZI_API_KEY"
```

---

### 第 4 步：跑一篇测试稿
当 `style-a`、微信、Tuzi 都准备好之后，可以跑测试稿。

当前脚本：

```bash
python3 scripts/run_draft_test.py \
  --style-id style-a \
  --source "https://example.com/article" \
  --publish
```

这一步会做什么：
- 读取 `style-a`
- 渲染一篇测试稿
- 生成测试封面
- 上传到微信
- 保存到草稿箱

---

## 用户使用时，最终会得到什么？

正常情况下，用户最终会得到：

- 一个学出来的风格文件，例如 `style-a.md`
- 一个账号映射配置 `account-style-map.json`
- 一篇保存到公众号草稿箱里的测试稿
- 一条微信草稿 `media_id`

然后用户就可以去公众号后台看效果：
- 像不像自己的号
- 标题对不对
- 段落节奏对不对
- 结尾风格对不对
- 要不要继续优化 style

---

## 这版现在的状态

这不是一个“已经完美”的正式版，
而是一个：

**可试机、可验证路径的 MVP 版本。**

它的重点是先证明：

- 路径能跑通
- style 能学出来
- 微信能接上
- 草稿能存进去

如果这条路验证通过，后面再继续打磨：
- 更聪明的 style 学习
- 更像真人号的测试稿
- 更好的图文策略
- 更多 style 的扩展

---

## 目前仓库结构

```text
wechat-style-publisher/
├── SKILL.md
├── README.md
├── .gitignore
├── references/
│   ├── style-learning-workflow.md
│   ├── style-schema.md
│   ├── wechat-setup-guide.md
│   ├── tuzi-setup-guide.md
│   ├── draft-test-workflow.md
│   └── styles/
│       ├── account-style-map.json
│       └── style-a.md
└── scripts/
    ├── learn_style.py
    └── run_draft_test.py
```

---

## 一句话总结

这个 skill 想做的事情很简单：

**先学会你的公众号怎么写，再帮你把测试稿真正存进微信草稿箱。**

如果它能把这条路跑顺，后面才值得继续往上加更复杂的功能。
