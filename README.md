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

### 3️⃣ 设置环境变量（支持多账号）
```bash
export NL_COOKIE="_t=xxxxx; _forum_session=xxxxxx"
```

**（可选）配置 Telegram 推送**
```bash
export TG_BOT_TOKEN="你的Bot Token"
export TG_CHAT_ID="你的Chat ID"
```

**如何获取 Telegram Bot Token 和 Chat ID：**
1. 与 [@BotFather](https://t.me/BotFather) 对话，创建一个新的 Bot，获取 Bot Token
2. 将你的 Bot 添加到你的频道或群组，或直接与 Bot 私聊
3. 与 [@userinfobot](https://t.me/userinfobot) 对话，获取你的 Chat ID
4. 如果是群组，可以通过访问 `https://api.telegram.org/bot<你的token>/getUpdates` 获取 Chat ID

### 4️⃣ 运行脚本
```bash
python main.py
```

> **注意**：如果未配置 Telegram 推送环境变量，程序仍会正常运行，只是不会推送消息。
## 📜 License
本项目采用 MIT License 开源协议。

## ⭐ Star
如果这个项目对你有帮助，欢迎点个 ⭐
你的支持是我继续维护和优化的动力 ❤️
