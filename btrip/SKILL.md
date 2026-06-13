# Btrip - 旅行攻略 HTML 地图生成器

生成单文件 HTML 旅行攻略，包含交互式地图、时间线行程、小红书数据驱动的店铺推荐。

## 核心特性
- Leaflet 地图 + CartoDB Light 底图
- 按天分 Tab，Apple 风格 UI
- 小红书高赞笔记 + 评论区实测数据
- **SHOPS 店铺推荐系统**：可展开的区域店铺列表，含评论引用、推荐菜品
- 导航 Action Sheet（Apple Maps / 高德 / 百度）
- 移动端优先，触控友好

## 使用方式
读取 `template.html` 作为模板，根据用户提供的旅行信息（目的地、天数、日期、人数、住宿等）修改数据。

## 数据结构
- `XHS[]`：小红书笔记数组 `{id, title, likes}`
- `SHOPS{}`：按区域分组的店铺推荐 `{name, desc, quote, tags, dishes, url}`
- `DAYS[]`：每日行程 `{id, label, date, title, color, locations[]}`
- `location.xhs[]`：关联笔记 `[{id, label}]`
- `location.detail`：展开详情（保留换行）

## 关键规则
1. **XHS 用 id 拼 URL**：`xhsUrl(id)` → `https://www.xiaohongshu.com/explore/{id}`
2. **SHOPS 必须有 quote**：小红书评论区精选，用「」包裹
3. **shopToggle 自动关联**：在 locCard 中按景点名称匹配 SHOPS key
4. **酒店常驻地图**：橙色 marker，始终显示
5. **路线 bar**：每个 D-day 头部显示当日路线概要
6. **总览页**：info-grid（人数/住宿/节奏/离开）+ 行程概览 + 住宿推荐 + 预约清单 + 小红书合集

## 参考文件
- `template.html`：v5.0 最新模板
- `references/design-spec.md`：设计规范
