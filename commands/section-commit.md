# /section-commit

调用插件内置的 `notion-paper` Skill，并将本次调用严格路由为 `section-commit`。

只提交当前任务缓存的 `active_section_page_id`。若当前章节状态缺失，或页面中不存在恰好一份 `润色稿（待确认）`，停止并说明原因；不要搜索其他页面或猜测目标。
