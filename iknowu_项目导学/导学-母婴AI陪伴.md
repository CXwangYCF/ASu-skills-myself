# 母婴 AI 陪伴项目导学

> 目标岗位：2028 届大模型应用开发 / AI Agent / RAG 实习。
> 证据范围：已对工作区 10 个代码仓、PRD、评测报告、语料处理产物、设计资料和现有简历做全量文件清单盘点；对可支撑学习与求职主张的源码、报告和数据说明做重点深读。密钥文件未读取，超大原始语料不逐行复述。
> 建议简称：**母婴 AI 陪伴**。

## 1. 前置知识

| 知识点 | 为何需要 | 在项目位置 | 高频度 |
| --- | --- | --- | --- |
| TypeScript、Node.js、NestJS/Hono | AI 编排、BFF、SSE 和协议实现的主语言 | `repository/ai-server-main/src`、`repository/P01-backendServer-main/src` | ★★★★★ |
| React Native、Expo Router | 理解聊天 UI、确认卡和记录同步的客户端入口 | `repository/P01-frontendExpo-main/src` | ★★★ |
| SSE、WebSocket、幂等与事务提交 | 解释流式回复、重连、重复请求与最终权威结果 | `chat.controller.ts`、`chat.service.ts`、`companion-chat-websocket.ts` | ★★★★★ |
| Zod 与契约驱动开发 | 跨仓维护 REST、WS、SSE 和内部 AI 协议 | `ai-contracts-main`、`P01-sharedProtocol-main` | ★★★★ |
| RAG、Embedding、混合检索、Rerank | 理解母婴知识如何被检索、门控和注入 | `memory-service-main`、`ai-server-main/.../knowledge` | ★★★★★ |
| OCR、数据清洗、Chunk 与元数据 | 理解知识库质量从哪里来 | `memory-service-codex-knowledge-corpus-1861-test/chenxi_collect_data` | ★★★★★ |
| LLM 评测、F1、分层样本与回归集 | 正确解释安全、情商、知识和时延指标 | `model_evals-main`、`model_eval_data`、`模型评测` | ★★★★★ |
| 安全分级与医疗边界 | 这是母婴产品能否上线的核心约束 | PRD、`turn-analysis.service.ts`、风险评测报告 | ★★★★★ |
| PostgreSQL、pgvector、HNSW、BM25 | 理解结构化记录、向量召回和可追溯入库 | `memory-service-main`、后端数据库目录 | ★★★★ |
| 可观测性、超时、熔断和降级 | 解释知识服务失败时为何不拖垮聊天主链路 | `remote-knowledge.client.ts`、遥测代码 | ★★★★ |

## 2. 重点亮点与学习顺序

| 亮点标题 | 为什么重要 | 通用关键词 | 先看文件 | 顺序 |
| --- | --- | --- | --- | --- |
| 产品闭环与安全边界 | 先知道系统为何存在，避免只背技术名词 | AI-native、候选确认、事实与表达分离 | `PRD/AI +陪伴 App 改版 母婴版本V2.0 PRD.md` | 1 |
| 十仓职责与协议边界 | 建立全局地图，面试时能说明请求在哪一层负责 | BFF、契约、服务边界、SSE/WS | `P01-backendServer-main/docs/iknowu/IKNOWU_PROJECT_GUIDE.md`、各仓 README | 2 |
| 对话主链路 | 项目最核心的调用链 | 流式生成、幂等、延迟提交、done 权威 | 前端 `PetLauncherScreen.tsx` → 后端 `companion-chat-websocket.ts` → AI `chat.controller.ts`/`chat.service.ts` | 3 |
| 安全权威与知识门控 | 医疗场景不能“先检索再说”，安全必须先于知识注入 | safety authority、approved_rag、零知识降级 | `turn-analysis.service.ts`、`knowledge-routing-phase.ts` | 4 |
| 母婴语料工程 | 这是当前材料里个人责任最清晰、最适合简历展开的部分 | OCR、清洗、元数据、切块、可追溯 | `chenxi_collect_data/cleaned/清洗说明.md` 与 `clean_*.py` | 5 |
| 远程 RAG 与可靠性 | 从“能检索”上升到“能在线稳定使用” | Connect RPC、800ms 超时、熔断、鉴权区分 | `remote-knowledge.client.ts`、`memory-service-main` | 6 |
| 多维评测闭环 | 量化项目效果并避免只讲主观体验 | P0-P7、judge、veto、回归、长尾 | `model_evals-main/README.md`、`model_eval_data/*.md` | 7 |
| 客户端确认与业务入库 | 解释 AI 为什么不能直接写健康记录 | candidate、confirm、backend validation | 前端确认处理、后端记录接口、Feature Matrix | 8 |

