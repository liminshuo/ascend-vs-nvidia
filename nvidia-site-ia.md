# NVIDIA 子站体系信息架构参考

> 站点层 IA 参考 · 对接 [模型对比/journey-taxonomy.md](../模型对比/journey-taxonomy.md) 与 [大模型抓取](../大模型抓取) 信源路由研究

## 概述

NVIDIA 的 Web 体系按 **受众与职能** 划分为若干群组，核心逻辑是：

- **主站** `nvidia.com` 承担品牌与产品展示
- **`developer.*` 系列** 服务技术用户（SDK、文档入口、论坛、AI API）
- **`blogs` / `nvidianews`** 做内容传播
- **`ngc` / `build` / `marketplace`** 是实际的服务交付入口
- **`investor`** 面向资本市场
- **GTC** 以主站路径形式承载会议活动

各群组 **基本互不交叉**，受众泾渭分明。对 LLM 与爬虫而言，URL 域名本身即携带 IA 信号——技术问答应优先路由到 `docs.nvidia.com` / `developer.*`，而非 `blogs.*` 或 `investor.*`。

---

## 群组总览

| 群组 | 代表域名 | 主受众 | 核心职能 | 典型内容类型 |
|------|----------|--------|----------|--------------|
| 主站 | `nvidia.com` | 大众 / 采购决策 | 品牌、产品、行业方案 | 产品页、解决方案、Shop 入口 |
| 开发者生态 | `developer.*` | 工程师 | SDK 下载、文档入口、论坛、AI API | 教程、SDK、Q&A、NIM 试用 |
| 技术文档 | `docs.nvidia.com` | 工程师 | 官方技术文档中心 | API 参考、安装指南、Release Notes |
| 内容与媒体 | `blogs.*` / `nvidianews.*` | 媒体 / 泛用户 | 传播、公告 | 新闻稿、行业观点、案例故事 |
| 云服务与目录 | `catalog.ngc.*` / `build.*` / `marketplace.*` | 部署者 / 采购 | 软件与模型交付 | 容器、Helm Charts、模型 API |
| 资本市场 | `investor.nvidia.com` | 投资者 / 分析师 | 合规披露 | 财报、SEC 文件、演示材料 |
| 会议活动 | `nvidia.com/gtc/` | 开发者 / 媒体 | 活动聚合 | 议程、回放、注册 |

---

## 各群组详述

### 1. 主站 · `nvidia.com`

**域内导航 IA：** 顶栏 Mega Menu 的 L0/L1 结构（Products / Solutions / Industries 三维矩阵）见 **[nvidia-main-nav-ia.md](./nvidia-main-nav-ia.md)**。

**两层 IA 对比：** 子站层按受众/职能分域、基本互不交叉；主站导航层允许同一产品从 Products、Solutions、Industries 多路径触达（刻意交叉重复），以适配不同用户心智模型。

**域名形态：** 根域，部分职能以路径挂载（如 `/gtc/`、`/shop` 跳转）

**核心站点：**

| URL | 职能 |
|-----|------|
| `www.nvidia.com` | 品牌首页、产品导航、行业解决方案 |
| `www.nvidia.com/gtc/` | GTC 大会专区（路径型子站，见下文） |
| Shop 入口 | 跳转至 `marketplace.nvidia.com` |

**与其他群组的边界：**

- 出站链接为主：Developer 入口 → `developer.nvidia.com`；文档 → `docs.nvidia.com`；Shop → `marketplace.nvidia.com`
- 主站几乎不含 API 级技术细节；产品页侧重营销与选型，不含集成步骤

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 低 | 产品概览、行业方案可作背景，不可替代技术文档 |
| 排除 | 不作为 API 参考、安装步骤、报错排查的首选源 |

---

### 2. 开发者生态 · `developer.*`

**域名形态：** 独立子域集群，部分内容以路径挂载

**核心站点：**

