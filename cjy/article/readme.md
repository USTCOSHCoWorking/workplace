# 重要性 前瞻性

包分发与包管理器，可以说是一个非常伟大的发明。曾经，当人们想安装软件时，要么去找编译好的二进制包，要么自己下载源码进行编译。然而，前者数量极少，后者动辄1小时的编译时间和可能出现的各种错误令普通用户望而却步，加之版本与依赖的混乱，安装需要的软件成了一件非常耗时的事情。

直到第一个包管理器的出现改变了这一切。

```
部分包管理器及平台
apt-get Ubuntu
yum CentOS
pip python
npm javascript
...
```

聪明的开发者们开始收集稳定且常用的软件，以特定规范打包放在一个服务器上，安装流程和依赖库自然都附带在其中，任何一台安装有包管理器的主机都可以从服务器获取这些软件并自动安装到本机，整个流程基本不需要人去干预，也很少会出错误。

> wiki 对于包管理器功能的[介绍](https://en.wikipedia.org/wiki/Package_manager)
>
> - Working with [file archivers](https://en.wikipedia.org/wiki/File_archiver) to extract package archives
> - Ensuring the integrity and authenticity of the package by verifying their [digital certificates](https://en.wikipedia.org/wiki/Digital_certificate) and [checksums](https://en.wikipedia.org/wiki/Checksum)
> - Looking up, downloading, installing or updating existing software from a [software repository](https://en.wikipedia.org/wiki/Software_repository) or [app store](https://en.wikipedia.org/wiki/App_store)
> - Grouping packages by function to reduce user confusion
> - Managing dependencies to ensure a package is installed with all packages it requires, thus avoiding "[dependency hell](https://en.wikipedia.org/wiki/Dependency_hell)"

随着包管理器开始流行，这种一键式安装方式收到大家的追捧。单一的主服务器已经开始难以满足越来越多的下载需求，镜像服务器随之诞生。

镜像服务器一般都会定时与主服务器进行同步以保证软件最新，这样做的好处是明显的：主服务器的压力减轻了。人们开始从离自己比较近的镜像服务器获取软件包

在单个服务器带宽日益成为瓶颈的今天，同一地区数量不多的镜像服务器已经越来越难给所有用户提供稳定高速及时的同步更新。在默认服务器出现故障时，修改软件源服务器这样的操作，对于普通用户也并非很便捷。除此以外，镜像服务器之间上游源和下游源之间同步的时间差，有时也会造成一定影响。

当前，面临这种情况，我们的解决方法是设立更多的镜像服务器，号召更多大学和机构的志愿者投入人力去维护。然而，当服务器增多时，新的问题出现了：负载难以均匀

```
中国国内的部分镜像源
mirrors.ustc.edu.cn
mirrors.aliyun.com
mirrors.tuna.tsinghua.com
...
```

人们都有趋利避害的本能，然而每个镜像服务器性能是不同的，无论是从带宽还是从可容纳的最大并发量来说。不难理解，人们会聚集到下载速度最快的镜像服务器。而那些比较慢的镜像服务器则会无人问津。当大家都聚集在少数服务器时，最初的问题再次发生了。由此可见，【设立更多的镜像服务器】只能缓解下载压力，却并不能彻底解决这样的问题。

也就是说，我们需要一个分布式，相对公平，动态调整的全新的包分发机制，最好还要是能自动同步，不需要人力去维护，而且能在少数节点出现故障时不影响整个网络的工作。

在这种情形下，一个基于区块链技术的分布式包分发网络将能起到不小的作用。

我们期望，通过这个项目，能够构建一个至少由几千个节点构建的包分发网络。所有的软件包，被分散储存在各个节点上。而每个节点，都能迅速地从其他节点获取软件包。当一次下载请求成功或失败后，请求节点会给来源节点打分，分数则作为激励，决定在网络拥塞的时候，哪些节点能获得更高的速度。那些比较稳定的节点会变成一级服务器给其他节点提供索引。对于每一个软件包，它会被储存多少份完全由它的热度与重要性决定。

同步是由节点完成的，当官方源发生更新时，新的软件包在被确认后会加入这个网络，并被部分节点储存。

而最有趣的地方是，这一切将完全由网络本身计算出来并且动态调整。设定好规则之后，它不需要人力去维护，也不可能被摧毁（只要按照规则运行的节点还有一定数量）。当然，对于伪造的节点和中间人攻击，这个网络也应有足够的健壮性来判断和驳回虚假的更新请求。

linux与开源界的核心哲学是"自由"，与其依赖一个传统的镜像源，使用分布式的P2P分发网络想必更加能贯彻"人人为我，我为人人"的自由思路。