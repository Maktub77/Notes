#### Docker

**容器即应用，应用即虚拟化**

##### 虚拟化技术

* 虚拟化技术是一种将计算机物理资源进行抽象、转换为虚拟的计算机资源提供给程序使用的技术

##### Docker的核心组成

---

挂载（mounting ）

* 操作系统使一个存储设备（诸如硬盘、CD-ROM或共享资源）上的计算机文件和目录可供用户通过计算机的文件系统访问的一个过程

> “单向的映射”，原始目录映射/挂在到容器内，修改原始目录会影响到容器内目录变化，但是容器内的目录变化却不会影响原始目录 

---

##### 写时复制机制

* 在编程里，写时复制常常用于对象或数组的拷贝中，当我们拷贝对象或数组时，复制的过程并不是马上发生在内存中，而只是先让两个变量同时指向同一个内存空间，并进行一些标记，当我们要对对象或数组进行修改时，才真正进行内存的拷贝
* docker利用UnionFS将镜像以只读的方式挂载带沙盒文件系统中，只有在容器中发生对文件的修改时，修改才会体现到沙盒环境上

##### 随手删除容器

* 如果我们要为程序准备一些环境或者配置，完全可以直接将它们打包至新的镜像中,下次直接使用这个新的镜像创建容器即可
* 容器中应用程序所产生的一些文件数据，是非常重要的，如果这些数据随着容器的删除而丢失，其损失是非常大的。对于这类由应用程序所产生的数据，并且需要保证它们不会随着容器的删除而消失的，可以使用docker中的数据卷来单独存放。由于数据卷是独立于容器存在的，所以其能保证数据不会随着容器的删除而丢失。

##### 容器网络

在Docker网络中，有三个比较核心的概念，也就是：**沙盒（Sandox）、网络（Network）、端点（Endpoint）**

* 沙盒：提供了容器的虚拟网络线，也就是之前所提到的端口套接字、IP路由表、防火墙等的内容。其实现隔离了容器网络与宿主主机网络，形成了完全独立的容器网络环境
* 网络：可以理解为Docker内部的虚拟子网，网络内的参与者相互可见并能够进行通讯。Docker的这种虚拟网络也是于宿主网络存在隔离关系的，其目的主要是形成容器间的安全通讯环境
* 端点：位于容器或网络隔离墙之上的洞，其主要目的是形成一个可以控制的突破封闭的网络环境的出入口。当容器的端点与网络的端点形成配对后，就如同在这两者之间搭建了桥梁，便能够进行数据传输了

##### 数据管理实现方式

容器弊端

* 沙盒文件系统使跟随容器生命周期所创建和移除的，数据无法被持久化存储
* 由于容器隔离，很难从容器外部获得或操作容器内部文件中

###### 挂载方式

Docker提供了三种适用于不同场景的文件系统挂载方式：**Bind Mount、Volume和Tmpfs Mount**

* Bind Mount：能够直接将宿主操作系统中的目录和文件挂载到容器内的文件系统中，通过指定容器外的路径和容器内的路径，就可以形成挂载映射关系，在容器内外对文件的读写，都是相互可见的
* Volume：也是对宿主操作系统中挂载目录到容器内，只不过这个挂载的目录由Docker进行管理，我们只需要指定容器内的目录，不需要关系具体挂载到了宿主操作系统中的哪里
* Tmpfs Mount：支持挂载系统内存中的一部分到容器的文件系统里，不过由于内存和容器的特征，它的存储并不是持久的，其中的内容会随着容器的停止而消失

##### Dockerfile的结构

Dockerfile的指令简单分为五大类

* 基础指令：用于定义新镜像的基础和性质
* 控制指令：是指导镜像构建的核心部分，用于描述镜像在构建过程中需要执行的命令
* 引入指令：用于将外部文件直接引入到构建镜像内部
* 执行指令：能够为基于镜像所创建的容器，指定在启动时需要执行的脚本或命令
* 配置指令：对镜像以及基于镜像所创建的容器，可以通过配置指令对其网络、用户等内容进行配置

1. From  基于已有镜像创建我们的镜像
2. Run   向控制台发送命令 
3. ENTRYPOINT 和 CMD    启动容器中进程号为 1 的进程 
4. EXPOSE  向其它容器暴露的端口
5. VOLUME  创建数据卷 
6. COPY 和 ADD    直接从宿主机的文件系统里拷贝内容到镜像里的文件系统中 
7. docker build  -f 指定Dockerfile路径  -t  指定镜像名称 

