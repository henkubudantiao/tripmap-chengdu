# Btrip - 旅行攻略 HTML 地图生成器

生成单文件 HTML 旅行攻略，包含交互式地图、时间线行程、小红书数据驱动的店铺推荐。

## 核心特性
- Leaflet 地图 + CartoDB Light 底图
- 按天分 Tab，Apple 风格 UI
- 小红书高赞笔记 + 评论区实测数据（MediaCrawler 采集）
- **SHOPS 店铺推荐系统**：可展开的区域店铺列表，含评论引用、推荐菜品
- 导航 Action Sheet（Apple Maps / 高德 / 百度）
- 移动端优先，触控友好

---

## ⚠️ 攻略制作流程（必须遵循）

### 第一步：需求确认
收集用户信息：目的地、天数、日期、人数、住宿位置、特殊要求（喝茶/避坑/必去景点等）

### 第二步：初步线路规划
根据用户需求，先规划大致行程线路（按天分配景点/区域），**询问用户是否需要调整**。

### 第三步：逐个采集数据（⚠️ 不能并发）
确认线路后，**一个关键词一个关键词地采集**：
1. 用 MediaCrawler 采集小红书笔记 + 评论
2. 每个关键词采集完后**立即保存**到数据文件夹
3. 采集完一个再采下一个，**不要并发**（并发会导致评论文件被清空）
4. Cookie 可能过期，采集前先测试一个关键词验证 Cookie 有效性

### 第四步：数据汇总
将所有采集数据合并到一个文件夹中，去重，统计覆盖情况。

### 第五步：生成攻略
用汇总后的数据生成 HTML 攻略，推送到 GitHub。

### 数据保存规范
所有采集数据保存在 `{目的地}/xhs-data/` 文件夹中：
```
{目的地}/
├── xhs-data/
│   ├── notes.json      # 所有笔记（去重）
│   └── comments.json   # 所有评论（去重）
├── {目的地}-v{版本}.html
└── ...
```

---

## MediaCrawler 使用要点
- **路径**：`/root/MediaCrawler/`
- **运行**：`cd /root/MediaCrawler && ./venv/bin/python main.py --platform xhs --type search --keywords "关键词"`
- **Cookie 配置**：`config/base_config.py` 中的 `COOKIES` 字段
- **⚠️ 评论文件每次启动会清空**：必须先备份再重启爬虫
- **保存数据**：笔记在 `search_contents_*.json`，评论在 `search_comments_*.json`
- **采集间隔**：每个关键词之间等待足够时间（120-180秒），避免被封

## 数据结构
- `XHS[]`：小红书笔记数组 `{id, title, likes}`
- `SHOPS{}`：按区域分组的店铺推荐 `{name, desc, quote, tags, dishes, url}`
- `DAYS[]`：每日行程 `{id, label, date, title, color, locations[]}`
- `location.xhs[]`：关联笔记 `[{id, label}]`
- `location.detail`：展开详情（保留换行）

## HTML 关键规则
1. **XHS 用 id 拼 URL**：`xhsUrl(id)` → `https://www.xiaohongshu.com/explore/{id}`
2. **SHOPS 必须有 quote**：小红书评论区精选，用「」包裹，带❤️点赞数
3. **shopToggle 自动关联**：在 locCard 中按景点名称匹配 SHOPS key
4. **酒店常驻地图**：橙色 marker，始终显示
5. **路线 bar**：每个 D-day 头部显示当日路线概要
6. **总览页**：info-grid（人数/住宿/节奏/离开）+ 行程概览 + 住宿推荐 + 预约清单 + 小红书合集
7. **避坑警告**：评论区差评多的景点/店铺要标注⚠️

## 参考文件
- `template.html`：最新模板（v7.0+）
- `references/design-spec.md`：设计规范