## 3. 必备知识检查清单

- [ ] 能用 90 秒画出“Expo 客户端 → BFF → AI 编排 → memory-service → 模型”的调用链。
- [ ] 能解释公开协议仓与内部 AI 合同仓为什么分开。
- [ ] 能解释 SSE 中 `delta`、撤回、候选记录和 `done` 的权威关系。
- [ ] 能说明 `conversationId + idempotencyKey` 如何避免重复生成和重复提交。
- [ ] 能解释为什么安全结果缺失时要“零知识”，而不是继续检索。
- [ ] 能说明 `approved_rag`、场景白名单、目标 `doc_key` 和 `topK` 的职责。
- [ ] 能解释 OCR 双栏页面为何要比较 PSM，以及怎样避免跨页切断句子。
- [ ] 能说清内容、页码、主题、生命周期阶段、审核状态等元数据的用途。
- [ ] 能区分“知识库里有数据”“可召回”“可注入”“回答正确”四层质量。
- [ ] 能解释向量召回、BM25、Rerank 的互补关系与阈值风险。
- [ ] 能解释 800ms 远程检索超时、连续 5 次失败熔断 30 秒的取舍。
- [ ] 能区分空召回、鉴权失败、传输失败，说明为什么不能都吞成空数组。
- [ ] 能解释风险评测中的 Precision、Recall、F1、路由准确率和 veto。
- [ ] 能准确说明 280 题指标是 `AnalyzeTurn` 整体链路指标，不是 QLoRA 单模型指标。
- [ ] 能说明 P4A 均值、EQ-Chat、EQ-Need、时延 P95 各回答什么问题。
- [ ] 能指出“均值高但长尾差”“judge 偏差”“样本泄漏”三类评测风险。
- [ ] 能说明当前代码默认生成模型与历史报告模型版本不一致，避免混用结论。
- [ ] 能解释“AI 给候选、用户确认、后端校验、正式入库”的产品安全闭环。

## 4. 推荐阅读路径

