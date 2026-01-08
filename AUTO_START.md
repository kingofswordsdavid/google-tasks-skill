# Google Tasks 自动启动配置

## 工作原理

通过Claude Code的**SessionStart Hook**，在每次会话开始时自动注入Google Tasks管理提示，实现：

1. ✅ **自动启用** - 每次启动Claude时自动激活
2. ✅ **跨对话持久** - 在所有对话中都保持活跃
3. ✅ **无感知运行** - 不需要手动说"启用tasks"

## 配置位置

配置文件：`~/.claude/settings.json`

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Google Tasks管理助手已自动启用...",
            "model": "claude-sonnet-4-5-20250929"
          }
        ]
      }
    ]
  }
}
```

## 关键配置说明

### SessionStart Hook
- **触发时机**：每次新的Claude Code会话开始时
- **作用范围**：所有对话（"matcher": ".*" 匹配所有情况）
- **执行方式**：通过prompt类型，将系统提示注入到会话中

### 为什么这样设计？

1. **非侵入式**：不修改Claude Code源码
2. **可配置**：随时可以在settings.json中调整
3. **持久化**：配置保存在用户目录，不会丢失
4. **无感知**：Claude自动接收指令，用户无感知

## 验证是否生效

### 方法1：观察对话行为
在任意新对话中，当你：
- 说"下午3点要开会"
- Claude应该问："要添加到Google Tasks吗？"

### 方法2：检查配置
```bash
cat ~/.claude/settings.json | grep -A 20 "SessionStart"
```

### 方法3：查看会话开始日志
Claude Code会在每次会话开始时执行hook，可以在日志中看到。

## 自定义配置

### 修改提示词
编辑`settings.json`中的`prompt`字段，调整行为：

```json
"prompt": "你的自定义提示..."
```

### 更换模型
如果你想使用其他模型：

```json
"model": "claude-haiku-4-20250515"  // 更快
"model": "claude-opus-4-20250515"   // 更智能
```

### 添加更多hook
可以添加其他hook，比如SessionEnd：

```json
"SessionEnd": [
  {
    "matcher": ".*",
    "hooks": [
      {
        "type": "prompt",
        "prompt": "对话结束前，检查是否有任务需要更新到Google Tasks"
      }
    ]
  }
]
```

## 高级配置

### 仅在特定目录启用
使用不同的matcher：

```json
"matcher": "^C:\\\\Users\\\\spelu\\\\work.*"
```

### 条件性启用
结合多个hook实现复杂逻辑。

## 故障排除

### 配置不生效
1. 检查JSON格式是否正确
2. 确认settings.json路径
3. 重启Claude Code

### Hook执行失败
查看Claude Code日志：
```bash
tail -f ~/.claude/logs/latest.log
```

### 临时禁用
设置`disableAllHooks`：

```json
{
  "disableAllHooks": true
}
```

## 更新GitHub仓库

配置完成后，记得更新到GitHub：

```bash
cd ~/.claude/skills/system/google-tasks
git add .
git commit -m "添加自动启动配置说明"
git push
```

## 参考

- Claude Code Hooks文档：[官方文档链接]
- Google Tasks Skill仓库：https://github.com/kingofswordsdavid/google-tasks-skill

---

**最后更新**：2026-01-08
**作者**：Created with Claude Code
