# 森空岛签到管理系统 - 阿里云部署白皮书

## 📋 项目说明

本项目是一个基于 Flask 的森空岛自动签到 Web 管理系统，支持：
- ✅ 用户注册/登录
- ✅ TOKEN 管理（获取、添加、编辑、删除）
- ✅ 自动签到测试
- ✅ 签到记录查询
- ✅ 定时任务（每日自动执行）
- ✅ 管理员后台

## 🎯 服务器环境要求

- **服务器**: 阿里云轻量应用服务器（2 核 2G 或更高）
- **操作系统**: Ubuntu 20.04 / 22.04 LTS（推荐）或 CentOS 7/8
- **面板**: 宝塔面板（已安装）
- **域名**: handsong.cc（已解析到服务器）
- **Python 版本**: 3.8 或更高

---

## 📦 第一部分：服务器环境配置

### 1.1 登录宝塔面板

1. 打开浏览器访问：`http://你的服务器 IP:8888`
   - 例如：`http://47.250.175.106:8888`
2. 输入宝塔面板账号密码登录

### 1.2 安装必要软件

在宝塔面板中，依次操作：

#### 步骤 1：安装 Python 项目管理器
1. 点击左侧菜单 **【软件商店】**
2. 搜索 **"Python 项目管理器"**
3. 点击 **【安装】**
4. 等待安装完成

#### 步骤 2：安装 Nginx
1. 在软件商店搜索 **"Nginx"**
2. 选择 **Nginx 1.20+** 版本
3. 点击 **【安装】**
4. 安装方式选择 **"快速编译"**

#### 步骤 3：安装 MySQL（可选，本项目使用 JSON 文件存储）
> ⚠️ **注意**: 本项目使用 JSON 文件存储数据，不需要 MySQL。但如果您的其他网站需要，可以安装。

---

## 🚀 第二部分：部署项目

### 方法一：通过宝塔面板部署（推荐，最简单）

#### 2.1.1 创建网站目录

1. 登录宝塔面板
2. 点击左侧菜单 **【文件】**
3. 进入 `/www/wwwroot/` 目录
4. 点击 **【新建文件夹】**
5. 命名为：`skyland-sign`
   - 完整路径：`/www/wwwroot/skyland-sign`

#### 2.1.2 上传项目文件

**方式 A：在线上传（适合小文件）**
1. 进入 `/www/wwwroot/skyland-sign` 目录
2. 点击工具栏的 **【上传】**
3. 将本地项目文件逐个拖拽上传
   - 必需文件：
     - `web_app.py`（主程序）
     - `requirements.txt`（依赖列表）
     - `src/` 目录（包含 skyland.py 等核心模块）
     - `config/` 目录（配置文件，可空白）

**方式 B：通过 Git 克隆（推荐）**
1. 先将代码推送到 GitHub/Gitee
2. 在宝塔面板中打开 **【终端】**
3. 执行命令：
```bash
cd /www/wwwroot/skyland-sign
git clone https://github.com/你的用户名/skyland-auto-sign.git .
```

**方式 C：通过 SFTP 上传（最快）**
1. 使用 FileZilla 或 WinSCP 连接服务器
   - 主机：`47.250.175.106`
   - 端口：`22`
   - 用户名：`root`
   - 密码：你的服务器密码
2. 本地目录：选择项目文件夹
3. 远程目录：`/www/wwwroot/skyland-sign`
4. 开始传输

#### 2.1.3 设置目录权限

1. 在宝塔面板文件管理器中，右键点击 `/www/wwwroot/skyland-sign`
2. 选择 **【权限】**
3. 设置权限为：**755**
4. 所有者：`www`
5. 勾选 **"应用到子目录"**
6. 点击 **【确定】**

---

### 方法二：通过 SSH 命令行部署（适合熟悉 Linux 的用户）

#### 2.2.1 连接到服务器

**Windows 用户：**
1. 打开 PowerShell 或 CMD
2. 输入命令：
```bash
ssh root@47.250.175.106
```
3. 输入服务器密码

**Mac/Linux 用户：**
1. 打开终端
2. 输入命令：
```bash
ssh root@47.250.175.106
```

#### 2.2.2 安装 Python 和依赖

**Ubuntu/Debian 系统：**
```bash
# 更新软件包
apt update && apt upgrade -y

# 安装 Python 3 和 pip
apt install python3 python3-pip python3-venv -y

# 验证安装
python3 --version
pip3 --version
```

**CentOS/RHEL 系统：**
```bash
# 安装 EPEL 源
yum install epel-release -y

# 安装 Python 3
yum install python3 python3-pip -y

# 验证安装
python3 --version
pip3 --version
```

#### 2.2.3 创建项目目录并上传

