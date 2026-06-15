# NVIDIA 官方信源路由矩阵

> 对接 [journey-taxonomy.md](../模型对比/journey-taxonomy.md) L1/L2 任务 · 定义 LLM 检索时的站点级优先级

## 使用说明

| 列 | 含义 |
|----|------|
| **优先域** | 该技术问题的权威首选；答案应从这里开始 |
| **次选域** | 补充、教程、社区经验；需与优先域交叉验证 |
| **应排除** | 不应作为该技术问题的信源；纳入则降低「官方源覆盖」得分 |

域缩写：

- `docs` = `docs.nvidia.com`
- `dev` = `developer.nvidia.com`
- `forum` = `forums.developer.nvidia.com`
- `ngc` = `catalog.ngc.nvidia.com`
- `build` = `build.nvidia.com`
- `blogs` = `blogs.nvidia.com`
- `news` = `nvidianews.nvidia.com`
- `investor` = `investor.nvidia.com`
- `main` = `www.nvidia.com`（主站产品页）

---

## L1 · 环境与安装

### I · 版本兼容排查

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（CUDA 兼容性文档） | `developer.nvidia.com`（Release Notes 入口） | `blogs.*`, `investor.*`, `news.*` |

**说明：** 版本矩阵以 docs 为准；PyTorch 等框架兼容需叠加 `docs.pytorch.org`，不在 NVIDIA 域内。

### J · 安装与环境变量

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/cuda-installation-guide-*/` | `developer.nvidia.com`（下载页） | `blogs.*`, `investor.*`, `main`（产品页） |

**说明：** 安装步骤、环境变量（`PATH`、`LD_LIBRARY_PATH`）以 Installation Guide 为权威。

### K · 容器 / 镜像搭建

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `catalog.ngc.nvidia.com` | `docs.nvidia.com/ngc/` | `blogs.*`, `nvidianews.*`, `investor.*` |

**说明：** NGC 容器 tag、Helm Chart 从 catalog 获取；`nvidia-container-toolkit` 配置见 docs/ngc 或 docs/cuda。

---

## L1 · 算子开发

### D · 自定义算子开发

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（CUDA C++ Programming Guide） | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** 自定义 kernel 以 CUDA Programming Guide 为准；技术博客可作优化案例参考。

### L · 算子精度排查

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `forums.developer.nvidia.com` | `blogs.*`, `investor.*` |

**说明：** 浮点精度、舍入行为见 CUDA 文档；论坛有社区调试经验。

### M · 动态 shape / Tiling

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/cudnn/` | `docs.nvidia.com/cuda/` | `blogs.*`, `investor.*` |

**说明：** cuDNN API 对动态 shape 的支持以 cuDNN 文档为准；自定义 Tiling 见 CUDA 文档。

### N · 算子融合

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/cudnn/` | `docs.nvidia.com/tensorrt/` | `blogs.*`, `investor.*` |

**说明：** cuDNN Fusion API、TensorRT 融合策略分属不同产品文档。

### O · 注册与框架集成

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `developer.nvidia.com` | `blogs.*`, `investor.*` |

**说明：** CUDA 与 PyTorch/TensorFlow 集成见 CUDA 文档及框架官方文档；NVIDIA 域内以 docs 为主。

### C · 算子库选型

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/cublas/`、`docs.nvidia.com/deeplearning/cudnn/` | `developer.nvidia.com/blog` | `blogs.*`, `investor.*`, `main` |

**说明：** cuBLAS / cuDNN / cuFFT 选型对照 API 文档；技术博客可作场景案例。

---

## L1 · 训练

### F · 分布式训练配置

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/nccl/` | `docs.nvidia.com/cuda/` | `blogs.*`, `investor.*` |

**说明：** NCCL 通信原语与多卡配置以 NCCL 文档为准；PyTorch DDP 等需叠加框架文档。

### P · 混合精度训练

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/cudnn/` | `docs.nvidia.com/cuda/` | `blogs.*`, `investor.*` |

**说明：** AMP、FP16/BF16 支持见 cuDNN 与 CUDA 文档；框架侧 loss scaling 见 PyTorch 文档。

### Q · 显存 / OOM 优化

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `forums.developer.nvidia.com` | `blogs.*`, `investor.*` |

**说明：** 显存管理、Unified Memory 见 CUDA 文档；论坛有 OOM 调试案例。

### R · 精度 / 收敛排查

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/cudnn/` | `forums.developer.nvidia.com` | `blogs.*`, `investor.*` |

**说明：** 数值行为以 cuDNN/CUDA 文档为准；收敛问题常需结合框架文档。

---

## L1 · 推理与部署

### A · 模型转换 / 导出

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/tensorrt/` | `docs.nvidia.com/nim/` | `blogs.*`, `investor.*` |

**说明：** ONNX/TensorRT 转换以 TensorRT 文档为准；NIM 导出见 NIM 文档。

### S · 推理服务部署

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `build.nvidia.com` | `catalog.ngc.nvidia.com`、`docs.nvidia.com/nim/` | `blogs.*`, `investor.*` |

**说明：** NIM 在线试用与部署向导在 build；容器化部署见 NGC catalog + docs。

