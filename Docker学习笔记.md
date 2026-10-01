# 01. 课程简介

![image-20260930230832552](assets/image-20260930230832552.png)

## 课程介绍与学习目标
- Docker 的出现推动了运维部署进入容器化时代，极大提升了开发与运营效率。
- Docker 已成为程序员必备技能。
- 本课程通过提炼核心内容，以实战方式全程实操，配合动画辅助，帮助学习者快速掌握 Docker 的核心功能。
- 学习门槛较低，具备 Linux 基础即可。

## 课程结构
课程分为五个章节：
1. Docker 命令
2. Docker 存储
3. Docker 网络
4. Docker Compose
5. Dockerfile

## 实战演示内容
课程最后将实战演示：使用 Docker 一键批量启动常用中间件，包括：
- MySQL
- Redis
- RabbitMQ
- Zookeeper
- OpenSearch
- Kafka
- Nacos
- Prometheus
- Grafana

通过该实战，帮助学习者更进一步接近架构师水平。

## 学习前提与适用人群
- 学习者需具备 Linux 基础。
- 适合希望快速掌握 Docker、提升部署与运维能力的开发者。

# 02. 基础 - 为什么有Docker

![image-20260930231734347](assets/image-20260930231734347.png)

## Docker产生的背景与核心痛点

![image-20260930231756353](assets/image-20260930231756353.png)

### 开发环境搭建的复杂性
- 作为全干工程师，接手项目后需在服务器上安装MySQL、Redis等中间件。
- 搜索Linux安装文档进行复制粘贴时，**<span style="color:#ff0000;">常因操作系统适配问题</span>**（如无法区分Ubuntu、CentOS或Debian）导致运行失败。

### 项目交付部署的低效性
- 项目开发完成后，需为客户编写长达30页的详细部署文档（包括安装Java环境、MySQL、Redis等步骤）。
- 客户认为操作繁琐，期望像下载游戏一样一键安装。

### 软件版本管理与分发的困难
- 通过U盘、网盘或文件传输助手发送软件包时，面临链接丢失、版本混淆（如1.0版本找回困难）以及多版本维护不易的问题。
- 用户期望拥有类似手机应用商店的便捷体验。

## Docker的解决方案

### 跨平台快速运行
- Docker技术能够无视平台差异，通过一行命令快速安装并运行所需软件，解决环境配置难题。

### 快速构建与打包
- Docker能够将整个项目打包成一个软件包，实现应用的快速构建，让客户拿到软件包后可直接运行，简化交付流程。

### 快速分享与应用商店

![image-20260930231816503](assets/image-20260930231816503.png)

- Docker提供**Docker Hub**作为应用商店，开发者可将软件包发布至Docker Hub，用户可直接从中下载安装使用。
- Docker Hub中包含MySQL、Nginx、MongoDB、Python环境、Memcached等常用软件的镜像。

## Docker的核心概念定义

### 镜像（Image）
- **<span style="color:#ff0000;">发布到Docker Hub上的软件包被称为“镜像”。</span>**

### Docker的官方定义

![image-20260930231832138](assets/image-20260930231832138.png)

- **<span style="color:#ff0000;">Docker是用来加速应用的构建（Build）、分享（Share）和运行（Run）的工具。</span>**
- **构建**：快速将应用打包。
- **分享**：将软件包快速发布到Docker Hub等应用商店。
- **运行**：通过一行命令启动应用。

## 课程学习目标
- 后续学习将围绕Docker的三大核心功能展开：如何构建应用、分享应用以及运行应用。





# 03. 基础 - Docker架构与容器化

## 一、Docker 架构与工作流程

### Docker 环境组成

![image-20260930232554244](assets/image-20260930232554244.png)

- **Docker 主机（Docker Host）**：安装了 Docker 环境的机器。
  - **Docker Daemon**：后台进程，时刻运行并准备提供服务。
  - **Docker CLI**：命令行客户端，用于向 Docker Daemon 发送操作命令。
  - **Docker Registry**：应用市场，集中存储如 MySQL、Ubuntu、Redis 等软件镜像。

### 镜像下载与容器启动流程（以 Redis 为例）
1. 执行 `docker pull redis` 命令，CLI 将请求发送给 Docker Daemon。
2. Docker Daemon 连接应用市场下载 Redis 镜像至本机。
3. 执行 `docker run redis` 命令，Docker Daemon 在本地查找镜像（若不存在则自动下载）。
4. 利用该镜像启动应用，此时运行的应用实例称为“容器”。

### 镜像构建与分享流程
- **构建镜像**：使用 `docker build` 命令制作软件镜像。
- **推送镜像**：使用 `docker push` 命令将本地镜像推送到应用市场。
- **分享机制**：其他用户可通过 `docker pull` 下载该镜像，并使用 `docker run` 在各自机器上运行。

