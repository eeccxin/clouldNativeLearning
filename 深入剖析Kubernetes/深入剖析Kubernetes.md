# 介绍

> 本文是极客时间课程《[深入剖析Kubernetes](https://time.geekbang.org/column/intro/100015201)》的相关笔记

<img src="深入剖析Kubernetes.assets/image-20240130161244074.png" alt="image-20240130161244074" style="zoom:50%;" />

课程目录

```bash
# 开篇词
打通“容器技术”的任督二脉

# 预习篇
01 | 预习篇 · 小鲸鱼大事记（一）：初出茅庐
02 | 预习篇 · 小鲸鱼大事记（二）：崭露头角
03 | 预习篇 · 小鲸鱼大事记（三）：群雄并起
04 | 预习篇 · 小鲸鱼大事记（四）：尘埃落定

# 容器技术概念入门篇
05 | 白话容器基础（一）：从进程说开去
06 | 白话容器基础（二）：隔离与限制
07 | 白话容器基础（三）：深入理解容器镜像
08 | 白话容器基础（四）：重新认识Docker容器
09 | 从容器到容器云：谈谈Kubernetes的本质

# Kubernetes集群搭建与实践
10 | Kubernetes一键部署利器：kubeadm
11 | 从0到1：搭建一个完整的Kubernetes集群
12 | 牛刀小试：我的第一个容器化应用钟

# 容器编排与Kubernetes作业管理
13 | 为什么我们需要Pod？
14 | 深入解析Pod对象（一）：基本概念
15 | 深入解析Pod对象（二）：使用进阶
16 | 编排其实很简单：谈谈“控制器”模型
17 | 经典PaaS的记忆：作业副本与水平扩展
18 | 深入理解StatefulSet（一）：拓扑状态
19 | 深入理解StatefulSet（二）：存储状态
20 | 深入理解StatefulSet（三）：有状态应用实践
21 | 容器化守护进程的意义：DaemonSet
22 | 撬动离线业务：Job与CronJob
23 | 声明式API与Kubernetes编程范式
24 | 深入解析声明式API（一）：API对象的奥秘
25 | 深入解析声明式API（二）：编写自定义控制器
26 | 基于角色的权限控制：RBAC
27 | 聪明的微创新：Operator工作原理解读

# Kubernetes容器持久化存储
28 | PV、PVC、StorageClass，这些到底在说啥？
29 | PV、PVC体系是不是多此一举？从本地持久化卷谈起
30 | 编写自己的存储插件：FlexVolume与CSI
31 | 容器存储实践：CSI插件编写指南

# Kubernetes容器网络
32 | 浅谈容器网络
33 | 深入解析容器跨主机网络
34 | Kubernetes网络模型与CNI网络插件
35 | 解读Kubernetes三层网络方案
36 | 为什么说Kubernetes只有soft multi-tenancy？
37 | 找到容器不容易：Service、DNS与服务发现
38 | 从外界连通Service与Service调试“三板斧”
39 | 谈谈Service与Ingress

# Kubernetes作业调度与资源管理
40 | Kubernetes的资源模型与资源管理
41 | 十字路口上的Kubernetes默认调度器
42 | Kubernetes默认调度器调度策略解析
43 | Kubernetes默认调度器的优先级与抢占机制
44 | Kubernetes GPU管理与Device Plugin机制

# Kubernetes容器运行时
45 | 幕后英雄：SIG-Node与CRI
46 | 解读 CRI 与 容器运行时
47 | 绝不仅仅是安全：Kata Containers 与 gVisor

# Kubernetes容器监控与日志
48 | Prometheus、Metrics Server与Kubernetes监控体系
49 | Custom Metrics: 让Auto Scaling不再“食之无味”
50 | 让日志无处可逃：容器日志收集与管理

# 再谈开源与社区
51 | 谈谈Kubernetes开源社区和未来走向

# 答疑文章
52 | 答疑：在问题中解决问题，在思考中产生思考

# 特别放送
特别放送 | 2019 年，容器技术生态会发生些什么？
特别放送 | 基于 Kubernetes 的云原生应用管理，到底应该怎么做？

# 结束语
结束语 | Kubernetes：赢开发者赢天下
```



> k8s技能图谱

![img](深入剖析Kubernetes.assets/0da944e5bac4fe1d00d3f01a747e86cb.jpg)

# 预习篇

> docker历史

## 01  小鲸鱼大事记（一）：初出茅庐

从Cloud Foundry项目的PaaS技术讲**“应用托管”的能力**

- “沙盒”的隔离环境：Cgroups 和 Namespace 机制

2013年，眼看就要被如火如荼的 PaaS 风潮抛弃，dotCloud 公司却做出了这样一个决定：开源自己的容器项目 Docker：

- 凭借Docker镜像功能（解决便捷打包的问题），击败了PaaS。
- 镜像的精髓是**直接打包了应用运行所需要的整个操作系统**，保证打包的本地环境和云端环境的高度一致！

2014 年，Docker 公司对外发布了“Docker 原生”容器集群管理项目 Swarm，不仅将这波“CaaS”热推向了一个前所未有的高潮，更是寄托了整个 Docker 公司重新定义 PaaS 的宏伟愿望。



## 02  小鲸鱼大事记（二）：崭露头角

Docker 项目在短时间内迅速崛起的三个重要原因：

1. Docker 镜像通过技术手段解决了 PaaS 的根本性问题；（打包）
2. Docker 容器同开发者之间有着与生俱来的密切关系；（环境、配置不一致导致的部署难问题）
3. PaaS 概念已经深入人心的完美契机。

崭露头角的 Docker 公司，也终于能够以一个更加强硬的姿态来面对这个曾经无比强势，但现在却完全不知所措的云计算市场。而 2014 年底的 DockerCon 欧洲峰会，则正式拉开了 Docker 公司扩张的序幕。



为何又发布了Swarm？

- Docker 公司，兜兜转转了一年多，却还是回到了 PaaS 项目原本深耕了多年的那个战场：**如何让开发者把应用部署在我的项目上。**

- 只不过这时，“PaaS”的定义已经全然不是 Cloud Foundry 描述的那个样子，而是变成了**一套以 Docker 容器为技术核心，以 Docker 镜像为打包标准的、全新的“容器化”思路**。

  

## 03 小鲸鱼大事记（三）：群雄并起

### CoreOS公司

​		CoreOS 公司的参与，想要把“容器”的概念无缝集成到自己的这套方案中，从而为用户提供更高层次的 PaaS 能力。但与docker公司有冲突，并在2014 年底推出了自己研制的 Rocket（后来叫 rkt）容器。

​		Swarm 的最大亮点，则是它完全使用 Docker 项目原本的容器管理 API 来完成集群管理，比如：

- 单机 Docker 项目：

```
$ docker run "我的容器
```

- 多机 Docker 项目：

```
$ docker run -H "我的Swarm集群API地址" "我的容器"
```



### Fig项目

> Fig 项目在开发者面前第一次提出了**“容器编排”（Container Orchestration）**的概念。

“编排”（Orchestration）在云计算行业里不算是新词汇，它主要是指用户如何通过某些工具或者配置来完成一组虚拟机以及关联资源的定义、配置、创建、删除等工作，然后由云计算平台按照这些指定的逻辑来完成的过程。



> Fig 项目被收购后改名为 Compose，它成了 Docker 公司到目前为止第二大受欢迎的项目，一直到今天也依然被很多人使用。



###  Mesos 项目

> 老牌集群管理项目 Mesos 和它背后的创业公司 Mesosphere

​		Mesos 作为 Berkeley 主导的**大数据套件**之一，是大数据火热时最受欢迎的资源管理项目，也是跟 Yarn 项目杀得难舍难分的实力派选手。

​		不过，大数据所关注的计算密集型离线业务，其实并不像常规的 Web 服务那样适合用容器进行托管和扩容，也没有对应用打包的强烈需求，所以 Hadoop、Spark 等项目到现在也没在容器技术上投下更大的赌注；但是对于 Mesos 来说，天生的两层调度机制让它非常容易从大数据领域抽身，转而去支持受众更加广泛的 PaaS 业务。

​	在这种思路的指导下，Mesosphere 公司发布了一个名为 Marathon 的项目，而这个项目很快就成为了 Docker Swarm 的一个有力竞争对手。

​	**虽然不能提供像 Swarm 那样的原生 Docker API，Mesos 社区却拥有一个独特的竞争力：超大规模集群的管理经验。**

​	早在几年前，Mesos 就已经通过了**万台节点**的验证，2014 年之后又被广泛使用在 eBay 等大型互联网公司的生产环境中。而这次通过 Marathon 实现了诸如应用托管和负载均衡的 PaaS 功能之后，Mesos+Marathon 的组合实际上进化成了一个高度成熟的 PaaS 项目，同时还能很好地支持大数据业务。



### k8s项目

2014 年注定是一个神奇的年份。就在这一年的 6 月，基础设施领域的翘楚 Google 公司突然发力，正式宣告了一个名叫 Kubernetes 项目的诞生。而这个项目，不仅挽救了当时的 CoreOS 和 RedHat，还如同当年 Docker 项目的横空出世一样，再一次改变了整个容器市场的格局。



## 04  小鲸鱼大事记（四）：尘埃落定



###  OCI标准

2015 年 6 月 22 日，由 Docker 公司牵头，CoreOS、Google、RedHat 等公司共同宣布，Docker 公司将 Libcontainer 捐出，并改名为 RunC 项目，交由一个完全中立的基金会管理，然后以 RunC 为依据，大家共同制定一套容器和镜像的标准和规范。

这套标准和规范，就是 OCI（ Open Container Initiative ）。**OCI 的提出，意在将容器运行时和镜像的实现从 Docker 项目中完全剥离出来**。



### CNCF基金会

​	Google、RedHat 等开源基础设施领域玩家们，共同牵头发起了一个名为 CNCF（Cloud Native Computing Foundation）的基金会。这个基金会的目的其实很容易理解：它希望，以 Kubernetes 项目为基础，建立一个由开源基础设施领域厂商主导的、按照独立基金会方式运营的平台级社区，来对抗以 Docker 公司为核心的容器商业生态。

​	而为了打造出这样一条围绕 Kubernetes 项目的“护城河”，CNCF 社区就需要至少确保两件事情：

1. Kubernetes 项目必须能够在容器编排领域取得足够大的竞争优势；
2. CNCF 社区必须以 Kubernetes 项目为核心，覆盖足够多的场景。



### 三足鼎立

- Swarm 擅长的是跟 Docker 生态的无缝集成
- Mesos 擅长的则是大规模集群的调度与管理
- k8s利用Borg的内部特性，如Pod、Sidecar 等功能和设计模式，避免同质化。



在 2016 年，Docker 公司宣布了一个震惊所有人的计划：放弃现有的 Swarm 项目，将容器编排和集群管理功能全部内置到 Docker 项目当中。

而 **Kubernetes 的应对策略则是反其道而行之，开始在整个社区推进“民主化”架构**，即：从 API 到容器运行时的每一层，Kubernetes 项目都为开发者暴露出了可以扩展的插件机制，鼓励用户通过代码的方式介入 Kubernetes 项目的每一个阶段。

Kubernetes 项目的这个变革的效果立竿见影，很快在整个容器社区中催生出了大量的、基于 Kubernetes API 和扩展接口的二次创新工作，比如：

- 目前热度极高的微服务治理项目 Istio；
- 被广泛采用的有状态应用部署框架 Operator；
- 还有像 Rook 这样的开源创业项目，它通过 Kubernetes 的可扩展接口，把 Ceph 这样的重量级产品封装成了简单易用的容器存储插件。



### 尘埃落定

从 2017 年开始，Docker 公司先是将 Docker 项目的容器运行时部分 Containerd 捐赠给 CNCF 社区，标志着 Docker 项目已经全面升级成为一个 PaaS 平台；紧接着，Docker 公司宣布将 Docker 项目改名为 Moby，然后交给社区自行维护，而 Docker 公司的商业产品将占有 Docker 这个注册商标。

Docker 公司这些举措背后的含义非常明确：它将全面放弃在开源社区同 Kubernetes 生态的竞争，转而专注于自己的商业业务，并且通过将 Docker 项目改名为 Moby 的举动，将原本属于 Docker 社区的用户转化成了自己的客户。

2017年 10 月，Docker 公司出人意料地宣布，将在自己的主打产品 Docker 企业版中内置 Kubernetes 项目，这标志着持续了近两年之久的“编排之争”至此落下帷幕。

2018 年 1 月 30 日，RedHat 宣布斥资 2.5 亿美元收购 CoreOS。

**2018 年** 3 月 28 日，这一切纷争的始作俑者，Docker 公司的 CTO Solomon Hykes 宣布辞职，曾经纷纷扰扰的容器技术圈子，到此尘埃落定。



### 总结

Kubernetes 社区，正是以一种更加温和的方式，承接了 Docker 项目的未尽事业，即：以开发者为核心，构建一个相对民主和开放的容器生态。

这也是为何，Kubernetes 项目的成功其实是必然的。



# 容器技术概念入门篇

## 05 白话容器基础（一）：从进程说开去

​		一旦“程序”被执行起来，它就从磁盘上的二进制文件，变成了计算机内存中的数据、寄存器里的值、堆栈中的指令、被打开的文件，以及各种设备的状态信息的一个集合。**像这样一个程序运行起来后的计算机执行环境的总和，就是我们今天的主角：进程。**

​		而**容器技术的核心功能，就是通过约束和修改进程的动态表现，从而为其创造出一个“边界”。**

​		对于 Docker 等大多数 Linux 容器来说，**Cgroups 技术**是用来制造约束的主要手段(限制），而 **Namespace 技术**则是用来修改进程视图的主要方法（隔离）。

### namespace--隔离

>  Linux 容器最基本的实现原理：

Docker 容器实际上是在创建容器进程时，指定了这个进程所需要启用的一组 Namespace 参数。这样，容器就只能“看”到当前 Namespace 所限定的资源、文件、设备、状态，或者配置。而对于宿主机以及其他不相关的程序，它就完全看不到了。

**Namespace 技术实际上修改了应用进程看待整个计算机“视图”，即它的“视线”被操作系统做了限制，只能“看到”某些指定的内容**。但对于宿主机来说，这些被“隔离”了的进程跟其他进程并没有太大区别。

**所以说，容器，其实是一种特殊的进程而已。**

> 底层实现

​		当我们用 clone() 系统调用创建一个新进程时，就可以在参数中指定 CLONE_NEWPID 信号量参数，比如：

```c
int pid = clone(main_function, stack_size, CLONE_NEWPID | SIGCHLD, NULL); 
```

​		这时，新创建的这个进程将会“看到”一个全新的进程空间，在这个进程空间里，它的 PID 是 1。之所以说“看到”，是因为这只是一个“障眼法”，在宿主机真实的进程空间里，这个进程的 PID 还是真实的数值，比如 100。

​	而**除了我们刚刚用到的 PID Namespace，Linux 操作系统还提供了 Mount、UTS、IPC、Network 和 User 这些 Namespace，用来对各种不同的进程上下文进行“障眼法”操作。**



## 06 白话容器基础（二）：隔离与限制

​		**“敏捷”和“高性能”是容器相较于虚拟机最大的优势，也是它能够在 PaaS 这种更细粒度的资源管理平台上大行其道的重要原因。**

​		不过，有利就有弊，基于 Linux Namespace 的隔离机制相比于虚拟化技术也有很多不足之处，其中最主要的问题就是：**隔离得不彻底。*：

- 多个容器之间使用的就还是同一个宿主机的操作系统内核
- 其次，在 Linux 内核中，有很多资源和对象是不能被 Namespace 化的，最典型的例子就是：时间。（截止 2018 年 9 月本专栏发布时）



### cGroups --限制

**Linux Cgroups 就是 Linux 内核中用来为进程设置资源限制的一个重要功能。**

有意思的是，Google 的工程师在 2006 年发起这项特性的时候，曾将它命名为“进程容器”（process container）。实际上，在 Google 内部，“容器”这个术语长期以来都被用于形容被 Cgroups 限制过的进程组。

**Linux Cgroups 的全称是 Linux Control Group。它最主要的作用，就是限制一个进程组能够使用的资源上限，包括 CPU、内存、磁盘、网络带宽等等。**

在 Linux 中，Cgroups 给用户暴露出来的操作接口是文件系统，即它以文件和目录的方式组织在操作系统的 /sys/fs/cgroup 路径下。在宿主机器里，我可以用 mount 指令把它们展示出来，这条命令是：

```bash
$ mount -t cgroup 
cpuset on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
cpu on /sys/fs/cgroup/cpu type cgroup (rw,nosuid,nodev,noexec,relatime,cpu)
cpuacct on /sys/fs/cgroup/cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpuacct)
blkio on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
memory on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
...
```

> 以限制cpu为例

比如，对 CPU 子系统来说，我们就可以看到如下几个配置文件，这个指令是：

```bash
$ ls /sys/fs/cgroup/cpu
cgroup.clone_children cpu.cfs_period_us cpu.rt_period_us  cpu.shares notify_on_release
cgroup.procs      cpu.cfs_quota_us  cpu.rt_runtime_us cpu.stat  tasks
```

cfs_period 和 cfs_quota 这样的关键词。这两个参数需要组合使用，可以用来限制进程在长度为 cfs_period 的一段时间内，只能被分配到总量为 cfs_quota 的 CPU 时间。而tasks中配置生效的PID。



除 CPU 子系统外，Cgroups 的每一个子系统都有其独有的资源限制能力，比如：

- blkio，为块设备设定I/O 限制，一般用于磁盘等设备；
- cpuset，为进程分配单独的 CPU 核和对应的内存节点；
- memory，为进程设定内存使用的限制。

> docke利用cGroup实现限制

**Linux Cgroups 的设计还是比较易用的，简单粗暴地理解呢，它就是一个子系统目录加上一组资源限制文件的组合**。而对于 Docker 等 Linux 容器项目来说，它们只需要在每个子系统下面，为每个容器创建一个控制组（即创建一个新目录），然后在启动容器进程之后，把这个进程的 PID 填写到对应控制组的 tasks 文件中就可以了。

而至于在这些控制组下面的资源文件里填上什么值，就靠用户执行 docker run 时的参数指定了，比如这样一条命令：

```bash
$ docker run -it --cpu-period=100000 --cpu-quota=20000 ubuntu /bin/bash
```

在启动这个容器后，我们可以通过查看 Cgroups 文件系统下，CPU 子系统中，“docker”这个控制组里的资源限制文件的内容来确认：

```bash
$ cat /sys/fs/cgroup/cpu/docker/5d5c9f67d/cpu.cfs_period_us 
100000
$ cat /sys/fs/cgroup/cpu/docker/5d5c9f67d/cpu.cfs_quota_us 
20000
```

补充：需要`/etc/docker/daemon.json` 文件设置cgroupdriver为cgroupfs才是使用这套机制。k8s的推荐driver是systemd。

```json
{
  "exec-opts": ["native.cgroupdriver=cgroupfs"]
}
```



### 总结

​		一个正在运行的 Docker 容器，其实就是一个启用了多个 Linux Namespace 的应用进程，而这个进程能够使用的资源量，则受 Cgroups 配置的限制。

​		这也是容器技术中一个非常重要的概念，即：**容器是一个“单进程”模型。**

​		由于一个容器的本质就是一个进程，用户的应用进程实际上就是容器里 PID=1 的进程，也是其他后续创建的所有进程的父进程。这就意味着，在一个容器中，你没办法同时运行两个不同的应用，除非你能事先找到一个公共的 PID=1 的程序来充当两个不同应用的父进程，这也是为什么很多人都会用 systemd 或者 supervisord 这样的软件来代替应用本身作为容器的启动进程。



### 思考题

> 你是否知道如何修复容器中的 top 指令以及 /proc 文件系统中的信息呢？（提示：lxcfs）

1.在宿主机上安装 lxcfs。具体安装方法可能因你的操作系统而有所不同。例如，在 Ubuntu 上，你可以使用以下命令安装 lxcfs：

```bash
sudo apt-get install lxcfs
```

​    2.在容器中启用 lxcfs。你需要在容器的启动命令中添加挂载 lxcfs 的选项。具体命令可能因你使用的容器运行时而有所不同。以下是一个示例命令，使用 Docker 运行容器并挂载 lxcfs：

```bash
docker run -it --privileged --pid=host -v /var/lib/lxcfs/proc/cpuinfo:/proc/cpuinfo:rw -v /var/lib/lxcfs/proc/diskstats:/proc/diskstats:rw -v /var/lib/lxcfs/proc/meminfo:/proc/meminfo:rw -v /var/lib/lxcfs/proc/stat:/proc/stat:rw -v /var/lib/lxcfs/proc/swaps:/proc/swaps:rw -v /var/lib/lxcfs/proc/uptime:/proc/uptime:rw <容器镜像>
```

这个命令会将 lxcfs 的虚拟 /proc 文件系统挂载到容器的相应目录中。

3.启动容器并运行 top 命令。现在，容器中的 top 命令应该能够正常工作，并且 /proc 文件系统中的信息也应该正确显示。



## 07 白话容器基础（三）：深入理解容器镜像

> rootfs

### mount namespace

示例代码：

```c
#define _GNU_SOURCE
#include <sys/mount.h> 
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <sched.h>
#include <signal.h>
#include <unistd.h>
#define STACK_SIZE (1024 * 1024)
static char container_stack[STACK_SIZE];
char* const container_args[] = {
  "/bin/bash",
  NULL
};

int container_main(void* arg)
{  
  printf("Container - inside the container!\n");
  execv(container_args[0], container_args);
  printf("Something's wrong!\n");
  return 1;
}

int main()
{
  printf("Parent - start a container!\n");
  int container_pid = clone(container_main, container_stack+STACK_SIZE, CLONE_NEWNS | SIGCHLD , NULL);
  waitpid(container_pid, NULL, 0);
  printf("Parent - container stopped!\n");
  return 0;
}

```

我们来一起编译一下这个程序：

```
$ gcc -o ns ns.c$ ./nsParent - start a container!Container - inside the container!
```

这样，我们就进入了这个“容器”当中。可是，如果在“容器”里执行一下 ls 指令的话，我们就会发现一个有趣的现象： /tmp 目录下的内容跟宿主机的内容是一样的。

```
$ ls /tmp# 你会看到好多宿主机的文件
```

也就是说：即使开启了 Mount Namespace，容器进程看到的文件系统也跟宿主机完全一样。

这是怎么回事呢？

仔细思考一下，你会发现这其实并不难理解：**Mount Namespace 修改的，是容器进程对文件系统“挂载点”的认知**。但是，这也就意味着，只有在“挂载”这个操作发生之后，进程的视图才会被改变。而在此之前，新创建的容器会直接继承宿主机的各个挂载点。

这时，你可能已经想到了一个解决办法：创建新进程时，除了声明要启用 Mount Namespace 之外，我们还可以告诉容器进程，有哪些目录需要重新挂载，就比如这个 /tmp 目录。于是，我们在容器进程执行前可以添加一步重新挂载 /tmp 目录的操作：

```c
int container_main(void* arg)
{
  printf("Container - inside the container!\n");
  // 如果你的机器的根目录的挂载类型是shared，那必须先重新挂载根目录
  // mount("", "/", NULL, MS_PRIVATE, "");
  mount("none", "/tmp", "tmpfs", 0, "");
  execv(container_args[0], container_args);
  printf("Something's wrong!\n");
  return 1;
}
```

可以看到，在修改后的代码里，我在容器进程启动之前，加上了一句 mount(“none”, “/tmp”, “tmpfs”, 0, “”) 语句。就这样，我告诉了容器以 tmpfs（内存盘）格式，重新挂载了 /tmp 目录。

我们可以用 mount -l 检查一下：

```
$ mount -l | grep tmpfs
none on /tmp type tmpfs (rw,relatime)
```

可以看到，容器里的 /tmp 目录是以 tmpfs 方式单独挂载的。

**这就是 Mount Namespace 跟其他 Namespace 的使用略有不同的地方：它对容器进程视图的改变，一定是伴随着挂载操作（mount）才能生效。**



### 切换容器根目录

#### rootfs 根文件系统

​		**实际上，Mount Namespace 正是基于对 chroot 的不断改良才被发明出来的，它也是 Linux 操作系统里的第一个 Namespace。**

​		**而这个挂载在容器根目录上、用来为容器进程提供隔离后执行环境的文件系统，就是所谓的“容器镜像”。它还有一个更为专业的名字，叫作：rootfs（根文件系统）。**

​		所以，一个最常见的 rootfs，或者说容器镜像，会包括如下所示的一些目录和文件，比如 /bin，/etc，/proc 等等：

```
$ ls /
bin dev etc home lib lib64 mnt opt proc root run sbin sys tmp usr var
```

​		而你进入容器之后执行的 /bin/bash，就是 /bin 目录下的可执行文件，与宿主机的 /bin/bash 完全不同。

​		现在，你应该可以理解，对 Docker 项目来说，它最核心的原理实际上就是为待创建的用户进程：

1. 启用 Linux Namespace 配置；
2. 设置指定的 Cgroups 参数；
3. 切换进程的根目录（Change Root）。



另外，**需要明确的是，rootfs 只是一个操作系统所包含的文件、配置和目录，并不包括操作系统内核。**

**在 Linux 操作系统中，这两部分是分开存放的，操作系统只有在开机启动时才会加载指定版本的内核镜像。**

所以说，rootfs 只包括了操作系统的“躯壳”，并没有包括操作系统的“灵魂”。

实际上，同一台机器上的所有容器，都共享宿主机操作系统的内核。==>缺陷：宿主机内核对于该机器上的所有容器来说是一个“全局变量”，牵一发而动全身。



#### 容器的一致性

​		**由于 rootfs 的存在，容器才有了一个被反复宣传至今的重要特性：一致性。**它指的是容器的环境、配置、依赖、文件等与打包的本地环境一致。无论在本地、云端，还是在一台任何地方的机器上，用户只需要解压打包好的容器镜像，那么这个应用运行所需要的完整的执行环境就被重现出来了。

​		**由于 rootfs 里打包的不只是应用，而是整个操作系统的文件和目录，也就意味着，应用以及它运行所需要的所有依赖，都被封装在了一起。**



#### 镜像分层--以增量的方式进行修改

Docker 公司在实现 Docker 镜像时并没有沿用以前制作 rootfs 的标准流程，而是做了一个小小的创新：

> Docker 在镜像的设计中，引入了层（layer）的概念。也就是说，用户制作镜像的每一步操作，都会生成一个层，也就是一个增量 rootfs。

当然，这个想法不是凭空臆造出来的，而是用到了一种叫作联合文件系统（Union File System）的能力。

Union File System 也叫 UnionFS，最主要的功能是将多个不同位置的目录联合挂载（union mount）到同一个目录下。

比如，我现在有两个目录 A 和 B，它们分别有两个文件：

```
$ tree
.
├── A
│  ├── a
│  └── x
└── B
  ├── b
  └── x
```

然后，我使用联合挂载的方式，将这两个目录挂载到一个公共的目录 C 上：

```
$ mkdir C
$ mount -t aufs -o dirs=./A:./B none ./C
```

这时，我再查看目录 C 的内容，就能看到目录 A 和 B 下的文件被合并到了一起：

```
$ tree ./C
./C
├── a
├── b
└── x
```

可以看到，在这个合并后的目录 C 里，有 a、b、x 三个文件，并且 x 文件只有一份。这，就是“合并”的含义。此外，如果你在目录 C 里对 a、b、x 文件做修改，这些修改也会在对应的目录 A、B 中生效。

AuFS 的全称是 Another UnionFS，后改名为 Alternative UnionFS，再后来干脆改名叫作 Advance UnionFS，从这些名字中你应该能看出这样两个事实：

1. 它是对 Linux 原生 UnionFS 的重写和改进；
2. 它的作者怨气好像很大。我猜是 Linus Torvalds（Linux 之父）一直不让 AuFS 进入 Linux 内核主干的缘故，所以我们只能在 Ubuntu 和 Debian 这些发行版上使用它。

对于 AuFS 来说，它最关键的目录结构在 /var/lib/docker 路径下的 diff 目录：

```
/var/lib/docker/aufs/diff/<layer_id>
```



镜像分层：

```bash
$ docker image inspect ubuntu:latest
...
     "RootFS": {
      "Type": "layers",
      "Layers": [
        "sha256:f49017d4d5ce9c0f544c...",
        "sha256:8f2b771487e9d6354080...",
        "sha256:ccd4d61916aaa2159429...",
        "sha256:c01d74f99de40e097c73...",
        "sha256:268a067217b5fe78e000..."
      ]
    }

```

可以看到，这个 Ubuntu 镜像，实际上由五个层组成。这五个层就是五个增量 rootfs，每一层都是 Ubuntu 操作系统文件与目录的一部分；而在使用镜像时，Docker 会把这些增量联合挂载在一个统一的挂载点上（等价于前面例子里的“/C”目录）。

这个挂载点就是 /var/lib/docker/aufs/mnt/，比如：

```
/var/lib/docker/aufs/mnt/6e3be5d2ecccae7cc0fcfa2a2f5c89dc21ee30e166be823ceaeba15dce645b3e
```

那么，前面提到的五个镜像层，又是如何被联合挂载成这样一个完整的 Ubuntu 文件系统的呢？

这个信息记录在 AuFS 的系统目录 /sys/fs/aufs 下面。

首先，通过查看 AuFS 的挂载信息，我们可以找到这个目录对应的 AuFS 的内部 ID（也叫：si）：

```bash
$ cat /proc/mounts| grep aufs
none /var/lib/docker/aufs/mnt/6e3be5d2ecccae7cc0fc... aufs rw,relatime,si=972c6d361e6b32ba,dio,dirperm1 0 0
```

然后使用这个 ID，你就可以在 /sys/fs/aufs 下查看被联合挂载在一起的各个层的信息：

```
$ cat /sys/fs/aufs/si_972c6d361e6b32ba/br[0-9]*
/var/lib/docker/aufs/diff/6e3be5d2ecccae7cc...=rw
/var/lib/docker/aufs/diff/6e3be5d2ecccae7cc...-init=ro+wh
/var/lib/docker/aufs/diff/32e8e20064858c0f2...=ro+wh
/var/lib/docker/aufs/diff/2b8858809bce62e62...=ro+wh
/var/lib/docker/aufs/diff/20707dce8efc0d267...=ro+wh
/var/lib/docker/aufs/diff/72b0744e06247c7d0...=ro+wh
/var/lib/docker/aufs/diff/a524a729adadedb90...=ro+wh
```

从这些信息里，我们可以看到，镜像的层都放置在 /var/lib/docker/aufs/diff 目录下，然后被联合挂载在 /var/lib/docker/aufs/mnt 里面。

**而且，从这个结构可以看出来，这个容器的 rootfs 由如下图所示的三部分组成：**

<img src="深入剖析Kubernetes.assets/8a7b5cfabaab2d877a1d4566961edd5f.png" alt="img" style="zoom:50%;" />

**第一部分，只读层。**

```
$ ls /var/lib/docker/aufs/diff/72b0744e06247c7d0...
etc sbin usr var
$ ls /var/lib/docker/aufs/diff/32e8e20064858c0f2...
run
$ ls /var/lib/docker/aufs/diff/a524a729adadedb900...
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
```

这些层，都以增量的方式分别包含了 Ubuntu 操作系统的一部分。



**第二部分，可读写层。**

它是这个容器的 rootfs 最上面的一层（6e3be5d2ecccae7cc），它的挂载方式为：rw，即 read write。在没有写入文件之前，这个目录是空的。而一旦在容器里做了写操作（增、删、改），你修改产生的内容就会以增量的方式出现在这个层中。

为了实现这样的删除操作，AuFS 会在可读写层创建一个 whiteout （白障）文件，把只读层里的文件“遮挡”起来。

比如，你要删除只读层里一个名叫 foo 的文件，那么这个删除操作实际上是在可读写层创建了一个名叫.wh.foo 的文件。这样，当这两个层被联合挂载之后，foo 文件就会被.wh.foo 文件“遮挡”起来，“消失”了。这个功能，就是“ro+wh”的挂载方式，即只读 +whiteout 的含义



**第三部分，Init 层。**

它是一个以“-init”结尾的层，夹在只读层和读写层之间。Init 层是 Docker 项目单独生成的一个内部层，专门用来存放 /etc/hosts、/etc/resolv.conf 等信息。

需要这样一层的原因是，这些文件本来属于只读的 Ubuntu 镜像的一部分，但是用户往往需要在启动容器时写入一些指定的值比如 hostname，所以就需要在可读写层对它们进行修改。（docker commit时不会提交init层的修改）

可是，这些修改往往只对当前的容器有效，我们并不希望执行 docker commit 时，把这些信息连同可读写层一起提交掉。

所以，Docker 做法是，在修改了这些文件之后，以一个单独的层挂载了出来。而用户执行 docker commit 只会提交可读写层，所以是不包含这些内容的。



## 08 白话容器基础（四）：重新认识Docker容器

### docker exec原理

**详细解读了 docker exec 这个操作背后，Linux Namespace 更具体的工作原理**：

1、一个进程的每种 Linux Namespace，都在它对应的 /proc/[进程号]/ns 下有一个对应的虚拟文件，并且链接到一个真实的 Namespace 文件上。

```bash
$ ls -l  /proc/25686/ns
total 0
lrwxrwxrwx 1 root root 0 Aug 13 14:05 cgroup -> cgroup:[4026531835]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 ipc -> ipc:[4026532278]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 mnt -> mnt:[4026532276]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 net -> net:[4026532281]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 pid -> pid:[4026532279]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 pid_for_children -> pid:[4026532279]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 user -> user:[4026531837]
lrwxrwxrwx 1 root root 0 Aug 13 14:05 uts -> uts:[4026532277]

```

**这也就意味着：一个进程，可以选择加入到某个进程已有的 Namespace 当中，从而达到“进入”这个进程所在容器的目的，这正是 docker exec 的实现原理。**

2、setns() 的 Linux 系统调用，可以将一个进程加入到某个进程已有的 Namespace 当中

```c
#define _GNU_SOURCE
#include <fcntl.h>
#include <sched.h>
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>

#define errExit(msg) do { perror(msg); exit(EXIT_FAILURE);} while (0)

int main(int argc, char *argv[]) {
    int fd;
    
    fd = open(argv[1], O_RDONLY);
    if (setns(fd, 0) == -1) {
        errExit("setns");
    }
    execvp(argv[2], &argv[2]); 
    errExit("execvp");
}

```

这段代码功能非常简单：它一共接收两个参数，第一个参数是 argv[1]，即当前进程要加入的 Namespace 文件的路径，比如 /proc/25686/ns/net；而第二个参数，则是你要在这个 Namespace 里运行的进程，比如 /bin/bash。

```bash
$ gcc -o set_ns set_ns.c 
$ ./set_ns /proc/25686/ns/net /bin/bash 
$ ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:02  
          inet addr:172.17.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe11:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
     collisions:0 txqueuelen:0 
          RX bytes:976 (976.0 B)  TX bytes:796 (796.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
    collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

新创建的这个 /bin/bash 进程，由于加入了该容器进程（PID=25686）的 Network Namepace，它看到的网络设备与这个容器里是一样的，即：/bin/bash 进程的网络设备视图，也被修改了（只能看到2个网卡，而不是host的全部网卡）。

在宿主机上，你可以用 ps 指令找到这个 set_ns 程序执行的 /bin/bash 进程，其真实的 PID 是 28499：

```
# 在宿主机上
ps aux | grep /bin/bash
root     28499  0.0  0.0 19944  3612 pts/0    S    14:15   0:00 /bin/bash

```

这时，如果按照前面介绍过的方法，查看一下这个 PID=28499 的进程的 Namespace，你就会发现这样一个事实：

```
$ ls -l /proc/28499/ns/net
lrwxrwxrwx 1 root root 0 Aug 13 14:18 /proc/28499/ns/net -> net:[4026532281]

$ ls -l  /proc/25686/ns/net
lrwxrwxrwx 1 root root 0 Aug 13 14:05 /proc/25686/ns/net -> net:[4026532281]

```

在 /proc/[PID]/ns/net 目录下，这个 PID=28499 进程，与我们前面的 Docker 容器进程（PID=25686）指向的 Network Namespace 文件完全一样。这说明这两个进程，共享了这个名叫 net:[4026532281]的 Network Namespace。



**这种通过操作系统进程相关的知识，逐步剖析 Docker 容器的方法，是理解容器的一个关键思路，希望你一定要掌握。**



### docker commit原理

​		docker commit，实际上就是在容器运行起来后，把最上层的“可读写层”，加上原先容器镜像的只读层，打包组成了一个新的镜像。当然，下面这些只读层在宿主机上是共享的，不会占用额外的空间。

​		而由于使用了联合文件系统，你在容器里对镜像 rootfs 所做的任何修改，都会被操作系统先复制到这个可读写层，然后再修改。这就是所谓的：**Copy-on-Write**。（其实就是增量修改吧？）

​		而正如前所说，Init 层的存在，就是为了避免你执行 docker commit 时，把 Docker 自己对 /etc/hosts 等文件做的修改，也一起提交掉。



### Volume机制

容器技术使用了 rootfs 机制和 Mount Namespace，构建出了一个同宿主机完全隔离开的文件系统环境。这时候，我们就需要考虑这样两个问题：

1. 容器里进程新建的文件，怎么才能让宿主机获取到？
2. 宿主机上的文件和目录，怎么才能让容器里的进程访问到？

==》

**Volume 机制，允许你将宿主机上指定的目录或者文件，挂载到容器里面进行读取和修改操作。**

在 Docker 项目里，它支持两种 Volume 声明方式，可以把宿主机目录挂载进容器的 /test 目录当中：

```bash
$ docker run -v /test ...
$ docker run -v /home:/test ...
```

而这两种声明方式的本质，实际上是相同的：都是把一个宿主机的目录挂载进了容器的 /test 目录。

只不过，在第一种情况下，由于你并没有显示声明宿主机目录，那么 Docker 就会默认在宿主机上创建一个临时目录 /var/lib/docker/volumes/[VOLUME_ID]/_data，然后把它挂载到容器的 /test 目录上。而在第二种情况下，Docker 就直接把宿主机的 /home 目录挂载到容器的 /test 目录上。



> 原理

​		当容器进程被创建之后，尽管开启了 Mount Namespace，但是在它执行 chroot（或者 pivot_root）之前，容器进程一直可以看到宿主机上的整个文件系统。

​		而宿主机上的文件系统，也自然包括了我们要使用的容器镜像。这个镜像的各个层，保存在 /var/lib/docker/aufs/diff 目录下，在容器进程启动后，它们会被联合挂载在 /var/lib/docker/aufs/mnt/ 目录中，这样容器所需的 rootfs 就准备好了。

​		所以，我们只需要在 rootfs 准备好之后，在执行 chroot 之前，把 Volume 指定的宿主机目录（比如 /home 目录），挂载到指定的容器目录（比如 /test 目录）在宿主机上对应的目录（即 /var/lib/docker/aufs/mnt/[可读写层 ID]/test）上，这个 Volume 的挂载工作就完成了。		

​		更重要的是，由于执行这个挂载操作时，“容器进程”已经创建了，也就意味着此时 Mount Namespace 已经开启了。所以，这个挂载事件只在这个容器里可见。你在宿主机上，是看不见容器内部的这个挂载点的。这就**保证了容器的隔离性不会被 Volume 打破**。

PS：这里提到的"容器进程"，是 Docker 创建的一个容器初始化进程 (dockerinit)，而不是应用进程 (ENTRYPOINT + CMD)。dockerinit 会负责完成根目录的准备、挂载设备和目录、配置 hostname 等一系列需要在容器内进行的初始化操作。最后，它通过 execv() 系统调用，让应用进程取代自己，成为容器里的 PID=1 的进程。



> Linux 的**绑定挂载（bind mount）机制**

它的主要作用就是，允许你将一个目录或者文件，而不是整个设备，挂载到一个指定的目录上。并且，这时你在该挂载点上进行的任何操作，只是发生在被挂载的目录或者文件上，而原挂载点的内容则会被隐藏起来且不受影响。

其实，如果你了解 Linux 内核的话，就会明白，绑定挂载实际上是一个 inode 替换的过程。在 Linux 操作系统中，inode 可以理解为存放文件内容的“对象”，而 dentry，也叫目录项，就是访问这个 inode 所使用的“指针”。

<img src="深入剖析Kubernetes.assets/95c957b3c2813bb70eb784b8d1daedc6.png" alt="img" style="zoom:50%;" />



正如上图所示，mount --bind /home /test，会将 /home 挂载到 /test 上。其实相当于将 /test 的 dentry，重定向到了 /home 的 inode。这样当我们修改 /test 目录时，实际修改的是 /home 目录的 inode。这也就是为何，一旦执行 umount 命令，/test 目录原先的内容就会恢复：因为修改真正发生在的，是 /home 目录里。

**所以，在一个正确的时机，进行一次绑定挂载，Docker 就可以成功地将一个宿主机上的目录或文件，不动声色地挂载到容器中。**



那么，这个 /test 目录里的内容，既然挂载在容器 rootfs 的可读写层，它会不会被 docker commit 提交掉呢？

也不会。

这个原因其实我们前面已经提到过。容器的镜像操作，比如 docker commit，都是发生在宿主机空间的。而由于 Mount Namespace 的隔离作用，宿主机并不知道这个绑定挂载的存在。所以，在宿主机看来，容器中可读写层的 /test 目录（/var/lib/docker/aufs/mnt/[可读写层 ID]/test），**始终是空的。**



### 总结

在今天的这次分享中，我用了一个非常经典的 Python 应用作为案例，讲解了 Docke 容器使用的主要场景。熟悉了这些操作，你也就基本上摸清了 Docker 容器的核心功能。

更重要的是，我着重介绍了如何使用 Linux Namespace、Cgroups，以及 rootfs 的知识，对容器进行了一次庖丁解牛似的解读。

借助这种思考问题的方法，最后的 Docker 容器，我们实际上就可以用下面这个“全景图”描述出来：

<img src="深入剖析Kubernetes.assets/3116751445d182687ce496f2825117e5.jpg" alt="img" style="zoom: 33%;" />



这个容器进程`“python app.py”`，运行在由 Linux Namespace 和 Cgroups 构成的隔离环境里；而它运行所需要的各种文件，比如 python，`app.py`，以及整个操作系统文件，则由多个联合挂载在一起的 rootfs 层提供。

这些 rootfs 层的最下层，是来自 Docker 镜像的只读层。

在只读层之上，是 Docker 自己添加的 Init 层，用来存放被临时修改过的 /etc/hosts 等文件。

而 rootfs 的最上层是一个可读写层，它以 Copy-on-Write 的方式存放任何对只读层的修改，容器声明的 Volume 的挂载点，也出现在这一层。



## 09 从容器到容器云：谈谈Kubernetes的本质

> 主题是：从容器到容器云，谈谈 Kubernetes 的本质。

一个“容器”，实际上是一个由 Linux Namespace、Linux Cgroups 和 rootfs 三种技术构建出来的进程的隔离环境。

一个正在运行的 Linux 容器，其实可以被“一分为二”地看待：

1. 一组联合挂载在 /var/lib/docker/aufs/mnt 上的 rootfs，这一部分我们称为**“容器镜像”（Container Image）**，是容器的静态视图；
2. 一个由 Namespace+Cgroups 构成的隔离环境，这一部分我们称为**“容器运行时”（Container Runtime）**，是容器的动态视图。

最具代表性的容器编排工具，当属 Docker 公司的 Compose+Swarm 组合，以及 Google 与 RedHat 公司共同主导的 Kubernetes 项目。

起点： Google 公司在 2015 年 4 月发布的 Borg 论文，Borg 要承担的责任，是承载 Google 公司整个基础设施的核心依赖。

<img src="深入剖析Kubernetes.assets/c7ed0043465bccff2efc1a1257e970bd.png" alt="img" style="zoom: 50%;" />

Borg 和它的继任者 Omega 位于整个技术栈的最底层。

正是由于这样的定位，Borg 可以说是 Google 最不可能开源的一个项目。而幸运的是，得益于 Docker 项目和容器技术的风靡，它却终于得以以另一种方式与开源社区见面，这个方式就是 Kubernetes 项目。

所以，相比于“小打小闹”的 Docker 公司、“旧瓶装新酒”的 Mesos 社区，**Kubernetes 项目从一开始就比较幸运地站上了一个他人难以企及的高度**：在它的成长阶段，这个项目每一个核心特性的提出，几乎都脱胎于 Borg/Omega 系统的设计与经验。



### 全局架构

<img src="深入剖析Kubernetes.assets/8ee9f2fa987eccb490cfaa91c6484f67.png" alt="img" style="zoom:50%;" />

​		Kubernetes 项目的架构，跟它的原型项目 Borg 非常类似，都由 Master 和 Node 两种节点组成，而这两种角色分别对应着控制节点和计算节点。

> master节点

​		其中，控制节点，即 Master 节点，由三个紧密协作的独立组件组合而成，它们分别是负责 API 服务的 kube-apiserver、负责调度的 kube-scheduler，以及负责容器编排的 kube-controller-manager。整个集群的持久化数据，则由 kube-apiserver 处理后保存在 Etcd 中。

​		那么，Borg 对于 Kubernetes 项目的指导作用又体现在哪里呢？答案是，Master 节点。

​		虽然在 Master 节点的实现细节上 Borg 项目与 Kubernetes 项目不尽相同，但它们的出发点却高度一致，即：如何编排、管理、调度用户提交的作业？

​		这些经验最主要的表现就是，**从一开始，Kubernetes 项目就没有像同时期的各种“容器云”项目那样，把 Docker 作为整个架构的核心，而仅仅把它作为最底层的一个容器运行时实现。**==》随时可能干掉docker

​		**Kubernetes 项目最主要的设计思想是，从更宏观的角度，以统一的方式来定义任务之间的各种关系，并且为将来支持更多种类的关系留有余地。**

> node工作节点	

​		计算节点上最核心的部分，则是一个叫作 kubelet 的组件。**在 Kubernetes 项目中，kubelet 主要负责同容器运行时（比如 Docker 项目）打交道**。而这个交互所依赖的，是一个称作 CRI（Container Runtime Interface）的远程调用接口，这个接口定义了容器运行时的各项核心操作，比如：启动一个容器需要的所有参数。

​		而具体的容器运行时，比如 Docker 项目，则一般通过 OCI 这个容器运行时规范同底层的 Linux 操作系统进行交互，即：把 CRI 请求翻译成对 Linux 操作系统的调用（操作 Linux Namespace 和 Cgroups 等）。

​		**此外，kubelet 还通过 gRPC 协议同一个叫作 Device Plugin 的插件进行交互**。这个插件，是 Kubernetes 项目用来管理 GPU 等宿主机物理设备的主要组件，也是基于 Kubernetes 项目进行机器学习训练、高性能作业支持等工作必须关注的功能。

​		而 **kubelet 的另一个重要功能，则是调用网络插件和存储插件为容器配置网络和持久化存储**。这两个插件与 kubelet 进行交互的接口，分别是 CNI（Container Networking Interface）和 CSI（Container **Storage** Interface）。



### 项目核心功能的“全景图”

<img src="深入剖析Kubernetes.assets/16c095d6efb8d8c226ad9b098689f306.png" alt="img" style="zoom: 67%;" />



在 Kubernetes 项目中，我们所推崇的使用方法是：

- 首先，通过一个“编排对象”，比如 Pod、Job、CronJob 等，来描述你试图管理的应用；
- 然后，再为它定义一些“服务对象”，比如 Service、Secret、Horizontal Pod Autoscaler（自动水平扩展器）等。这些对象，会负责具体的平台级功能。

**这种使用方法，就是所谓的“声明式 API”（声明资源的状态）。这种 API 对应的“编排对象”和“服务对象”，都是 Kubernetes 项目中的 API 对象（API Object）。**



### 总结

​		首先，我和你一起回顾了容器的核心知识，说明了容器其实可以分为两个部分：容器运行时和容器镜像。

​		然后，我重点介绍了 Kubernetes 项目的架构，详细讲解了它如何使用“声明式 API”来描述容器化业务和容器间关系的设计思想。

​		实际上，过去很多的集群管理项目（比如 Yarn、Mesos，以及 Swarm）所擅长的，都是把一个容器，按照某种规则，放置在某个最佳节点上运行起来。这种功能，我们称为“**调度**”。

​		而 Kubernetes 项目所擅长的，是按照用户的意愿和整个系统的规则，完全自动化地处理好容器之间的各种关系。**这种功能，就是我们经常听到的一个概念：编排。**

​		所以说，Kubernetes 项目的本质，是为用户提供一个具有普遍意义的容器编排工具。



# Kubernetes集群搭建与实践

## 10 Kubernetes一键部署利器：kubeadm

2017 年，在志愿者的推动下，社区终于发起了一个独立的部署工具，名叫：[kubeadm](https://github.com/kubernetes/kubeadm)。

这个项目的目的，就是要让用户能够通过这样两条指令完成一个 Kubernetes 集群的部署：

```bash
# 创建一个Master节点
$ kubeadm init

# 将一个Node节点加入到当前集群中
$ kubeadm join <Master节点的IP和端口>
```



到目前为止，在容器里运行 kubelet，依然没有很好的解决办法，也不推荐你用容器去部署 Kubernetes 项目。

正因为如此，kubeadm 选择了一种妥协方案：

> 把 kubelet 直接运行在宿主机上，然后使用容器部署其他的 Kubernetes 组件。

所以，你使用 kubeadm 的第一步，是在机器上手动安装 kubeadm、kubelet 和 kubectl 这三个二进制文件。



在 Kubernetes 中，有一种特殊的容器启动方法叫做“Static Pod”。它允许你把要部署的 Pod 的 YAML 文件放在一个指定的目录里。这样，当这台机器上的 kubelet 启动时，它会自动检查这个目录，加载所有的 Pod YAML 文件，然后在这台机器上启动它们。

从这一点也可以看出，kubelet 在 Kubernetes 项目中的地位非常高，在设计上它就是一个完全独立的组件，而其他 Master 组件，则更像是辅助性的系统容器。



最后，我再来回答一下问题：kubeadm 能够用于生产环境吗？

到目前为止（2018 年 9 月），这个问题的答案是：不能。

因为 kubeadm 目前最欠缺的是，一键部署一个高可用的 Kubernetes 集群，即：Etcd、Master 组件都应该是多节点集群，而不是现在这样的单点。这，当然也正是 kubeadm 接下来发展的主要方向。



## 11 从0到1：搭建一个完整的Kubernetes集群

在本次部署中，我准备的机器配置如下：

1. 2 核 CPU、 7.5 GB 内存；
2. 30 GB 磁盘；
3. Ubuntu 16.04；
4. 内网互通；
5. 外网访问权限不受限制。

> 备注：在开始部署前，我推荐你先花几分钟时间，回忆一下 Kubernetes 的架构。

然后，我再和你介绍一下今天实践的目标：

1. 在所有节点上安装 Docker 和 kubeadm；
2. 部署 Kubernetes Master；
3. 部署容器网络插件；
4. 部署 Kubernetes Worker；
5. 部署 Dashboard 可视化插件；
6. 部署容器存储插件。

> 部署网络插件

Kubernetes 支持容器网络插件，使用的是一个名叫 CNI 的通用接口，它也是当前容器网络的事实标准，市面上的所有容器网络开源项目都可以通过 CNI 接入 Kubernetes，比如 Flannel、Calico、Canal、Romana 等等，它们的部署方式也都是类似的“一键部署”。



> 部署容器存储插件

选择部署一个很重要的 Kubernetes 存储插件项目：Rook。

Rook 项目是一个基于 Ceph 的 Kubernetes 存储插件（它后期也在加入对更多存储实现的支持）。不过，不同于对 Ceph 的简单封装，Rook 在自己的实现中加入了水平扩展、迁移、灾难备份、监控等大量的企业级功能，使得这个项目变成了一个完整的、生产级别可用的容器存储插件。

如果你去研究一下 Rook 项目的实现，就会发现它巧妙地依赖了 Kubernetes 提供的编排能力，合理的使用了很多诸如 Operator、CRD 等重要的扩展特性（这些特性我都会在后面的文章中逐一讲解到）。这使得 Rook 项目，成为了目前社区中基于 Kubernetes API 构建的最完善也最成熟的容器存储插件。我相信，这样的发展路线，很快就会得到整个社区的推崇。

> 备注：其实，在很多时候，大家说的所谓“云原生”，就是“Kubernetes 原生”的意思。而像 Rook、Istio 这样的项目，正是贯彻这个思路的典范。在我们后面讲解了声明式 API 之后，相信你对这些项目的设计思想会有更深刻的体会。



## 12 牛刀小试：我的第一个容器化应用

如果你想要快速熟悉 Kubernetes，请按照下面的流程进行练习：

- 首先，在本地通过 Docker 测试代码，制作镜像；
- 然后，选择合适的 Kubernetes API 对象，编写对应 YAML 文件（比如，Pod，Deployment）；
- 最后，在 Kubernetes 上部署这个 YAML 文件。

更重要的是，在部署到 Kubernetes 之后，接下来的所有操作，要么通过 kubectl 来执行，要么通过修改 YAML 文件来实现，**就尽量不要再碰 Docker 的命令行了**。



# 容器编排与Kubernetes作业管理

> Pod和pod控制器

## 13 为什么我们需要Pod？

Pod是 Kubernetes 项目中最小的 API 对象。如果换一个更专业的说法，我们可以这样描述：Pod，是 Kubernetes 项目的原子调度单位。

> 为何需要pod

1、“组”的概念，解决**成组调度（gang scheduling）**问题：Pod 是 Kubernetes 里的原子调度单位。这就意味着，Kubernetes 项目的调度器，是统一按照 Pod 而非容器的资源需求进行计算的。

2、Pod，其实是一组共享了某些资源的容器。具体的说：**Pod 里的所有容器，共享的是同一个 Network Namespace，并且可以声明共享同一个 Volume。**

​		在 Kubernetes 项目里，Pod 的实现需要使用一个中间容器，这个容器叫作 Infra 容器。在这个 Pod 中，Infra 容器永远都是第一个被创建的容器，而其他用户定义的容器，则通过 Join Network Namespace 的方式，与 Infra 容器关联在一起:

<img src="深入剖析Kubernetes.assets/8c016391b4b17923f38547c498e434cf.png" alt="img" style="zoom: 67%;" />

​		在 Kubernetes 项目里，Infra 容器一定要占用极少的资源，所以它使用的是一个非常特殊的镜像，叫作：`k8s.gcr.io/pause`。这个镜像是一个用汇编语言编写的、永远处于“暂停”状态的容器，解压后的大小也只有 100~200 KB 左右。

​		对于 Pod 里的容器 A 和容器 B 来说：

- 它们可以直接使用 localhost 进行通信；
- 它们看到的网络设备跟 Infra 容器看到的完全一样；
- 一个 Pod 只有一个 IP 地址，也就是这个 Pod 的 Network Namespace 对应的 IP 地址；
- 当然，其他的所有网络资源，都是一个 Pod 一份，并且被该 Pod 中的所有容器共享；
- Pod 的生命周期只跟 Infra 容器一致，而与容器 A 和 B 无关。



### pod编排例子

通过pod实现“超亲密关系”容器的设计思想，实际上就是希望，当用户想在一个容器里跑多个功能并不相关的应用时，应该优先考虑它们是不是更应该被描述成一个 Pod 里的多个容器。

**第一个最典型的例子是：WAR 包与 Web 服务器。**

我们现在有一个 Java Web 应用的 WAR 包，它需要被放在 Tomcat 的 webapps 目录下运行起来。

假如，你现在只能用 Docker 来做这件事情，那该如何处理这个组合关系呢？

- 一种方法是，把 WAR 包直接放在 Tomcat 镜像的 webapps 目录下，做成一个新的镜像运行起来。可是，这时候，如果你要更新 WAR 包的内容，或者要升级 Tomcat 镜像，就要重新制作一个新的发布镜像，非常麻烦。
- 另一种方法是，你压根儿不管 WAR 包，永远只发布一个 Tomcat 容器。不过，这个容器的 webapps 目录，就必须声明一个 hostPath 类型的 Volume，从而把宿主机上的 WAR 包挂载进 Tomcat 容器当中运行起来。不过，这样你就必须要解决一个问题，即：如何让每一台宿主机，都预先准备好这个存储有 WAR 包的目录呢？这样来看，你只能独立维护一套分布式存储系统了。

实际上，有了 Pod 之后，这样的问题就很容易解决了。我们可以把 WAR 包和 Tomcat 分别做成镜像，然后把它们作为一个 Pod 里的两个容器“组合”在一起。这个 Pod 的配置文件如下所示：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: javaweb-2
spec:
  initContainers:
  - image: geektime/sample:v2
    name: war
    command: ["cp", "/sample.war", "/app"]
    volumeMounts:
    - mountPath: /app
      name: app-volume
  containers:
  - image: geektime/tomcat:7.0
    name: tomcat
    command: ["sh","-c","/root/apache-tomcat-7.0.42-v2/bin/start.sh"]
    volumeMounts:
    - mountPath: /root/apache-tomcat-7.0.42-v2/webapps
      name: app-volume
    ports:
    - containerPort: 8080
      hostPort: 8001 
  volumes:
  - name: app-volume
    emptyDir: {}

```

​		像这样，我们就用一种“组合”方式，解决了 WAR 包与 Tomcat 容器之间耦合关系的问题。这个所谓的“组合”操作，正是容器设计模式里最常用的一种模式，它的名字叫：**sidecar**。

​		顾名思义，sidecar 指的就是我们可以在一个 Pod 中，启动一个辅助容器，来完成一些独立于主进程（主容器）之外的工作。

​		比如，在我们的这个应用 Pod 中，Tomcat 容器是我们要使用的主容器，而 WAR 包容器的存在，只是为了给它提供一个 WAR 包而已。所以，我们**用 Init Container 的方式优先运行 WAR 包容器，扮演了一个 sidecar 的角色**。



> initContainer容器

在 Pod 中，所有 Init Container 定义的容器，都会比 spec.containers 定义的用户容器先启动。并且，Init Container 容器会按顺序逐一启动，而直到它们都启动并且退出了，用户容器才会启动。



**第二个例子，则是容器的日志收集。**

比如，我现在有一个应用，需要不断地把日志文件输出到容器的 /var/log 目录中。

这时，我就可以把一个 Pod 里的 Volume 挂载到应用容器的 /var/log 目录上。

然后，我在这个 Pod 里同时运行一个 sidecar 容器，它也声明挂载同一个 Volume 到自己的 /var/log 目录上。

这样，接下来 sidecar 容器就只需要做一件事儿，那就是不断地从自己的 /var/log 目录里读取日志文件，转发到 MongoDB 或者 Elasticsearch 中存储起来。这样，一个最基本的日志收集工作就完成了。

跟第一个例子一样，这个例子中的 sidecar 的主要工作也是使用共享的 Volume 来完成对文件的操作。



**第三个例子：istio微服务治理**

Pod 的另一个重要特性是，它的所有容器都共享同一个 Network Namespace。这就使得很多与 Pod 网络相关的配置和管理，也都可以交给 sidecar 完成，而完全无须干涉用户容器。这里最典型的例子莫过于 Istio 这个微服务治理项目了。



### 总结

一个运行在虚拟机里的应用，哪怕再简单，也是被管理在 systemd 或者 supervisord 之下的**一组进程，而不是一个进程**。这跟本地物理机上应用的运行方式其实是一样的。这也是为什么，从物理机到虚拟机之间的应用迁移，往往并不困难。

可是对于容器来说，一个容器永远只能管理一个进程。更确切地说，一个容器，就是一个进程。

你现在可以这么理解 Pod 的本质：

> Pod，实际上是在扮演传统基础设施里“虚拟机”的角色；而容器，则是这个虚拟机里运行的用户程序。



## 14 深入解析Pod对象（一）：基本概念

一个很自然的问题就是：到底哪些属性属于 Pod 对象，而又有哪些属性属于 Container 呢？

**凡是调度、网络、存储，以及安全相关的属性，基本上是 Pod 级别的。**

### 一些属性

> 跟“机器”相关的配置

**NodeSelector：是一个供用户将 Pod 与 Node 进行绑定的字段**

```
apiVersion: v1
kind: Pod
...
spec:
 nodeSelector:
   disktype: ssd
```

这样的一个配置，意味着这个 Pod 永远只能运行在携带了“disktype: ssd”标签（Label）的节点上；否则，它将调度失败。



**NodeName**：一旦 Pod 的这个字段被赋值，Kubernetes 项目就会被认为这个 Pod 已经经过了调度，调度的结果就是赋值的节点名字。所以，这个字段一般由调度器负责设置，但用户也可以设置它来“骗过”调度器，当然这个做法一般是在测试或者调试的时候才会用到。



**HostAliases：定义了 Pod 的 hosts 文件（比如 /etc/hosts）里的内容**，用法如下：

```yaml
apiVersion: v1
kind: Pod
...
spec:
  hostAliases:
  - ip: "10.1.2.3"
    hostnames:
    - "foo.remote"
    - "bar.remote"
...
```

在这个 Pod 的 YAML 文件中，我设置了一组 IP 和 hostname 的数据。这样，这个 Pod 启动后，/etc/hosts 文件的内容将如下所示（追加的方式）：

```
cat /etc/hosts
# Kubernetes-managed hosts file.
127.0.0.1 localhost
...
10.244.0.164    pod-base

# Entries added by HostAliases.
10.1.2.3        foo.remote      bar.remote

```



> **跟容器的 Linux Namespace 相关的属性**

shareProcessNamespace=true： 设置Pod 里的容器要共享 PID Namespace

例子：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  shareProcessNamespace: true
  containers:
  - name: nginx
    image: nginx
  - name: shell
    image: busybox
    stdin: true
    tty: true

```



```bash
$ kubectl create -f nginx.yaml
$ kubectl attach -it nginx -c shell
/ # ps ax
PID   USER     TIME  COMMAND
    1 root      0:00 /pause
    8 root      0:00 nginx: master process nginx -g daemon off;
   14 101       0:00 nginx: worker process
   15 root      0:00 sh
   21 root      0:00 ps ax
```

可以看到，在这个容器里，我们不仅可以看到它本身的 ps ax 指令，还可以看到 nginx 容器的进程，以及 Infra 容器的 /pause 进程。这就意味着，整个 Pod 里的每个容器的进程，对于所有容器来说都是可见的：它们共享了同一个 PID Namespace。



**凡是 Pod 中的容器要共享宿主机的 Namespace，也一定是 Pod 级别的定义**，比如：

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  hostNetwork: true
  hostIPC: true
  hostPID: true
  containers:
  - name: nginx
    image: nginx
  - name: shell
    image: busybox
    stdin: true
    tty: true

```

在这个 Pod 中，我定义了共享宿主机的 Network、IPC（Inter-Process Communication，进程间通信） 和 PID Namespace。这就意味着，这个 Pod 里的所有容器，会直接使用宿主机的网络、直接与宿主机进行 IPC 通信、看到宿主机里正在运行的所有进程。



> Containers属性

Image（镜像）、Command（启动命令）、workingDir（容器的工作目录）、Ports（容器要开发的端口），以及 volumeMounts（容器要挂载的 Volume）都是构成 Kubernetes 项目中 Container 的主要字段。



 **ImagePullPolicy 字段**定义了镜像拉取的策略（Always，Never ，IfNotPresent）



**Lifecycle 字段**。它定义的是 Container Lifecycle Hooks。顾名思义，它是在容器状态发生变化时触发一系列“钩子”。

例子：

```
apiVersion: v1
kind: Pod
metadata:
  name: lifecycle-demo
spec:
  containers:
  - name: lifecycle-demo-container
    image: nginx
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
      preStop:
        exec:
          command: ["/usr/sbin/nginx","-s","quit"]

```



> **Pod 对象在 Kubernetes 中的生命周期**

Pod 生命周期的变化，主要体现在 Pod API 对象的 **Status 部分**，这是它除了 Metadata 和 Spec 之外的第三个重要字段。

其中，pod.status.phase，就是 Pod 的当前状态，它有如下几种可能的情况：

1. Pending。这个状态意味着，Pod 的 YAML 文件已经提交给了 Kubernetes，API 对象已经被创建并保存在 Etcd 当中。但是，这个 Pod 里有些容器因为某种原因而不能被顺利创建。比如，调度不成功。
2. Running。这个状态下，Pod 已经调度成功，跟一个具体的节点绑定。它包含的容器都已经创建成功，并且至少有一个正在运行中。
3. Succeeded。这个状态意味着，Pod 里的所有容器都正常运行完毕，并且已经退出了。这种情况在运行一次性任务时最为常见。
4. Failed。这个状态下，Pod 里至少有一个容器以不正常的状态（非 0 的返回码）退出。这个状态的出现，意味着你得想办法 Debug 这个容器的应用，比如查看 Pod 的 Events 和日志。
5. Unknown。这是一个异常状态，意味着 Pod 的状态不能持续地被 kubelet 汇报给 kube-apiserver，这很有可能是主从节点（Master 和 Kubelet）间的通信出现了问题。

更进一步地，Pod 对象的 Status 字段，还可以再细分出一组 Conditions。这些细分状态的值包括：PodScheduled、Ready（准备好对外提供服务了）、Initialized，以及 Unschedulable。它们主要用于描述造成当前 Status 的具体原因是什么。



### 总结

在今天这篇文章中，我详细讲解了 Pod API 对象，介绍了 Pod 的核心使用方法，并分析了 Pod 和 Container 在字段上的异同。希望这些讲解能够帮你更好地理解和记忆 Pod YAML 中的核心字段，以及这些字段的准确含义。

在学习完这篇文章后，我希望你能仔细阅读 $GOPATH/src/k8s.io/kubernetes/vendor/k8s.io/api/core/v1/types.go 里，

type Pod struct ，尤其是 PodSpec 部分的内容。

争取做到下次看到一个 Pod 的 YAML 文件时，不再需要查阅文档，就能做到把常用字段及其作用信手拈来。

```go
// Pod is a collection of containers that can run on a host. This resource is created
// by clients and scheduled onto hosts.
type Pod struct {
	metav1.TypeMeta `json:",inline"`
	// Standard object's metadata.
	// More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata
	// +optional
	metav1.ObjectMeta `json:"metadata,omitempty" protobuf:"bytes,1,opt,name=metadata"`

	// Specification of the desired behavior of the pod.
	// More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
	// +optional
	Spec PodSpec `json:"spec,omitempty" protobuf:"bytes,2,opt,name=spec"`

	// Most recently observed status of the pod.
	// This data may not be up to date.
	// Populated by the system.
	// Read-only.
	// More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
	// +optional
	Status PodStatus `json:"status,omitempty" protobuf:"bytes,3,opt,name=status"`
}
```



## 15 深入解析Pod对象（二）：使用进阶

### Projected Volume

Projected Volume “投射数据卷”，作用是为容器提供预先定义好的数据。

1. Secret；
2. ConfigMap；
3. Downward API；
4. ServiceAccountToken（特殊的secret，用来保存服务账号的token）



> secret

命令行方式：

```
$ cat ./username.txt
admin
$ cat ./password.txt
c1oudc0w!

$ kubectl create secret generic user --from-file=./username.txt
$ kubectl create secret generic pass --from-file=./password.txt

```

yaml方式：

```
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  user: YWRtaW4=
  pass: MWYyZDFlMmU2N2Rm
```

注意：data的value需要使用base64编码

```
$ echo -n 'admin' | base64
YWRtaW4=
$ echo -n '1f2d1e2e67df' | base64
MWYyZDFlMmU2N2Rm
```



在pod声明volumn使用：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-projected-volume 
spec:
  containers:
  - name: test-secret-volume
    image: busybox
    args:
    - sleep
    - "86400"
    volumeMounts:
    - name: mysql-cred
      mountPath: "/projected-volume"
      readOnly: true
  volumes:
  - name: mysql-cred
    projected:
      sources:
      - secret:
          name: user
      - secret:
          name: pass

```

==》进入容器查看内容

```bash
$ kubectl exec -it test-projected-volume -- /bin/sh
$ ls /projected-volume/
user
pass
$ cat /projected-volume/user
root
$ cat /projected-volume/pass
1f2d1e2e67df

```



> ConfigMap

与secret相似，只是数据不加密



> **Downward API**

作用是：让 Pod 里的容器能够直接获取到这个 Pod API 对象本身的信息

例子：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-downwardapi-volume
  labels:
    zone: us-est-coast
    cluster: test-cluster1
    rack: rack-22
spec:
  containers:
    - name: client-container
      image: k8s.gcr.io/busybox
      command: ["sh", "-c"]
      args:
      - while true; do
          if [[ -e /etc/podinfo/labels ]]; then
            echo -en '\n\n'; cat /etc/podinfo/labels; fi;
          sleep 5;
        done;
      volumeMounts:
        - name: podinfo
          mountPath: /etc/podinfo
          readOnly: false
  volumes:
    - name: podinfo
      projected:
        sources:
        - downwardAPI:
            items:
              - path: "labels"
                fieldRef:
                  fieldPath: metadata.labels

```



内容：

```
$ kubectl create -f dapi-volume.yaml
$ kubectl logs test-downwardapi-volume
cluster="test-cluster1"
rack="rack-22"
zone="us-est-coast"

```



目前，Downward API 支持的字段已经非常丰富了，比如：

```
1. 使用fieldRef可以声明使用:
spec.nodeName - 宿主机名字
status.hostIP - 宿主机IP
metadata.name - Pod的名字
metadata.namespace - Pod的Namespace
status.podIP - Pod的IP
spec.serviceAccountName - Pod的Service Account的名字
metadata.uid - Pod的UID
metadata.labels['<KEY>'] - 指定<KEY>的Label值
metadata.annotations['<KEY>'] - 指定<KEY>的Annotation值
metadata.labels - Pod的所有Label
metadata.annotations - Pod的所有Annotation

2. 使用resourceFieldRef可以声明使用:
容器的CPU limit
容器的CPU request
容器的memory limit
容器的memory request

```

需要注意的是，Downward API 能够获取到的信息，**一定是 Pod 里的容器进程启动之前就能够确定下来的信息**。而如果你想要获取 Pod 容器运行后才会出现的信息，比如，容器进程的 PID，那就肯定不能使用 Downward API 了，而应该考虑在 Pod 里定义一个 sidecar 容器。



> **ServiceAccountToken**

为了方便使用，Kubernetes 已经为你提供了一个默认“服务账户”（default Service Account）。并且，任何一个运行在 Kubernetes 里的 Pod，都可以直接使用这个默认的 Service Account，而无需显示地声明挂载它。

如果你查看一下任意一个运行在 Kubernetes 集群里的 Pod，就会发现，每一个 Pod，都已经自动声明一个类型是 Secret、名为 default-token-xxxx 的 Volume，然后 自动挂载在每个容器的一个固定目录上。

<img src="深入剖析Kubernetes.assets/image-20240221200203769.png" alt="image-20240221200203769" style="zoom:50%;" />

对应的yaml文件：

```
# kubectl get pod nginx-64c59f7fd4-5sr8t  -o yaml
.....
spec:
  containers:
  - image: nginx:1.14-alpine
    xxx
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-rx2jb
      readOnly: true
  volumes:
  - name: kube-api-access-rx2jb
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
```

进入容器查看：

```bash
# kubectl  exec -it nginx-64c59f7fd4-5sr8t /bin/sh
/ # ls /var/run/secrets/kubernetes.io/serviceaccount
ca.crt     namespace  token
/ # cat /var/run/secrets/kubernetes.io/serviceaccount/token 
eyJhbGciOiJSUzxxxxxxx
```



**这种把 Kubernetes 客户端以容器的方式运行在集群里，然后使用 default Service Account 自动授权的方式，被称作“InClusterConfig”，也是我最推荐的进行 Kubernetes API 编程的授权方式。**

当然，考虑到自动挂载默认 ServiceAccountToken 的潜在风险，Kubernetes 允许你设置默认不为 Pod 里的容器自动挂载这个 Volume。



### 容器健康检查和恢复机制

存活钩子：livenessProbe

例子：

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: test-liveness-exec
spec:
  containers:
  - name: liveness
    image: busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5

```



而创建pod 30 s 之后，我们再查看一下 Pod 的 Events：

```
$ kubectl describe pod test-liveness-exec
```

你会发现，这个 Pod 在 Events 报告了一个异常：

```
Events:
  Type     Reason     Age              From               Message
  ----     ------     ----             ----               -------
  Normal   Created    41s              kubelet            Created container liveness
  Normal   Started    41s              kubelet            Started container liveness
  Warning  Unhealthy  1s (x2 over 6s)  kubelet            Liveness probe failed: cat: can't open '/tmp/healthy': No such file or directory
```



我们不妨再次查看一下这个 Pod 的状态：

```bash
$ kubectl get pod test-liveness-exec
NAME           READY     STATUS    RESTARTS   AGE
liveness-exec   1/1       Running   1          1m

```

这时我们发现，Pod 并没有进入 Failed 状态，而是保持了 Running 状态。这是为什么呢？

其实，如果你注意到 **RESTARTS 字段**从 0 到 1 的变化，就明白原因了：这个异常的容器已经被 Kubernetes 重启了。在这个过程中，Pod 保持 Running 状态不变。

需要注意的是：Kubernetes 中并没有 Docker 的 Stop 语义。所以虽然是 Restart（重启），但实际却是重新创建了容器。

这个功能就是 Kubernetes 里的 **Pod 恢复机制**，也叫 **restartPolicy**。它是 Pod 的 Spec 部分的一个标准字段（pod.spec.restartPolicy），默认值是 Always：

- Always：在任何情况下，**只要容器不在运行状态**，就自动重启容器（重建）；
- OnFailure: 只在容器 异常时才自动重启容器；
- Never: 从来不重启容器。

如果你要**关心这个容器退出后的上下文环境**，比如容器退出后的日志、文件和目录，就需要将 restartPolicy 设置为 Never。因为一旦容器被自动重新创建，这些内容就有可能丢失掉了（被垃圾回收了）



记住如下两个基本的设计原理：

1. **只要 Pod 的 restartPolicy 指定的策略允许重启异常的容器（比如：Always），那么这个 Pod 就会保持 Running 状态，并进行容器重启**。否则，Pod 就会进入 Failed 状态 。
2. **对于包含多个容器的 Pod，只有它里面所有的容器都进入异常状态后，Pod 才会进入 Failed 状态**。在此之前，Pod 都是 Running 状态。此时，Pod 的 READY 字段会显示正常容器的个数，比如：

```bash
$ kubectl get pod test-liveness-exec
NAME           READY     STATUS    RESTARTS   AGE
liveness-exec   0/1       Running   1          1m
```



在 Kubernetes 的 Pod 中，还有一个叫 readinessProbe 的字段。虽然它的用法与 livenessProbe 类似，但作用却大不一样。

readinessProbe 检查结果的成功与否，决定的这个 Pod 是不是能被通过 Service 的方式访问到（是否能够对外提供服务），而并不影响 Pod 的生命周期。



>  PodPreset（Pod 预设置）

详见：https://jimmysong.io/kubernetes-handbook/concepts/pod-preset.html

Kubernetes 能不能自动给 Pod 填充某些字段呢？

这个需求实际上非常实用。比如，开发人员只需要提交一个基本的、非常简单的 Pod YAML，Kubernetes 就可以自动给对应的 Pod 对象加上其他必要的信息，比如 labels，annotations，volumes 等等。而这些信息，可以是运维人员事先定义好的。

这么一来，开发人员编写 Pod YAML 的门槛，就被大大降低了。所以，这个叫作 PodPreset（Pod 预设置）的功能 已经出现在了 v1.11 版本的 Kubernetes 中。

举个例子，现在开发人员编写了如下一个 pod.yaml 文件：

```
apiVersion: v1
kind: Pod
metadata:
  name: website
  labels:
    app: website
    role: frontend
spec:
  containers:
    - name: website
      image: nginx
      ports:
        - containerPort: 80

```



运维人员就可以定义一个 PodPreset 对象。在这个对象中，凡是他想在开发人员编写的 Pod 里追加的字段，都可以预先定义好。比如这个 preset.yaml：

```
apiVersion: settings.k8s.io/v1alpha1
kind: PodPreset
metadata:
  name: allow-database
spec:
  selector:
    matchLabels:
      role: frontend
  env:
    - name: DB_PORT
      value: "6379"
  volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
    - name: cache-volume
      emptyDir: {}
```

在这个 PodPreset 的定义中，首先是一个 selector。这就意味着后面这些追加的定义，只会作用于 selector 所定义的、带有“role: frontend”标签的 Pod 对象，这就可以防止“误伤”。

接下来，我们假定运维人员先创建了这个 PodPreset，然后开发人员才创建 Pod：

```
$ kubectl create -f preset.yaml
$ kubectl create -f pod.yaml
```

这时，Pod 运行起来之后，我们查看一下这个 Pod 的 API 对象：

```
$ kubectl get pod website -o yaml
apiVersion: v1
kind: Pod
metadata:
  name: website
  labels:
    app: website
    role: frontend
  annotations:
    podpreset.admission.kubernetes.io/podpreset-allow-database: "resource version"
spec:
  containers:
    - name: website
      image: nginx
      volumeMounts:
        - mountPath: /cache
          name: cache-volume
      ports:
        - containerPort: 80
      env:
        - name: DB_PORT
          value: "6379"
  volumes:
    - name: cache-volume
      emptyDir: {}
```

这个时候，我们就可以清楚地看到，这个 Pod 里多了新添加的 labels、env、volumes 和 volumeMount 的定义，它们的配置跟 PodPreset 的内容一样。此外，这个 Pod 还被自动加上了一个 annotation 表示这个 Pod 对象被 PodPreset 改动过。

需要说明的是，**PodPreset 里定义的内容，只会在 Pod API 对象被创建之前追加在这个对象本身上，而不会影响任何 Pod 的控制器的定义。**

比如，我们现在提交的是一个 nginx-deployment，那么这个 Deployment 对象本身是永远不会被 PodPreset 改变的，被修改的只是这个 Deployment 创建出来的所有 Pod。

这里有一个问题：如果你定义了同时作用于一个 Pod 对象的多个 PodPreset，会发生什么呢？实际上，Kubernetes 项目会帮你**合并（Merge）**这两个 PodPreset 要做的修改。而如果它们要做的修改有冲突的话，这些**冲突字段就不会被修改**。



### 总结

认真体会一下 Kubernetes“一切皆对象”的设计思想：比如应用是 Pod 对象，应用的配置是 ConfigMap 对象，应用要访问的密码则是 Secret 对象。

所以，也就自然而然地有了 PodPreset 这样专门用来对 Pod 进行批量化、自动化修改的工具对象。





## 16 编排其实很简单：谈谈“控制器”模型



前面介绍 Kubernetes 架构的时候，曾经提到过一个叫作 **kube-controller-manager** 的组件。

实际上，这个组件，就是一系列控制器的集合。我们可以查看一下 Kubernetes 项目的 pkg/controller 目录：

```
$ cd kubernetes/pkg/controller/
$ ls -d */              
deployment/             job/                    podautoscaler/          
cloud/                  disruption/             namespace/              
replicaset/             serviceaccount/         volume/
cronjob/                garbagecollector/       nodelifecycle/          replication/            statefulset/            daemon/
...
```

<img src="深入剖析Kubernetes.assets/image-20240223171749147.png" alt="image-20240223171749147" style="zoom:50%;" />



它们都遵循 Kubernetes 项目中的一个通用编排模式，即：控制循环（control loop）。

比如，现在有一种待编排的对象 X，它有一个对应的控制器。那么，我就可以用一段 Go 语言风格的伪代码，为你描述这个**控制循环**：

```go
for {
  实际状态 := 获取集群中对象X的实际状态（Actual State）
  期望状态 := 获取集群中对象X的期望状态（Desired State）
  if 实际状态 == 期望状态{
    什么都不做
  } else {
    执行编排动作，将实际状态调整为期望状态
  }
}
```

**在具体实现中，实际状态往往来自于 Kubernetes 集群本身**。

比如，kubelet 通过心跳汇报的容器状态和节点状态，或者监控系统中保存的应用监控数据，或者控制器主动收集的它自己感兴趣的信息，这些都是常见的实际状态的来源。

**而期望状态，一般来自于用户提交的 YAML 文件**。

比如，Deployment 对象中 Replicas 字段的值。很明显，这些信息往往都保存在 Etcd 中。

接下来，以 Deployment 为例，我和你简单描述一下它对控制器模型的实现：

1. Deployment 控制器从 Etcd 中获取到所有携带了“app: nginx”标签的 Pod，然后统计它们的数量，这就是实际状态；
2. Deployment 对象的 Replicas 字段的值就是期望状态；
3. Deployment 控制器将两个状态做比较，然后根据比较结果，确定是创建 Pod，还是删除已有的 Pod（具体如何操作 Pod 对象，我会在下一篇文章详细介绍）。

可以看到，一个 Kubernetes 对象的主要编排逻辑，实际上是在第三步的“对比”阶段完成的。

这个操作，通常被叫作**调谐（Reconcile）**。这个调谐的过程，则被称作“Reconcile Loop”（调谐循环）或者“Sync Loop”（同步循环）。



我们就可以对 Deployment 以及其他类似的控制器，做一个简单总结了：

<img src="深入剖析Kubernetes.assets/72cc68d82237071898a1d149c8354b26.png" alt="img" style="zoom:50%;" />

如上图所示，**类似 Deployment 这样的一个控制器，实际上都是由上半部分的控制器定义（包括期望状态），加上下半部分的被控制对象的模板组成的。**



在所有 API 对象的 Metadata 里，都有一个字段叫作 ownerReference，用于保存当前这个 API 对象的拥有者（Owner/parent）的信息。

```yaml
# pod =》replicaSet =》deployment
# kubectl get pod deploy-nginx-7f7499b7f6-w7j7g -n dev -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2024-01-26T03:38:29Z"
  generateName: deploy-nginx-7f7499b7f6-
  labels:
    app: nginx
    pod-template-hash: 7f7499b7f6
  name: deploy-nginx-7f7499b7f6-w7j7g
  namespace: dev
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: ReplicaSet
    name: deploy-nginx-7f7499b7f6
    uid: 05bb20df-132e-41b9-a067-e326d2efd1fd
  resourceVersion: "891063"
  uid: 53571663-3349-4f6e-9f42-a731fab7d9ec
  
# kubectl get rs deploy-nginx-7f7499b7f6  -n dev -o yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  annotations:
    deployment.kubernetes.io/desired-replicas: "1"
    deployment.kubernetes.io/max-replicas: "2"
    deployment.kubernetes.io/revision: "2"
  creationTimestamp: "2024-01-26T03:38:29Z"
  generation: 3
  labels:
    app: nginx
    pod-template-hash: 7f7499b7f6
  name: deploy-nginx-7f7499b7f6
  namespace: dev
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: Deployment
    name: deploy-nginx
    uid: 0221fca2-ce9d-4c82-a04b-94012ef5f20a
  resourceVersion: "1364013"
  uid: 05bb20df-132e-41b9-a067-e326d2efd1fd
```



### 总结

在今天这篇文章中，我以 Deployment 为例，和你详细分享了 Kubernetes 项目如何通过一个称作**“控制器模式”（controller pattern）**的设计方法，来统一地实现对各种不同的对象或者资源进行的编排操作。



在后面的讲解中，我还会讲到很多不同类型的容器编排功能，比如 StatefulSet、DaemonSet 等等，它们无一例外地都有这样一个甚至多个控制器的存在，并遵循控制循环（control loop）的流程，完成各自的编排逻辑。

实际上，跟 Deployment 相似，这些控制循环最后的执行结果，要么就是创建、更新一些 Pod（或者其他的 API 对象、资源），要么就是删除一些已经存在的 Pod（或者其他的 API 对象、资源）。

但也正是在这个统一的编排框架下，不同的控制器可以在具体执行过程中，设计不同的业务逻辑，从而达到不同的编排效果。



这个实现思路，正是 Kubernetes 项目进行容器编排的核心原理。在此后讲解 Kubernetes 编排功能的文章中，我都会遵循这个逻辑展开，并且带你逐步领悟控制器模式在不同的容器化作业中的实现方式。

### 思考题

你能否说出，Kubernetes 使用的这个“控制器模式”，跟我们平常所说的“事件驱动”，有什么区别和联系吗？



当谈到 Kubernetes 中的 "控制器模式" 和 "事件驱动" 时，它们实际上是相关但不完全相同的概念。

1. 控制器模式（Controller Pattern）：在 Kubernetes 中，控制器模式是一种用于管理和维护系统状态的设计模式。控制器负责监视集群中的资源对象，并根据定义的规则和策略对这些资源进行操作和调节。控制器可以确保所需的状态和配置与期望的状态保持一致，以实现系统的自动化管理和调整。控制器模式的一个常见示例是 ReplicaSet 控制器，它负责确保指定数量的 Pod 实例在集群中运行。
2. 事件驱动（Event-Driven）：事件驱动是一种编程模型，其中系统的行为和响应是由事件的发生和触发来驱动的。在事件驱动的环境中，组件（或服务）通过订阅和处理事件来实现异步通信和协作。当事件发生时，相关的组件将触发相应的处理逻辑。在 Kubernetes 中，事件驱动的概念通常与事件处理器（Event Handlers）和事件触发器（Event Triggers）相关联，用于在资源状态发生变化时触发相应的操作。

联系和区别：

- 联系：控制器模式和事件驱动都是用于管理和调节系统状态的方法。它们都关注系统中的资源和状态变化，并根据这些变化触发相应的操作。
- 区别：控制器模式更侧重于定义和实现对资源的管理和调节策略，以确保系统状态的一致性和可靠性。而事件驱动更侧重于通过事件的发生和触发来驱动系统的行为和协作，以实现异步通信和处理。

在 Kubernetes 中，控制器模式和事件驱动可以结合使用。例如，Kubernetes 的控制器可以通过监听资源对象的事件来触发相应的操作，以保持系统状态的一致性。这种结合使用可以实现更灵活和自动化的系统管理和调节。



## 17 经典PaaS的记忆：作业副本与水平扩展



**Deployment 控制器实际操纵的，正是这样的 ReplicaSet 对象，而不是 Pod 对象。**

<img src="深入剖析Kubernetes.assets/711c07208358208e91fa7803ebc73058.jpg" alt="img" style="zoom:25%;" />



通过这张图，我们就很清楚地看到，一个定义了 replicas=3 的 Deployment，与它的 ReplicaSet，以及 Pod 的关系，实际上是一种“层层控制”的关系。

其中，ReplicaSet 负责通过“控制器模式”，保证系统中 Pod 的个数永远等于指定的个数（比如，3 个）。这也正是 Deployment 只允许容器的 restartPolicy=Always 的主要原因：只有在容器能保证自己始终是 Running 状态的前提下，ReplicaSet 调整 Pod 的个数才有意义。

而在此基础上，Deployment 同样通过“控制器模式”，来操作 ReplicaSet 的个数和属性，进而实现“水平扩展 / 收缩”和“滚动更新”这两个编排动作。

> “水平扩展 / 收缩”

“水平扩展 / 收缩”非常容易实现，Deployment Controller 只需要修改它所控制的 ReplicaSet 的 Pod 副本个数就可以了（不會重新創建新的rs）。

可以用指令实现：

```bash
$ kubectl scale deployment nginx-deployment --replicas=4
deployment.apps/nginx-deployment scaled
```



> 滚动更新

會重新创建一个rs，然后新增一个pod，旧的rs减少一个pod，交替进行，直至完成滚动更新。

一起分析一个如下所示的 Deployment：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80

```

然后，我们来检查一下 nginx-deployment 创建后的状态信息：

```
$ kubectl get deploymentsNAME              
DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGEnginx-deployment  
3         0         0            0           1s
```

在返回结果中，我们可以看到四个状态字段，它们的含义如下所示。

1. DESIRED：用户期望的 Pod 副本个数（spec.replicas 的值）；
2. CURRENT：当前处于 Running 状态的 Pod 的个数；
3. UP-TO-DATE：当前处于最新版本的 Pod 的个数，所谓最新版本指的是 Pod 的 Spec 部分与 Deployment 里 Pod 模板里定义的完全一致；
4. AVAILABLE：当前已经可用的 Pod 的个数，即：既是 Running 状态，又是最新版本，并且已经处于 Ready（健康检查正确）状态的 Pod 的个数。

可以看到，只有这个 AVAILABLE 字段，描述的才是用户所期望的最终状态。



而 Kubernetes 项目还为我们提供了一条指令，让我们可以实时查看 Deployment 对象的状态变化。这个指令就是 kubectl rollout status：

```bash
$ kubectl rollout status deployment/nginx-deployment
Waiting for rollout to finish: 2 out of 3 new replicas have been updated...
deployment.apps/nginx-deployment successfully rolled out
```

此时，你可以尝试查看一下这个 Deployment 所控制的 ReplicaSet：

```
$ kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-3167673210   3         3         3       20s
```

ReplicaSet 的名字后面随机字符串叫作 pod-template-hash，ReplicaSet 会把这个随机字符串加在它所控制的所有 Pod 的标签里，从而保证这些 Pod 不会与集群里的其他 Pod 混淆。

```yaml
# kubectl get pod nginx-deployment-9d6cbcc65-rgz6b -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2024-02-23T09:43:07Z"
  generateName: nginx-deployment-9d6cbcc65-
  labels:
    app: nginx
    pod-template-hash: 9d6cbcc65
  name: nginx-deployment-9d6cbcc65-rgz6b
```

修改容器的镜像版本触发滚动更新。

```
$ kubectl edit deployment/nginx-deployment
... 
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1 # 1.7.9 -> 1.9.1
        ports:
        - containerPort: 80
...
deployment.extensions/nginx-deployment edited
```

这时，你可以通过查看 Deployment 的 Events，看到这个“滚动更新”的流程：

```
$ kubectl describe deployment nginx-deployment
...
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
...
  Normal  ScalingReplicaSet  24s   deployment-controller  Scaled up replica set nginx-deployment-1764197365 to 1
  Normal  ScalingReplicaSet  22s   deployment-controller  Scaled down replica set nginx-deployment-3167673210 to 2
  Normal  ScalingReplicaSet  22s   deployment-controller  Scaled up replica set nginx-deployment-1764197365 to 2
  Normal  ScalingReplicaSet  19s   deployment-controller  Scaled down replica set nginx-deployment-3167673210 to 1
  Normal  ScalingReplicaSet  19s   deployment-controller  Scaled up replica set nginx-deployment-1764197365 to 3
  Normal  ScalingReplicaSet  14s   deployment-controller  Scaled down replica set nginx-deployment-3167673210 to 0
```

首先，当你修改了 Deployment 里的 Pod 定义之后，Deployment Controller 会使用这个修改后的 Pod 模板，创建一个新的 ReplicaSet（hash=1764197365），这个新的 ReplicaSet 的初始 Pod 副本数是：0。

然后，在 Age=24 s 的位置，Deployment Controller 开始将这个新的 ReplicaSet 所控制的 Pod 副本数从 0 个变成 1 个，即：“水平扩展”出一个副本。

紧接着，在 Age=22 s 的位置，Deployment Controller 又将旧的 ReplicaSet（hash=3167673210）所控制的旧 Pod 副本数减少一个，即：“水平收缩”成两个副本。

如此交替进行，新 ReplicaSet 管理的 Pod 副本数，从 0 个变成 1 个，再变成 2 个，最后变成 3 个。而旧的 ReplicaSet 管理的 Pod 副本数则从 3 个变成 2 个，再变成 1 个，最后变成 0 个。这样，就完成了这一组 Pod 的版本升级过程。

```bash
# kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-7ffd5c8dc9   3         3         3       7m5s
nginx-deployment-9d6cbcc65    0         0         0       17m
```



配置滚动更新策略：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
...
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1  # 最多多创建的pod，默认25%
      maxUnavailable: 1 # 最大的不可用pod，默认25%
```



> 回滚操作

 **kubectl set image**  直接修改 nginx-deployment 所使用的镜像，但把这个镜像名字修改成为了一个错误的名字，比如：nginx:1.91。这样，这个 Deployment 就会出现一个升级失败的版本。

```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.91
deployment.extensions/nginx-deployment image updated
```

我们来检查一下 ReplicaSet 的状态，如下所示：

```
$ kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-1764197365   2         2         2       24s
nginx-deployment-2156724341   2         2         0       7s
```



我们只需要执行一条 kubectl rollout undo 命令，就能把整个 Deployment 回滚到上一个版本：

```bash
[root]# kubectl rollout undo deployment/nginx-deployment
deployment.apps/nginx-deployment rolled back

[root]# kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-7ffd5c8dc9   3         3         3       83m
nginx-deployment-b59f7f787    0         0         0       4m15s
```

更进一步地，如果我想回滚到更早之前的版本，要怎么办呢？

**首先，我需要使用 kubectl rollout history 命令，查看每次 Deployment 变更对应的版本**。而由于我们在创建这个 Deployment 的时候，指定了–record 参数，所以我们创建这些版本时执行的 kubectl 命令，都会被记录下来（spec.revisionHistoryLimit 字段控制保留的历史版本数，默认3）。

```bash
$ kubectl rollout history deployment/nginx-deployment
deployments "nginx-deployment"
REVISION    CHANGE-CAUSE
1           kubectl create -f nginx-deployment.yaml --record
2           kubectl edit deployment/nginx-deployment
3           kubectl set image deployment/nginx-deployment nginx=nginx:1.91

# 查看具体版本信息
$ kubectl rollout history deployment/nginx-deployment --revision=1
deployment.apps/nginx-deployment with revision #1
Pod Template:
  Labels:       app=nginx
        pod-template-hash=9d6cbcc65
  Annotations:  kubernetes.io/change-cause: kubectl create --filename=nginx-deployment.yaml --record=true
  Containers:
   nginx:
    Image:      nginx:1.7.9
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>
```



**然后，我们就可以在 kubectl rollout undo 命令行最后，加上要回滚到的指定版本的版本号，就可以回滚到指定版本了**。这个指令的用法如下：

```
$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
deployment.extensions/nginx-deployment
```



不过，你可能已经想到了一个问题：我们对 Deployment 进行的每一次更新操作，都会生成一个新的 ReplicaSet 对象，是不是有些多余，甚至浪费资源呢？

没错。

所以，Kubernetes 项目还提供了一个指令，**使得我们对 Deployment 的多次更新操作，最后 只生成一个 ReplicaSet**。

具体的做法是，在更新 Deployment 前，你要先执行一条 kubectl rollout pause 指令。它的用法如下所示：

```
$ kubectl rollout pause deployment/nginx-deployment
deployment.extensions/nginx-deployment paused
```

这个 kubectl rollout pause 的作用，是让这个 Deployment 进入了一个“暂停”状态。

所以接下来，你就可以随意使用 kubectl edit 或者 kubectl set image 指令，修改这个 Deployment 的内容了。

由于此时 Deployment 正处于“暂停”状态，所以我们对 Deployment 的所有修改，都不会触发新的“滚动更新”，也不会创建新的 ReplicaSet。

而等到我们对 Deployment 修改操作都完成之后，只需要再执行一条 kubectl rollout resume 指令，就可以把这个 Deployment“恢复”回来，如下所示：

```bash
$ kubectl rollout resume deployment/nginx-deployment
deployment.extensions/nginx-deployment resumed
```

而在这个 kubectl rollout resume 指令执行之前，在 kubectl rollout pause 指令之后的这段时间里，我们对 Deployment 进行的所有修改，最后只会触发一次“滚动更新”。





## 22 撬动离线业务：Job与CronJob

​		Deployment、StatefulSet，以及 DaemonSet 主要编排的对象，都是“在线业务”，即：Long Running Task（长作业）。

​		还有一类作业是“离线业务”，或者叫作 Batch Job（计算业务），这种业务在计算完成后就直接退出了，k8s中的Job和CronJob。

​		早在 Borg 项目（2015 年）中，Google 就已经对作业进行了分类处理，提出了 LRS（Long Running Service）和 Batch Jobs 两种作业形态，对它们进行“分别管理”和“混合调度”。



### Job

#### 例子

例子：

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: resouer/ubuntu-bc 
        command: ["sh", "-c", "echo 'scale=10000; 4*a(1)' | bc -l "]
      restartPolicy: Never
  backoffLimit: 4
```

创建这个 Job并查看：

```bash
$ kubectl create -f job.yaml
$ kubectl describe jobs/pi
Name:             pi
Namespace:        default
Selector:         controller-uid=c2db599a-2c9d-11e6-b324-0209dc45a495
Labels:           controller-uid=c2db599a-2c9d-11e6-b324-0209dc45a495
                  job-name=pi
Annotations:      <none>
Parallelism:      1
Completions:      1
..
Pods Statuses:    0 Running / 1 Succeeded / 0 Failed
Pod Template:
  Labels:       controller-uid=c2db599a-2c9d-11e6-b324-0209dc45a495
                job-name=pi
  Containers:
   ...
  Volumes:              <none>
Events:
  FirstSeen    LastSeen    Count    From            SubobjectPath    Type        Reason            Message
  ---------    --------    -----    ----            -------------    --------    ------            -------
  1m           1m          1        {job-controller }                Normal      SuccessfulCreate  Created pod: pi-rq5rl

```

可以看到，这个 Job 对象在创建后，它的 Pod 模板，被自动加上了一个 controller-uid=< 一个随机字符串 > 这样的 **Label**。而这个 Job 对象本身，则被自动加上了这个 Label 对应的 **Selector**，从而 保证了 Job 与它所管理的 Pod 之间的匹配关系。



我们可以看到这个 Job 创建的 Pod 进入了 Running 状态，这意味着它正在计算 Pi 的值。

```
$ kubectl get pods
NAME                                READY     STATUS    RESTARTS   AGE
pi-rq5rl                            1/1       Running   0          10s
```

而几分钟后计算结束，这个 Pod 就会进入 Completed 状态：

```
$ kubectl get pods
NAME                                READY     STATUS      RESTARTS   AGE
pi-rq5rl                            0/1       Completed   0          4m
```

这也是我们需要在 Pod 模板中定义 restartPolicy=Never 的原因：离线计算的 Pod 永远都不应该被重启，否则它们会再重新计算一遍。

job的template不加Never也会自动设置吧？

事实上，**restartPolicy 在 Job 对象里只允许被设置为 Never 和 OnFailure**；而在 Deployment 对象里，restartPolicy 则只允许被设置为 Always。

此时，我们通过 kubectl logs 查看一下这个 Pod 的日志，就可以看到计算得到的 Pi 值已经被打印了出来：

```BASH
$ kubectl logs pi-rq5rl
3.141592653589793238462643383279...
```



> 一些字段

```
spec:
 backoffLimit: 6
 activeDeadlineSeconds: 100
```

- spec.activeDeadlineSeconds 字段可以设置最长运行时间，一旦运行超时，这个 Job 的所有 Pod 都会被终止，并且，你可以在 Pod 的状态里看到终止的原因是 reason: DeadlineExceeded
- backoffLimit 字段里定义了重试次数，默认值是 6。
- spec.parallelism，它定义的是一个 Job 在任意时间最多可以启动多少个 Pod 同时运行；
- spec.completions，它定义的是 Job 至少要完成的 Pod 数目，即 Job 的最小完成数



并行度例子：

```
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  parallelism: 2
  completions: 4
  template:
    spec:
      containers:
      - name: pi
        image: resouer/ubuntu-bc
        command: ["sh", "-c", "echo 'scale=5000; 4*a(1)' | bc -l "]
      restartPolicy: Never
  backoffLimit: 4
```



```
# kubectl create -f job.yaml
job.batch/pi created

# kubectl get job
NAME   COMPLETIONS   DURATION   AGE
pi     0/4           8s         8s
```

COMPLETIONS == SUCCESSFUL/DESIRED



可以看到，每当有一个 Pod 完成计算进入 Completed 状态时，就会有一个新的 Pod 被自动创建出来，并且快速地从 Pending 状态进入到 ContainerCreating 状态：

```
$ kubectl get pods
NAME       READY     STATUS    RESTARTS   AGE
pi-gmcq5   0/1       Completed   0         40s
pi-84ww8   0/1       Pending   0         0s
pi-5mt88   0/1       Completed   0         41s
pi-62rbt   0/1       Pending   0         0s

$ kubectl get pods
NAME       READY     STATUS    RESTARTS   AGE
pi-gmcq5   0/1       Completed   0         40s
pi-84ww8   0/1       ContainerCreating   0         0s
pi-5mt88   0/1       Completed   0         41s
pi-62rbt   0/1       ContainerCreating   0         0s

```



#### Job Controller 的工作原理

首先，Job Controller 控制的对象，直接就是 Pod。

其次，Job Controller 在控制循环中进行的调谐（Reconcile）操作，是根据实际在 Running 状态 Pod 的数目、已经成功退出的 Pod 的数目，以及 parallelism、completions 参数的值共同计算出在这个周期里，应该创建或者删除的 Pod 数目，然后调用 Kubernetes API 来执行这个操作。



#### 三种常用的 Job 使用方法

**第一种用法，也是最简单粗暴的用法：外部管理器 +Job 模板。**

这种模式的特定用法是：把 Job 的 YAML 文件定义为一个“模板”，然后用一个外部工具控制这些“模板”来生成 Job。（批量生成job的yaml文件）

这时，Job 的定义方式如下所示：

```
apiVersion: batch/v1
kind: Job
metadata:
  name: process-item-$ITEM
  labels:
    jobgroup: jobexample
spec:
  template:
    metadata:
      name: jobexample
      labels:
        jobgroup: jobexample
    spec:
      containers:
      - name: c
        image: busybox
        command: ["sh", "-c", "echo Processing item $ITEM && sleep 5"]
      restartPolicy: Never

```

可以看到，我们在这个 Job 的 YAML 里，定义了 $ITEM 这样的“变量”。

所以，在控制这种 Job 时，我们只要注意如下两个方面即可：

1. 创建 Job 时，替换掉 $ITEM 这样的变量；
2. 所有来自于同一个模板的 Job，都有一个 jobgroup: jobexample 标签，也就是说这一组 Job 使用这样一个相同的标识。

而做到第一点非常简单。比如，你可以通过这样一句 shell 把 $ITEM 替换掉：

```
$ mkdir ./jobs
$ for i in apple banana cherry
do
  cat job-tmpl.yaml | sed "s/\$ITEM/$i/" > ./jobs/job-$i.yaml
done

```

这样，一组来自于同一个模板的不同 Job 的 yaml 就生成了。接下来，你就可以通过一句 kubectl create 指令创建这些 Job 了：

```
$ kubectl create -f ./jobs
$ kubectl get pods -l jobgroup=jobexample
NAME                        READY     STATUS      RESTARTS   AGE
process-item-apple-kixwv    0/1       Completed   0          4m
process-item-banana-wrsf7   0/1       Completed   0          4m
process-item-cherry-dnfu9   0/1       Completed   0          4m
```

这个模式看起来虽然很“傻”，但却是 Kubernetes 社区里使用 Job 的一个很普遍的模式。

原因很简单：大多数用户在需要管理 Batch Job 的时候，都已经有了一套自己的方案，需要做的往往就是**集成工作**。这时候，Kubernetes 项目对这些方案来说最有价值的，就是 Job 这个 API 对象。所以，你只需要编写一个外部工具（等同于我们这里的 for 循环）来管理这些 Job 即可。

这种模式最典型的应用，就是 TensorFlow 社区的 **KubeFlow 项目**。

很容易理解，在这种模式下使用 Job 对象，completions 和 parallelism 这两个字段都应该使用默认值 1，而不应该由我们自行设置。而作业 Pod 的并行控制，应该完全交由外部工具来进行管理（比如，KubeFlow）。



**第二种用法：拥有固定任务数目的并行 Job**。

这种模式下，我只关心最后是否有指定数目（spec.completions）个任务成功退出。至于执行时的并行度是多少，我并不关心。

比如，我们这个计算 Pi 值的例子，就是这样一个典型的、拥有固定任务数目（completions=4）的应用场景。 它的 parallelism 值是 2；或者，你可以干脆不指定 parallelism，直接使用默认的并行度（即：1）。



**第三种用法，也是很常用的一个用法：指定并行度（parallelism），但不设置固定的 completions 的值。**

这时候，Job 的定义基本上没变化，只不过是不再需要定义 completions 的值了而已

```
apiVersion: batch/v1
kind: Job
metadata:
  name: job-wq-2
spec:
  parallelism: 2
  template:
    metadata:
      name: job-wq-2
    spec:
      containers:
      - name: c
        image: gcr.io/myproject/job-wq-2
        env:
        - name: BROKER_URL
          value: amqp://guest:guest@rabbitmq-service:5672
        - name: QUEUE
          value: job2
      restartPolicy: OnFailure
```



在实际场景里，要么干脆就用第一种用法来自己管理作业；要么，这些任务 Pod 之间的关系就不那么“单纯”，甚至还是“有状态应用”（比如，任务的输入 / 输出是在持久化数据卷里）。在这种情况下，我在后面要重点讲解的 Operator，加上 Job 对象一起，可能才能更好地满足实际离线任务的编排需求。



### CronJob

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

在这个 YAML 文件中，最重要的关键词就是 **jobTemplate**。看到它，你一定恍然大悟，原来 CronJob 是一个 Job 对象的控制器（Controller）！

一些字段：

> concurrencyPolicy

需要注意的是，由于定时任务的特殊性，很可能某个 Job 还没有执行完，另外一个新 Job 就产生了。这时候，你可以通过 spec.concurrencyPolicy 字段来定义具体的处理策略。比如：

1. concurrencyPolicy=Allow，这也是默认情况，这意味着这些 Job 可以同时存在；
2. concurrencyPolicy=Forbid，这意味着不会创建新的 Pod，该创建周期被跳过；
3. concurrencyPolicy=Replace，这意味着新产生的 Job 会替换旧的、没有执行完的 Job。



> spec.startingDeadlineSeconds

而如果某一次 Job 创建失败，这次创建就会被标记为“miss”。当在指定的时间窗口内，miss 的数目达到 100 时，那么 CronJob 会停止再创建这个 Job。

这个时间窗口，可以由 spec.startingDeadlineSeconds 字段指定。比如 startingDeadlineSeconds=200，意味着在过去 200 s 里，如果 miss 的数目达到了 100 次，那么这个 Job 就不会被创建执行了。





### 思考題

根据 Job 控制器的工作原理，如果你定义的 parallelism 比 completions 还大的话，比如：

```
 parallelism: 4
 completions: 2
```

那么，这个 Job 最开始创建的时候，会同时启动几个 Pod 呢？原因是什么？



答：

根据 Job 控制器的工作原理，如果你定义的 `parallelism`（并行度）比 `completions`（完成数）大，例如 `parallelism: 4` 和 `completions: 2`，那么在 Job 最开始创建时，会同时启动 4 个 Pod。

这是因为 `parallelism` 定义了 Job 同时运行的 Pod 的最大数量。而 `completions` 定义了 Job 完成所需的 Pod 的数量。当 Job 创建时，它会尽可能地启动 `parallelism` 个 Pod，以便加快任务的执行速度。

在这种情况下，Job 会启动 4 个 Pod，这些 Pod 将并行地执行任务。然而，由于 `completions` 设置为 2，Job 只有在成功完成 2 个 Pod 后才会被标记为完成。这意味着即使有 4 个 Pod 启动，只有前两个成功完成的 Pod 会被计算在 `completions` 中。

这种设置可以用于实现一些特定的需求，例如在处理大量任务时，通过并行执行多个 Pod 来加快任务的处理速度，并且只需要满足一定数量的成功完成 Pod 来满足 Job 的完成条件。



























# Kubernetes容器持久化存储

> PV和PVC



# Kubernetes容器网络

> service



# Kubernetes作业调度与资源管理

> shedule



# Kubernetes容器运行时

> CRI



# Kubernetes容器监控与日志

> Prometheus、日志



# 再谈开源与社区
