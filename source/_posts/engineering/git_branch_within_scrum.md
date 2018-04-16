---
title: scrum和分支管理策略
postslug: git_branch_within_scrum
date: 2015-05-05
category: 软件工程
tags: [scrum, git]
---

用scrum管理开发时，如何规划git的分支。

<!--more-->

# scrum回顾

## scrum中的角色划分

- 【角色组】pigs(项目参与人员)
  + 【角色】Scrum master
    * 【职责】会议主持人：负责主持四个主要的会议：策划会议、每日站立会议、迭代评审会议、迭代回顾会议
    * 【职责】牧羊犬：负责屏蔽项目组外部的干扰
    * 【职责】雷锋：帮助product owner确定需求、排定优先级，帮助team做估算、分解任务、完成任务
    * 【职责】外交官：当项目组外部有人不理解项目组的工作时，他负责去解释说明，负责对外发布项目组的信息
    * 【职责】教练：指导、培训项目组的成员按照SCRUM的原则、方法做事情，当出现偏差时负责纠正
    * 【职责】清道夫：负责或协调项目组的其他成员一起，排除在项目进展中遇到的各种障碍
  + 【角色】Product owner
    * 【职责】领域专家：知道真正需求是什么。他负责编写、维护用户需求
    * 【职责】需求决策人：决定需求的优先级，产品每次发布需要实现的需求。负责来平衡需求、进度与资源的关系。
    * 【职责】需求讲师：负责需求含义的讲解和答疑
    * 【职责】测试员：负责编写每个需求的验收标准，功能测试用例
    * 【职责】验收人：进行功能测试，进行验收，认可后，确认需求完成
  + 【角色】Team member
    * 【职责】设计人员：对系统进行简单设计
    * 【职责】实现人员：负责实现整个系统，并对系统执行单元测试，构建整个系统
    * 【职责】管理人员：大家一起来估算、一起来选择任务、一起来跟踪进展情况
- 【角色组】chickens(项目外部人员)
  + 【角色】经理
  + 【角色】最终用户
  + 【角色】Scrum master

## scrum流程


scrum的精髓是两个“队列”：Product Backlog 和 Sprint Backlog。
在scrumer眼中，需求就是Product
Backlog中积压的一个个Story（有优先级），Prodcut
Backlog是动态的，永远处于不完整状态，被持续修订。
为了完成项目，团队要进行一次次的“冲刺”（sprint)，每次冲刺只前进一小段距离——完成一个或几个Story，甚至只完成一个Story的一部分。

当完成一个或几个Story后，Product owner可以决定是否要发布一个版本。

# git最佳实践


在 《A successful Git branching model》中，Vincent Driessen 给出了git
分支管理的“最佳实践”：

{% asset_img bigpicture-git-branch-all.png %}

其中各个分支的用途如下：

-   长期分支
    -   master
        用于发布正式版本。除了最初版本外，发布分支的版本应该来自develop分支或Hostfix分支。
    -   develop 用于开发。记录最新的代码。
-   临时分支
    -   feature-* 分支
        用于开发新功能。基于develop分支创建，最后合并到develop分支。不同的功能应该创建各自的分支去开发。
    -   release-* 分支
        用于发布测试版本。经过测试并修复后的版本merge到master分支。
    -   hotfix-* 分支
        用于开发补丁，补丁完成后要merge到develop分支；如果紧急还可以merge到master分支形成fix版本。

# 二者的结合


1.  scrum中，要求Product
    Backlog中的每个story要足够小。这天然就可以作为一个分支去开发：feature-*
    或 hotfix-* 分支
2.  Product Owner 可以决定哪几个story发布一个版本。对应的就是release-\*
    分支
3.  master分支应该归Product
    Owner管理，当版本正式发布时，需要在master分支上标记一个tag
4.  Team 的协同工作基于develop分支。

具体的流程如下：

1.  Sprint planning 会议后，制定sprint backlog，并进行任务分工
2.  项目成员根据所负责的backlog item的性质，创建分支
3.  Sprint review 会议后，经过验证的backlog item进行分支合并：
    -   对于`feature-*`分支，合并到develop
    -   对于`hotfix-*`分支，合并到develop和master分支
4.  如果本次迭代中开发了新功能，要进行发布：
    1.  从合并后的develop分支复制出`release-*`分支
    2.  进行一系列的发布准备工作，如：bump-version, UAT等
    3.  允许修改，修改仍在`release-*`分支中进行，最后要同步回develop分支
    4.  最终验证后的版本，同步到master分支；标记tag;正式发布

# 相关git命令


## 分支操作


``` {.bash}
#创建(并切换)分支
git checkout -b TARGET_BRANCH SRC_BRANCH

#切换分支
git checkout BRANCH

#合并分支（将SRC_BRANCH合并到TARGET_BRANCH)
git checkout TARGET_BRANCH
git merge --no-ff SRC_BRANCH

#提交分支
git push origin BRANCH

#删除分支
git branch -d BRANCH

```

## tag操作


``` {.bash}
#列出标签
git tag

#查询标签
git tag -l 通配符

#创建tag
git tag -a TAG_NAME -m 'COMMIT_INFO'
git tag TAG_NAME #创建轻量lightweight标签

#增加标签
git tag -a TAG_NAME COMMIT_HASH_CODE

#查看tag信息
git show TAG_NAME

#提交标签
git push –-tags

```

# 参考资料

1.  白话SCRUM之一：SCRUM 的三个角色
    <http://blog.csdn.net/dylanren/article/details/6939680>
2.  白话SCRUM 之二：product backlog
    <http://blog.csdn.net/dylanren/article/details/7072734>
3.  白话SCRUM 之三：sprint backlog
    <http://blog.csdn.net/dylanren/article/details/7298892>
4.  Scrum 之 product Backlog <http://www.zhoujingen.cn/blog/2767.html>
5.  阮一峰：Git分支管理策略 <http://blog.jobbole.com/23398/>
6.  Vincent Driessen：A successful Git branching model
    <http://nvie.com/posts/a-successful-git-branching-model/>
7.  Git 分支管理是一门艺术 <http://blog.jobbole.com/13916/>
8.  白话SCRUM之五：四种会议
    <http://blog.csdn.net/dylanren/article/details/7344151>
