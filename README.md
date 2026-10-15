# ai-auto-video

面向抖音的 AI 短视频制作与发布工具，计划整合 MoneyPrinterTurbo 和 social-auto-upload，实现选题、脚本、配音、剪辑、预览确认、发布与效果记录。

当前处于方案设计阶段，尚未实现或验证完整工作流。第一版以单账号、无 GPU 部署、确认后发布为目标。

当前经营主线为自营账号推广：用户负责选品、选题与内容判断，系统承担制作、确认后的发布及可取得的效果记录。定制服务不作为主线；仅导出视频属于技术里程碑，最终运营需验证自动发布、推广关联与收入记录。

当前优先跑通国内抖音的单账号商品橱窗带货，海外平台和手机 Agent 接入暂缓。第一次闭环按[国内试运行步骤](docs/domestic-pilot.md)执行。

## 项目文档

- [AI 短视频制作与发布实施方案](docs/implementation-plan.md)：产品目标、使用流程、模块分工、技术设计、费用控制、开发阶段与验收标准。
- [第一版开发细则](docs/mvp-development-plan.md)：分镜时间轴、配音字幕、业务接口、任务恢复、开发任务与具体验收门槛。
- [盈利方案与首月验证计划](docs/monetization-plan.md)：自营账号带货、用户与系统分工、盈亏测算和一个月的商业验证安排。
- [国内试运行步骤](docs/domestic-pilot.md)：抖音账号条件、首个商品和视频任务、自动发布与挂商品验收。
- [低成本商业思考](docs/business-thinking.md)：带货佣金与标准化小工具的取舍、付费验证顺序，以及按人工投入评价收益的方法。
- [海外盈利模式调研](docs/overseas-revenue-models.md)：软件订阅推广、Amazon 站内内容佣金、插件销售与游戏创作者分成的机制和参与条件。
- [视频生成项目调研](research/video-generation/README.md)：GitHub 候选项目对比、生成方法与后续样片验证方案。

## 相关项目

- [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)：视频生成组件。
- [social-auto-upload](https://github.com/dreammis/social-auto-upload)：发布组件候选。