| URL | 职能 |
|-----|------|
| `developer.nvidia.com` | 开发者主站：Technical Blog 入口、SDK 下载、文档导航 |
| `developer.nvidia.com/blog` | 技术博客（路径型，面向开发者，工程深度内容） |
| `forums.developer.nvidia.com` | 开发者论坛：CUDA、NIM、DGX、量子计算等板块 |
| `build.nvidia.com` | AI API 目录：在线试用与部署 NIM 模型 |

**与其他群组的边界：**

- `developer.nvidia.com` → `docs.nvidia.com`：文档深链，非重复托管
- `developer.nvidia.com/blog` vs `blogs.nvidia.com`：前者技术向，后者大众向，入口分离
- `build.nvidia.com` → `catalog.ngc.nvidia.com`：Build 侧重 API 试用，NGC 侧重容器/模型目录

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 高（次选） | SDK 下载页、Getting Started 教程 |
| 中 | 技术博客：适合概念与最佳实践，需与 docs 交叉验证 |
| 中 | 论坛：社区经验，非官方规范，作补充 |
| 高（行动） | `build.nvidia.com`：NIM 部署与 API 试用的首选入口 |

---

### 3. 技术文档 · `docs.nvidia.com`

**域名形态：** 独立子域；内部按 **产品线路径** 组织（第二层 IA）

**典型路径：**

| 路径 | 产品/主题 |
|------|-----------|
| `/cuda/` | CUDA Toolkit、cuBLAS、cuFFT 等 |
| `/deeplearning/cudnn/` | cuDNN |
| `/ngc/` | NGC 使用与部署文档 |
| `/drive/` | DRIVE 平台 |
| `/tensorrt/` | TensorRT |
| `/nim/` | NIM 集成文档 |

**与其他群组的边界：**

- 文档中心不承载博客式叙事；Release Notes 与 API 参考为权威来源
- 下载二进制 → `developer.nvidia.com`；拉取容器 → `catalog.ngc.nvidia.com`

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| **最高** | API 参考、安装指南、版本兼容矩阵、报错码说明 |
| 次选 | 与 `developer.nvidia.com` 教程交叉引用时，以 docs 为准 |

---

### 4. 内容与媒体 · `blogs.*` / `nvidianews.*`

**域名形态：** 独立子域；技术博客例外地挂在 `developer.nvidia.com/blog`

**核心站点：**

| URL | 职能 | 受众 |
|-----|------|------|
| `blogs.nvidia.com` | 官方博客：AI / Gaming / Robotics 等分类 | 大众、媒体 |
| `developer.nvidia.com/blog` | 技术博客 | 开发者 |
| `nvidianews.nvidia.com` | 新闻室：新闻稿与公告 | 媒体、投资者 |

**与其他群组的边界：**

- 不含 API 级参数说明；案例与观点为主
- 新闻稿可能早于 docs 发布产品名，但不含集成细节

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 低 | 背景、趋势、产品发布日期 |
| 排除 | 安装步骤、API 签名、报错码排查 |

---

### 5. 云服务与软件目录 · `catalog.ngc.*` / `build.*` / `marketplace.*`

**域名形态：** 独立子域

**核心站点：**

| URL | 职能 |
|-----|------|
| `catalog.ngc.nvidia.com` | NGC 软件目录：GPU 优化容器、模型、Helm Charts |
| `build.nvidia.com` | AI API 目录：NIM 在线试用与部署 |
| `marketplace.nvidia.com` | NVIDIA 商城（主站 Shop 跳转目标） |

**与其他群组的边界：**

- **行动点** 与 **说明性文档** 分离：Catalog/Build 用于「获取与部署」，docs 用于「如何集成」
- Marketplace 面向硬件与软件采购，与 NGC 容器目录职能不同

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 高 | 容器/模型/Helm 选型、镜像 tag、NGC 资源拉取 |
| 高 | NIM API 试用与部署入口 |
| 低 | Marketplace 产品页（采购向，非集成向） |

---

### 6. 资本市场 · `investor.nvidia.com`

**域名形态：** 独立子域

