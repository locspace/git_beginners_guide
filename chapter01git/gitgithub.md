# Git 与 GitHub

## 版本控制和软件配置管理
### 基本概念
+ [版本控制][1]（`Revision Control`）是维护工程蓝图的标准做法，能追踪工程蓝图从诞生一直到定案的过程。此外，版本控制也是一种软件工程技巧，借此能在软件开发的过程中，确保由不同人所编辑的同一代码文件案都得到同步。

+ 软件配置管理 `SCM` 是一套应用技术上和管理上的指导和监督的方法，用来：
    1. 识别和记录配置项的功能特征和物理特征
    2. 控制这些特征的变更
    3. 记录和报告变更的处理和执行的状态
    4. 验证其是否符合特定的需求

    （引自 《未雨绸缪 理解软件配置管理》 一书）

当然，`版本控制`或者`软件配置管理`是个很有深度的专题。这里作为初学者的指南，对此不作深入探讨。

### 一些术语

1. `基线`（`Baseline`）

    基线是软件文档或源码（或其它产出物）的一个稳定版本，它是进一步开发的基础。

2. `文件库`（`Repository`）

    存储文件的新版本还有历史数据的地方，通常是在服务器上。有时候也叫 Depot （像是在 SVK 、 AccuRev 还有 Perforce 中）

3. `工作版本`（`Working copy`）

    从文件库中取出一个本地端（客户端）的复制，针对一个特定的时间或是版本。所有在文件库中的文件更动，都是从一个工作版本中修改而来的，这也是这名称的由来。观念上，这是一个沙盒。

4. `提交`（`Commit`）

    将本地端的修改送回档案库。（由版本控制软件处理“跟上次更动相比，哪个文件又被更动”的事）

5. `变更`（`Change`）

    对一份文件作的特定更动。

6. `变更记录`（`Change List`）

    一系列变更的列表

7. `取出`（`Check-Out`）

    从文件库取出文件到本地端（客户端）。

8. `更新`（`Update`）

    将文件库的修改送到本地端（与提交相反）

9. `合并`（`Merge / Integration`）

    合并各个改变。

10. `版本`（`Revision`）

    一个 revision 或 version 指的是一系列版本变迁的其中之一。

11. `导入`（`Import`）
12. `导出`（`Export`）
13. `冲突`（`Conflict`）

    当两方改动同一份文件会发生冲突。

## 版本控制系统
现在一般意义下的版本控制系统（`VCS`）是指版本控制软件。其分类在很多网站上都有讲述，如 [Pro Git][2]，[维基][3]，張文鈿的 [Git 版本控制系統][4] 和廖雪峰的 [Git 教程][5] 等。这里简述成如下三种：

1. 文件式版本控制系统

    说得直白一点，就是简单的多个内容基本相同的文件版本保存在同一文件夹或者不同的文件夹内，如

        论文-最终版.txt
        论文-最终最终版.txt
        论文-最终最终最终版.txt
        论文-最终最终最终吐血版.txt

    再高级一点的会用上数据库记录一下各个版本，效果是一样的。

    这种版本控制适合于单人的写作和简单的协作等。

2. 中央式版本控制系统

    如字面意思，版本库集中在一个地方，离开相应的环境就不能干活（是指版本控制的工作）了。这种系统发展了很多年，一些软件（如 SVN）有相当严格的权限控制，适合于公司管理开发和一些协作工作（如翻译组工作、写书）等。

3. 分布式版本控制系统

    这种系统很重要的一点是没有所谓的中央服务器（从定义上来讲），本地端有完整的 Repository。它没有相应的集中式环境（如网络），可以在本地端提交、查看变更等操作。具有代表性的软件有 Git，Mercurial(Hg) 和 Bazaar 等。

在这里不讨论版本控制系统哪个最好。如果硬要比较它们的特点，请搜索相关内容并仔细查看，这里提供一个 [GitSvnComparison][6]。作为使用者而言，适合于个人所在环境的系统是最友好的。另外这里是 Git 的使用指南，仅对 Git 作相应深入的介绍。


## `Git` - 愚笨的内容跟踪器
![Git][21]
### 含义
`Git` 是一个快速的分布式版本控制系统。

