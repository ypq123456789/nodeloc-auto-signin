# 切换 Git 远程仓库指南

## 🔄 从原仓库切换到你的 Fork 仓库

如果你的 VPS 上原来克隆的是别人的仓库，现在需要切换到你自己 fork 的仓库：`https://github.com/ypq123456789/nodeloc-auto-signin`

## 方法一：修改远程仓库地址（推荐）✨

保留本地代码和历史记录，只修改远程仓库地址：

```bash
# 1. 进入项目目录
cd /path/to/nodeloc-auto-signin

# 2. 查看当前的远程仓库地址
git remote -v

# 3. 修改远程仓库地址为你的 fork 仓库
git remote set-url origin https://github.com/ypq123456789/nodeloc-auto-signin.git

# 4. 验证是否修改成功
git remote -v

# 5. 拉取你 fork 仓库的最新代码
git pull origin main

# 6. 安装新增的依赖
pip install -r requirements.txt
```

### 预期输出示例：

执行 `git remote -v` 后应该看到：
```
origin  https://github.com/ypq123456789/nodeloc-auto-signin.git (fetch)
origin  https://github.com/ypq123456789/nodeloc-auto-signin.git (push)
```

---

## 方法二：完全重新克隆（简单直接）

如果不需要保留本地修改，直接重新克隆：

```bash
# 1. 备份环境变量配置（如果有）
cd /path/to/nodeloc-auto-signin
# 如果有配置文件，先备份
cp .env /tmp/.env.backup 2>/dev/null || true

# 2. 返回上级目录
cd ..

# 3. 删除旧目录
rm -rf nodeloc-auto-signin

# 4. 克隆你的 fork 仓库
git clone https://github.com/ypq123456789/nodeloc-auto-signin.git

# 5. 进入新目录
cd nodeloc-auto-signin

# 6. 安装依赖
pip install -r requirements.txt

# 7. 恢复配置（如果有）
cp /tmp/.env.backup .env 2>/dev/null || true
```

---

## 方法三：添加为新的远程仓库

保留原仓库作为 upstream，你的 fork 作为 origin：

```bash
# 1. 进入项目目录
cd /path/to/nodeloc-auto-signin

# 2. 查看当前远程仓库
git remote -v

# 3. 重命名当前的 origin 为 upstream
git remote rename origin upstream

# 4. 添加你的 fork 仓库为新的 origin
git remote add origin https://github.com/ypq123456789/nodeloc-auto-signin.git

# 5. 验证配置
git remote -v

# 6. 从你的 fork 仓库拉取代码
git fetch origin
git checkout main
git branch --set-upstream-to=origin/main main
git pull origin main

# 7. 安装依赖
pip install -r requirements.txt
```

这样你就有两个远程仓库：
- `origin`: 你的 fork 仓库（用于日常更新）
- `upstream`: 原始仓库（用于同步上游更新）

---

## 🤖 青龙面板特殊处理

如果你在青龙面板中使用，需要：

### 方案 A：修改订阅地址
1. 进入青龙面板 → 订阅管理
2. 找到原来的订阅，点击编辑
3. 修改仓库地址为：`https://github.com/ypq123456789/nodeloc-auto-signin.git`
4. 点击"更新"拉取最新代码

### 方案 B：删除重新添加
1. 删除原有的订阅/脚本
2. 添加新订阅，使用你的仓库地址
3. 重新配置环境变量

### 方案 C：SSH 手动修改
```bash
# 1. 进入青龙容器
docker exec -it qinglong bash

# 2. 进入脚本目录（具体路径可能不同）
cd /ql/scripts/nodeloc-auto-signin
# 或者
cd /ql/repo/nodeloc-auto-signin*

# 3. 修改远程仓库地址
git remote set-url origin https://github.com/ypq123456789/nodeloc-auto-signin.git

# 4. 拉取更新
git pull origin main

# 5. 安装依赖
pip3 install -r requirements.txt

# 6. 退出容器
exit
```

---

## 🔍 验证切换是否成功

```bash
# 1. 查看远程仓库地址
git remote -v

# 应该显示你的仓库地址：
# origin  https://github.com/ypq123456789/nodeloc-auto-signin.git (fetch)
# origin  https://github.com/ypq123456789/nodeloc-auto-signin.git (push)

# 2. 查看当前分支
git branch -vv

# 3. 查看最新提交
git log --oneline -5

# 应该能看到你的最新提交：feat: 添加 Telegram 推送功能

# 4. 检查新文件是否存在
ls -la | grep telegram_notifier.py

# 5. 检查依赖文件
cat requirements.txt | grep requests
```

---

## 📝 常见问题

### Q1: 提示 "fatal: refusing to merge unrelated histories"
```bash
git pull origin main --allow-unrelated-histories
```

### Q2: 有本地修改导致无法拉取
```bash
# 暂存本地修改
git stash

# 拉取更新
git pull origin main

# 恢复本地修改
git stash pop
```

### Q3: 直接丢弃本地所有修改
```bash
git fetch origin
git reset --hard origin/main
```

### Q4: 提示权限问题
如果需要认证，可以使用个人访问令牌（PAT）：
```bash
git remote set-url origin https://YOUR_TOKEN@github.com/ypq123456789/nodeloc-auto-signin.git
```

或者使用 SSH：
```bash
git remote set-url origin git@github.com:ypq123456789/nodeloc-auto-signin.git
```

---

## 🎯 推荐流程（最简单）

对于大多数情况，推荐使用**方法一**：

```bash
cd /path/to/nodeloc-auto-signin
git remote set-url origin https://github.com/ypq123456789/nodeloc-auto-signin.git
git pull origin main
pip install -r requirements.txt
python main.py
```

只需 4 条命令，轻松切换！✅

---

## 🔄 后续更新

切换完成后，以后在 VPS 上更新代码就很简单了：

```bash
cd /path/to/nodeloc-auto-signin
git pull origin main
pip install -r requirements.txt
```

或者使用青龙面板的"更新"按钮即可。

---

✅ 切换完成后，你就可以从自己的仓库接收更新，也可以推送自己的修改了！
