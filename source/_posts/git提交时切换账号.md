---
title: GIT提交时切换账号
date: 2023-07-27 23:19:06
tags: GIT
categories: GIT
---

# GIT提交时切换账号

 <!-- more -->

## 要在git提交时切换账号，你可以按照以下步骤进行操作：

1. 查看当前git用户信息：在命令行中输入以下命令，可以查看当前git账号的用户名和邮箱。

   ```
   
   git config user.name
   
   git config user.email
   
   ```

   

2. 切换到新的git账号：如果要使用不同的git账号提交，可以按照以下步骤进行切换。

   – 修改全局git用户信息：在命令行中输入以下命令，将全局的git用户名和邮箱修改为新的账号信息。

   ```
     git config –global user.name “New User Name”
   
     git config –global user.email “New User Email”
   ```

   – 修改当前仓库的git用户信息：在命令行中进入到对应的git仓库目录，输入以下命令，将当前仓库的git用户名和邮箱修改为新的账号信息。

   ```
     git config user.name “New User Name”
   
     git config user.email “New User Email”
   ```

3. 提交代码：现在你可以使用新的git账号提交代码了。

   – 添加修改到暂存区：在命令行中使用git add命令将修改的文件添加到暂存区。

   ```cmd
   git add      #– 提交代码到本地仓库：在命令行中使用git commit命令将暂存区的修改提交到本地仓库。 
   git commit -m "Commit message"   #推送代码到远程仓库：在命令行中使用git push命令将本地仓库的修改推送到远程仓库。     git push origin     
   ```

通过以上步骤，你可以在git提交时切换到不

3. 重新提交代码

假设你已经在本地进行了更改并且想用新账号提交这些更改，你可以执行以下步骤：

**3.1 取消上一次提交（如果已经提交但未推送）**

如果你已经提交了代码，但想使用新账号重新提交，可以先撤销最后一次提交：

```
git reset --soft HEAD~1
```

这样做会保留你的更改，但取消提交。

**3.2 重新提交**

现在用新账号提交代码：

```
git commit -m "Your commit message"
```

**3.3 推送到远程仓库**

最后，将提交推送到远程仓库：

```
git push origin branch-name
```

其中 branch-name 是你要推送的分支名称。

