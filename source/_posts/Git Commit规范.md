---
layout: title
title: Git Commit规范
date: 2023-04-01 21:45:36
tags: GIT
categories: GIT

---

#  Git Commit规范

<!-- more -->

**参考资料**

https://juejin.cn/post/7243451555930898469

**关键词**

……

**结构样式**

每个 commit message 包含一个 header，body 和 footer。header 有一个特殊的格式包含有 type，scope 和 subject：

```
<type>(<scope>): <subject> <issus-ID>

<BLANK LINE>

<body>

<BLANK LINE>
```



<footer>

header、body、footer 之间都要空一行，header 是必填项，scope 是选填项。commit message 的每一行的文字不能超过 100 个字符。这样子在 github 和 git 工具上更便于阅读。

**header**

**Type**

type 用于说明 commit 的类别，必须为以下类型的一种：

- **feat**：增加新功能（feature）
- **fix**： 修复问题/BUG

- **style**： 代码风格相关无影响运行结果的

- **perf**： 优化相关，比如提升性能、体验。

- **refactor**：重构，既不是修复 bug 也不是添加新功能的代码更改

- **revert**：回滚到上一个版本，撤销功能，撤销修改

- **test**： 测试相关

- **docs**：文档/注释（documentation）
- **chore**： 依赖更新/脚手架配置修改等

- **workflow：**工作流改进

- **ci**： 持续集成

- **types：** 类型定义文件更改

- **merge**：代码合并

- **sync**：同步主线或分支的Bug

- **build**： 影响构建系统或外部依赖关系的更新(范围:gulp，broccoli，npm)

**Scope**

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同，当影响的范围有多个时候，可以使用 *。

例如在Angular，可以是location，browser，compile，compile，rootScope， ngHref，ngClick，ngView等。

**Subject**

subject 用于对 commit 变化的简洁描述，不超过50个字符：

- 使用祈使句，一般以动词原形开始，例如使用 change 而不是 changed 或者 changes

- 第一个字母小写

- 结尾不加句号或其他标点符号。（.或 。）


**Body**

- body 用于对 commit 详细描述。使用祈使句，一般以动词原形开始，例如使用 change 而不是 changed 或者 changes。

- body 应该包含这次变化的动机以及与之前行为的对比。


**Footer**

- footer 目前用于两种情况。


**不兼容的变动**

所有不兼容的变动都必须在 footer 区域进行说明，以 BREAKING CHANGE: 开头，后面的是对变动的描述，变动的理由和迁移注释。

BREAKING CHANGE: isolate scope bindings definition has changed and

    the inject option for the directive controller injection was removed.
    
    Before:
    
    scope: {
    
      myAttr: 'attribute',
    
      myBind: 'bind',
    
      myExpression: 'expression',
    
      myEval: 'evaluate',
    
      myAccessor: 'accessor'
    
    }
    
    After:
    
    scope: {
    
      myAttr: '@',
    
      myBind: '@',
    
      myExpression: '&',
    
      // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
    
      myAccessor: '=' // in directive's template change myAccessor() to myAccessor
    
    }

 The removed `inject` wasn't generaly useful for directives so there should be no code using it.

**关闭 issue**

如果 commit 是针对某个 issue，可以在 footer 关闭这个 issue。

```
## 关闭单个

Closes #234

## 关闭多个

Closes #123, #245, #992
```

**Revert**

如果 commit 用于撤销之前的 commit，这个 commit 就应该以 revert: 开头，后面是撤销这个 commit 的 header。在 body 里面应该写 This reverts commit <hash>.，其中的 hash 是被撤销 commit 的 SHA 标识符。

```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

**示例**

```
feat($browser): onUrlChange event (popstate/hashchange/polling)

Added new event to $browser:

- forward popstate event if available

- forward hashchange event if popstate not available

- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)
```

```
fix($compile): couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...

Would be better to expect case insensitive, unfortunately jasmine does

not allow to user regexps for throw expectations.

Closes #351
```

```
style($location): add couple of missing semi colons
```

- ```
  docs(guide): updated fixed docs from Google Docs
  
  Couple of typos fixed:
  
  - indentation
  
  - batchLogbatchLog -> batchLog
  
  - start periodic checking
  
  - missing brace
  ```

  

