# 部署dockerharbor(私有镜像仓库)

|      操作系统      |          centos 7.6          |
| :----------------: | :--------------------------: |
|      系统架构      |             x86              |
|      运行内存      |              2G              |
|     docker版本     |            26.1.4            |
| docker-compose版本 |           v2.39.2            |
|      内核版本      | 3.10.0-1160.119.1.el7.x86_64 |



### 1.部署依赖环境

#### 1.1.安装docker

```shell
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils

# Step 2: 添加软件源信息
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# Step 3: 安装Docker
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Step 4: 开启Docker服务
sudo service docker start

# 注意：
# 官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，您可以通过以下方式开启。同理可以开启各种测试版本等。
# vim /etc/yum.repos.d/docker-ce.repo
#   将[docker-ce-test]下方的enabled=0修改为enabled=1
#
# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# yum list docker-ce.x86_64 --showduplicates | sort -r
#   Loading mirror speeds from cached hostfile
#   Loaded plugins: branch, fastestmirror, langpacks
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
#   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
#   Available Packages
# Step2: 安装指定版本的Docker-CE: (VERSION例如上面的17.03.0.ce.1-1.el7.centos)
# sudo yum -y install docker-ce-[VERSION]
```

#### 1.2.安装docker-compose

[官网]: https://github.com/docker/compose

```shell
# 获取安装包
wget https://github.com/docker/compose/releases/download/v2.39.2/docker-compose-linux-x86_64

# 安装并验证
[root@VM-0-12-centos ~]# cp docker-compose-linux-x86_64 /usr/local/bin/
[root@VM-0-12-centos ~]# cd /usr/local/bin/
[root@VM-0-12-centos bin]# mv docker-compose-linux-x86_64 docker-compose
[root@VM-0-12-centos bin]# chmod +x docker-compose
[root@VM-0-12-centos bin]# docker-compose version
Docker Compose version v2.39.2
```

### 2.安装Harbor

