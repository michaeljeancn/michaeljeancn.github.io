# 米糕的免费信息小站 · GitHub Pages 静态镜像

> **本仓库是只读静态镜像，不是源码仓库。** 这里只存放已经生成好的 HTML / JSON / RSS 产物，不含构建脚本、后端服务、后台管理页或任何凭据。源码与每日构建都运行在本机的工作区里。

| | |
|---|---|
| **镜像站点（本仓库）** | https://michaeljeancn.github.io/ |
| **原站（被镜像的源站）** | `free-audio-hub.app.workbuddy.host`（WorkBuddy 平台托管，需分享口令访问；**口令不在本公开仓库中记录**） |
| **部署方式** | GitHub Pages → `main` 分支 → `/(root)` |
| **更新频率** | 每日 18:00（北京时间 UTC+8），由本地自动化构建后推送 |

---

## 这个站点是做什么的

「米糕的免费信息小站」由米糕声音坊维护，面向有声书 / 广播剧 / 影视配音从业者、学生与同业朋友，整理**正版免费的音频插件、音源、效果器与限时免费情报**。

- 只聚合**官方授权免费 / 限时免费**的信息，不收录破解版、盗版或来路不明的分发站点。
- **本站不托管任何插件、音源或采样文件**，所有领取链接一律跳转到厂商官方页面。
- 可按用途、厂商、截止时间检索，分「永久免费」与「限时免费」两个入口；效果器另有 8 个二级子类（组合包 / 声音修复 / 频段控制 / 响度控制 / 空间响应 / 声音美化 / 综合效果 / 其他）。
- 附「精选专题」「每日更新简报」「即将到期提醒」与 RSS 订阅。

页面：`index.html` 资源库 · `topics.html` 精选专题 · `about.html` 关于与数据来源 · `works.html` 我的作品。

---

## 数据从哪里来

每日构建时由本地 `scripts/build-data.mjs` 汇总以下几类来源，只保留 `price = 0` 的真正免费条目：

| 来源 | 内容 | 采集方式 |
|---|---|---|
| sound-news 聚合 API | 主体数据（约 1.9 万条原始情报，逐条筛选） | `wangshubianqu.com/api/sound-news`，公开 HTTP，无需密钥 |
| 厂商官方商品接口 | Native Instruments、Waves、iZotope、Spitfire Audio、Soundiron、Heavyocity 等大厂的 0 元商品 | Shopify `/products.json` / JSON-LD 直采，带官方商品图 |
| 免费情报资讯站 | BPB、MIDIFAN、Rekkerd 的限免报道 | RSS 与页面抓取（MIDIFAN 需 GBK 转 UTF-8） |
| 手动官方收录 | 例如 Sennheiser / Dear Reality 系列 11 款永久免费插件、Waves 黑五 2026 限免 | 人工核对官网后写入 `scripts/manual-items.json`，每次构建无条件注入 |
| 原站后台 | 自有情报录入、锁定的人工分类修正 | 原站 `/admin`，构建时读取；失败时回退本地缓存 |

构建过程中的处理：剔除付费与折扣项、隐藏已结束的限免、按标题与正文二次判定「限时 vs 永久」、与 API 同款去重、按规则生成大类和效果器二级分类、解析截止日期、人工分类修正最后套用且不会被自动更新覆盖。中文译名与配图来自本地富化缓存。

> 条目信息以厂商官方页面为准，领取前请自行核对授权条款与系统要求。各商标与产品归其各自厂商所有。

---

## 页面什么时候更新

```
每日 18:00 (UTC+8)
  本地自动化 → ① node scripts/build-data.mjs 重算 public/
              → ② bash gh-pages-sync.sh  推送到本仓库 main   ← 最高优先级，最先完成
              → ③ 若原站在线，再部署原站；若已停用则跳过
  GitHub Pages 检测到 main 变更后自动重建（通常几分钟内生效）
```