> 林纳斯·托瓦兹自嘲地取了这个名字 `git`，读音为 /ɡɪt/，该词源自英国俚语，意思大约是 `unpleasant person`（[维基][7]）。

> 'git' is British slang for "pig headed, think they are always correct, argumentative"（[git-faq][8]）

### 主要功能
Git 是个版本控制工具，运行速度快，最为出色的是合并追踪（merge tracing）能力。作为开源自由原教旨主义项目，Git 没有对版本库的浏览和修改做任何的权限限制，但可以通过其他工具达到有限的权限控制。

### 基础
1. 快照式管理

    Git 把数据考虑成小型文件系统的快照一般。每次你在 Git 里提交或保存项目的状态，它会对当时所有的文件进行“拍照”并保存一个指向快照的索引。为提高性能，若文件没有变化，Git 不会再次保存，而只对上次保存的快照作一链接。Git 更像把数据考虑成快照流（a stream of snapshots）。Git 更像是个小型的文件系统，但它同时还提供了许多以此为基础的超强工具，而不只是一个简单的 VCS。

    ![snapshots][9]
    
    （以上摘自 [Pro Git][10]）

    > 注：这种快照式管理一方面在某种程度上会增加小文件的数量。尤其是当一个大型 Git 托管平台需要进行迁移时，其数据量固然大，但最难处理的是小数据，难免在复制迁出过程中丢失一些。另一方面是对大文件的管理相对无力。如果项目中有大文件，且大文件变更也频繁，那么这种快照式管理带来的麻烦就是项目库相当庞大。在这方面 GitHub 深有体会，并专门对大文件的托管作了研究——[GitHub 自身采用的大文件管理][11] 和 [GitHub 开发的 Git 大文件管理扩展][12]。

2. 本地操作与数据完整性

    几乎所有的操作都是本地的。多人网络间的协作倒是需要网络来拉取和发出合并请求等。

    在保存到 Git 之前，所有数据都要进行内容的校验和（checksum）计算，并将此结果作为数据的唯一标识和索引。换句话说，不可能在你修改了文件或目录之后，Git 一无所知。这项特性作为 Git 的设计哲学，建在整体架构的最底层。所以如果文件在传输时变得不完整，或者磁盘损坏导致文件数据缺失，Git 都能立即察觉。

    > 注：
    > 
    > 1. 许多人在问各种由 svn 的理念代入的问题的时候并没有想过 git 的大部分操作只在本地完成，如提交、查看日志、合并等。而只有推送到远程仓库，拉取远程仓库更新和在一些托管平台的 web 界面上发出合并请求等操作是需要网络连接的。
    > 
    > 2. Git 的数据完整性是相当严格的，每个提交都有一个唯一哈希值作为提交的“指纹”——40位十六进制数字。但这也带来个问题，如果 Git 察觉到某个提交有问题，那么这麻烦可能是相当大的。具体怎么做，在指南的后面可能会提及。

3. 多数的操作仅添加数据

    常用的 Git 操作大多仅仅是把数据添加到数据库。因为任何一种不可逆的操作，比如删除数据，都会使回退或重现历史版本变得困难重重。在 Git 里，一旦提交快照之后就完全不用担心丢失数据，特别是养成定期推送到其他仓库的习惯的话。这就是分布式的一个好处。