```bash
# 创建目录
mkdir -p /www/wwwroot/skyland-sign
cd /www/wwwroot/skyland-sign

# 方式 1：从 Git 克隆
git clone https://github.com/你的用户名/skyland-auto-sign.git .

# 方式 2：解压上传的 ZIP 包
# unzip skyland-auto-sign.zip
# mv skyland-auto-sign/* .
# rm -rf skyland-auto-sign.zip
```

#### 2.2.4 创建虚拟环境并安装依赖

```bash
# 创建虚拟环境
python3 -m venv venv

# 激活虚拟环境
source venv/bin/activate

# 升级 pip
pip install --upgrade pip

# 安装项目依赖
pip install -r requirements.txt

# 验证安装成功
pip list
```

---

## 🌐 第三部分：配置网站和域名

### 3.1 在宝塔面板中添加网站

1. 点击左侧菜单 **【网站】**
2. 点击 **【添加站点】**
3. 填写信息：
   - **域名**: `sign.handsong.cc`（或使用二级域名）
   - **根目录**: `/www/wwwroot/skyland-sign`
   - **PHP 版本**: 纯静态（本项目是 Python）
   - **数据库**: 无需数据库
4. 点击 **【提交】**

### 3.2 配置 Python 项目

1. 在网站列表中，找到刚创建的网站
2. 点击右侧的 **【设置】**
3. 选择 **【Python 项目】** 标签
4. 点击 **【添加 Python 项目】**
5. 填写配置：
   ```
   项目名称：skyland-sign
   启动文件：web_app.py
   端口：5000
   项目目录：/www/wwwroot/skyland-sign
   虚拟环境：/www/wwwroot/skyland-sign/venv
   启动方式：gunicorn
   ```
6. 点击 **【保存】**

### 3.3 配置反向代理

1. 在網站設置中，選擇 **【反向代理】** 標籤
2. 點擊 **【添加反向代理】**
3. 填寫：
   - **代理名称**: `skyland_proxy`
   - **目标 URL**: `http://127.0.0.1:5000`
   - **发送域名**: `$host`
4. 点击 **【确定】**

### 3.4 配置域名解析

1. 登录你的域名注册商（如阿里云万网）
2. 进入域名控制台
3. 找到 `handsong.cc`
4. 添加 DNS 记录：
   ```
   记录类型：A
   主机记录：sign（或 @ 如果想用主域名）
   记录值：47.250.175.106
   TTL：600
   ```
5. 保存后等待 5-10 分钟生效

### 3.5 配置 SSL 证书（HTTPS）

**免费 Let's Encrypt 证书：**
1. 在宝塔面板网站列表中，点击网站对应的 **【SSL】**
2. 选择 **【Let's Encrypt】**
3. 确认域名正确
4. 输入邮箱（用于续期通知）
5. 点击 **【申请】**
6. 申请成功后，开启 **【强制 HTTPS】**

---

## ⚙️ 第四部分：启动和配置服务

### 4.1 启动 Flask 应用

**宝塔面板方式：**
1. 在【Python 项目管理器】中找到项目
2. 点击 **【启动】**
3. 状态变为 **"运行中"** 即成功

**命令行方式：**
```bash
cd /www/wwwroot/skyland-sign
source venv/bin/activate

# 方式 1：直接启动（开发环境）
python web_app.py

# 方式 2：使用 Gunicorn（生产环境，推荐）
gunicorn -w 2 -b 127.0.0.1:5000 web_app:app

# 方式 3：后台运行
nohup gunicorn -w 2 -b 127.0.0.1:5000 web_app:app > app.log 2>&1 &
```

### 4.2 配置开机自启动

创建 systemd 服务文件：

```bash
nano /etc/systemd/system/skyland-sign.service
```

粘贴以下内容：

```ini
[Unit]
Description=Skyland Auto Sign Web Application
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/www/wwwroot/skyland-sign
Environment="PATH=/www/wwwroot/skyland-sign/venv/bin"
ExecStart=/www/wwwroot/skyland-sign/venv/bin/gunicorn -w 2 -b 127.0.0.1:5000 web_app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

保存后执行：

```bash
# 重载 systemd 配置
systemctl daemon-reload

# 启动服务
systemctl start skyland-sign

# 设置开机自启
systemctl enable skyland-sign

# 查看运行状态
systemctl status skyland-sign
```

---

## 🔧 第五部分：故障排查

### 问题 1：网站无法访问

**检查步骤：**
```bash
# 1. 检查服务是否运行
systemctl status skyland-sign

# 2. 检查端口是否监听
netstat -tlnp | grep 5000

# 3. 检查防火墙
ufw status  # Ubuntu
firewall-cmd --list-all  # CentOS

# 4. 查看日志
tail -f /www/wwwroot/skyland-sign/app.log
journalctl -u skyland-sign -f
```

**解决方案：**
```bash
# 重启服务
systemctl restart skyland-sign

