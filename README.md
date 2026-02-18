# NodeLoc Auto-Check-in

> 基于 Selenium + undetected-chromedriver 的 NodeLoc 自动签到脚本  
> 适用于 **青龙（QingLong） / Linux Server / 本地 Python 环境**

---

## ✨ 项目简介

本项目是一个 **NodeLoc 论坛自动签到脚本**，  
通过模拟真实浏览器行为完成每日签到操作，支持：

- ✅ 多账号顺序签到  
- ✅ 无头 Chrome（Headless）  
- ✅ Cookie 登录（无需账号密码）  
- ✅ 适配青龙面板（QingLong）  
- ✅ 自动规避常见 Selenium 特征检测  
- ✅ Telegram 推送签到结果

项目结构清晰，代码已模块化拆分，  
同时也适合作为 **Selenium 自动化学习示例**。

---

## 📁 项目结构

```text
.
├── browser.py            # 浏览器创建 & Cookie 注入
├── checkin.py            # 登录检测 & 签到逻辑
├── main.py               # 程序入口 & 多账号调度
├── telegram_notifier.py  # Telegram 推送模块
├── requirements.txt
├── README.md
└── LICENSE
```

## 🚀 使用方式

### 1️⃣ 安装依赖
请先确保系统中已安装 **Chrome / Chromium**，然后执行：
```bash
pip install -r requirements.txt
```
### 2️⃣ 获取 Cookie
使用浏览器登录：https://www.nodeloc.com
打开开发者工具（F12）
在 Network / 请求头 / Application → Cookies 中获取完整 Cookie

### 3️⃣ 配置环境变量

#### 方式 A：使用 .env 文件（推荐）⭐

```bash
# 1. 复制示例文件
cp .env.example .env

# 2. 编辑 .env 文件并填入实际配置
nano .env
```

#### 方式 B：使用启动脚本（推荐用于定时任务）⭐

```bash
# 创建 run.sh
cat > run.sh << 'EOF'
#!/bin/bash
export TG_BOT_TOKEN="你的Bot_Token"
export TG_CHAT_ID="你的Chat_ID"
export NL_COOKIE="你的Cookie"
cd "$(dirname "$0")"
python3 main.py
EOF

chmod +x run.sh
```

#### 方式 C：临时环境变量

```bash
export NL_COOKIE="_t=xxxxx; _forum_session=xxxxxx"
export TG_BOT_TOKEN="你的Bot Token"  # 可选
export TG_CHAT_ID="你的Chat ID"      # 可选
```

**Telegram 推送配置（可选）：**
- 与 [@BotFather](https://t.me/BotFather) 创建 Bot 获取 Token
- 与 [@userinfobot](https://t.me/userinfobot) 获取 Chat ID

### 4️⃣ 运行脚本
```bash
python main.py
# 或使用启动脚本
./run.sh
```

### 5️⃣ 设置定时任务（可选）

使用 crontab 设置每天自动签到：

```bash
# 编辑 crontab
crontab -e

# 添加定时任务（每天 0:01 执行）
1 0 * * * /root/nodeloc-auto-signin/run.sh >> /root/nodeloc-auto-signin/logs/cron.log 2>&1
```

**注意事项：**
- 确保脚本路径正确
- 创建日志目录：`mkdir -p /root/nodeloc-auto-signin/logs`
- 确保服务器时区为北京时间：`timedatectl set-timezone Asia/Shanghai`

> **提示**：未配置 Telegram 推送时程序仍会正常运行，只是不会发送通知消息。

---

## 📜 License
本项目采用 MIT License 开源协议。

## ⭐ Star
如果这个项目对你有帮助，欢迎点个 ⭐
你的支持是我继续维护和优化的动力 ❤️
