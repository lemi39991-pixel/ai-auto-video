# 视频生成方法与 GitHub 项目对比

调研日期：2026 年 10 月 14 日。适用目标：中文抖音短视频、单机起步、尽量不依赖本地 GPU、后续接入统一后台。

## 1 先分清视频是怎么做出来的

“AI 生成视频”可能指不同的工作流程。是否需要 GPU、能否精确解释产品、制作费用和接入难度，都取决于具体路径。

| 方法 | 输入与过程 | 适合内容 | 主要验证点 |
| --- | --- | --- | --- |
| 文案配库存素材 | 文案 → 配音 → 搜索或指定素材 → 字幕 → 合成 | 知识讲解、资讯、观点 | 素材是否贴题，是否只是通用画面 |
| 已有视频生成解说 | 视频 → 理解内容 → 解说脚本 → 选择片段 → 配音与剪辑 | 自有视频解说、内容摘要 | 理解是否准确，解说与时间轴是否匹配 |
| AI 画面生成后合成 | 脚本 → 分镜 → 云端或本地生图、生视频 → 配音与剪辑 | 故事、概念展示、氛围内容 | 画面一致性、生成费用、失败重试 |
| 真实录屏与固定模板 | 录屏或截图 → 指定画面顺序 → 标注与字幕 → 配音合成 | 软件教程、产品使用说明 | 操作是否真实，步骤是否清楚 |

同一项目可能同时支持多条路径。尤其是“无本地 GPU”可能意味着模型在云端运行，仍需网络和调用费用。

对于我们的 AI 工具教程，建议优先使用真实录屏与指定素材；对于通用知识讲解，可以测试库存素材路线。这个判断来自内容需求，不代表任一项目已经能直接完成精确教程制作。

## 2 六个候选项目

下表用于筛选后续试验对象。支持情况来自链接所指的上游说明或抽查文件；未运行的能力不标为实测通过。“中文待验证”也不表示一定不支持。