- 每次构建生成唯一 `buildId`（UTC 时间戳），写在 `data-meta.json`，用于前端缓存失效；当前规模**约 8300 条永久免费 + 40 余条限免**，每日随上游波动，准确数字见 `data-meta.json` 的 `counts`。
- **本仓库没有 GitHub Actions、没有 CI、不调用任何 GitHub API 做构建**。GitHub 侧零计算、零密钥、零外部请求，只是一个静态托管端点。
- 数据无变化时同步脚本会跳过推送，不产生空提交。
- 因为产物由每日构建整体覆盖，**请不要通过 Pull Request 修改 `data-*.json`**——改动会在下次构建被覆盖。内容勘误请更新本地源数据或 `manual-items.json`。

---

## 双站关系

| | 原站（WorkBuddy） | 本镜像（GitHub Pages） |
|---|---|---|
| 地址 | `free-audio-hub.app.workbuddy.host`（口令访问） | https://michaeljeancn.github.io/ |
| 可见性 | 分享口令半公开，不面向搜索引擎 | **完全公开**，会被搜索引擎收录 |
| 后端 | Node 服务、云数据库、后台 `/admin`、运行时 LLM 翻译 | **无**，纯静态只读 |
| 数据 | 同一份本地构建产物 | 同一份本地构建产物 |
| 更新 | 每日 18:00（仅在线时） | 每日 18:00，且优先于原站部署 |

两站并行运行，本仓库只是原站 `public/` 的一份快照，不含 `admin.html`（后台页含密钥与内部 API）、`server.js`、`admin.js`、`cloud.config.json` 或任何凭据。

关于页与订阅说明里「分享口令」的措辞沿用原站；**在本镜像上 `/feed.xml` 可直接公开访问，无需口令**。

---

## 停掉原站后，本镜像会怎样

**页面照常打开、浏览、搜索、看详情，且仍每天更新**，不需要改动任何配置：

- 数据文件里没有指向原站的 URL；图片、来源与领取链接都是外链到厂商官网。前端运行时不请求原站域名；唯一的 `/api/category-overrides` 是相对路径且有完整容错，404 时沿用构建时已固化进数据的分类。
- 构建脚本检测到原站不可达后切换 `OUR_OFFLINE=1` 离线模式，跳过两处原站 API、直接读本地缓存，不等网络超时也不中断。
- RSS 与 JSON-LD 的站点地址统一指向本镜像，停原站不会产生死链。
- **会冻结的**：原站后台手工新增的自有情报、人工分类修正（停在上次缓存）。
- **仍每天更新的**：第三方聚合数据、厂商官方直采、手动官方收录、分类规则、更新简报与 RSS。

---

## 仓库里有什么

```
index.html  topics.html  about.html  works.html   四个页面
assets/                                           favicon 与亮/暗两版 Logo
data-meta.json                                    buildId、条数、分类分布
data-free.N.json                                  永久免费分页（每页 2000 条，当前 5 页）
data-limited.json                                限时免费
data-digest.json                                 每日更新简报
topics/*.json  topics/slim.json                  专题数据与精简索引
feed.xml                                         RSS 2.0 订阅源
.nojekyll                                        禁用 Jekyll，保证 JSON 原样发布
README.md                                        本文件
```

**同步脚本如何工作**：本机另有一个独立的 `gh-pages-mirror/` git 仓库，`gh-pages-sync.sh` 每次把 `public/` 复制进去，采用**单 commit + `commit --amend` / `push --force-with-lease`**，让本仓库永远只保留最新一份约 12 MB 的快照，不会每天堆积历史把仓库撑爆。本 `README.md` 也由该脚本从本地源文件自动带入，重建镜像目录不会丢失。

---

## 维护与联系

- 源码、构建脚本、每日自动化都保存在本地工作区，**不在 GitHub 上**；本仓库只做发布。
- 镜像仓库：`michaeljeancn/michaeljeancn.github.io`，公开，`main` 分支根目录部署。
- 联系邮箱：michaeljeancn@outlook.com
