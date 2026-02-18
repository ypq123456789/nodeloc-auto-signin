# VPS 更新代码指南

## 📦 方法一：使用 Git Pull（推荐）

如果你的 VPS 上已经 clone 了这个仓库，直接拉取最新代码：

```bash
# 1. 进入项目目录
cd /path/to/nodeloc-auto-signin

# 2. 拉取最新代码
git pull origin main

# 3. 安装新增的依赖
pip install -r requirements.txt
```

## 🔄 方法二：重新克隆仓库

如果遇到 Git 冲突或其他问题，可以重新克隆：

```bash
# 1. 备份当前配置（如果有）
cp /path/to/nodeloc-auto-signin/.env /tmp/.env.backup  # 如果使用了 .env 文件

# 2. 删除旧目录
rm -rf /path/to/nodeloc-auto-signin

# 3. 重新克隆
git clone https://github.com/ypq123456789/nodeloc-auto-signin.git

# 4. 进入目录
cd nodeloc-auto-signin

# 5. 安装依赖
pip install -r requirements.txt

# 6. 恢复配置（如果有）
cp /tmp/.env.backup .env
```

## ⚙️ 配置 Telegram 推送

更新后，需要配置 Telegram 环境变量：

### Linux/VPS 临时配置
```bash
export TG_BOT_TOKEN="你的Bot Token"
export TG_CHAT_ID="你的Chat ID"
export NL_COOKIE="你的Cookie"

# 运行脚本
python main.py
```

### Linux/VPS 永久配置
```bash
# 编辑 ~/.bashrc 或 ~/.bash_profile
nano ~/.bashrc

# 在文件末尾添加
export TG_BOT_TOKEN="你的Bot Token"
export TG_CHAT_ID="你的Chat ID"
export NL_COOKIE="你的Cookie"

# 保存后重新加载
source ~/.bashrc
```

### 使用 .env 文件（可选）
创建 `.env` 文件：
```bash
nano .env
```

添加内容：
```
TG_BOT_TOKEN=你的Bot Token
TG_CHAT_ID=你的Chat ID
NL_COOKIE=你的Cookie
```

修改 `main.py`，在开头添加：
```python
from dotenv import load_dotenv
load_dotenv()
```

安装 python-dotenv：
```bash
pip install python-dotenv
```

## 🤖 青龙面板配置

如果使用青龙面板，在环境变量中添加：

1. 进入青龙面板 → 环境变量
2. 添加以下变量：
   - `TG_BOT_TOKEN`: 你的 Bot Token
   - `TG_CHAT_ID`: 你的 Chat ID
   - `NL_COOKIE`: 你的 Cookie

3. 更新脚本：
   - 方法 1: 在青龙面板中点击"更新"按钮
   - 方法 2: SSH 进入服务器，手动执行 `git pull`

## 🔍 验证更新

检查是否有新文件：
```bash
ls -la | grep telegram_notifier.py
```

查看 requirements.txt 是否包含 requests：
```bash
cat requirements.txt
```

测试运行：
```bash
python main.py
```

## 📝 常见问题

### Q1: git pull 提示冲突怎么办？
```bash
# 方案 1: 丢弃本地修改
git reset --hard origin/main

# 方案 2: 暂存本地修改
git stash
git pull origin main
git stash pop
```

### Q2: 缺少 requests 模块
```bash
pip install requests
# 或
pip3 install requests
```

### Q3: Telegram 推送不工作
检查环境变量是否设置：
```bash
echo $TG_BOT_TOKEN
echo $TG_CHAT_ID
```

查看日志中是否有 "📱 Telegram 推送已启用"。

## 🎯 快速命令合集

```bash
# 一键更新并运行（适用于已有仓库）
cd /path/to/nodeloc-auto-signin && \
git pull origin main && \
pip install -r requirements.txt && \
python main.py

# 带环境变量的一键运行
cd /path/to/nodeloc-auto-signin && \
TG_BOT_TOKEN="your_token" TG_CHAT_ID="your_chat_id" NL_COOKIE="your_cookie" python main.py
```

## 📞 获取 Telegram Bot Token 和 Chat ID

### 获取 Bot Token：
1. 打开 Telegram，搜索 [@BotFather](https://t.me/BotFather)
2. 发送 `/newbot` 命令
3. 按提示设置 Bot 名称
4. 获得类似 `1234567890:ABCdefGHIjklMNOpqrsTUVwxyz` 的 Token

### 获取 Chat ID：
**方法 1（个人聊天）：**
1. 搜索 [@userinfobot](https://t.me/userinfobot)
2. 向它发送任意消息
3. 获得你的 Chat ID（纯数字）

**方法 2（群组/频道）：**
1. 将你的 Bot 添加到群组
2. 在群组中发送一条消息
3. 浏览器访问：`https://api.telegram.org/bot<你的TOKEN>/getUpdates`
4. 在返回的 JSON 中找到 `"chat":{"id":-1001234567890}` 
5. 这个负数就是群组的 Chat ID

---

✅ 更新完成后，脚本会在每次签到结束时自动推送结果到 Telegram！
