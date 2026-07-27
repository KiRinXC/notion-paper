---
name: notion-paper
description: "在用户的 Notion 阅读年鉴中协作管理论文精读笔记。用于执行 paper-start、paper-init、abstract-run、introduction-run、background-run、overview-run、implementation-run、evaluation-run、discussion-run、related-work-run、technology-run、section-run 或 section-commit，回答正文中的即时疑问、补充明确要求的公式、创建明确要求的知识子页、生成章节润色稿并安全提交用户确认后的稿件。"
---

# Notion Paper

## 定位

把用户视为论文的主要读者和笔记作者。负责核对 PDF、解释即时疑问、润色用户笔记、补公式、维护 Notion 结构；不要主动代写尚未阅读的整篇精读笔记。

默认一篇论文对应一个 Codex 任务。只在任务上下文中维护当前论文和当前章节，不创建持久状态文件。

## 必备能力

- 使用 Notion 连接器读取和更新阅读年鉴。
- 使用可用的 PDF 阅读能力读取用户提供的论文 PDF。
- 必要时搜索外部权威资料；在对话中给出来源，不把冗余来源链接写入 Notion。
- 在当前任务第一次写入 Notion 内容前，读取 `notion://docs/enhanced-markdown-spec`。
- 如果 PDF 或 Notion 连接不可用，停止依赖它的命令并说明缺失项。

## 命令路由

识别以下命令词。它们是 Skill 的自然语言命令，不是 shell 命令。
同名 Plugin 斜杠命令只是这些路由的快捷入口；收到斜杠命令时，执行完全相同的契约。

### `paper-start <PDF>`

1. 阅读 PDF 首页和足以确认正式标题、作者、年份、出版信息与论文结构的内容。
2. 在阅读年鉴中按正式完整标题精确查重。
3. 若条目存在，读取论文首页与“论文精读”页一次，建立章节标题到页面 ID 的映射。
4. 若条目不存在，只保留论文上下文并提示可执行 `paper-init`；不要自动创建条目。
5. 记录任务内状态：
   - `paper_title`
   - `pdf_reference`
   - `paper_page_id`
   - `reading_root_page_id`
   - `section_map`
   - `active_section_title`
   - `active_section_page_id`
6. 不改变数据库状态。

不要用 `In Progress` 状态猜当前论文，不要搜索整个工作区来推断当前章节。

### `paper-init`

要求已经执行 `paper-start` 且当前 PDF 可读。完整执行 [references/paper-init.md](references/paper-init.md)。初始化成功后刷新论文页面、精读目录和 `section_map`。

### 章节运行命令

支持：

- `abstract-run`
- `introduction-run`
- `background-run`
- `overview-run`
- `implementation-run`
- `evaluation-run`
- `discussion-run`
- `related-work-run`
- `technology-run <页面标题>`
- `section-run <页面标题>`

对固定命令使用 `section_map` 中的规范标题或明显同义标题。对动态技术页使用用户提供的标题。若无法唯一匹配，停止并列出少量候选；不要执行全局模糊搜索。

每次章节运行：

1. 从 `section_map` 解析目标页面 ID，并立即设置为当前章节。
2. 读取目标 Notion 页面全文。
3. 阅读 PDF 对应章节及解决问题所需的相邻内容。
4. 按 [references/section-models.md](references/section-models.md) 分类页面中的用户笔记、即时疑问、公式要求、显式子页要求和保留问题。
5. 在对话中回答保留问题区以外的即时疑问。
6. 判断答案是否需要转化为正文：
   - 对理解论文必要的确定事实，删除问答形式后融入润色稿。
   - 仅用于消除临时困惑的答案，只在对话中回答，并从润色稿删除问题。
   - 尚不确定或依赖额外假设的内容，不写成确定结论。
7. 在对话报告中逐项说明上述选择。
8. 对“需要补公式”等明确要求，从 PDF 找到对应公式，补充公式、变量含义、计算目的和输出含义；不要补推导。无法唯一确认时只在对话中说明，不猜测写入。
9. 只有用户明确要求“单独开子页面”等操作时，才按 [references/notion-editing.md](references/notion-editing.md) 创建正式知识子页。认为适合拆页但用户未要求时，只在对话中建议。
10. 润色整个章节，包括最后的保留问题；保留用户判断、怀疑与证据边界。
11. 保留原始笔记，在页面底部创建或更新唯一的 `# 润色稿（待确认）`。不要叠加多份草稿。
12. 合理移动或复用原页面的图片、表格、公式、链接和 page mention，使其出现在润色稿的适当位置。
13. 第一次运行任意章节命令时，将论文状态从 `Unread` 更新为 `In Progress`；之后不重复改变状态。
14. 重新读取目标页面，验证原始笔记、唯一草稿、子页与媒体均存在。
15. 给出简短运行报告。

### `section-commit`

只提交当前任务中的 `active_section_page_id`。完整执行 [references/notion-editing.md](references/notion-editing.md) 的提交契约。

如果当前章节状态缺失，或无法找到恰好一份 `润色稿（待确认）`，停止；不要搜索其他页面或猜测目标。

`section-commit` 不改变论文数据库状态。不要实现或调用 `paper-complete`；用户通过自己的流程与自动化完成论文。

## 核心不变量

- 不回答“后文需要验证的问题”“疑问”“实验问题”“未解决的问题”“待验证问题”等保留问题区中的问题。
- 不因润色而删除保留问题，只压缩措辞、合并重复项和整理编号。
- 不把用户的“我的理解”“启发”“我认为”改写成论文事实。
- 区分论文声称、实验事实、代码或 Artifact 事实、外部资料与自己的判断。
- 不主动补写用户尚未记录的整章内容；可指出缺口，但不能以完整代写替代用户阅读。
- 不把外部资料链接或参考文献列表自动写入 Notion。
- 不主动创建知识子页。
- 不在初始化时自动重命名 `Technology 1–3`。
- 不覆盖原始笔记；只有显式 `section-commit` 授权删除当前章节的原始笔记和草稿包装。
- 即使提交，也不得删除当前章节的子页面、图片、附件或仍需保留的 page mention。
- 不设置 `Completed` 或 Completion Date。

## 运行报告

章节运行完成后报告：

- 回答了哪些即时疑问。
- 哪些答案融入正文，哪些只在对话中回答。
- 补充了哪些公式。
- 创建了哪些子页及其位置。
- 保留了多少章末问题。
- 修正了哪些与 PDF 不一致的事实。
- 润色稿是否已写入并验证。

提交完成后只报告提交的章节、保留的子页/媒体以及验证结果。

## 按需读取 References

- 执行 `paper-init` 前读取 [references/paper-init.md](references/paper-init.md)。
- 执行任意章节运行前读取 [references/section-models.md](references/section-models.md)。
- 写入草稿、创建子页或提交前读取 [references/notion-editing.md](references/notion-editing.md)。
- 需要判断写作风格或边界时读取 [references/examples.md](references/examples.md)。