### 核心命令总结
- 构建软件包：`docker build`
- 分享应用：`docker push`（推送）、`docker pull`（下载）
- 运行应用：`docker run`

## 二、核心概念：镜像与容器
- **镜像（Image）**：**<span style="color:#ff0000;">本质是软件包。</span>**
- **容器（Container）**：<span style="color:#ff0000;">**由镜像启动的运行中的应用实例。**</span>

## 三、容器化技术的演进与优势

![image-20260930232704960](assets/image-20260930232704960.png)

### 传统部署的问题
- 直接在操作系统上部署应用缺乏隔离性。
- 若某应用发生内存泄露等资源故障，会挤压其他应用生存空间，导致整体系统崩溃。

### 虚拟化部署的局限
- **原理**：利用虚拟化技术创建虚拟机，每个应用部署在独立的虚拟机内，实现了隔离。
- **缺点**：每个虚拟机包含完整的操作系统，导致资源占用大、笨重。

### 容器化部署的原理
- **原理**：**<span style="color:#ff0000;">在操作系统上安装容器运行时环境，每个应用运行在独立的容器中。</span>**
- **隔离性**：容器类似于集装箱，封装了应用及其完整运行环境，容器间互相隔离。某容器故障仅影响自身，不波及其他容器。
- **轻量化**：容器共享宿主机的操作系统内核，无需为每个应用配备完整操作系统。
- **文件系统独立性**：虽然共享内核，但每个容器拥有独立的文件系统、CPU 和内存竞争空间。

## 四、容器化技术的核心优势
- **轻量级与快速启动**：不包含笨重的操作系统，资源占用极少，可实现秒级启动，远快于虚拟机。
- **隔离性**：容器之间互相隔离，互不影响，保障了应用的稳定性。
- **跨平台与可移植性**：只要目标机器安装了容器运行时环境，容器即可直接迁移运行。
- **高密度部署**：单位资源空间内可部署的容器数量远多于虚拟机，提升了资源利用率。
- **行业地位**：容器化部署已成为事实标准，被绝大多数公司采用。

# 04.基础 - 购买云服务器![image-20260930233529042](assets/image-20260930233529042.png)

## 一、前期准备与平台共享
- 安装规划：分为两步。第一步开通云服务器（或使用本地 Linux 虚拟机），并在其上安装 Docker；建议开通云服务器，用完即删以节约时间。
- 平台选择：选择**腾讯云**（阿里云最低充值 100 元对新手不友好）。云服务器厂商对新手有优惠活动，但实验期间建议采用**按量付费**模式。

## 二、云服务器选择与开通
- **登录腾讯云**：扫码登录腾讯云控制台，进入产品页，点击“云服务器”进行开通。
- **地域选择**：选择所在的地域以提升访问速度（示例选择“南京一区”）。
- **计费模式**：选择**按量计费**（即用几小时花几小时的钱，类似网吧上网）。
- **具体配置**：
  - 服务器配置选择“两核两G”。
  - 费用约为每小时 0.21 元，加上配置费用合计约每小时 0.26 元。

## 三、操作系统与网络设置
- **镜像选择**：选择操作系统镜像，示例中选择 **CentOS 7.9**（语音识别误为 windows 7.9）。
- **网络设置**：
  - 使用默认分配的网络信息。
  - 选中“分配独立公网IP”，以便能远程连接服务器。
  - 带宽计费：选择“按流量计费”，带宽大小不影响费用，仅收取流量费（1G 约 0.8 元）。

## 四、安全组与密码设置
- **安全组配置**（相当于服务器防火墙）：
  - 需放行端口：**80 端口**（Web）、**443 端口**（HTTPS）、**22 端口**（远程登录端口）。
- **登录密码设置**：
  - 密码必须包含大写字母、小写字母、数字及特殊符号。
  - 设置完成后点击下一步确认配置。

## 五、服务器创建与启动
- 确认配置信息：两核两G、CentOS 7.9 镜像、拥有公网IP、按流量计费。
- 点击“我已阅读协议开通”启动服务器。
- 预计整个实验期间费用十余元即可满足需求。
- 服务器状态变为“运行中”后，系统会分配一个公网IP地址。

## 六、远程连接服务器
- **复制公网IP**：复制服务器分配的公网IP地址。
- **连接工具**：使用 **WindTerm**（语音识别误为 winter term）。
  - 下载链接：https://github.com/kingToolbox/WindTerm/releases/download/2.6.0/WindTerm_2.6.1_Windows_Portable_x86_64.zip
- **新建会话**：
  - 主机地址填写复制的公网IP。
  - 标签命名为“docker host”。
- **登录验证**：
  - 账号输入“root”。
  - 输入开通服务器时设置的复杂密码。
  - 连接成功后进入主机界面，准备在下节课进行 Docker 安装。

![image-20260930234302408](assets/image-20260930234302408.png)

# 05. 基础 - 停机不收费

![image-20260930234539984](assets/image-20260930234539984.png)

