
# 定义

这些年的教育导致凡事学个啥都想着先找个定义，云计算的定义相信这些年随着时间和技术的变革，不停的在发生着变化，我试着找了找定义：
## 云计算
> “云计算的本质是一种服务提供模型，通过这种模型可以随时、随地、按需地通过网络访问共享资源池的资源，这个资源池的内容包括计算资源、网络资源、存储资源等，这些资源能够被动态地分配和调整，在不同用户之间灵活的划分。凡是符合这些特征的IT服务都可以成为云计算服务。  
> ——Security Guidance for Critical Areas of Focus In Cloud Computing V3.0”

NIST(U.S. National Institute of Standards and Technology)提出了一个定义云计算的标准“NIST Working Definition of Cloud Computing/NIST 800-145”，这个标准中提出云计算具备的五个基本要素：**通过网络分发服务、自助服务、可衡量的服务、资源的灵活调度、资源池化**。另外，这个标准还提到，云计算按照服务类型可以分为 **IaaS、PaaS、SaaS** 三类，按照部署模式分为 **公有云、私有云、混合云和社区云** 四种。  

![IaaS-PaaS-SaaS](/images/IaaS-PaaS-SaaS.PNG)

### IaaS (Infrastructure as a service) 

> Infrastructure as a service (IaaS)  is a cloud computing offering in which a vendor provides users access to computing resources such as servers, storage and networking. Organizations use their own platforms and applications within a service provider’s infrastructure.  
Examples: DigitalOcean, Linode, Rackspace, Amazon Web Services (AWS), Microsoft Azure, Google Compute Engine (GCE), 阿里云，腾讯云

IaaS 要解决什么问题？   
1. 用户不用购买硬件设备了，直接按需购买资源就行了。  
2. 用户可以按需扩容缩容自己所需要的资源。 (业务忙时多申请几个机子，业务不忙了少几个机子，反正时按资源收费的。)  
3. 不用安排专人维护硬件设备了，也不会有什么机房断电，设备单点故障了影响业务。  
4. 用起来让你感觉跟物理机没啥区别。   

### PaaS (Platform as a Service)  

> Platform as a service (PaaS) is a cloud computing offering that provides users with a cloud environment in which they can develop, manage and deliver applications. In addition to storage and other computing resources, users are able to use a suite of prebuilt tools to develop, customize and test their own applications.  
Examples: AWS Elastic Beanstalk, Windows Azure, Heroku, Force.com, Google App Engine, Apache Stratos, OpenShift

PaaS 要解决什么问题？  
云上有了资源（虚拟机）后，提供给用户一套开发，测试，打包，发布，管理和运维的平台，让你很方便的在IaaS上搞你的业务，不用care底层资源 。

### SaaS (Software as a Service)  

> Software as a service (SaaS)  is a cloud computing offering that provides users with access to a vendor’s cloud-based software. Users do not install applications on their local devices. Instead, the applications reside on a remote cloud network accessed through the web or an API. Through the application, users can store and analyze data and collaborate on projects.  
> Examples: Google Apps, Dropbox, Salesforce, Cisco WebEx, Concur, GoToMeeting

SaaS 要解决什么问题？  
这个我也没接触过，理解就是所有软件都在云上了，你只要类似打开浏览器的东东就可以用各种软件，不用再安装了，你的数据啥的都在云上了。

### 参考:    
> https://www.ibm.com/cloud/learn/iaas-paas-saas  
> https://www.computenext.com/blog/when-to-use-saas-paas-and-iaas/  
> https://en.wikipedia.org/wiki/Platform_as_a_service  
> https://www.bmc.com/blogs/saas-vs-paas-vs-iaas-whats-the-difference-and-how-to-choose/

# 历史  

## Docker 的出现
2013年那会云计算不再时虚无缥缈的概念了，已经商业化比较成熟了，AWS如日中天，Openstack火的一塌糊涂，以Cloud Foundry为代表的开源PaaS项目度过了最艰难的的概念普及和用户教育阶段，吸引力一堆知名技术厂商的投入，开始了以开源PaaS为核心构建平台层服务能力的变革，PaaS的时代来临了。   

