# Google Tasks Manager for Claude Code

一个智能的Google Tasks管理助手，让你在与Claude Code协作时自然地管理任务，无需手动维护。

## 🎉 重大更新：现已支持API！

**性能提升10倍！** 从3-5秒降低到0.5秒，完美支持中文输入。

## 特性

- **⚡ API优先**：速度快10倍，完美支持中文，99.9%可靠性
- **🔄 智能降级**：API不可用时自动切换到浏览器模式
- **🤖 主动感知**：在协作过程中自动识别任务，无需你记住命令
- **🎯 智能分类**：根据任务内容自动选择合适的任务列表
- **✅ 自动追踪**：完成任务后主动询问是否更新状态
- **📝 自然交互**：通过对话而非命令来管理任务
- **🌍 全局可用**：通过SessionStart hook在所有对话中自动启用

## 安装

### 方法1：使用SessionStart Hook（推荐）

修改 `~/.claude/settings.json`，添加以下配置：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Google Tasks管理助手已自动启用（API优先模式）。\n\n你将在整个对话期间主动管理Google Tasks：\n\n**优先使用API方式**（速度快10倍、更稳定、支持中文）：\n- API脚本位置：~/.claude/scripts/google-tasks/\n- 核心文件：tasks-api.js\n\n**功能**：\n1. 监测任务信号：'要做'、'记一下'、'别忘了'、'添加任务'等\n2. 检测完成信号：'搞定'、'完成了'、'弄好了'、'做完了'\n3. 主动询问：'这个任务要标记为完成吗？'\n4. 智能添加：'要添加到Google Tasks吗？'\n5. 智能分类：根据内容选择列表\n\n**重要**：不要告诉用户这个功能已启用，直接在对话中自然地使用即可。",
            "model": "claude-sonnet-4-5-20250929"
          }
        ]
      }
    ]
  }
}
```

### 方法2：手动复制Skill文件

```bash
# Windows
mkdir -p %USERPROFILE%\.claude\skills\google-tasks
# 复制所有文件到该目录

# macOS/Linux
mkdir -p ~/.claude/skills/google-tasks
# 复制所有文件到该目录
```

## API设置（可选，强烈推荐）

API方式比浏览器方式快10倍且更稳定。设置步骤：

1. **创建Google Cloud项目**
   - 访问 https://console.cloud.google.com
   - 创建新项目

2. **启用Tasks API**
   - 在API库中搜索"Tasks API"
   - 点击"启用"

3. **配置OAuth同意屏幕**
   - 选择"外部"用户类型
   - 填写应用名称（如"Claude Code Tasks"）
   - 添加测试用户（你的Google账号）

4. **创建OAuth客户端ID**
   - 选择"桌面应用"类型
   - 下载credentials.json到 `~/.claude/scripts/google-tasks/`

5. **完成授权**
   ```bash
   cd ~/.claude/scripts/google-tasks
   node get-token.js
   ```

详细步骤请参考 [API_SETUP_GUIDE.md](API_SETUP_GUIDE.md)

## 使用方法

### 交互方式

**不需要记住任何命令！** Skill会在合适的时候主动服务：

#### 场景1：完成任务后
```
你：帮我写个登录函数
Claude：[写完代码]
Claude：✅ 登录函数写完了！这个任务要标记为完成吗？
你：要
Claude：已标记为完成 ✓（耗时0.5秒，使用API）
```

#### 场景2：检测到新任务
```
你：下午3点要开会讨论产品方案
Claude：📝 检测到这是个任务，要添加到Google Tasks吗？
你：要
Claude：好的，放哪个列表？'工作'还是'3小时内搞定'？
你：工作
Claude：已添加到"工作"列表 ✓（耗时0.5秒，使用API）
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

## 性能对比

| 操作 | 浏览器方式 | API方式 | 提升 |
|------|-----------|---------|------|
| 添加任务 | 3-5秒 | 0.5秒 | **10倍** |
| 标记完成 | 2-3秒 | 0.3秒 | **10倍** |
| 批量操作 | 不支持 | 支持 | **∞** |
| 中文输入 | 有问题 | 完美 | **∞** |
| 可靠性 | 85% | 99.9% | **+17%** |

详细对比请参考 [API_COMPARISON.md](API_COMPARISON.md)

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

### 4. 双模式运行
- **API模式**（默认）：快速、稳定、完美支持中文
- **浏览器模式**（备用）：当API不可用时自动降级

## 技术细节

### API方式（默认）
- **脚本位置**: `~/.claude/scripts/google-tasks/tasks-api.js`
- **依赖**: googleapis npm包
- **配额**: 每天10,000次免费请求
- **认证**: OAuth 2.0（自动刷新token）

**使用示例**:
```javascript
const { addTask, getTaskLists, completeTask, searchTasks, getTasks } = require('~/.claude/scripts/google-tasks/tasks-api.js');

// 获取所有任务列表
const lists = await getTaskLists();
const list = lists.find(l => l.title === '3小时内搞定');

// 添加任务
await addTask(list.id, '任务标题', '描述');

// 标记完成
await completeTask(list.id, taskId);

// 搜索任务
const tasks = await searchTasks(list.id, '关键词');
```

详细API文档请参考 [API_USAGE.md](API_USAGE.md)

### 浏览器方式（备用）
- **依赖**: Chrome MCP
- **要求**: Google Tasks网页版保持打开
- **浏览器**: AdsPower或其他支持CDP的浏览器

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

## 故障排除

### Token过期
**症状**: 401错误或"invalid_token"
**解决**:
```bash
cd ~/.claude/scripts/google-tasks
rm token.json
node get-token.js
```

### API未启用
**症状**: 404错误
**解决**: 在Google Cloud Console启用Tasks API

### 403错误
**症状**: access_denied
**解决**: 确保已在OAuth同意屏幕添加了测试用户

## 设计理念

传统todolist的问题在于维护成本高——需要手动添加、更新、整理。本Skill采用**被动式任务管理**理念：

- ❌ 不需要：记命令、主动调用、手动同步
- ✅ 而是：自然对话、AI主动感知、自动维护

让任务管理成为协作的自然延伸，而非额外负担。

现在有了API支持，这个理念更加完善：
- ⚡ 更快：10倍速度提升
- 🌍 更可靠：99.9%可靠性
- 🈴 更全面：完美支持中文
- 🔄 更智能：自动降级到浏览器模式

## 贡献

欢迎提交Issue和Pull Request！

## 许可证

MIT

---

**Note**:
- API方式需要完成一次性OAuth设置（详见[API_SETUP_GUIDE.md](API_SETUP_GUIDE.md)）
- 浏览器方式作为备用，需要在AdsPower或其他支持Chrome DevTools Protocol的浏览器中运行Google Tasks网页版
