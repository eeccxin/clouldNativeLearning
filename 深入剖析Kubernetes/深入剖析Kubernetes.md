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