> Cloud Foundry is an open source, multi cloud application platform as a service (PaaS) governed by the Cloud Foundry Foundation, a 501(c)(6) organization.The software was originally developed by VMware and then transferred to Pivotal Software, a joint venture by EMC, VMware and General Electric.  
> -- https://en.wikipedia.org/wiki/Cloud_Foundry  

Docker公司那会还叫dotCloud也是PaaS热潮的中一个小公司，主打产品与主流的Cloud Foundry社区脱节，长期无人问津。  
* 2013年3月，dotCloud公司推出自己的容器开源项目Docker。  
* 2013年10月，dotCloud公司正式转换业务核心并将自身重新定名为Docker。到这时，Docker已经拥有超过200名贡献者，其中九成以上来自公司之外。Docker的下载量超过10万次，包括eBay在内的众多企业开始对其加以利用，相关社区也在全球范围内快速建立。短短几个月Cloud Foundry和其他PaaS社区还没来得及成为它的对手就已经出局了。  dotCloud公司改名为Docker公司。  


## 遇到和解决了什么问题

### Cloud Foundry的PaaS解决了什么问题  

当时大家租赁AWS或者Openstack的虚机机，还是像以前管理物理机那样，通过脚本或者手动的方式在虚拟机上部署应用。但是云上的资源环境和物理机还是不一致的，当时的云计算服务比的是谁能更好的模拟本地服务器环境，能带来更好的“云上”体验。  

PaaS的出现就是解决这个问题的一个最佳方案。举个例子，虚拟机建好后，运维人员只需在这些机子上部署一个Cloud Foundry项目，然后执行一条命令就可以把本地的应用部署到云上。

```
cf push "我的应用"
```

Cloud Foundry这样的PaaS项目最核心的组件就是打包和分发机制。为每种语言都定义一种打包机制，push时相当于把可执行文件和启动脚本打包到一个压缩包里面，上传到云上的存储种，然后调度一个可用的虚机，这个虚机的agent下载应用启动运行。  

由于同一个虚机会运行不同的应用，Cloud Foundry调用系统的Cgroups和Namespace机制为每一个应用单独创建一个叫“沙盒”的隔离环境，在“沙盒”里启动这些应用进程，实现一个虚机里面同时运行不同的应用，互不干涉。沙盒也就是后来的容器。  

### Docker解决了什么问题  

Docker发布后，技术上跟Cloud Foundry的沙盒没有啥本质区别，但是最大不同就是容器的镜像。  

问题出在Cloud Foundry的一键部署很方便，但是打包却比较麻烦，一旦用上PaaS，用户必须为每一种语言，框架，甚至版本维护一给打好的包，而这个打包过程，除了可执行文件和启动脚本外，需要修改好多配置才能跑起来，这个修改没有经验可用借鉴，全凭试错尝试。  

Docker 镜像恰巧解决了这个问题。这个镜像就是一个压缩包，直接由一整套完整的操作系统的文件和目录组成的，有应用所需要的完整的依赖环境，内容可用和测试环境完全一样，所有不需要任何配置和修改直接保证了本地环境和云端环境的高读一致。拿着这个压缩包，使用某种技术创建一个“沙盒”环境，在沙盒里面解压缩，运行程序就可以了。**这就是Docker的精髓。**  

PaaS最核心的打包系统一下子无用武之地了，抓狂的打包过程消失了。  

提供一个下载好的操作系统文件和目录，制作一个压缩包
```
docker build "镜像"
```

Docker创建一个“沙盒”，解压压缩包，运行自己的应用。这个沙盒也是通过Cgroups和Namespace技术来实现环境隔离的。
```
docker run "镜像"
```
### Docker公司是怎么把Docker搞火的

Docker公司的重要战略是“坚持把开发者群体放在至高无上的位置”，所以一开始Docker的推广是以开发者为主导的，简单的UI，有趣的demo，无论是你懂不懂后端，很简单就可以发布自己的应用，PaaS的受益者和最终用户，肯定都是开发者。  
Docker只是个开源项目的名称，dotCloud公司将自己的公司名字改为了Docker，鲸鱼的Logo也成为了商业商标。  