| 主题 | 通用技术点 | 相对路径 | 时间 | 读完要回答的问题 |
| --- | --- | --- | --- | --- |
| 产品范围 | AI 产品边界、生命周期、医学风险 | `PRD/AI +陪伴 App 改版 母婴版本V2.0 PRD.md` | 45 分钟 | 为什么 AI 不能静默写健康数据？ |
| 系统地图 | 多仓架构、职责分层 | `repository/P01-backendServer-main/docs/iknowu/IKNOWU_PROJECT_GUIDE.md` | 60 分钟 | 每个仓的唯一职责是什么？ |
| 功能现状 | 已实现/预研/未实现 | `repository/P01-backendServer-main/docs/iknowu/IKNOWU_FEATURE_MATRIX.md` | 20 分钟 | 哪些能力不能在面试里说成已上线？ |
| 前端入口 | 状态、发送、确认卡 | `repository/P01-frontendExpo-main/src/features/companion/screens/PetLauncherScreen.tsx` | 60 分钟 | 用户消息和候选确认如何进入系统？ |
| BFF 长连接 | WebSocket、鉴权、消息桥接 | `repository/P01-backendServer-main/src/ws/companion-chat-websocket.ts` | 60 分钟 | BFF 做了什么，为什么不让前端直连 AI？ |
| 流式入口 | SSE、幂等、提交 | `repository/ai-server-main/src/ai-orchestrator/chat/chat.controller.ts` | 45 分钟 | `done` 为什么必须等提交胜出后再发？ |
| 编排主链路 | 分阶段 pipeline、降级 | `repository/ai-server-main/src/ai-orchestrator/chat/chat.service.ts` | 90 分钟 | 一轮聊天经过哪些阶段，哪里可以提前返回？ |
| 安全分析 | 超时预算、AbortController、权威安全结果 | `repository/ai-server-main/src/ai-orchestrator/analysis/turn-analysis.service.ts` | 45 分钟 | 安全服务超时后系统如何处理？ |
| 知识路由 | 安全优先、场景路由、门控、遥测 | `repository/ai-server-main/src/ai-orchestrator/chat/knowledge-routing-phase.ts` | 60 分钟 | 哪些条件同时满足才会检索和注入？ |
| 远程检索 | 超时、熔断、鉴权、半开 | `repository/ai-server-main/src/ai-orchestrator/knowledge/remote-knowledge.client.ts` | 45 分钟 | 为什么错误必须分型？ |
| 记忆服务 | pgvector、数据权利、删除语义 | `repository/memory-service-main/README.md` | 45 分钟 | 长期记忆与知识库有什么不同？ |
| 个人语料工作 | OCR、清洗、切块、元数据 | `repository/memory-service-codex-knowledge-corpus-1861-test/chenxi_collect_data/cleaned/清洗说明.md` | 90 分钟 | 你个人做了什么，如何验证质量？ |
| 清洗脚本 | 数据工程可复现性 | 同目录下 `clean_pregnancy_all.py`、`clean_usda.py`、`clean_books.py` 等 | 120 分钟 | 每类数据的输入、规则、输出是什么？ |
| 入库运维 | dry-run、review、enabled、vector | `repository/memory-service-codex-knowledge-corpus-1861-test/docs/RUNBOOK.md` | 30 分钟 | 706 与 543 分别代表什么？ |
| 评测框架 | P0-P7、模型/样本/规则可追溯 | `repository/model_evals-main/README.md` | 45 分钟 | 一次模型选型至少要经过哪些门？ |
| 安全评测 | 分层样本、混淆矩阵、归因 | `model_eval_data/risk-judge-280-report-technical.md` | 45 分钟 | 为什么不能把链路指标归给分类器？ |
| 知识评测 | 同题盲评、对照组、加权得分 | `model_eval_data/母婴知识测评报告(通用agent及对应裸模).md` | 45 分钟 | +10.73 分能证明什么、不能证明什么？ |
| 情商评测 | 平均分、长尾、需求维度 | `model_eval_data/ai-server-10.8-EQ-Chat结果报告.md`、`ai-server-EQ-Need对比报告-20261024.md` | 60 分钟 | 高均值下最明显的失败模式是什么？ |
| 性能评测 | TTFT、总时延、成功率、路由分布 | `model_eval_data/总链路时延报告-技术版-20260904.md` | 30 分钟 | 2.44 秒是什么口径？ |

> 若某文件或原理看不懂，请继续追问 AI；本技能负责给学习路径与题目，不提供逐行讲解。

## 5. 技术定位

这是一个面向经期、备孕、孕期用户的 **AI-native 母婴陪伴与健康记录系统**。它不是单一聊天机器人，而是把对话、结构化记录、提醒、知识检索、长期记忆和安全评测连接起来的多服务系统。技术上最适合定位为：

> **有强安全边界的 LLM 应用工程项目：以流式 Agent 编排为主链，以母婴 RAG 与记忆服务为上下文，以候选确认机制连接业务写入，并用质量、安全和性能评测驱动迭代。**

项目中最有区分度的不是“调用了大模型”，而是三层约束：

1. 医学事实由结构化记录、确定性规则和审核知识库提供，模型主要负责表达与追问。
2. 模型只能产生记录/提醒候选，用户确认和后端校验后才进入正式业务数据。
3. 安全分析先于知识注入；检索失败可降级，不能阻塞聊天，也不能伪装成“知识库为空”。

