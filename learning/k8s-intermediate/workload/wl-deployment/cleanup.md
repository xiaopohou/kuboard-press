---
vssueId: 40
layout: LearningLayout
description: 本文描述了如何在 Kubernetes 中清理 Deployment 中旧的 ReplicaSet
---

# 清理策略

[返回 Deployment](./#deployment-概述)

通过 Deployment 中 `.spec.revisionHistoryLimit` 字段，可指定为该 Deployment 保留多少个旧的 ReplicaSet。超出该数字的将被在后台进行垃圾回收。该字段的默认值是 10。

::: tip
如果该字段被设为 0，Kubernetes 将清理掉该 Deployment 的所有历史版本（revision），因此，您将无法对该 Deployment 执行回滚操作 `kubectl rollout undo`。
:::

<!-- <el-tabs type="border-card">

<el-tab-pane label="使用 kubectl 指定 revisionHistroyLimit">
正在撰写中

</el-tab-pane>

<el-tab-pane label="使用 Kuboard 指定 revisionHistroyLimit">
正在撰写中

</el-tab-pane>

</el-tabs> -->

[返回 Deployment](./#deployment-概述)