**核心内容：** 财报、SEC 文件、投资者演示、公司治理

**与其他群组的边界：** 完全独立，无技术文档交叉

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 排除 | 一切工程技术问答 |

---

### 7. 会议活动 · `nvidia.com/gtc/`

**域名形态：** 主站路径型子站（内容体量相当于独立子站）

**核心内容：** 大会议程、注册、回放、主题演讲

**与其他群组的边界：**

- 回放与讲义可能链至 `developer.nvidia.com` 或 `docs.nvidia.com`
- 职能上接近「内容与媒体」，但周期性、活动驱动

**LLM 信源路由：**

| 优先级 | 说明 |
|--------|------|
| 中 | 新特性概览、架构趋势（需与 docs Release Notes 交叉验证） |
| 低 | 不作为 API 或安装步骤的权威源 |

---

## 边界说明

### 路径型子站

以下站点在职能上等同独立子站，但在 SEO、Cookie 域与导航上仍挂靠父域：

| 路径 | 挂靠父域 | 职能等价于 |
|------|----------|------------|
| `developer.nvidia.com/blog` | developer.* | 独立技术博客子站 |
| `www.nvidia.com/gtc/` | nvidia.com | 独立会议活动子站 |

### 重定向别名与主站路径（附录）

以下 hostname **存在但非独立 IA 子站**——HTTP 301 至主站路径或 catalog，职能上应归入主站路径型内容区：

| 别名域名 | 实际落地 | 建议归类 | 职能 |
|----------|----------|----------|------|
| `research.nvidia.com` | `nvidia.com/en-us/research/` | 主站路径型 | 研究论文、Demo、Research Code |
| `learn.nvidia.com` | `nvidia.com/en-us/training` | 主站路径型 | DLI 培训、认证课程 |
| `ngc.nvidia.com` | `catalog.ngc.nvidia.com` | 云服务与目录 | NGC 入口别名（与 catalog 同 IA） |

**页脚出站补充：** Mega Menu 出站表未覆盖的常用入口见页脚三列——Developers → `developer.*`；Documentation → `docs.*`；Technical Blog → `developer.nvidia.com/blog`；Newsroom / Company Blog → `nvidianews.*` / `blogs.*`；Investors → `investor.*`；GTC → `nvidia.com/gtc/`。

### docs 第二层 IA

`docs.nvidia.com` 之下还有 **按产品线划分** 的文档树，站点层 IA 仅解决「去哪个域」，产品线路径解决「域内去哪篇」：

```
docs.nvidia.com
├── /cuda/              → CUDA Toolkit、cuBLAS、cuFFT …
├── /deeplearning/      → cuDNN、TensorRT …
├── /ngc/               → NGC 部署与使用
├── /nim/               → NIM 集成
└── /drive/             → DRIVE 平台
```

---

## 站点层与页层 IA 的关系

本工作区描述 **站点层**（去哪个域）；[大模型抓取](../大模型抓取) 项目描述 **页层**（单篇文档的轻量 IA 元数据、双轨交付、抓取入口）。二者关系：

```
站点层（本目录）          页层（大模型抓取）
─────────────────        ─────────────────
nvidia.com               渲染页 vs 机器源
developer.*       →      面包屑 / 版本 / 文档 ID
docs.*                   跨页 prev/next 关系
catalog.ngc.*            llms.txt / sitemap
```

技术问答的完整路由 = **站点层域过滤** + **页层文档定位**。

---

## 参考链接

- [nvidia-main-nav-ia.md](./nvidia-main-nav-ia.md) — 主站 Mega Menu 导航 IA（L0/L1）
- [journey-taxonomy.md](../模型对比/journey-taxonomy.md) — L1/L2 任务分类
- [source-routing-matrix.md](./source-routing-matrix.md) — 按任务的 NVIDIA 信源路由表
- [design-principles.md](./design-principles.md) — 可迁移 IA 设计原则
- [nvidia-site-map.mmd](./nvidia-site-map.mmd) — 站点关系 Mermaid 图
