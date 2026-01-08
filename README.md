# Google Tasks Manager for Claude Code

一个智能的Google Tasks管理助手，让你在与Claude Code协作时自然地管理任务，无需手动维护。

## 特性

- **🤖 主动感知**：在协作过程中自动识别任务，无需你记住命令
- **🎯 智能分类**：根据任务内容自动选择合适的任务列表
- **✅ 自动追踪**：完成任务后主动询问是否更新状态
- **📝 自然交互**：通过对话而非命令来管理任务

## 安装

将此skill复制到你的Claude skills目录：

```bash
# Windows
mkdir -p %USERPROFILE%\.claude\skills\google-tasks
# 复制所有文件到该目录

# macOS/Linux
mkdir -p ~/.claude/skills/google-tasks
# 复制所有文件到该目录
```

## 使用方法

### 前置条件

1. 在AdsPower或其他浏览器中打开Google Tasks
2. 确保Chrome MCP已配置并能访问该浏览器
3. Google Tasks标签页需要保持打开

### 交互方式

**不需要记住任何命令！** Skill会在合适的时候主动服务：

#### 场景1：完成任务后
```
你：帮我写个登录函数
Claude：[写完代码]
Claude：✅ 登录函数写完了！这个任务要标记为完成吗？
你：要
Claude：已标记为完成 ✓
```

#### 场景2：检测到新任务
```
你：下午3点要开会讨论产品方案
Claude：📝 检测到这是个任务，要添加到Google Tasks吗？
你：要
Claude：好的，放哪个列表？'工作'还是'3小时内搞定'？
你：工作
Claude：已添加到"工作"列表 ✓
```

#### 场景3：对话结束时
```
Claude：📋 回顾一下这次工作：
   - 完成了：实现登录功能
   - 新产生的任务：修复发现的问题

   要我更新Google Tasks吗？
```

### 任务列表

Skill支持以下任务列表（可自定义）：

1. **3小时内搞定** - 紧急短期任务
2. **每日/探索/特殊任务** - 日常任务和探索性工作
3. **其他** - 不便分类的任务
4. **工作** - 与工作相关的任务
5. **杂项** - 零散任务
6. **探索任务** - 探索性项目

## 工作原理

### 1. 上下文感知
- 监测对话中的任务信号："要做"、"记得"、"别忘了"
- 检测完成信号："搞定"、"完成了"、"弄好了"
- 根据工作内容推断任务类型

### 2. 智能匹配
- 根据关键词自动选择任务列表
- 例如："3小时" → "3小时内搞定"，"开会" → "工作"

### 3. 主动服务
- 完成工作后询问任务状态
- 检测到新任务时询问是否记录
- 对话结束时提供任务更新建议

## 技术细节

### 依赖
- Chrome MCP (Model Context Protocol)
- Google Tasks (网页版)
- AdsPower浏览器或其他支持CDP的浏览器

### 核心功能
- `addTask(taskContent, listName)` - 添加任务
- `markTaskDone(keyword, done)` - 标记任务完成状态
- `searchTasks(keyword)` - 搜索任务
- `showTasks(listName)` - 显示任务列表
- `findListByName(listName)` - 智能匹配任务列表

## 配置

### 自定义任务列表
编辑 `prompt.md` 中的 `taskLists` 对象：

```javascript
const taskLists = {
  "你的列表名": ["关键词1", "关键词2", "关键词3"],
  // ...
}
```

### 调整主动服务频率
在 `prompt.md` 中修改触发条件和提示语。

## 设计理念

传统todolist的问题在于维护成本高——需要手动添加、更新、整理。本Skill采用**被动式任务管理**理念：

- ❌ 不需要：记命令、主动调用、手动同步
- ✅ 而是：自然对话、AI主动感知、自动维护

让任务管理成为协作的自然延伸，而非额外负担。

## 贡献

欢迎提交Issue和Pull Request！

## 许可证

MIT

## 作者

Created with ❤️ by [Your Name]

---

**Note**: 此Skill需要在AdsPower或其他支持Chrome DevTools Protocol的浏览器中运行Google Tasks网页版。
