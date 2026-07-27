# /paper-init

调用插件内置的 `notion-paper` Skill，并将本次调用严格路由为 `paper-init`。

要求当前任务已经成功执行 `paper-start` 且 PDF 可读。若上下文缺失，停止并提示先运行 `/paper-start`；不要从工作区状态猜测论文。
