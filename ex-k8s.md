

## 创建基础虚拟机 centos-docker

1. 准备环境
    - 你的主机（host）处于一个私有网络中，如：处于一个路由后面
    - 主机能够上网
    - 主机安装了 Virtualbox，且构建了 minimal centos 7 安装的虚拟机
2. 用链接方式，复制已有虚拟机， 取名 centos-docker
    - 检查该虚拟机设置 - 网络，确保只有一块网卡有效
    - 设置网卡 `连接方式` = `桥接网卡`
    - `界面名称` = 你访问外网的网卡
3. 配置虚拟机
    - 启动虚拟机，用 `ip address` 检查网络是否动态获得路由分配的地址
    - 检查网络网桥是否工作正常，如 `ping www.sysu.edu.cn`
    - 升级 centos 以保证 docker 能正常启动。 `yum update`
    - 设虚拟机网络为固定地址（如：192.168.3.200），使用 `nmtui`
        - 将网卡设为手动， 网络地址 `192.168.3.200/24` , 
        - gateway `192.168.3.1` ，即路由的地址
        - DNS `223.5.5.5` 阿里的 DNS
        - 然后， deactivitate, activitate 该网卡
        - 修改 hostname = centos-docker
        - 退出
        - 用 `ip address` 检查设置是否生效!
    - 检查网络，如 `ping www.sysu.edu.cn`
4. 安装 docker 17.03.2 ce
    - 在 host 使用 ssh 连接虚拟机，例如：`ssh root@192.168.3.200` 
    - 按[官网指南](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)？ 这个[中文指南更详细](http://www.cnblogs.com/freefei/p/9263998.html)。请务必对比官网指南，避免中文落伍给你带来麻烦！
    - 最后，启动 docker 服务并验证
        - `systemctl enable docker`
        - `systemctl start docker`
        - `docker verson`

## 安装 kubernetes 初体验 

1. 准备环境（在前面 centos docker 基础上）
    - 准备 master node。 ip = 192.168.3.201, 主机名 k8s-master, CPU 配置 2 ，内存配置 4096
    - 准备 worker node1。 ip = 192.168.3.202, 主机名 k8s-worker1
    - 准备 worker node2。 ip = 192.168.3.203, 主机名 k8s-worker2
2. 安装 rancher 服务器与 bubernetes 节点
    - `docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher`





