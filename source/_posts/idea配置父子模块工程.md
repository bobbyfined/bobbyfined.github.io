---
title: idea配置父子模块工程
date: 2023-04-01 21:45:36
tags: GIT
categories: java

---

#  idea配置父子模块工程

<!-- more -->

## 流程

1. 在Gitee创建一个新仓库

2. 初始化本地仓库

3. 创建README.md并提交，保证仓库不是空仓库

   ```
   git init
   ```

4. 进入父仓库目录，输入以下命令

```
touch README.md
git commit -m "说明"
# 关联远程仓库（在Gitee新建仓库后跳转到的页面会给出刚创建的远程仓库地址 也就是clone地址）
git remote add origin https://gitee.com/xxx/xxx
# 提交到远程仓库 使用 -u 下次提交就可以直接使用git push
git push -u origin "master"
```

## git 命令 -u 含义

```
官网解释（大概意思就是使用-u时的本地分支与远程分支建立联系，下次使用要指定分支的命令时可以不用再指定分支）
-u
--set-upstream
For every branch that is up to date or successfully pushed, add upstream (tracking) reference, used by argument-less git-pull and other commands.
```

# 如果idea拉取之后父模块没有代码

如果你在IntelliJ IDEA中没有看到子模块相关的选项，并且确认项目中确实没有子模块，那么你可能需要检查项目的`.gitmodules`文件和`.git/config`文件来确保子模块配置正确。以下是一些步骤来帮助你解决这个问题：

### 检查 `.gitmodules` 文件

1. **打开 `.gitmodules` 文件**：

   - 在项目根目录下找到 `.gitmodules` 文件并打开它。
   - 确认文件中是否有正确的子模块配置。

2. **添加或修改子模块配置**：

   - 如果文件中没有子模块配置，你可以手动添加：

     ```
     [submodule "path/to/submodule"]
         path = path/to/submodule
         url = https://example.com/path/to/submodule.git
     ```

### 初始化和更新子模块

1. **通过命令行初始化子模块**：

   - 打开IntelliJ IDEA的终端（通常位于底部工具栏，标签名为“Terminal”）。

   - 输入以下命令来初始化和更新子模块：

     ```
     git submodule init
     git submodule update
     ```

2. **通过图形界面初始化子模块**：

- 打开 `VCS` 菜单。
- 选择 `Git` > `Submodule` > `Update...`。
- 在弹出的对话框中，确保选中了所有需要初始化和更新的子模块，然后点击“OK”。

### 示例操作

假设你的项目结构如下：

```
project-root/
├── .git/
├── .gitmodules
├── src/
└── submodules/
    └── submodule1/
```

1. **打开 `.gitmodules` 文件**：

   ```
   [submodule "submodules/submodule1"]
       path = submodules/submodule1
       url = https://example.com/path/to/submodule1.git
   ```

2. **通过命令行初始化子模块**：

   ```
   git submodule init
   git submodule update
   ```

3. **通过图形界面初始化子模块**：

   - 打开 `VCS` 菜单。
   - 选择 `Git` > `Submodule` > `Update...`。
   - 确保选中了所有需要初始化和更新的子模块，然后点击“OK”。

通过以上步骤，你应该能够成功初始化并更新子模块，解决子模块没有代码的问题。