## 6. 核心原理

### 6.1 问题：流式对话如何既快又不重复提交？

机制：BFF 将用户消息转交 AI 服务，AI 服务以 SSE 逐步发出元信息、情绪、文本增量、候选记录和结束事件；入口使用 `conversationId` 与 `idempotencyKey` 认领请求，只有提交胜出的实例才能发出权威 `done`。
项目落点：`PetLauncherScreen.tsx` → `companion-chat-websocket.ts` → `chat.controller.ts` → `chat.service.ts`。

### 6.2 问题：医疗场景如何防止错误知识放大风险？

机制：轮次先经过安全权威判断，再决定知识策略；只有策略为 `approved_rag`、RAG 开关开启且安全结果可用时才检索。危机轮提前返回固定安全模板，安全结果缺失时保持零知识。
项目落点：`turn-analysis.service.ts`、`knowledge-routing-phase.ts`、风险路由评测。

### 6.3 问题：知识库如何从扫描材料变成可追溯上下文？

机制：对扫描书籍做 300DPI 灰度化、Otsu 二值化和中文 OCR，通过页面质量与双栏版式选择参数；再按句子跨页切块，补充书名、页码范围、主题、生命周期阶段、审核状态和来源键。结构化数据则做字段标准化、去噪和来源追踪。
项目落点：`chenxi_collect_data/cleaned/清洗说明.md` 与 `clean_*.py`。

### 6.4 问题：远程知识服务失败时如何保护首 token 延迟？

机制：远程检索单次预算 800ms，连续 5 次失败后熔断 30 秒，再半开试探；鉴权失败、传输失败和空召回分别记录。调用失败时跳过知识注入，主对话继续。
项目落点：`remote-knowledge.client.ts`、知识路由遥测。

### 6.5 问题：如何证明模型“更好”而不是只看几个案例？

机制：按能力拆分评测集与 rubric，使用同题对照、盲评 judge、严重错误 veto、混淆矩阵、长尾样本和版本回归；质量、安全、知识正确性和性能分别统计。
项目落点：`model_evals-main` 与 `model_eval_data` 中 P4A、EQ、风险、知识和时延报告。

### 6.6 问题：AI 如何连接真实业务又不越权？

机制：AI 输出结构化候选，前端展示确认卡，用户确认后由后端做 schema 与业务规则校验，才写入记录/提醒；撤销、修改和删除遵循用户控制权。
项目落点：PRD 的“对话即操作”、前端确认处理、后端记录接口、共享协议与内部合同。

## 7. 关键设计决策

| 决策 | 备选 | 取舍 | 主要风险 | 验证方式 |
| --- | --- | --- | --- | --- |
| AI 只给候选，不直接写业务库 | 模型直接调用写接口 | 多一次确认换取可控、可撤销和审计 | 用户觉得步骤多 | 任务完成率、误写率、撤销率、确认时长 |
| 安全判断先于 RAG | 先检索再统一审核 | 避免危机/高风险轮注入不合适知识 | 安全服务超时导致知识缺失 | 安全超时率、零知识降级率、风险漏检率 |
| 场景白名单 + `doc_key` 分库 | 全库统一检索 | 降低跨域噪声和错误引用 | 场景识别错误导致漏召回 | 分场景 Recall@K、空召回率、错库率 |
| 远程知识服务快速失败 | 无超时等待远端 | 保护首 token 与聊天可用性 | 降级后回答缺乏事实支撑 | TTFT P95、检索超时率、降级回答质量 |
| 契约仓分离 | 各仓复制类型 | 保持前后端、AI 内部协议单一事实源 | 版本升级协调成本 | 契约测试、兼容性测试、消费者构建 |
| 多维评测而非单一均值 | 只看平均分 | 能暴露安全 veto 与长尾失败 | judge 偏差、指标过多 | 人审抽检、judge 校准、分桶回归 |
| 记忆与知识检索下沉独立服务 | 全留在 AI 单体 | 清晰数据权利、复用与独立扩缩容 | 网络与鉴权复杂度上升 | 服务 SLO、删除一致性、熔断恢复测试 |

