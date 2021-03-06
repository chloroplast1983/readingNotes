#  README

## 背景

`devops`, 所有培训方案分三个阶段。

1. 手动处理
2. `shell`脚本处理
3. `ansible`处理

### 1. ssh安全登录

#### 1.1 手动处理

* 要求
	1. SSH 登录服务器
	2. 修改登录端口号
	3. 禁止root用户登录服务器
	4. 免密码登录
	5. 使用密钥文件 ssh -i 登录服务器
* 考察
	* `sudo`
	* `useradd`
	* 是否切换不同账户完成操作，而非`root`账户
	* 是否修改时保障一个账户始终在线
* 结果
	* @贾晓莉 完成
	* @任子晖 未申请其他账户，导致`root` 账户不能登录
	* @倪静 后续未演示

#### 1.2 `shell`脚本自动处理

* 要求
	1. SSH 登录服务器-执行`shell`脚本(通过一条命令或`shell`脚本)
		* 修改登录端口号
		* 禁止root用户登录服务器
		* 添加用户`ansible`
		* `ansible`免密码登录
		* `ansible`可以`sudo`
* 考察
	* `useradd`
	* `read -p`
	* `sed -i`

echo $?

#### 1.3 `ansible`

* 要求
	1. 使用**1.2**项的`ansible`用户可以**批量**链接服务器

### 2. 用户管理和权限

* 要求
	* 添加一个用户`test`, 属组为`test`
	* 测试并验证
		* 文件的读，写，执行权限
		* 文件夹的读，写，执行权限
		* `suid`, `sgid`作用

### 3. 磁盘管理

#### 3.1 手动处理

* 要求
	* 格式化目前以后数据磁盘**A**, 格式化为**lvm**
		* 创建`pv`
		* 创建`vg`
		* 创建`lvm`
		* 格式化为`xfs`
	* 生成一个空的大文件`2G`
	* 添加一块磁盘**B**，格式化为`8e`
		* 添加到逻辑卷内，对磁盘进行**扩容**
* 考察
	* `lvm`
	* `fdisk`,`8e`
	* `dd`
	
#### 3.2 `shell`脚本自动处理

#### 3.3 `ansible`

### 4. `docker`

* 安装`docker`
* 安装`docker-compose`

* 用以后镜像编写`docker-compose`文件
* 创建网络，并启动环境
* 可以`ping`通数据库`mysql`
* `nginx`可以访问正常

* 制作`dockerfile`, `phpfpm`和`nginx`

* `shell`
* `ansible`

### 5. 离线

* 不能使用网络环境，使用离线安装包的方式安装

### 6. k8s

* 安装`k8s`
* 部署服务
* 可以正常访问自己制作的镜像网站

* `ansible`部署`k8s`

### 7. 综合

使用`ansible`批量部署

