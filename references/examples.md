# 行为示例

## 参考页面

- TensorShield 首页：<https://app.notion.com/p/34c8a5f3d37080dda51cf0950ae69484>
- TensorShield 精读目录：<https://app.notion.com/p/34c8a5f3d3708032965fc773d1cd9e2a>
- No Privacy Left Outside 首页：<https://app.notion.com/p/3768a5f3d37081278da3fc0b4250e388>
- No Privacy Left Outside 精读目录：<https://app.notion.com/p/3768a5f3d3708177aaf4f3c9937dfc8a>

把 TensorShield 视为协作阅读过程示范：保留即时问题、用户理解、proxy 审查和知识子页。把 No Privacy Left Outside 视为较成熟的证据整理示范：区分论文、代码、实验事实和最终评价。

## 即时问题

原笔记：

> 攻击者是否知道 hybrid model 的 Slice 位置？这里需要你解答。

处理：

- 在对话中回答。
- 若 PDF 明确攻击者知道完整架构，把“攻击者知道 Slice 的位置和结构，但不知道受保护参数”融入威胁模型正文。
- 从润色稿删除问题和操作提示。
- 报告“答案已融入攻击者能力，因为它决定安全结论边界”。

若问题只是“TEE 的英文是什么”，在对话中回答后直接从润色稿删除，不增加冗余正文。

## 保留问题

原笔记：

```markdown
### 后文需要验证的问题
1. criticality value 是否真的等价于 MS 防御收益？
2. Top-k 证明的是排序准确还是前缀集合有效？
```

处理：

- 不回答。
- 可压缩、去重和统一术语。
- 在运行报告中说明保留 2 个问题。

## 公式

原笔记：

> 这里需要补充 criticality value 的公式，并说明每一项是什么意思。

处理：

- 从 PDF 插入原公式。
- 解释 intrinsic importance、attention transition、参数数量归一化和最终输出。
- 不推导梯度。
- 删除操作提示。

## 知识子页

原笔记：

> 需要单独开一个子页面介绍权重依赖性。

处理：

- 在当前技术章节下创建 `📌 权重依赖性`。
- 直接写成正式解释页。
- 在精读目录中与该技术章节并排添加 mention。
- 主章节只保留该概念与论文公式的关系。

若用户只写“权重依赖性可能值得展开”，不要自动创建；在对话中建议。

## 润色与提交

`abstract-run` 后：

```markdown
[原始笔记，保持不变]

# 润色稿（待确认）
[完整润色稿]
```

用户修改润色稿后执行 `section-commit`：

```markdown
[用户确认后的完整正文]
```

移除原始笔记和草稿标题，保留必要的图片、表格、链接与子页面。
