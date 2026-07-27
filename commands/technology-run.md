# /technology-run

调用插件内置的 `notion-paper` Skill，并将本次调用严格路由为 `technology-run <页面标题>`。

## 参数

- `page_title`：当前论文“论文精读”目录中的技术页面标题（必需）。

若标题缺失或无法在当前任务的 `section_map` 中唯一匹配，停止并给出少量候选；不要执行全局模糊搜索。