[Harbor官网地址](https://goharbor.io/)

[Github开源地址](https://github.com/goharbor/harbor)

```bash
# 获取安装包
wget https://github.com/goharbor/harbor/releases/download/v2.13.2/harbor-offline-installer-v2.13.2.tgz

# 解压安装包
tar -xzvf harbor-offline-installer-v2.13.2.tgz

# 修改配置文件
cd harbor/
cp harbor.yml.tmpl harbor.yml

#修改hostname的值，如果没有域名就使用本机IP地址
vi harbor.yml
hostname: 192.168.42.133

#配置启动端口号
# http related config 
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 5000

# 如果没有申请证书，需要隐藏https
#https:
  # https port for harbor, default is 443
#  port: 443
  # The path of cert and key files for nginx
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path

#启动成功后，admin用户登录密码
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: AdminHarbor12345

# 启动
./install.sh

[Step 5]: starting Harbor ...
[+] Running 10/10
 ✔ Network harbor_harbor        Created                                                                                                             0.0s
 ✔ Container harbor-log         Started                                                                                                             0.5s
 ✔ Container redis              Started                                                                                                             1.1s
 ✔ Container registryctl        Started                                                                                                             1.1s
 ✔ Container harbor-db          Started                                                                                                             1.2s
 ✔ Container harbor-portal      Started                                                                                                             1.1s
 ✔ Container registry           Started                                                                                                             1.1s
 ✔ Container harbor-core        Started                                                                                                             1.4s
 ✔ Container nginx              Started                                                                                                             1.9s
 ✔ Container harbor-jobservice  Started                                                                                                             1.8s
✔ ----Harbor has been installed and started successfully.----
```

#### 2.1.验证

![](https://gitee.com/yqjgxx/image/raw/master/docker-harbor/20250830-173221.jpg)

![](https://gitee.com/yqjgxx/image/raw/master/docker-harbor/20250830-173409.jpg)

#### 2.2 配置ssl证书

```shell
# 把证书上传到指定的位置
[root@VM-0-12-centos harbor]# ls /root/harbor/ssl/
www.gxxyqj.chat_bundle.crt  www.gxxyqj.chat.key

# 更改配置文件
https:
  # https port for harbor, default is 443
  port: 443
  # The path of cert and key files for nginx
  certificate: /root/harbor/ssl/www.gxxyqj.chat_bundle.crt
  private_key: /root/harbor/ssl/www.gxxyqj.chat.key
  # enable strong ssl ciphers (default: false)
  # strong_ssl_ciphers: false

# 重新安装
[root@VM-0-12-centos harbor]# ./prepare
[root@VM-0-12-centos harbor]# ./install.sh
[Step 5]: starting Harbor ...
[+] Running 10/10
 ✔ Network harbor_harbor        Created                                    0.0s
 ✔ Container harbor-log         Started                                    0.6s
 ✔ Container registryctl        Started                                    1.2s
 ✔ Container registry           Started                                    1.2s
 ✔ Container redis              Started                                    1.1s
 ✔ Container harbor-db          Started                                    1.1s
 ✔ Container harbor-portal      Started                                    1.0s
 ✔ Container harbor-core        Started                                    1.4s
 ✔ Container nginx              Started                                    1.7s
 ✔ Container harbor-jobservice  Started                                    1.6s
✔ ----Harbor has been installed and started successfully.----
```

![](https://gitee.com/yqjgxx/image/raw/master/docker-harbor/3be1818b-b710-4d65-b259-abbbdbc3bc43.png)

✅ 配置external_url

| 配置方式              | Harbor 是否能自动推断？ | 是否需要 `external_url` |
| --------------------- | ----------------------- | ----------------------- |
| HTTP + 默认端口 80    | ✅ 能                    | ❌ 不需要                |
| HTTPS                 | ❌ **不能自动推断**      | ✅ **必须配置**          |
| 自定义端口（如 8443） | ❌ 不能                  | ✅ 必须配置              |

> 🔔 **Harbor 官方文档明确要求：使用 HTTPS 时必须设置 `external_url`**

```shell
# 修改harbor.yml 
cat harbor.yml | grep external_url
external_url: https://www.gxxyqj.chat

# 重启harbor仓库
docker-compose restart
```

### 3.操作使用

#### 3.1.上传下载镜像

![](https://gitee.com/yqjgxx/image/raw/master/docker-harbor/15720fc3-478e-4d5d-aa92-96348120bf8e.png)

```shell
# 登录harbor仓库，根据提示输入账号密码即可
docker login www.gxxyqj.chat 

# 镜像改tag
docker tag txpoc:v8 www.gxxyqj.chat/test-server/txpoc:v1

# 上传镜像
docker push www.gxxyqj.chat/test-server/txpoc:v1

# 下载镜像
docker pull www.gxxyqj.chat/test-server/txpoc:v1
```

![](https://gitee.com/yqjgxx/image/raw/master/docker-harbor/ebeed3eb-17e6-4b58-8dbd-576c72fe794a.png)

#### 3.2 用户权限

在Harbor仓库中，不同的角色拥有不同的权限和职责

**1.项目管理员（Project Admin）**

- **权限：**拥有对项目的完全控制权

 * **职责：**

   - 可以添加，删除项目成员

   * 管理项目的配置，包括配额，扫描策略等
   * 可以推送、拉取、删除镜像
   * 可以查看和管理项目的操作日志

**2.维护人员（Maintainer）** 

 * **权限**：拥有除管理员之外的所有权限
 * **职责**
       * 可以推送、拉取、删除镜像
       * 可以管理标签、扫描结果、Webhook等
       * 但不能添加或删除项目成员，也不能修改项目的全局配置

**3.开发者（Developer）**

 * **权限**：可以对镜像进行读写操作
 * **职责**：
   * 可以推送、拉取镜像
   * 可以删除自己推送的镜像
   * 通常用于开发团队，负责构建和测试镜像

**4.访客（Guest）**

 * **权限**：只读权限
 * **职责：**
   * 只能拉取镜像，不能推送或者删除镜像
   * 通常用于需要访问镜像但不需要修改的场景

**5.受限访客（Limited Guest）**

   * **权限：**非常有限的只读权限
   * **职责：**
     * 只能拉取特定的镜像，不能推送或删除任何镜像
     * 通常用于需要严格限制访问的场景，例如只允许访问最新的镜像

**6.移除成员（R二move Member）**

	* **功能：**不是一种角色，而是用于将成员从项目中移除的操作
 * **职责：**
   * 点击“移除成员”后，该用户将失去对该项目的所有访问权限

