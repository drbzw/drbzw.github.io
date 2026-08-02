# 移动端响应式适配设计（2026-08-02）

## 目标

在保持桌面端（≥901px）视觉效果完全不变的前提下，完善 375–900px 移动端浏览体验。

## 背景

现有 `@media (max-width: 900px)` 仅处理了页头、导航换行、照片、统计栏、页脚。
已知移动端问题（经 375px 模拟验证）：
1. 学生页 5 列表格整体溢出屏幕
2. 研究兴趣条目标题（min-width 140px）与内容同行互相挤压
3. 简历条目（cv-entry）日期与职务/英文注释挤在同一行
4. 标题字号、section 留白在窄屏偏大；联系页/卡片网格双列过窄

## 方案（用户已批准）

### 断点
- 沿用现有 900px 断点并补足规则；新增 480px 极窄屏断点
- 900px 以上的所有规则零改动

### 规则明细
1. **表格**：`students.html`/`en/students.html` 的 `<table class="simple-table">` 外包
   `<div class="table-scroll">`；`.table-scroll { overflow-x: auto; -webkit-overflow-scrolling: touch; }`；
   手机下表格 `min-width: 520px`、字号 13px、th/td padding 收紧（列宽完整保留，横向滑动阅读）
2. **研究兴趣**（≤900px）：`.interest-item` 改纵向堆叠（标题在上、内容在下），`.it` 取消 min-width，
   英文注释随标题同行
3. **简历条目**（≤900px）：`.cv-entry` 加 `flex-wrap: wrap`，日期自动换行
4. **网格**（≤900px）：`.contact-grid`、`.card-grid` 改单列
5. **导航**（≤900px）：按钮字号 15px、padding 收紧、gap 缩小
6. **标题/留白**（≤900px）：`.section` padding 40px 0、`.page-header` 32px 0、
   `.page-header h1` 26px、`.section-title h2` 22px、`.about-text p` 16.5px
7. **极窄屏**（≤480px）：容器 padding 18px、`.name-en` 32px、`.name-cn` 19px、
   `.stat .num` 30px、`.section-title h2` 20px、keywords 标签紧凑化、`.pdf-btn` 全宽居中

### 明确不做
- 不改任何 HTML 除 students 表格外层包裹；不动桌面端任何样式值
- 不做汉堡菜单（7 项导航 wrap 后可用性可接受）
- 不做表格转卡片（用户否决，保持滑动）

## 验证
- CSS 括号平衡、无桌面断点规则回归
- 本地与线上页面在 375px/480px/900px 模拟下检查
- 推送后用户真机复核
