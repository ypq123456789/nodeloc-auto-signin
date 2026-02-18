# Crontab 定时任务配置指南

## ⏰ 设置每天 0:01 自动签到

### 方法一：直接编辑 crontab（推荐）

```bash
# 1. 编辑 crontab
crontab -e

# 2. 添加以下内容（假设脚本在 /root/nodeloc-auto-signin 目录）
1 0 * * * cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py >> /root/nodeloc-auto-signin/logs/cron.log 2>&1

# 3. 保存退出（vim: 按 ESC，输入 :wq，回车）
```

### 解释：
- `1 0 * * *` - 每天 0 点 01 分执行
  - 第1位(1): 分钟 (0-59)
  - 第2位(0): 小时 (0-23)
  - 第3位(*): 日期 (1-31)
  - 第4位(*): 月份 (1-12)
  - 第5位(*): 星期 (0-6，0表示周日)
- `cd /root/nodeloc-auto-signin` - 切换到项目目录
- `/usr/bin/python3 main.py` - 使用 python3 执行脚本
- `>> /root/nodeloc-auto-signin/logs/cron.log 2>&1` - 输出日志

---

## 📝 完整配置步骤

### 1️⃣ 确认 Python 路径
```bash
which python3
# 输出可能是: /usr/bin/python3 或 /usr/local/bin/python3
```

### 2️⃣ 创建日志目录
```bash
mkdir -p /root/nodeloc-auto-signin/logs
```

### 3️⃣ 设置环境变量

有两种方式设置环境变量：

#### 方式 A：在 crontab 中设置（推荐）
```bash
crontab -e
```

添加内容：
```bash
# 环境变量
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TG_BOT_TOKEN=你的Bot_Token
TG_CHAT_ID=你的Chat_ID
NL_COOKIE=你的Cookie

# 定时任务（每天 0:01 执行）
1 0 * * * cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py >> /root/nodeloc-auto-signin/logs/cron.log 2>&1
```

#### 方式 B：使用脚本包装（更灵活）
创建执行脚本：
```bash
nano /root/nodeloc-auto-signin/run.sh
```

添加内容：
```bash
#!/bin/bash

# 设置环境变量
export TG_BOT_TOKEN="你的Bot_Token"
export TG_CHAT_ID="你的Chat_ID"
export NL_COOKIE="你的Cookie"

# 切换到脚本目录
cd /root/nodeloc-auto-signin

# 执行 Python 脚本
/usr/bin/python3 main.py
```

赋予执行权限：
```bash
chmod +x /root/nodeloc-auto-signin/run.sh
```

在 crontab 中添加：
```bash
crontab -e
```
```bash
1 0 * * * /root/nodeloc-auto-signin/run.sh >> /root/nodeloc-auto-signin/logs/cron.log 2>&1
```

### 4️⃣ 查看 crontab 配置
```bash
crontab -l
```

### 5️⃣ 测试执行
不等到 0:01，手动测试一次：
```bash
# 方式 A：直接执行
cd /root/nodeloc-auto-signin
python3 main.py

# 方式 B：执行脚本
/root/nodeloc-auto-signin/run.sh

# 方式 C：模拟 cron 环境测试
cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py
```

---

## 🌏 时区配置

确保服务器使用北京时间（CST，UTC+8）：

### 检查当前时区
```bash
date
timedatectl
```

### 设置为北京时间
```bash
# 方法 1: 使用 timedatectl
timedatectl set-timezone Asia/Shanghai

# 方法 2: 创建软链接
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 方法 3: 设置环境变量（仅当前会话）
export TZ='Asia/Shanghai'
```

### 验证时区
```bash
date
# 应该显示: CST (中国标准时间)
```

---

## 📊 Crontab 时间格式说明

```
* * * * * 命令
│ │ │ │ │
│ │ │ │ └─ 星期 (0-6, 0=周日)
│ │ │ └─── 月份 (1-12)
│ │ └───── 日期 (1-31)
│ └─────── 小时 (0-23)
└───────── 分钟 (0-59)
```

### 常用示例：
```bash
# 每天 0:01 执行
1 0 * * *

# 每天 8:30 执行
30 8 * * *

# 每小时执行一次
0 * * * *

# 每 6 小时执行一次
0 */6 * * *

# 每周一 9:00 执行
0 9 * * 1

# 每月 1 号 0:01 执行
1 0 1 * *
```

---

## 🔍 查看和管理 Crontab

### 查看当前用户的 crontab
```bash
crontab -l
```

### 编辑 crontab
```bash
crontab -e
```

### 删除 crontab
```bash
crontab -r
```

### 查看 cron 服务状态
```bash
# Debian/Ubuntu
systemctl status cron

# CentOS/RHEL
systemctl status crond
```