## 一、功能背景与计费原理
- 服务器在开机状态下会持续计费，每小时约一块多，即使夜间闲置也会产生费用。
- 为节省成本，可使用“停机不收费”功能。

## 二、操作步骤与费用说明
- **执行关机**：点击关机并进行扫码确认。
- **选择模式**：在弹出的停机界面中选择“关机不收费”。
- **费用减免详情**：
  - CPU 和内存费用停止计费。
  - 硬盘镜像费用继续计费，因为需要保存操作系统及用户数据，防止改动丢失。
  - 硬盘费用极低，每月仅一两块钱，几乎可忽略不计。
- **确认操作**：选择“关机不收费”模式后点击确定。

## 三、关键注意事项：公网IP变化
- **IP释放机制**：由于服务器默认绑定普通公网IP，关机后该公网IP会被系统释放。
- **IP变更风险**：下次开机时，服务器可能会分配一个新的公网IP，导致原IP失效。
- **应对方法**：若IP发生变化，需使用新分配的IP进行连接，操作稍显繁琐。

## 四、重启与重新连接演示
- **状态确认**：界面显示服务器“已关机”。
- **执行开机**：点击“开机”按钮并确认，等待开机完成。
- **获取新IP**：开机完成后，系统生成新的公网IP，需复制该新IP。
- **更新连接配置**：
  - 在连接工具界面点击属性。
  - 将旧IP替换为新复制的IP。
  - 点击保存。
- **重新连接**：双击连接，确认指纹（yes to all），成功重新连接服务器。

## 五、总结与建议
- 每次操作结束后，建议让服务器停机以停止计费。
- 此方式适合非24小时连续运行的场景，可有效节省成本，让用户安心休息或处理其他事务。



# 06. 基础 - 安装 Docker（结合 Ubuntu 系统调整）

> 说明：原视频基于 CentOS 系统（使用 `yum` 包管理器）。由于你的系统为 Ubuntu（`6.8.0-124-generic`），以下命令已适配为 `apt` 包管理器。

## 一、参考官方文档与准备工作
- 安装 Docker 前可访问 docker.com 开发者文档，选择 **Docker Engine Install**。

  [https://docs.docker.com/]: https://docs.docker.com/

- 根据系统版本（此处为 Ubuntu）获取对应的安装步骤和命令。

## 二、第一步：移除旧版本 Docker
- 执行命令移除系统中可能存在的旧版本 Docker。
- 若是新系统可跳过此步，但建议执行以确保环境干净。
```bash
# 移除旧版本docker (Ubuntu)
sudo apt remove docker docker-engine docker.io containerd runc
```

## 三、第二步：配置 Docker 下载源
- **更新系统并安装依赖工具**：安装 `ca-certificates`、`curl`、`gnupg` 等工具，用于配置 Docker 的下载地址源。
- **配置阿里云镜像源**：由于直接连接 Docker 官网下载速度较慢，需将下载源配置为阿里云地址。执行以下命令让系统知道从何处下载 Docker。
```bash
# 更新apt包索引并安装依赖
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

# 添加Docker官方GPG密钥
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 设置稳定版仓库 (使用阿里云镜像加速源)
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 四、第三步：安装 Docker 引擎
- **安装内容说明**：安装包含 **Docker CE**（社区版引擎）、**Docker CLI**（命令行程序，用于向后台进程发送命令）、**containerd.io**（运行时容器环境）以及构建镜像所需的插件工具和 **Docker Compose**。
- **执行安装命令**：复制官方提供的安装命令并执行。
```bash
# 更新apt源并安装最新docker
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 五、启动 Docker 服务
- **立即启动**：输入以下命令启动 Docker 服务。当前用户为 root，可直接执行。
- **验证启动状态**：运行 `docker ps` 查看正在运行的应用，若无报错且列表为空，说明启动成功。
- **设置开机自启**：仅执行 start 命令只在当前会话有效，关机后需重新手动启动。执行以下命令确保 Docker 在系统开机时自动启动：
```bash
# 启动& 开机启动docker； enable + start 二合一
sudo systemctl enable docker --now
```

## 六、配置 Docker 镜像加速
- **配置原因**：Docker 默认从 Docker Hub 官网下载镜像，连接国外服务器速度较慢，因此需配置国内镜像源地址进行加速。
- **配置原理**：修改 Docker 后台进程的配置文件 `/etc/docker/daemon.json`，在 JSON 文件中配置 `registry-mirrors` 选项，指向国内的镜像源地址。
- **执行配置命令**：
```bash
# 配置加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
 "registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
EOF
```

## 七、重启服务与最终验证
- **重启 Docker**：修改配置文件后，需重启 Docker 后台进程及服务以使配置生效。
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```
- **最终验证**：再次运行 `docker ps` 等命令，若能正常执行，则表明 Docker 安装及加速配置全部成功。

```