### Docker为啥发布Swarm项目

Docker项目的出现，让PaaS的定义由之前的CLoud Foundry描述的那样变成了一个由Docker镜像为标准的全新的概念。   

Docker虽然解决了打包的问题，还不能叫做PaaS，因为PaaS的另一个重要功能是大规模部署。Docker在2014年的DockerCon上推出了自己的容器集群管理项目Swarm，预示着Docker公司想重新定义PaaS的愿望。

Docker的快速崛起后，CoreOS公司快速将容器融入自己的PaaS解决方案中，是当时DOcker项目的第二重要力量，但是随着Docker公司战略和对Docker项目的定位的改变，Docker公司想提供更多平台层的能力，向PaaS项目发展。显然和CoreOS的核心产品和战略冲突，2014年底CoreOS退出Docker项目，发布了自己Rocket(rkt)容器。  

CoreOS是一系列开源项目的组合，包括Container Linux操作系统，Fleet作业调度工具，systemd进程管理，rkt容器。Swarm则是以一个完整的整体来对外管理集群，最大亮点是完全使用Docker项目的API来完成管理集群，比如：
```
#单机Docker项目：
docker run “我的镜像”

#集群Docker项目：
docker run -H “Swarm集群IP” “我的镜像”
```

### 编排概念的出现
Docker的崛起，2014-2015年催生了一个繁荣的Docker生态，Docker收购了Fig项目，后面改名为Compose项目。  

编排在云计算领域是指通过工具或者配置来完成一组虚拟机以及相关资源的定义，配置，创建，删除等工作。  

Fig项目首次提出了容器编排概念“Container Orchestration”。通过执行一条简单命令，将一个配置文件里面定义的不同的容器，按照他们的指定关联关系创建起来。  

### Mesos的转型
Mesos昨晚Berkeley主导的大数据套件之一，是当时大数据最受欢迎的资源管理项目，跟Yarn项目杀的难舍难分。大数据项目关注的是计算密集型离线业务，对应用打包和集群扩容托管没啥强烈需求，Hadoop，Spark等项目到现在也没有在容器上投入更大赌注。  

 Mesos的两层调度系统，天然可以支持PaaS业务，Mesos+Marathon项目很快成了Docker Swarm的有力竞争对手。Mesos拥有超大规模集群管理的经验，也有大规模生产环境在使用了，比如eBa，。Marathon提供了应用托管和负载均功能。Mesos公司提出了DC/OS的口号和产品，旨在使用户能够像管理一台机器一样管理一个万级别的物理集群，并且使用Docker容器在这个集群中自由部署应用。  

