# wechat-style-publisher

一个给 **微信公众号内容团队 / 个人号主** 用的 OpenClaw Skill。

它做的事情很简单：
**先学习你的公众号风格，再按这个风格生成测试稿，并尝试保存到微信公众号草稿箱。**

---

## 这个 skill 能做什么

- 学习 5-10 篇历史公众号文章风格
- 生成 `style-a / style-b` 这样的风格文件
- 建立账号与 style 的映射
- 接入微信公众号开发者平台
- 接入 Tuzi 文生图能力
- 按指定 style 生成测试稿
- 自动保存到微信公众号草稿箱

当前版本定位：**MVP / 可试机版**。
重点是先把“学习 style → 接微信 → 跑测试稿 → 存草稿”这条链路跑通。

---

## 适合谁

适合：
- 自己运营公众号，想把“号味”沉淀下来的人
- 有多个公众号，想训练不同 style 的团队
- 想把“写稿 + 配图 + 草稿测试”串成一条流程的人

不适合：
- 只想临时写一篇、不关心风格沉淀的人
- 不打算接入微信公众号开发者平台的人

---

## 仓库结构

```text
wechat-style-publisher/
├── SKILL.md
├── README.md
├── .env.example
├── requirements.txt
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

## 最简单使用方法

### 第一步：把仓库放到 OpenClaw 的 skills 目录

放到以下任一位置：

```bash
~/.openclaw/skills/wechat-style-publisher
```

或者：

```bash
<workspace>/skills/wechat-style-publisher
```

### 第二步：重启 OpenClaw

```bash
openclaw gateway restart
```

### 第三步：直接对助手说

例如：
- 学习这 5 篇公众号文章风格，生成 style-a
- 帮我接微信公众号发布
- 按 style-a 写一篇测试稿并保存到草稿箱

---

## 使用前要准备什么

### 1）学习风格时
请准备：
- 公众号名称
- 5-10 篇历史文章

建议这些文章能体现：
- 标题风格
- 摘要长度
- 正文语气
- 段落节奏
- 结尾 CTA
- 图片习惯

### 2）接微信公众号时
请准备：
- `AppID`
- `AppSecret`
- 当前运行机器公网 IP 已加入微信 API 白名单

如果没加白名单，常见错误是：

```text
invalid ip not in whitelist
```

### 3）接 Tuzi 时
请准备：
- `TUZI_API_KEY`

---

## 环境变量

可以参考 `.env.example`。

最常用的是：

```bash
export WECHAT_APP_ID="你的AppID"
export WECHAT_APP_SECRET="你的AppSecret"
export TUZI_API_KEY="你的TUZI_API_KEY"
```

兼容旧变量名：

```bash
export DASHU_MP_APP_ID="你的AppID"
export DASHU_MP_APP_SECRET="你的AppSecret"
export AI_GATEWAY_API_KEY="你的TUZI_API_KEY"
```

---

## 脚本用法

### 1）学习 style

```bash
python3 scripts/learn_style.py \
  --account-name "你的公众号名" \
  --style-id style-a \
  --inputs article1.md article2.md article3.md \
  --write-map
```

效果：
- 分析文章样本
- 生成 `style-a.md`
- 更新 `account-style-map.json`

### 2）跑测试稿并尝试发布

```bash
python3 scripts/run_draft_test.py \
  --style-id style-a \
  --source "https://example.com/article" \
  --publish
```

效果：
- 读取 `style-a`
- 生成测试稿
- 生成测试封面
- 上传到微信
- 保存到草稿箱

---

## 安装检查

安装后可以执行：

```bash
openclaw skills list
openclaw skills info wechat-style-publisher
openclaw skills check
```

如果能看到 `wechat-style-publisher`，说明已识别成功。

---

## 常见问题

### skill 装了但没生效
先执行：

```bash
openclaw gateway restart
```

然后再查：

```bash
openclaw skills list
```

### 微信接口报错
优先检查：
- AppID / AppSecret 是否正确
- 当前机器公网 IP 是否已加入白名单

### Tuzi 没出图
优先检查：
- `TUZI_API_KEY` 是否正确
- 是否用了旧变量名但没配上

### 草稿没有保存成功
优先检查：
- 微信 access token 是否获取成功
- 素材内容是否符合接口要求
- 是否真的执行了 `--publish`

---

## 最后一句话

这个 skill 的目标不是“随手写一篇”，而是：

**先学会你的公众号怎么写，再帮你按这个风格生成测试稿，并真正存进微信草稿箱。**
