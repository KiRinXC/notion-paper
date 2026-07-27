# /implementation-run

调用插件内置的 `notion-paper` Skill，并将本次调用严格路由为 `implementation-run`。

沿用当前任务内已绑定的论文和章节映射。若 `paper-start` 上下文缺失，停止并提示先运行 `/paper-start`；不要全局搜索或猜测当前论文。