构建中的变量

* 参数变量 ARG 

* 环境变量 ENV

参数变量只能影响构建过程不同，环境变量不仅能够影响构建，还能影响基于环境变量设置的实质，其实就是定义操作系统环境变量，所以在运行的容器里，一样拥有这些变量，而容器中运行的程序也能够得到这些变量的值

> 通过ENV指令和ARG指令所定义的参数，在使用时都是采用$+NAME这种形式来占位的，所以它们之间的定义就存在冲突的可能性。

> ENV指令所定义的变量，永远会覆盖ARG所定义的变量，即使顺序是相反的

###### 合并命令

* 每当一条能够形成对文件系统改动的指令在被执行前，Docker 先会基于上条命令的结果启动一个容器，在容器中运行这条指令的内容，之后将结果打包成一个镜像层，如此反复，最终形成镜像。 

* 镜像是由多个镜像层叠加而得，而这些镜像层其实就是在我们 Dockerfile 中每条指令所生成的。

  了解了这个原理，大家就很容易理解为什么绝大多数镜像会将命令合并到一条指令中，因为这种做法不但减少了镜像层的数量，也减少了镜像构建过程中反复创建容器的次数，提高了镜像构建的速度。

###### 构建缓存

* 建议将不容易发生变化的搭建过程放到Dockerfile的前部，充分利用构建缓存提高镜像的速度。

* 指令的合并也不宜过度，而是将易变和不易变的过程拆分，分别放到不同的指令里

* 如果不希望Docker在构建时使用缓存，这时我们可以通过--no-cache选项来禁用它

  ```docke
  docker build --no-cache ./webapp
  ```

###### ENTRYPOINT 和 CMD 搭配案例 

* ENTRYPOINT 用于对容器进行一些初始化
* CMD 用于真正定义容器中主程序的启动命令 
* CMD 里的内容会作为 ENTRYPOINT 的参数，两者拼接之后，才是最终执行的命令 

##### DockerHub

1.    dockerhub 上选择合适的镜像 
2.    Alpine 镜像小 
3.    通过dockerhub 上 mysql的镜像 来学习如何查看dockerhub上镜像的说明，以便于更好的使用镜像 
4.    共享自己的镜像 

##### Docker Compose

###### 解决容器管理问题

* DockerfIle是将容器内运行环境的搭建固化下来

* Docker Compose就可以理解为将多个容器运行的方式和配置固化下来

  1. 需要容器镜像的DockerFile

  2. 配置容器的docker-compose.yml

     ```yaml
     version: '3'  // docker-compose.yml 文件内容所采用的版本，目前 Docker Compose 的配置文件已经迭代至了第三版，其所支持的功能也越来越丰富，所以我们建议使用最新的版本来定义
     
     services:	//整个 docker-compose.yml 的核心部分，其定义了容器的各项细节
     
       webapp:
         build: ./image/webapp
         ports:
           - "5000:5000"
         volumes:
           - ./code:/code
           - logvolume:/var/log
         links:
           - mysql
           - redis
     
       redis:
         image: redis:3.2
       
       mysql:
         image: mysql:5.7
         environment:
           - MYSQL_ROOT_PASSWORD=my-secret-pw
     
     volumes:
       logvolume: {}
     ```

  3. 使用docker-compose命令启动应用

     * docker-compose up -d

     * docker-compose down

     * docker-compose 命令默认会识别当前控制台所在目录内的docker-compose.yml 文件，而会以这个目录的名字作为组装的应用项目的名称。

       如果我们需要改变它们，可以通过选项 `-f` 来修改识别的 Docker Compose 配置文件，通过 `-p` 选项来定义项目名。 

       ```shell
       $ sudo docker-compose -f ./compose/docker-compose.yml -p myapp up -d
       ```

     * ，我更建议大家像容器使用一样对待 Docker Compose 项目，做到随用随启，随停随删。也就是使用的时候通过 `docker-compose up` 进行，而短时间内不再需要时，通过 `docker-compose down` 清理它。 

* 容器命令

> YAML格式对xx:yy这种格式的解析有特殊性，在设置小于60的值时，会被当成时间而不是字符串来处理，所以我们最好使用引导将端口映射的定义包裹起来，避免歧义

**在真实的服务部署里，我们通常是使用Docker Compose来定义集群，而通过Docker Swarm来部署集群**

----

在创建临时容器的时候增加==--rm==选项，当使用==docker stop==停止容器，就可以停止容器的同时直接删除容器，实现直接清理的目的

