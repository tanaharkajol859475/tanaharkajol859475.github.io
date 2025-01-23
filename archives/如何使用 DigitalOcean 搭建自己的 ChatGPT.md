本教程基于开源项目 [ChatGPT-Web](https://github.com/Chanzhaoyu/chatgpt-web)，并使用 DigitalOcean 的服务器进行部署，亲测可用。以下是详细步骤。

---

## 所需费用

- **DigitalOcean 服务器**：4 美金/月（注册赠送 200 美金，2 个月有效）。
- **WildCard 开卡费用**：15 美金。
- **OpenAI Token 费用**：每 100,000 个 token 约 0.04 美金（约 5 万个汉字）。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)

---

## 先决条件

1. **DigitalOcean 账号**  
2. **OpenAI 账号**  
   OpenAI 仅支持信用卡支付，但不接受中国信用卡，同时需要手机号验证（不支持中国手机号）。推荐使用 [WildCard](https://bit.ly/bewildcard)，提供注册账号、验证手机号、开卡一条龙服务。开卡费 15 美金，充值费率 3%。完成后保存申请到的 OpenAI API Key，后续会用到。

---

## 开始搭建

### 一、创建 DigitalOcean 服务器

1. **选择数据中心**：推荐新加坡数据中心，操作系统选择 CentOS 8。
2. **选择配置**：个人使用推荐 4 美金/月的最低配置。
3. **设置认证方式**：在 `Authentication Method` 步骤中选择 SSH Key，DigitalOcean 提供创建 SSH Key 的教程。
4. **创建服务器**：点击 `Create Droplet`，等待服务器创建完成。创建成功后，记录服务器的 IP 地址备用。

---

### 二、安装 Docker

1. **访问服务器**：在 DigitalOcean 控制台中点击 `Access Console`，打开服务器终端。
2. **安装 Docker**：执行以下命令逐步安装 Docker 和 Docker Compose。

   bash
   # 更新 yum
   yum update

   # 下载 Docker CE 的 repo
   curl https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo

   # 安装依赖
   yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm

   # 安装 Docker CE
   yum install docker-ce

   # 启动 Docker
   systemctl start docker

   # 设置开机启动
   systemctl enable docker

   # 安装 Docker Compose
   sudo wget https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m) -O /usr/local/bin/docker-compose

   # 如果报错 "wget: command not found"，先安装 wget
   yum -y install wget

   # 添加执行权限
   sudo chmod +x /usr/local/bin/docker-compose

   # 创建快捷方式
   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

   # 检查 Docker Compose 版本
   docker-compose --version
   

---

### 三、部署 ChatGPT

1. **创建工作目录**：在服务器上创建 `chatgpt_web` 目录并进入。

   bash
   mkdir chatgpt_web && cd chatgpt_web
   

2. **创建配置文件**：使用 `vim` 创建 `docker-compose.yml` 文件。

   bash
   vim docker-compose.yml
   

   如果提示 `-bash: vim: command not found`，先安装 `vim`：

   bash
   yum -y install vim*
   

   然后在文件中输入以下内容：

   yaml
   version: '3'
   services:
     app:
       image: chenzhaoyu94/chatgpt-web:latest
       ports:
         - 3002:3002
       environment:
         # API 密钥
         OPENAI_API_KEY: sk-xxx  # 替换为申请的 OpenAI API Key
         # 超时时间（单位：毫秒，可选）
         TIMEOUT_MS: 60000
   

   保存文件：按下 `Esc`，输入 `:wq`，然后回车。

3. **启动服务**：运行以下命令部署并启动服务。

   bash
   docker-compose up -d
   

4. **访问服务**：在浏览器中访问 `http://服务器IP:3002`（确保开放 3002 端口）。

---

### 四、常见问题解决

1. **页面加载失败**：如果遇到 `fetch failed` 错误，可以尝试刷新页面。如果仍然无效，重启 Docker 并重新启动服务。

   bash
   systemctl restart docker
   docker-compose up -d
   

2. **其他问题**：检查配置文件是否正确，确保没有多余的注释。

---

至此，您已成功搭建自己的 ChatGPT 服务！

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)