### H · 量化

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/tensorrt/` | `docs.nvidia.com/deeplearning/cudnn/` | `blogs.*`, `investor.*` |

**说明：** INT8/FP8 量化以 TensorRT 文档为准；cuDNN 量化 API 见 cuDNN 文档。

### T · 动态 batch / shape 推理

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/tensorrt/` | `docs.nvidia.com/nim/` | `blogs.*`, `investor.*` |

**说明：** TensorRT 动态 shape profile 配置见 TensorRT 文档；NIM 动态推理见 NIM 文档。

---

## L1 · 性能优化

### B · Profiling 定位瓶颈

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/nsight-systems/`、`docs.nvidia.com/nsight-compute/` | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** Nsight Systems / Nsight Compute 使用见对应 docs；技术博客有 profiling 案例。

### U · 访存 / occupancy 优化

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（Best Practices Guide） | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** Occupancy Calculator、内存合并见 CUDA Best Practices Guide。

### V · 计算与传输重叠

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** Stream、Event、异步拷贝见 CUDA Programming Guide。

### W · 多卡通信优化

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/deeplearning/nccl/` | `docs.nvidia.com/cuda/` | `blogs.*`, `investor.*` |

**说明：** NCCL 集合通信调优见 NCCL 文档；P2P 访问见 CUDA 文档。

---

## L1 · 调试

### X · 内存越界定位

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（CUDA-MEMCHECK） | `docs.nvidia.com/nsight-compute/` | `blogs.*`, `investor.*` |

**说明：** `compute-sanitizer` / CUDA-MEMCHECK 见 CUDA 文档；Nsight 调试见 Nsight Compute 文档。

### E · 报错码 / 异常排查

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `forums.developer.nvidia.com` | `blogs.*`, `investor.*`, `news.*` |

**说明：** CUDA 错误码、驱动错误以 docs 为准；论坛有社区排查路径。

---

## L1 · 迁移

### G · CUDA → 昇腾 迁移

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（对照源） | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** 本任务跨厂商；NVIDIA 侧以 CUDA 文档为「源栈」参考，目标栈见 Ascend/CANN 文档（`hiascend.com` 等）。

### Y · 跨芯片迁移

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/` | `developer.nvidia.com/blog` | `blogs.*`, `investor.*` |

**说明：** 跨 NVIDIA GPU 代际（如 Ampere → Hopper）见 CUDA 兼容性文档与 Release Notes。

### Z · 概念心智模型对照

| 优先域 | 次选域 | 应排除 |
|--------|--------|--------|
| `docs.nvidia.com/cuda/`（Programming Guide 概念章） | `developer.nvidia.com/blog`、`learn.nvidia.com` | `blogs.*`, `investor.*` |

**说明：** Grid/Block/Thread、SM 等核心概念以 Programming Guide 为准；DLI 课程可作补充。

---

## 汇总：域级优先级

| 域 | 全局角色 | 典型任务 |
|----|----------|----------|
| `docs.nvidia.com` | **权威首选** | 几乎全部 L2 任务 |
| `developer.nvidia.com` | 次选 / 下载 / 教程 | J, D, C, Z |
| `forums.developer.nvidia.com` | 社区补充 | E, L, Q, R |
| `catalog.ngc.nvidia.com` | 部署行动点 | K |
| `build.nvidia.com` | 推理/API 行动点 | S |
| `developer.nvidia.com/blog` | 工程案例 | D, U, V, B |
| `learn.nvidia.com` | 概念培训 | Z |
| `blogs.nvidia.com` | **排除** | — |
| `nvidianews.nvidia.com` | **排除** | — |
| `investor.nvidia.com` | **排除** | — |
| `www.nvidia.com` | 低优先级背景 | — |

---

## 与跨厂商对照的衔接

本矩阵仅覆盖 **NVIDIA 域内** 路由。与 [大模型抓取 / 官方信源感知弱](../大模型抓取/references/官方信源感知弱/) 中华为/Ascend 站点归类的对照：

| NVIDIA 域 | 职能 | Ascend 侧近似 |
|-----------|------|---------------|
| `docs.nvidia.com` | 官方技术文档 | `hiascend.com/document` |
| `developer.nvidia.com` | 开发者门户 / 下载 | `developer.huawei.com`（论坛、CANN Kit 文档） |
| `catalog.ngc.nvidia.com` | 容器 / 模型目录 | 昇腾镜像仓库 / NGC 类比 |
| `forums.developer.nvidia.com` | 开发者论坛 | `developer.huawei.com` 昇腾论坛 |
| `build.nvidia.com` | AI API 试用 | 无直接对应（Ascend 侧待补充） |

后续可扩展为完整 **跨厂商信源路由对照表**。

---

## 相关文档

- [nvidia-site-ia.md](./nvidia-site-ia.md) — 群组详述
- [design-principles.md](./design-principles.md) — IA 设计原则
- [journey-taxonomy.md](../模型对比/journey-taxonomy.md) — L2 任务定义
- [llm-metrics-framework.html](../模型对比/llm-metrics-framework.html) — 「官方源覆盖」评估指标