4. 三种状态

    好，现在请注意，如果你想要更平滑地使用 Git，那么下面的请记住。在 Git 内你的文件都有三种主要的状态：已提交（committed），已修改（modified）和已暂存（staged）。已提交表示该文件已经被安全地保存在本地数据库中了；已修改表示修改了某个文件，但还没有提交保存；已暂存表示把已修改的文件放在下次提交时要保存的清单中。

    由此我们看到 Git 管理项目时，文件流转的三个工作区域：工作目录，暂存区域，以及 Git 目录。

    ![areas][13]

    Git 目录是 Git 用来保存项目元数据和对象数据库的地方。该目录非常重要，每次克隆镜像仓库的时候，实际拷贝的就是这个目录里面的数据。

    从项目中取出某个版本的所有文件和目录，用以开始后续工作的叫做工作目录。这些文件实际上都是从 Git 目录中的压缩对象数据库中提取出来的，接下来就可以在工作目录中对这些文件进行编辑。

    所谓的暂存区域只不过是个简单的文件，一般都放在 Git 目录中。有时候人们会把这个文件叫做索引文件，不过标准说法还是叫暂存区域。

    基本的 Git 工作流程如下：

    + 在工作目录中修改某些文件。
    + 对修改后的文件进行快照，然后保存到暂存区域。
    + 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。

    (以上摘自 [Pro Git][16])

    > 注：
    > 
    > 1. 如果要想对 Git 的这三个区域有更多的实践及理解，请参考 [这个对照表][15]。
    > 
    > 2. [Git 目录结构简介][14]
    > 
    > + branches : 存储分支
    > + hooks：存储钩子的文件夹
    > + logs：存储日志的文件夹
    > + refs：包含三个子文件夹 heads、remotes 和 tags，可以看到本地分支、远程分支和标签文件
    > + objects：存放git对象
    > + config：存放各种设置文档
    > + HEAD：指向当前所在分支的指针文件路径，一般指向refs下的某文件
    > + COMMIT_EDITMSG : 上次提交信息
    > + description : 仓库描述
    > + index : 暂存区域（就是上文的所谓简单的文件）
    > + info : 仓库的额外信息
    > + ORIG_HEAD : HEAD 的前一个状态
    > + packed-refs : 包含打包了的指针和标签

## GitHub
![GitHub][20]
### 简介
+ [**GitHub**][17] 是个基于网络的 Git 仓库托管服务，提供了 Git 所有的分布式版本控制和软件代码管理功能，同时也添加了许多自己的功能。
+ GitHub 并不像 Git 只是个控制台工具，它还提供了一个基于网络的可视化界面并集成了桌面端和移动端。它也为每个项目提供了权限管理和一些协作功能，如 wiki，任务管理，bug 跟踪和特性请求。
+ 到2015年，GitHub 报告说它拥有九百万的用户和托管超过两千一百万的仓库，已经成为世界上最大的源代码托管平台。
+ GitHub 是与朋友、同事、同学或者是毫不相识的陌生人之间共享代码的最好的地方。—— GitHub 宣传语

### 功能
GitHub 最主要的用途是代码托管。除了这个之外，它还支持以下一些东西：
+ 文档，包括用多种混合格式的 Markdown 文件格式进行自动渲染的 README 文件
+ 问题跟踪（包括特性请求）
+ Wiki
+ 托管于 GitHub 公开仓库上的小型网站
+ 内嵌于文件的任务列表
+ 地理空间数据的可视化
+ 甘特图
+ 3D 渲染文件预览
+ 预览 PhotoShop 原生的 PSD 文件并可做版本比较

（以上介绍摘自 [维基][18] 和 [GitHub][19]）


[1]: https://zh.wikipedia.org/wiki/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6
[2]: https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control#Local-Version-Control-Systems
[3]: https://zh.wikipedia.org/wiki/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6#.E4.B8.AD.E5.A4.AE.E5.BC.8F.E7.B3.BB.E7.B5.B1.E8.88.87.E5.88.86.E6.95.A3.E5.BC.8F.E7.B3.BB.E7.B5.B1
[4]: https://ihower.tw/git/vcs.html
[5]: http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374027586935cf69c53637d8458c9aec27dd546a6cd6000
[6]: https://git.wiki.kernel.org/index.php/GitSvnComparison
[7]: https://zh.wikipedia.org/wiki/Git#.E5.91.BD.E5.90.8D.E6.9D.A5.E6.BA.90
[8]: https://git.wiki.kernel.org/index.php/Git_FAQ#Why_the_.27Git.27_name.3F
[9]: images/snapshots.png "随着时间推移，把项目的数据存储成快照（snapshots）"
[10]: https://git-scm.com/book/en/v2/Getting-Started-Git-Basics
[11]: https://github.com/blog/1986-announcing-git-large-file-storage-lfs
[12]: https://git-lfs.github.com/
[13]: images/areas.png "工作目录、暂存区域和 Git 目录"
[14]: https://www.siteground.com/tutorials/git/directory.htm
[15]: http://ndpsoftware.com/git-cheatsheet.html
[16]: https://git-scm.com/book/en/v2/Getting-Started-Git-Basics#The-Three-States
[17]: https://github.com
[18]: https://en.wikipedia.org/wiki/GitHub
[19]: https://github.com/about
[20]: images/githublogo.png
[21]: images/gitlogo.png