### Kubernetes的诞生
 这个时候CoreOS完全被Docker压制，RedHat作为Docker项目早期重要贡献者也因为Docker公司的平台战略不满退出，OpenSift还勉强支撑，Mesos和Swarm是主要竞争对手。2014年6月Google发力，发布了Kubernetes项目，这个项目不仅挽救了CoreOS和RedHat，同时改变了整个容器市场的格局。  
  
  2014-2015年，整个容器社区热闹非凡，大量围绕Docker项目的网络，存储，监控，CI/CD，UI项目纷纷出台，也涌现出了Rancher，Tutun开源和商业上都取得成功的创业公司。Docker公司发布了Compose，Swarm和Machine三件套，Docker公司想从开源成功走向商业成功。  

  Google的容器项目也招架住Docker，Google本想提议关停自己容器项目，和Docker共同推出了一个中立的容器运行时库（container runtime）作为Docker项目的核心依赖。DOcker没有同意削弱自己地位的建议，推出了自己的容器运行时库Libcontainer。由于比较匆忙，代码可读性差，可维护不强，被社区长期诟病。

  2015年6月，各个玩家开始切割Docker项目的话语权，手段也很经典。由Docker牵头，CoreOS，Google，RedHat等公司宣布，Docker将LibContainer捐出，改名RunC项目，交由一个完全中立的基金会管理，然后以RunC为依据，共同指定一套容器和镜像的标准和规范。  
  
  这套标准规范就是OCI （open Container Initiative）。提出将容器运行和镜像的实现从Dokcer项目完全剥离出来，一方面改善了Docker在这一块一家独大的现状，另一方面各个玩家可以不依赖Docker项目构建各自的平台能力。这是一群玩家根据各自利益干涉的一个妥协结果。Docker公司虽然时OCI的发起者和创始成员，但是很少在标准指定上扮演关键角色，也没有动力去推进这些所谓的标准，这就是为啥OCI组织效率持续低下的根本原因。

  Docker不担心OCI的威胁，是因为Docker项目的容器生态的事实标准，社区足够庞大。但是斗争转移到容器之上的平台层，即PaaS，Docker公司就没有多大优势了。这个领域Google和RedHat有着深厚的技术积累，CoreOS这样的创业公司也有像Etcd这样的开源基础设施项目，Docker只有一个Swarm。

  Google，RedHat等开源基础设施玩家，共同牵头发起了一个CNCF（Cloud Native Computing Foundation）的基金会。目的就是希望以Kubernetes项目为基础，建立一个由开源基础设施厂商主导的，按照独立基金会运行的平台级社区，对抗Docker公司为核心的容器商业生态。  
  
  Kubernetes的竞争对手为Swarm和Mesos，Swarm擅长和Docker生态无缝集成，Mesos擅长大规模集群的调度和管理。Kubernetes另开辟径，将Borg和Omega系统的内部特性落到Kubernetes项目上，就是Pod，Sidecar等超前的功能和设计模式。这些都是Google公司在这个领域多年的经验积累沉淀。RedHat和Google达成联盟，为这个项目投入了很多贡献，保证了自己的影响力。  
  
  Mesos的Apache社区比较封闭，虽然成熟，但是缺乏创新，Swarm虽然强调Docker Native，但是杀伤力不大。Kubernetes项目耳目一新的设计理念和号召力，构建出了一个与众不同的容器编排和管理生态，迅速崛起，Github社区各项指标一骑绝尘，Swarm远远被甩身后。  
  
  CNCF社区添加了像Prometheus，Fluentd，CNI等一系列容器知名生态工具和项目，大量的公司和创业团队开始专门针对CNCF社区而非Docker公司制定推广策略。  
  
  Docker为了应对竞争，2016年宣布放弃Swarm项目，将容器编排和集群管理功能内置到Docker项目中，虽然可以使得Docker项目的边界扩大到一个完整的PaaS项目范畴，但是增加了技术复杂度和维护难度。   

  Kubernetes反其道行之，在社区推进“民主化”架构，从API到容器运行时的每一层，都为开发者提供可扩展的插件机制，鼓励通过代码方式介入到Kubernetes的每一个阶段。催生了大量基于Kubernetes API和扩展接口的二次创业工作，社区在2016年之后得到了空前的发展。不同于之前局限于打包发布的PaaS化路线，这一次是围绕Kubernetes项目为核心的百花争鸣。Docker公司不得不面对这次豪赌的失败，开始放弃开源社区，专注于自己的商业化转型。   
  
  2017年开始，Docker公司将DOcker项目的容器运行时部分Containerd捐赠给了CNCF社区，标志着Docker项目完全升级为一个PaaS平台，Docker宣布将Docker项目改名为Moby，交给社区自行维护。  
 
  2017年10月，Docker宣布在自己主打产品Docker企业版内置Kubernetes项目。    
 
  2018年1月20日，RedHat收购CoreOS。  

  2018年3月28日，Docker公司的CTO Solomon Hykes 宣布辞职。

> 后面这段历史原作者将的酣畅淋漓，基本照搬过来的。大家自行体会这几年这个领域的风云变化和各大玩家的角逐。

# 总结
* **容器技术兴起源于PaaS技术的普及**
* **Docker项目通过“容器镜像”，解决了应用打包这个根本性难题**
* **容器本身没有价值，有价值的是“容器编排”**
