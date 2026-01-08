# 安装指南

## 方式1：手动安装

### 1. 创建GitHub仓库

由于系统未安装GitHub CLI，请手动创建仓库：

1. 访问 https://github.com/new
2. 仓库名称：`google-tasks-skill`
3. 设置为Public
4. 不要初始化README（我们已有代码）
5. 点击"Create repository"

### 2. 推送代码

创建仓库后，执行以下命令：

```bash
cd .claude/skills/system/google-tasks
git remote add origin https://github.com/你的用户名/google-tasks-skill.git
git branch -M main
git push -u origin main
```

## 方式2：安装GitHub CLI（推荐）

### Windows

```powershell
winget install --id GitHub.cli
```

或访问：https://cli.github.com/

### 安装后自动创建

```bash
cd .claude/skills/system/google-tasks
gh repo create google-tasks-skill --public --source=. --push
```

## 安装到Claude Code

### 复制skill文件

```bash
# Windows
mkdir -p %USERPROFILE%\.claude\skills\system
xcopy /E /I .claude\skills\system\google-tasks %USERPROFILE%\.claude\skills\system\google-tasks

# macOS/Linux
mkdir -p ~/.claude/skills/system
cp -r .claude/skills/system/google-tasks ~/.claude/skills/system/
```

### 使用

在每次Claude Code对话开始时说：

```
"请启用Google Tasks管理"
```

或简短：

```
"启用tasks"
```

## 验证安装

检查文件是否正确安装：

```bash
# Windows
dir %USERPROFILE%\.claude\skills\system\google-tasks

# macOS/Linux
ls ~/.claude/skills/system/google-tasks
```

应该看到：
- README.md
- SYSTEM_SKILL.md
- skill.md
- prompt.md
- .gitignore
