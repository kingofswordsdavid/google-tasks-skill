# Google Tasks Manager

智能管理Google Tasks，支持API（主要）和浏览器（备用）两种方式。

## 🚀 特性

- **API优先**：速度快10倍（0.5秒 vs 5秒），完美支持中文，99.9%可靠性
- **浏览器备用**：当API不可用时自动降级到Chrome MCP
- **主动检测**：自动识别任务信号，无需记忆命令
- **智能分类**：根据内容自动选择任务列表

## 命令格式

### 简洁模式
- `/gt add <任务内容> [列表名]` - 添加新任务
- `/gt done <关键词>` - 标记任务为完成
- `/gt undone <关键词>` - 标记任务为未完成
- `/gt find <关键词>` - 搜索任务
- `/gt list [列表名]` - 查看任务列表

### 自然语言模式
- `/gt 在"每日/探索"添加任务：买牛奶`
- `/gt 把"理清tasks"标记为完成`
- `/gt 搜索"波波方向"`
- `/gt 显示"工作"列表的所有任务`

## 任务列表

当前可用的任务列表：
1. 3小时内搞定（API ID: MS0wNEpXQW9DTGRDdldBYw）
2. 每日/探索/特殊任务
3. 其他
4. 工作
5. 杂项
6. 探索任务

## 技术细节

### API方式（默认）
- **脚本位置**: `~/.claude/scripts/google-tasks/tasks-api.js`
- **使用方式**:
  ```javascript
  const { addTask, getTaskLists, completeTask, searchTasks, getTasks } = require('~/.claude/scripts/google-tasks/tasks-api.js');
  const lists = await getTaskLists();
  const list = lists.find(l => l.title === '3小时内搞定');
  await addTask(list.id, '任务标题', '描述');
  await completeTask(list.id, taskId);
  const tasks = await searchTasks(list.id, '关键词');
  ```

### 浏览器方式（备用）
- **AdsPower标签页ID**: 1479421779
- **仅当API不可用时使用**

## 说明

- 列表名支持智能匹配：如"3小时"会匹配"3小时内搞定"
- 默认添加到第一个可见的任务列表
- 标记任务时会搜索所有列表，找到匹配的任务
- 支持中英文关键词搜索
- API方式自动处理token刷新
