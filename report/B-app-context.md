# 附录 B 上下文结构（Context Structure）

> 原文：<https://eit-hai.github.io/thea/app-context.html> ｜ 中文编译导读，精确表述以原文为准
> 上一章：[附录 A 工具注册表](A-app-tools.md) ｜ 下一章：[附录 C 演示执行日志](C-app-log.md)

---

§3.2 介绍了三种上下文生命周期。Listing 11 展开表 1 中定义的各上下文项。System Prompt 给出要点（中文意译），其余条目只展示结构、不含任务相关的具体值。

## 常驻（Resident）——跨轮次保持稳定

### System Prompt（要点意译）

- 你是 Thea，一个通过调用工具完成物理任务的具身智能体。
- 除非任务已完成，每轮只选择一个工具调用。
- 每次工具返回后，根据最新证据更新计划。
- 用场景图简报获取物体 ref 和粗略的全局状态。
- 用最新的观测判断当前局部的可见性、对位和净空。
- 最新观测视为当前物理证据；更早的工具结果视为历史执行证据。
- 遵循工具描述中的前置条件、参数语义、失败模式和恢复提示。
- 不得编造物体标识、测量值、动作结果或用户意图；只有在指令和当前证据确实不足时才询问用户。
- 等当前工具执行完毕，再选择下一个工具。

### Memory

```text
{Preferences: [entry, …], Conventions: [entry, …],
 General Lessons: [entry, …]}
```

### Embodiment Profile

```text
Operational Envelope:     {Base Footprint; Base Mobility; Reachable Workspace}
Perception Configuration: {Sensor Modalities; Model-Visible Views}
Base-Relative Positions:  {Camera Positions; Initial Gripper Positions}
```

### Tool Definitions

```text
[ {name, description [+ experience summary],
   inputSchema: {type, properties, [required]}}, … ]
```

## 刷新（Refreshed）——每次决策前替换

### Scene Graph Brief

```text
Graph metadata: freshness=…; provenance=…; coordinate_frame=…; updated_at=…
Robot: pose=(…); holding=[obj, …]
Objects: - obj; position=(…); confidence=…; freshness=…;
           container_state=…; contents=[obj, …]
```

### Observations

```text
Base clearance: forward=… m, backward=… m, left=… m, right=… m
Visual evidence: view, … [随后是图像块]
```

## 累积（Accumulated）——随普通轮次增长

### Instructions

```text
[ {content: instruction}, … ]
```

### Task Notes

```text
Task summary: Goal=instruction; Current phase=phase
Timeline: task_start; tool: outcome [reason]
```

### Model Responses

```text
[ {content,
   tool_calls[]: {id, type: function, function: {name, arguments}}
  }, … ]
```

### Tool Results

```text
[ {tool_call_id,
   content: {success: true, 工具定义的 value 字段} 或 {success: false, reason}
  }, … ]
```

> 压缩（compaction）可能替换较早的累积前缀；常驻和刷新上下文保持不变。

**Listing 11.** 一次决策的上下文结构——即表 1 中三种生命周期在一次模型调用中的拼接。
