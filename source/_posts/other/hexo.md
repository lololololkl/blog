---
tags: [other, hexo]
categories: 
- other
- hexo
title: hexo
---

## [添加 gitalk](https://blog.csdn.net/weixin_43941364/article/details/104160976?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162692035316780271571612%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162692035316780271571612&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-23-104160976.first_rank_v2_pc_rank_v29&utm_term=hexo++%E8%AF%84%E8%AE%BA&spm=1018.2226.3001.4187)

### 创建 GitHub Application

### 引入 gitalk 的代码

在 themes/[theme_name]/layout/_script/_comments/ 目录下，创建gitalk.swig文件。
```js
<!-- gitalk.swig -->
<link href="https://cdn.bootcss.com/gitalk/1.4.0/gitalk.min.css" rel="stylesheet" />
<script src="https://cdn.bootcss.com/gitalk/1.4.0/gitalk.min.js"></script>
<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.min.js"></script>
<script type="text/javascript">
  var gitalk = new Gitalk({
    clientID: '{{ theme.gitalk.ClientID }}',
    clientSecret: '{{ theme.gitalk.ClientSecret }}',
    repo: '{{ theme.gitalk.repo }}',
    owner: '{{ theme.gitalk.owner }}',
    admin: ['{{ theme.gitalk.adminUser }}'],
    id: md5(location.pathname),   // ISSUE：location.href 限制50个字符 (应该是Issue 内容存数据库的标识， 具体在页面上无感)
    labels: '{{ theme.gitalk.labels }}'.split(',').filter(l => l),  // 需要的 labels需要一个数组，否则会报错
    perPage: {{ theme.gitalk.perPage }},
    pagerDirection: '{{ theme.gitalk.pagerDirection }}',
    createIssueManually: {{ theme.gitalk.createIssueManually }},
    distractionFreeMode: {{ theme.gitalk.distractionFreeMode }}
  })
  gitalk.render('gitalk-container')
</script>

```
<!-- more -->

### 在 comments.swig 引用gitalk.swig
```js
<!-- themes/[theme_name]/layout/_script/comments.swig -->
{% include '_comments/gitalk.swig' %}
```
如果没有找到这个 comments.swig ， 可以直接放到themes/polarbear/layout/_layout.swig 的 body 结束标签前

### 添加配置
```js
# theme/[theme_name]/_config.yaml 添加配置
# ===========================================
# Comments Settings
# ===========================================
gitalk:
  enable: true # 是否启动
  ClientID: Your ClientID # 之前的Client ID
  ClientSecret: Your ClientSecret # 之前的Client Secret
  repo: gitalk # 留言板内容需要存放的仓库名称
  owner: wsuo # 你 github 的帐号 eg: www.github.com/wsuo ,帐号就是wsuo
  adminUser: wsuo # 管理员用户
  labels: # issue 的标签
    - gitalk
  perPage: 15 # 每页展示条数
  pagerDirection: last # 排序方式是从旧到新（first）还是从新到旧（last）
  createIssueManually: false #如果当前页面没有相应的 isssue ，且登录的用户属于 admin，则会自动创建 issue。如果设置为 true，则显示一个初始化页面，创建 issue 需要点击 init 按钮。
  distractionFreeMode: false #是否启用快捷键(cmd|ctrl + enter) 提交评论.
```



## [stun主题配置](https://blog.csdn.net/qq_26624329/article/details/111411095)

### 添加tags、categories，增加页面导航

#### 指令
```shell
hexo new page tags
```

#### 修改主题配置文件
/themes/stun/_config.yml

```text
menu:
  categories: /categories/ || fas fa-layer-group
  tags: /tags/ || fas fa-tags
```

### 文章规范

#### 文章元数据
```text
title: 
date: 
categories
- 父类别
- 子类别
tags: 
- 标签1
- 标签2
```

#### 主页不显示全部

```md
<!-- more -->
```
