# Google Tasks API 设置完整指南

## 📋 前置准备

### 你需要的东西
- ✅ Google账号（kingofswordsdavid@gmail.com）- 已有
- ✅ Claude Code - 已有
- ✅ Node.js环境 - 需要确认

### 检查Node.js是否安装
```bash
node --version
# 应该显示 v16.x.x 或更高
# 如果没有，需要安装Node.js
```

---

## 🚀 设置步骤（预计30-60分钟）

### 步骤1：创建Google Cloud项目

#### 操作：
1. 访问 https://console.cloud.google.com
2. 登录你的Google账号（kingofswordsdavid@gmail.com）
3. 顶部点击项目选择器
4. 点击"新建项目"
5. 填写信息：
   - **项目名称**：`Google Tasks Assistant`
   - **组织**：可以留空
6. 点击"创建"
7. 等待项目创建完成（约10秒）

#### 验证：
- 顶部应该显示"Google Tasks Assistant"

---

### 步骤2：启用Google Tasks API

#### 操作：
1. 在左侧菜单，找到"API和服务" → "库"
2. 搜索框输入：`Tasks`
3. 找到"Tasks API"，点击
4. 点击"启用"按钮
5. 等待启用完成（约5秒）

#### 验证：
- 应该显示"已启用"

---

### 步骤3：配置OAuth同意屏幕

#### 操作：
1. 左侧菜单选择"API和服务" → "OAuth同意屏幕"
2. 选择用户类型：**外部**（External）
3. 点击"创建"
4. 填写第1步（OAuth同意屏幕）：
   ```
   应用名称 (必填)：Claude Code Tasks
   用户支持电子邮件 (必填)：kingofswordsdavid@gmail.com
   应用首页 (留空)：https://github.com/kingofswordsdavid/google-tasks-skill
   应用隐私政策 URL (留空)：https://github.com/kingofswordsdavid/google-tasks-skill
   应用服务条款 URL (留空)：[留空]
   已获授权的网域 (留空)：[留空]
   开发者联系信息 (必填)：kingofswordsdavid@gmail.com
   ```
5. 点击"保存并继续"
6. 第2步（范围）：点击"添加或删除范围"
7. 搜索框输入：`tasks`
8. 勾选：`.../auth/tasks` (查看、编辑和管理您的任务)
9. 点击"更新"
10. 点击"保存并继续"
11. 第3步（测试用户）：点击"添加用户"
    - 电子邮件：kingofswordsdavid@gmail.com
    - 点击"添加"
12. 点击"保存并继续"

#### 验证：
- OAuth同意屏幕配置完成

---

### 步骤4：创建OAuth客户端ID

#### 操作：
1. 左侧菜单选择"凭据"
2. 顶部点击"+ 创建凭据" → "OAuth客户端ID"
3. 填写信息：
   ```
   应用类型：桌面应用
   名称：Claude Code Tasks
   ```
4. 点击"创建"
5. **重要**：会弹出一个对话框，包含：
   ```
   客户端 ID: xxxx.apps.googleusercontent.com
   客户端密钥: GOCSPX-xxxx
   ```
6. **复制并保存这两个值**（建议保存到文本文件）
7. 点击"确定"

#### 验证：
- 凭据列表中应该看到"Claude Code Tasks"

---

### 步骤5：下载客户端密钥文件

#### 操作：
1. 在凭据列表中，找到"Claude Code Tasks"
2. 点击右侧的"下载图标"（下载JSON）
3. 文件会自动下载，重命名为：`credentials.json`
4. **安全提示**：不要把这个文件分享给任何人！

---

### 步骤6：安装Node.js依赖

#### 操作：
打开命令行，执行：

```bash
# 创建工作目录
mkdir -p ~/.claude/scripts/google-tasks
cd ~/.claude/scripts/google-tasks

# 初始化npm项目
npm init -y

# 安装依赖
npm install googleapis

# 复制credentials.json到这个目录
# 把下载的credentials.json文件移动到这个目录
```

---

## 📝 需要我帮你做什么？

请完成上面步骤1-5后，告诉我"已完成前面5步"，然后我会：

1. 帮你编写认证脚本
2. 获取授权token
3. 编写Tasks API封装
4. 集成到Google Tasks Skill

---

## ❓ 遇到问题？

如果任何步骤不清楚，告诉我：
- "第X步不懂，详细说说"
- "卡在了XXX"
- "遇到了错误：[错误信息]"

我会帮你解决！

---

**准备好了吗？开始第一步吧！** 🚀