# 开放端口（如果需要）
ufw allow 5000/tcp  # Ubuntu
firewall-cmd --permanent --add-port=5000/tcp && firewall-cmd --reload  # CentOS
```

### 问题 2：Python 依赖安装失败

```bash
# 清理 pip 缓存
pip cache purge

# 使用国内镜像
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 单独安装每个依赖
pip install flask
pip install requests
pip install schedule
pip install cryptography
pip install gunicorn
```

### 问题 3：权限错误

```bash
# 修改目录权限
chown -R www:www /www/wwwroot/skyland-sign
chmod -R 755 /www/wwwroot/skyland-sign

# 修改 config 目录权限（可写）
chmod -R 777 /www/wwwroot/skyland-sign/config
```

### 问题 4：域名解析不生效

**检查方法：**
```bash
# Windows
ping sign.handsong.cc
nslookup sign.handsong.cc

# Linux/Mac
ping sign.handsong.cc
dig sign.handsong.cc
```

**解决方案：**
- 等待 DNS 传播（最多 24 小时）
- 清除本地 DNS 缓存
- 检查域名解析设置是否正确

---

## 📊 第六部分：日常维护

### 查看运行日志

```bash
# 实时查看日志
tail -f /www/wwwroot/skyland-sign/app.log

# 查看最近 100 行
tail -n 100 /www/wwwroot/skyland-sign/app.log

# 宝塔面板查看
# 在【网站】->【设置】->【日志】中查看
```

### 更新项目代码

```bash
cd /www/wwwroot/skyland-sign

# 如果使用 Git
git pull origin main

# 手动更新
# 1. 备份当前版本
cp -r /www/wwwroot/skyland-sign /www/wwwroot/skyland-sign.backup

# 2. 上传新文件
# 3. 重启服务
systemctl restart skyland-sign
```

### 备份数据

```bash
# 备份配置文件
tar -czf skyland-backup-$(date +%Y%m%d).tar.gz /www/wwwroot/skyland-sign/config/

# 下载到本地
scp root@47.250.175.106:/www/wwwroot/skyland-sign/skyland-backup-*.tar.gz ./
```

### 重启服务

```bash
# 使用 systemctl
systemctl restart skyland-sign

# 或者在宝塔面板中重启 Python 项目
```

---

## 🎉 第七部分：访问和使用

### 访问地址

- **网站首页**: `http://sign.handsong.cc` 或 `https://sign.handsong.cc`
- **管理员登录**: 
  - 账号：`handsongskyland`
  - 密码：`handsongskyland@158`

### 首次使用步骤

1. 打开浏览器访问 `https://sign.handsong.cc`
2. 使用管理员账号登录
3. 进入 **"快速获取 TOKEN"** 模块
4. 选择登录方式获取 TOKEN
5. 在 **"签到测试"** 中测试 TOKEN 有效性
6. 配置定时任务时间（默认 00:01）

### 安全建议

1. **立即修改管理员密码**
   - 登录后在个人中心修改
   
2. **启用 HTTPS**
   - 使用 Let's Encrypt 免费证书
   
3. **配置防火墙**
   - 只开放必要端口（80, 443）
   
4. **定期备份**
   - 每周备份 config 目录
   
5. **监控资源使用**
   - 在宝塔面板查看 CPU 和内存使用率

---

## 💡 常见问题解答

### Q1: 2 核 2G 服务器能支撑多少用户？
**A**: 理论上可以支持 50-100 个注册用户同时在线，实际签到操作几乎不占用资源。

### Q2: 定时任务会占用很多资源吗？
**A**: 不会。定时任务只在每天指定时间执行一次，平时几乎不占用资源。

### Q3: 可以同时运行其他网站吗？
**A**: 可以。本项目占用资源很少（约 200-300MB 内存），剩余资源足够运行其他小型网站。

### Q4: 如何修改定时任务执行时间？
**A**: 登录管理员后台，在"定时执行脚本"模块中修改执行时间。

### Q5: 数据存储在哪个文件？
**A**: 所有数据存储在 `/www/wwwroot/skyland-sign/config/` 目录下：
- `users.json` - 用户数据
- `sign_records.json` - 签到记录
- `schedule.json` - 定时任务配置

---

## 📞 技术支持

如遇到问题，请检查：
1. 服务器运行状态
2. 项目日志文件
3. 宝塔面板错误提示
4. 本文档的故障排查部分

---

## ✅ 部署完成检查清单

- [ ] Python 环境安装完成
- [ ] 项目文件上传成功
- [ ] 依赖包安装成功
- [ ] 虚拟环境创建成功
- [ ] 网站在宝塔面板中添加成功
- [ ] 反向代理配置成功
- [ ] 域名解析生效
- [ ] SSL 证书申请成功
- [ ] 服务正常启动
- [ ] 能够正常访问网站
- [ ] 管理员能够登录
- [ ] TOKEN 功能测试通过
- [ ] 定时任务配置成功

**恭喜！部署完成！** 🎊
