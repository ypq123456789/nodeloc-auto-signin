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

项目结构清晰，代码已模块化拆分，  
同时也适合作为 **Selenium 自动化学习示例**。

---

## 📁 项目结构

browser.py   # 浏览器创建 & Cookie 注入
checkin.py   # 登录检测 & 签到逻辑
main.py      # 程序入口 & 多账号调度
README.md

## 🚀 使用方式

### 1️⃣ 安装依赖
请先确保系统中已安装 **Chrome / Chromium**，然后执行：
```bash
pip install selenium undetected-chromedriver
