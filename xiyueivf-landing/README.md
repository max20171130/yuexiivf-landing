# xiyueivf-landing

悦喜汇 · Google Ads A/B 测试备选落地页

聊天窗口式落地页（chat-first），用于辅助生殖咨询业务的留资转化。

## 部署

部署到 Vercel + 绑定 xiyueivf.com 即可。详见 `DEPLOY.md`。

## 主要交互

- 顶部"加微信"按钮：复制微信号 + 唤起微信 APP（手机端）
- 自动播放两段欢迎消息（带"正在输入"动画）
- 下方 6 题基本情况问卷（chip 单选）
- 底部 3 按钮：加微信 / 拨打电话 / 填问卷

## 后续要做

- [ ] 表单提交接入后端（目前只 console.log）
- [ ] gtag 转化追踪 ID 替换 `YOUR_CONVERSION_ID`
- [ ] 与 yuexihui.org 做 A/B 转化率对比