## 8. 量化与验证

| 维度 | 当前证据 | 正确解读 | 边界/待测 |
| --- | --- | --- | --- |
| 语料清洗 | 1,940 条孕期结构数据；395 种 USDA 食物，363 条有效描述；19 条食安结论 | 证明数据治理和来源追踪能力 | 仍需医学专家最终审核与线上命中率 |
| 扫描书籍 | 2,238 页 OCR，形成 2,716 个切块；元数据泄漏扫描为 0 | 证明大规模 OCR、切块和质量检查流程 | 切块长度统计存在不同阶段快照，应以最终入库清单为准 |
| 知识库存量 | 706 条记录，其中 543 条 enabled/reviewed/vector/reviewer 非空 | 证明入库状态可审计 | 706 不等于全部可用；543 才是当时可检索口径 |
| 母婴知识评测 | 200 同题盲评：Agent 97.838/100、通过率 95.5%；裸模 87.108/100、91.0% | 在该题集与 judge 口径下，知识链路提升 10.73 分与 4.5 个百分点 | 不能外推所有母婴问题；仍有 2 个严重行动错误和 2 个安全 veto |
| 安全链路 | 280 题：heavy F1 95.37%，P3 路由 96.43% | 这是外部 `AnalyzeTurn` 整体链路指标 | 不能归因给 QLoRA 分类器；报告还指出结果投影与并发瓶颈 |
| 对话质量 | P4A 3.538/4，4 分占 80.94%；EQ-Chat 87.90；EQ-Need 80.79 | 多维质量总体较好 | EQ-Need 的 N4 仅 64.85；开场同质化和长尾仍需优化 |
| 链路性能 | 200/200 成功，TTFT P95 2.436s，总时延 P95 4.411s | 历史链路在该压测口径下可用 | 报告使用历史 qwen3.6-flash；当前默认模型已变更，需重新压测 |
| 当前模型 | 项目说明记录当前默认为 qwen3.7-plus | 学习源码时应以当前配置为准 | 历史报告不可直接代表当前线上性能，待同口径复测 |

## 9. 七天学习安排

1. **第 1 天：产品与架构。** 读 PRD、Project Guide、Feature Matrix，手绘十仓边界图。
2. **第 2 天：主链路。** 沿发送消息函数追到 SSE `done`，写一份 300 字调用链复述。
3. **第 3 天：安全与 RAG。** 读安全分析、知识路由和远程客户端，画出正常、危机、超时三条分支。
4. **第 4 天：语料工程。** 复盘个人清洗脚本，任选一本书说明 OCR、切块、元数据与质检。
5. **第 5 天：评测。** 手算一次 F1 和分差，解释每份报告能证明什么、不能证明什么。
6. **第 6 天：业务闭环。** 追确认卡到正式记录写入，说明为何不能让 LLM 直接入库。
7. **第 7 天：模拟面试。** 按《面经-母婴AI陪伴.md》逐题口播，并补齐 Claim 证据账本中的待确认项。

## 10. 自测题

1. 如果安全服务 1.5 秒未返回，知识检索应该怎样处理，为什么？
2. 如果 memory-service 返回 401，为什么不能当作“0 个知识命中”？
3. 800ms 超时与 2.436s TTFT P95 之间是什么关系，能否直接相加？
4. 为什么 706 条记录不能写成“706 条已审核可用知识”？
5. 为什么 280 题 heavy F1 不能写成“QLoRA 模型 F1”？
6. 若 Agent 比裸模高 10.73 分，如何排除 judge 偏差和样本泄漏？
7. 为什么聊天记忆与医学结构化记录必须分开？
8. `done` 在流式协议中承担什么权威语义？
9. 如果用户说“我今天吃了牛肉面”，系统为什么不自动记饮食？
10. 你个人完成的语料工作与团队完成的完整 RAG 上线之间，责任边界是什么？
