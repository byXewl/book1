# Docker 环境识别
## 1.Docker内
* MAC地址为 `02:42:ac:11:00:00` - `02:42:ac:11:ff:ff`
* `ps aux` 大部分运行的程序 pid 都很小
* `cat /proc/1/cgroup` docker的进程 cat /proc/self/cgroup
* mount -v
* docker 环境下存在 `/.dockerenv`
* 部分容器中缺少许多常用的命令如 `ping` 等

## 2.Docker外
* `/var/run/docker.sock` 文件存在
* `2375` / `2376` 端口开启

^
#  容器内信息收集
* 用户信息 (当前用户、用户列表)
* 操作系统与内核版本
* 运行进程信息 (进程名、权限等)
* 容器是否为特权容器
* 环境变量
* 判断容器挂载信息，尝试挂载Docker Socket
* 网络环境，判断可以到达的网段
* 在云环境中，尝试获取元数据信息

