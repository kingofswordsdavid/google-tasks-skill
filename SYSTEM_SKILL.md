# Google Tasks System Skill

这是一个**System Skill**（系统级技能），设计为在Claude Code会话中始终活跃运行。

## 什么是System Skill？

System Skill不同于普通skill：
- ❌ 普通skill：需要用户显式调用（如 `/gt add`）
- ✅ System skill：始终在后台监听，主动提供服务

## 使用方法

### 方式1：在每次对话开始时提醒Claude

在开始新对话时说：

```
"请启用Google Tasks管理"
```

或者简短地说：

```
"启用tasks"
```

### 方式2：使用持久化会话

Claude Code的会话有记忆功能，一旦在一个会话中启用，后续对话会自动保持。

### 方式3：修改Claude Code系统提示（高级）

如果Claude Code支持自定义系统提示，可以添加：

```text
你是一个集成了Google Tasks的助手。在每次对话中：
1. 监测任务信号："要做"、"记得"、"别忘了"
2. 检测完成信号："搞定"、"完成了"、"弄好了"
3. 完成工作后询问："这个任务要标记为完成吗？"
4. 检测到新任务时询问："要添加到Google Tasks吗？"
5. 智能选择任务列表（根据内容推断）
```

## 理想配置（未来功能）

如果Claude Code将来支持配置文件中的`autoLoadSkills`，可以添加：

```json
{
  "systemSkills": ["google-tasks"],
  "autoLoadOnStart": true
}
```

## 当前限制

由于Claude Code目前不支持自动加载skills，我们需要：
- 在每次新会话开始时提醒一次
- 或者在重要对话中主动要求启用

## 建议的工作流

1. **日常会话**：始终提醒Claude启用Google Tasks
2. **项目会话**：在项目开始时启用，贯穿整个项目
3. **快速会话**：可以选择性启用（不需要时不说）

## 反馈

如果你希望Claude Code支持真正的自动加载skills功能，可以考虑：
1. 在GitHub上提feature request
2. 在Claude Code社区讨论这个需求
3. 参考其他AI助手（如Copilot）的实现方式