### 启动 cron 服务
```bash
# Debian/Ubuntu
systemctl start cron
systemctl enable cron

# CentOS/RHEL
systemctl start crond
systemctl enable crond
```

---

## 📜 查看执行日志

### 查看脚本输出日志
```bash
# 实时查看
tail -f /root/nodeloc-auto-signin/logs/cron.log

# 查看最近 50 行
tail -n 50 /root/nodeloc-auto-signin/logs/cron.log

# 查看全部日志
cat /root/nodeloc-auto-signin/logs/cron.log
```

### 查看系统 cron 日志
```bash
# Debian/Ubuntu
tail -f /var/log/syslog | grep CRON

# CentOS/RHEL
tail -f /var/log/cron
```

---

## 🐛 常见问题排查

### 问题 1: Crontab 没有执行

**检查 cron 服务：**
```bash
systemctl status cron  # 或 crond
```

**查看系统日志：**
```bash
grep CRON /var/log/syslog  # Debian/Ubuntu
tail -f /var/log/cron      # CentOS/RHEL
```

### 问题 2: 找不到 Python 或模块

**在 crontab 中指定完整路径：**
```bash
1 0 * * * cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py
```

**或使用 bash wrapper：**
```bash
1 0 * * * /bin/bash -c 'cd /root/nodeloc-auto-signin && source /root/.bashrc && python3 main.py'
```

### 问题 3: 环境变量未生效

在 crontab 顶部添加：
```bash
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOME=/root
```

### 问题 4: 权限问题

```bash
# 检查文件权限
ls -la /root/nodeloc-auto-signin/main.py

# 检查日志目录权限
ls -la /root/nodeloc-auto-signin/logs/

# 给脚本添加执行权限
chmod +x /root/nodeloc-auto-signin/run.sh
```

### 问题 5: Selenium/Chrome 无法在 cron 中运行

确保设置了无头模式，检查 `browser.py` 中是否有：
```python
options.add_argument('--headless')
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
```

---

## 📧 添加邮件通知（可选）

如果想在执行失败时收到邮件：

```bash
# 在 crontab 顶部设置邮箱
MAILTO=your-email@example.com

# 定时任务
1 0 * * * cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py >> /root/nodeloc-auto-signin/logs/cron.log 2>&1 || echo "签到失败" | mail -s "NodeLoc签到失败" your-email@example.com
```

---

## ✅ 完整配置示例

### 最简单的配置（推荐新手）：

```bash
crontab -e
```

添加：
```bash
# NodeLoc 自动签到 - 每天 0:01 执行
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TG_BOT_TOKEN=你的Bot_Token
TG_CHAT_ID=你的Chat_ID
NL_COOKIE=你的Cookie

1 0 * * * cd /root/nodeloc-auto-signin && /usr/bin/python3 main.py >> /root/nodeloc-auto-signin/logs/cron.log 2>&1
```

### 使用独立脚本的配置（推荐进阶）：

1. **创建 run.sh**：
```bash
cat > /root/nodeloc-auto-signin/run.sh << 'EOF'
#!/bin/bash
export TG_BOT_TOKEN="你的Bot_Token"
export TG_CHAT_ID="你的Chat_ID"
export NL_COOKIE="你的Cookie"
cd /root/nodeloc-auto-signin
/usr/bin/python3 main.py
EOF

chmod +x /root/nodeloc-auto-signin/run.sh
```

2. **配置 crontab**：
```bash
crontab -e
```
```bash
1 0 * * * /root/nodeloc-auto-signin/run.sh >> /root/nodeloc-auto-signin/logs/cron.log 2>&1
```

---

## 🎯 快速设置命令（复制粘贴）

```bash
# 创建日志目录
mkdir -p /root/nodeloc-auto-signin/logs

# 创建运行脚本
cat > /root/nodeloc-auto-signin/run.sh << 'EOF'
#!/bin/bash
export TG_BOT_TOKEN="替换为你的Bot_Token"
export TG_CHAT_ID="替换为你的Chat_ID"
export NL_COOKIE="替换为你的Cookie"
cd /root/nodeloc-auto-signin
/usr/bin/python3 main.py
EOF

# 赋予执行权限
chmod +x /root/nodeloc-auto-signin/run.sh

# 添加到 crontab
(crontab -l 2>/dev/null; echo "1 0 * * * /root/nodeloc-auto-signin/run.sh >> /root/nodeloc-auto-signin/logs/cron.log 2>&1") | crontab -

# 查看配置
crontab -l

# 测试执行
/root/nodeloc-auto-signin/run.sh
```

**记得修改 run.sh 中的环境变量！**

---

✅ 配置完成后，脚本将在每天北京时间 0:01 自动执行，并通过 Telegram 推送结果！