| 项目 | 主要用途 | 中文情况 | 本地 GPU 情况 | 集成入口 | 初步选择 |
| --- | --- | --- | --- | --- | --- |
| [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | 主题或脚本生成短视频，多种素材路线 | 明确支持中文内容 | 基础路径非必需 | API、CLI、WebUI | 优先试验 |
| [NarratoAI](https://github.com/linyqh/NarratoAI) | 对已有视频生成解说并剪辑 | 面向中文解说，成片需测试 | README 说明显卡非必须；可选本地模型另评估 | WebUI，服务化需检查 | 已有视频路线候选 |
| [ShortGPT](https://github.com/RayVentura/ShortGPT) | 可编程内容与剪辑框架 | README 列出普通话 | 无 GPU 全流程尚待验证 | Python 引擎、Gradio | 框架备选 |
| [short-video-maker](https://github.com/gyoridavid/short-video-maker) | 分场景文本与背景视频合成 | README 限定英文配音 | 提供 CPU 路径，CUDA 为可选加速 | REST API、MCP | 接口设计参考 |
| [MoneyPrinterV2](https://github.com/FujiwaraChoki/MoneyPrinterV2) | YouTube Shorts 与海外营销自动化 | 本轮未确认中文端到端能力 | 依赖所选模型，待验证 | CLI、调度脚本 | 运营流程参考 |
| [ai-shorts-generator](https://github.com/AbdullahNaveed/ai-shorts-generator) | AI 画面或库存素材生成短视频 | 中文脚本、字幕与字体待验证 | 云端画面＋CPU 字幕有配置依据；本地 SDXL 需 CUDA | CLI、本地预览后台 | 素材复用方案参考 |

## 3 MoneyPrinterTurbo

**匹配点：** 从主题或自定义文案到成片，支持本地图片、视频和多种在线素材。文档也列出云端 AI 生图、生视频能力，不能再把它简单视为只会拼接库存素材的工具。中文与无 GPU 配置有明确说明。[README](https://github.com/harry0703/MoneyPrinterTurbo/blob/main/README.md)

**接口证据：** 视频控制器包含创建视频、单独生成音频或字幕、查询任务、上传素材等路由。这里看到的是路由片段，完整前缀和请求字段需要固定版本后读取接口文档。[控制器代码](https://github.com/harry0703/MoneyPrinterTurbo/blob/main/app/controllers/v1/video.py)

**成本与接入判断：** 云端文案和配音、在线素材及 CPU 合成适合先跑样片。任务接口有利于接入后台，但断点恢复、费用记录和确认后发布仍需由本项目处理。

**优先验证：** 指定脚本会不会被再次改写；上传录屏能否按正确顺序使用；中文字幕、配音和素材匹配是否可用。上游列出的 TikTok 发布不等于抖音发布。

**选择建议：** 第一候选；以样片质量决定是否采用。

## 4 NarratoAI

**匹配点：** 工作重点是理解已有视频、生成解说并剪辑。文档列出短剧解说和剪映草稿导出，配置中将视觉理解与文本模型分开设置。[README](https://github.com/linyqh/NarratoAI/blob/main/README.md)、[配置示例](https://github.com/linyqh/NarratoAI/blob/main/config.example.toml)

**硬件与依赖：** README 建议至少 4 核 CPU、8GB 内存，显卡非必须；可选的本地声音克隆等引擎仍需单独检查。使用云端视觉和文本服务会产生相应调用成本。

**接入判断：** 更适合已经有视频、希望自动讲解的场景。当前检查重点是 WebUI 和配置，没有确认稳定的对外任务 API，因此接统一后台前需要检查可调用服务函数或补充适配。

**优先验证：** 能否正确识别自有录屏内容，能否生成时间轴准确的中文解说。README 中“根据口播、文案、视频素材自动匹配”仍列在未完成项，不能承诺任意文案精准配任意视频。

**选择建议：** 有自有长视频时作为第二条路线试验。

## 5 ShortGPT

**匹配点：** 提供短视频、长视频、翻译配音和编辑引擎。README 明确列出普通话，支持 EdgeTTS 等配音方式，并使用结构化编辑描述组织流程。[README](https://github.com/RayVentura/ShortGPT/blob/stable/README.md)

**集成入口：** Python 引擎适合进行程序级改造，也有 Gradio 和 Docker 使用说明。其定位是实验性框架，不能据此推断已具备生产任务服务。

**成本与待验证项：** 模型、配音和素材服务可能有调用费用；依赖安装、当前服务兼容性、中文字体与字幕，以及完整 CPU 路径需要实测。未看到 GPU 硬性要求并不足以证明全流程一定无 GPU 可用。

**选择建议：** 当 MoneyPrinterTurbo 的编排方式难以满足需求时，验证 Python 引擎是否更容易控制镜头和素材。

来源：[仓库](https://github.com/RayVentura/ShortGPT)、[安装说明](https://github.com/RayVentura/ShortGPT/blob/stable/installation-notes.md)。

## 6 short-video-maker

**匹配点：** 输入分场景文本和搜索关键词，通过 Kokoro、Whisper、Pexels、Remotion 合成视频；提供创建、查询状态、获取视频的 REST 接口和 MCP。[README](https://github.com/gyoridavid/short-video-maker/blob/main/README.md)

**关键限制：** 当前文档限定英文配音，FAQ 也说明不能直接上传自有图片和视频拼接。因此中文适配需要更换或调整配音、字幕和素材入口。

**硬件与依赖：** 文档提供 CPU Docker 路径，GPU 用于可选的 Whisper 加速。Kokoro 和 Remotion 主要使用 CPU。自托管仍有机器和素材访问成本。

**选择建议：** 适合作为“提交任务—查询进度—取得成片”的接口参考，当前不优先作为中文教程底座。若以后做英文库存素材视频，再评估直接使用。

其顶层许可证和 Remotion 依赖许可证需要分别查看，见下方许可证表。

## 7 MoneyPrinterV2

**匹配点：** README 重点列出 YouTube Shorts、定时任务和海外营销自动化，提供 CLI 及脚本入口，并要求 Python 3.12。[README](https://github.com/FujiwaraChoki/MoneyPrinterV2/blob/main/README.md)

**配置证据：** 示例配置有本地模型地址、图像服务与浏览器参数，体现它是一组可配置工作流。具体生成路径与资源需求应随所选配置检查。[配置示例](https://github.com/FujiwaraChoki/MoneyPrinterV2/blob/main/config.example.json)

**接入判断：** 当前项目以中文抖音内容为目标，不需要整体引入其海外营销功能。中文成片能力、无 GPU 运行和生成任务接口本轮未验证。

**选择建议：** 参考调度与运营流程，优先级低于前述中文生成候选。不能因名称带有 V2 就把它当作 MoneyPrinterTurbo 的升级版，两者是不同仓库和工作流。

## 8 ai-shorts-generator

**匹配点：** 支持库存素材和云端 AI 画面，包含语义素材库复用、配音、字幕、CLI 和本地预览后台。README 将其描述为周末项目，适合参考实现，不预设为成熟服务。[README](https://github.com/AbdullahNaveed/ai-shorts-generator)

**配置证据：** 示例把字幕设备设为 CPU，画面默认走云端；本地 SDXL 路线要求 CUDA。可以只使用图片推拉效果，或选择付费生成动态片段。[配置示例](https://github.com/AbdullahNaveed/ai-shorts-generator/blob/main/.env.example)

**接入判断：** 值得借鉴“先匹配已有素材、缺少时再生成”的费用控制方式。中文完整支持和适合我们调用的稳定任务 API 尚未确认。

**优先验证：** 汉字换行、中文配音字幕对齐、素材匹配准确性、重复生成时的费用和恢复行为。测试云端画面时记录实际用量，不直接沿用 README 的价格估计。

## 9 许可证与维护信息

这里只记录仓库标识，不展开法律结论。模型、字体、音乐、素材和依赖包的使用条件需另外确认。

| 项目 | 顶层许可证证据 | 维护与版本观察 |
| --- | --- | --- |
| MoneyPrinterTurbo | [MIT](https://github.com/harry0703/MoneyPrinterTurbo/blob/main/LICENSE) | 有配置和 API 路由可核对；试验前固定提交 |
| NarratoAI | [MIT](https://github.com/linyqh/NarratoAI/blob/main/LICENSE) | README 有更新记录；配置中的版本字符串与 README 展示不完全一致，应按提交固定整套文件 |
| ShortGPT | [仓库标识 MIT](https://github.com/RayVentura/ShortGPT) | 本轮读取 stable 分支；实验性框架，依赖可用性待测试 |
| short-video-maker | [MIT](https://github.com/gyoridavid/short-video-maker/blob/main/LICENSE) | README 列出 Remotion 的独立许可证，不能只看顶层 MIT |
| MoneyPrinterV2 | [AGPL-3.0](https://github.com/FujiwaraChoki/MoneyPrinterV2/blob/main/LICENSE) | 与其他候选许可证不同，分发或服务化前单独检查 |
| ai-shorts-generator | [MIT](https://github.com/AbdullahNaveed/ai-shorts-generator/blob/main/LICENSE) | 作者定位为周末项目；需要验证错误处理和可重复运行 |

本轮没有系统审计提交历史、发行频率、Issue 响应或所有依赖，因此不作“持续维护最好”或“已经停止维护”的排名。README 有功能、仓库有代码、运行实测成功是三种不同证据。

## 10 对当前实施方案的影响

建议保留 MoneyPrinterTurbo 为第一候选，把“采用”理解为待样片验证的选型。生成模块通过适配层接入，允许以后换成 NarratoAI 或其他引擎。

本次调研增加三点认识：

1. MoneyPrinterTurbo 文档已经包含云端画面生成；第一轮仍可以只选择本地素材和 CPU 合成，降低变量。
2. 中文教程最需要验证画面与操作对应关系，支持中文配音只是其中一项。
3. 自动发布、账号管理和运营数据继续由整体方案处理，本轮先测生成，不把 TikTok 或 YouTube 自动化能力视为抖音支持。

建议试验顺序：MoneyPrinterTurbo 的固定脚本与自有素材样片 → NarratoAI 的已有视频解说（内容确实需要时）→ ShortGPT 的编排备选。其他项目先保留参考，不同时安装六套环境。

具体输入、通过条件与结果表见[样片验证计划](evaluation-plan.md)。
