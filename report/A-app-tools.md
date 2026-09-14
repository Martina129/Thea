# 附录 A 工具注册表（Tool Registry）

> 原文：<https://eit-hai.github.io/thea/app-tools.html> ｜ 中文编译导读，精确表述以原文为准
> 上一章：[7 讨论](07-discussion.md) ｜ 下一章：[附录 B 上下文结构](B-app-context.md)

---

表 4 列出 Astribot S1 上的工具注册表。条目遵循 §3.3 的协议：物理能力使用部署后端，轻量的上下文与交互工具在进程内运行。斜杠分隔的列表表示可选取值；引号中的字符串是自由格式描述；`desk_24` 这样的 ref 指场景图中的实体。

**表 4.** Astribot S1 上部署的工具，按功能分组。`evaluate_run` 与它们一同注册，但由 harness 通过后置钩子调用，从不由模型选择（§4.2）。

| 类别 | 工具 | 描述 | 后端 | 参数 | 取值 |
|---|---|---|---|---|---|
| **导航** | `navigate_to` | 接近某物体或地点 | SysNav[28] | `target` | 如 “drink area” / `desk_24` |
| | `move_base` | 在安全范围内平移或旋转 | Astribot S1 SDK | `direction` | forward / backward / left / right / turn_left / turn_right |
| | | | | `distance_m` | 0.03–0.80 m |
| | | | | `angle_deg` | 3–30° |
| **操作** | `pick_up` | 抓取已定位的物体 | CaP-X / ACT / π0.5[32,39,10] | `obj` | 如 “orange juice” / `bottle_2` |
| | `place` | 把手中物体放到桌上或盒中 | CaP-X / ACT / π0.5 | `target` | 如 “table” / `table_2` |
| | `open_drawer` | 打开抽屉 | CaP-X / ACT / π0.5 | `container_ref` | 如 `cabinet_87` |
| | | | | `drawer_level` | higher / lower |
| | `close_drawer` | 关闭抽屉 | CaP-X / ACT / π0.5 | `container_ref` | 如 `cabinet_87` |
| | | | | `drawer_level` | higher / lower |
| | `trash_drop` | 把手中物体丢进垃圾桶 | CaP-X / ACT / π0.5 | `trash_ref` | 如 `trash_can_54` |
| **感知** | `tilt_head` | 把头部相机俯仰到指定角度 | Astribot S1 SDK | `pitch_deg` | 0–60° |
| | `get_object_relations` | 查询物体的空间与语义关系 | Scene Graph | `obj` | 如 `cup_2` |
| | | | | `relation`（可选） | on / inside / holding / near |
| | `get_image` | 获取物体的存储图像 | Scene Graph | `obj` | 如 `cup_2` |
| **技能** | `load_skill` | 加载某个已注册技能的指令 | Thea | `name` | 如 `tidy-workspace` |
| **用户交互** | `query_user` | 请求澄清并等待回复 | Lark（飞书） | `question` | 如 “没有纯净水，要拿哪种饮料？” |
| | | | | `candidate_refs`（可选） | 如 `bottle_3` |
| | | | | `observation_views`（可选） | 如 `torso_rgbd` |
| | `notify_user` | 发送非阻塞的更新 | Lark（飞书） | `message` | 如 “我在桌上发现一个空的橙汁瓶……” |
| | | | | `notification_type`（可选） | progress / warning / completion |

---

## 参考文献

10. Black et al. (2025), *π0.5: A Vision-Language-Action Model with Open-World Generalization*.
28. Zhu et al. (2026), *SysNav: Multi-Level Systematic Cooperation Enables Real-World, Cross-Embodiment Object Navigation*.
32. Fu et al. (2026), *CaP-X: A framework for benchmarking and improving coding agents for robot manipulation*.
39. Zhao et al. (2023), *Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware*.
