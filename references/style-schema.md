# Style Schema（统一风格结构）

每个 style 文件建议统一包含以下字段：

```yaml
account_name: 公众号名称
style_id: style-a
positioning: 账号定位

title:
  length: 10-15 或 15-20
  style: 悬念 / 利益 / 观点 / 冲突 / 反转

digest:
  length: 6-12 或 12-18
  style: 钩子 / 信息差 / 结论型

body:
  paragraph_count: 4-5
  sentence_mode: one-sentence-per-paragraph / normal
  tone: sharp / steady / story / tutorial
  pacing: dense / medium / loose

highlight:
  sentence: red-bold-italic / none
  keyword: red-bold-italic / none
  numbers: red-bold-italic / none

ending:
  hook: true/false
  cta_style: style-specific description / none
  qr: bottom / none

# 注意
# 总 skill 默认不强制二维码与私有引流
# 若某个账号需要二维码，应由该 style 明确声明 `qr: bottom`

images:
  cover:
    enabled: true/false
    text: none / light
    mood: abstract-humorous / serious / clean / data-like
  inline:
    enabled: true/false
    ratio: 16:9
    text: none
    placement: before-next-section-highlight / paragraph-end / none
    variety: required / optional

wechat:
  requires_appid_secret: true
  whitelist_required: true

tuzi:
  required: true/false
```

## 原则
- schema 要可执行，不要只写“更像真人”“更有感觉”这种虚词
- 每个字段都要能指导后续渲染或发布脚本
