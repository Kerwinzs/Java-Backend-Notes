# 笔记管理工作流与Git 学习

> 本文记录如何使用 Typora + Git + GitHub 管理个人 Java 后端学习笔记，以及日常使用 Git 必须掌握的基础知识。

# 一、个人技术笔记管理工作流

## 1. 整体工具分工

| 工具   | 作用                                 |
| ------ | ------------------------------------ |
| Typora | 编写、阅读和编辑 Markdown 笔记       |
| Git    | 追踪文件变化、管理版本、记录修改历史 |
| GitHub | 远程托管仓库，实现云端保存和分享     |

整体工作流程：

```
Typora
  ↓
编辑本地 Markdown 文件
  ↓
Git
  ↓
git add（添加到暂存区）
  ↓
git commit（提交到本地仓库）
  ↓
git push（推送到远程仓库）
  ↓
GitHub
```

**注意：GitHub 不会自动同步本地文件。需要通过 Git 命令主动推送。**

## 2. 理想的仓库目录结构

```
java-backend-notes/
│
├── README.md
│
├── 01-Java基础/
│   ├── 面向对象.md
│   ├── 继承与多态.md
│   ├── 接口与抽象类.md
│   ├── 集合框架.md
│   └── 异常处理.md
│
├── 02-Maven/
│   ├── Maven基础.md
│   └── 依赖管理.md
│
├── 03-Java Web/
│   ├── HTTP基础.md
│   └── REST.md
│
├── 04-Spring/
│   ├── Spring基础.md
│   └── SpringBoot.md
│
├── 05-数据库/
│   ├── MySQL.md
│   └── SQL.md
│
└── 06-工程实践/
    ├── Git.md
    └── 项目开发流程.md
```

## 3. 日常笔记更新流程

以后每次学完一个知识点，都按照下面的流程操作。

### 第一步：使用 Typora 编辑笔记

在本地仓库中创建或修改 Markdown 文件。

例如：

```
02-Maven/
├── Maven基础.md
└── 依赖管理.md
```

Typora 修改的是本地文件，不会直接影响 GitHub。

### 第二步：查看文件修改情况

```
git status
```

作用：查看当前工作区和暂存区的状态。

重点关注：

- modified：已有文件被修改。
- untracked：新建但尚未被 Git 跟踪的文件。
- Changes to be committed：已经添加到暂存区的文件。
- nothing to commit：当前没有尚未提交的修改。

### 第三步：查看修改内容

```
git diff
```

作用：查看工作区中尚未暂存的修改。

输出中：

- `+`：新增内容。
- `-`：删除内容。

如果想查看已经添加到暂存区的修改：

```
git diff --staged
```

建议提交之前检查一次，避免误提交不需要的内容。

### 第四步：添加到暂存区

```
git add .
```

作用：将当前目录下的修改和新文件添加到暂存区。

如果只想添加某一个文件：

```
git add "02-Maven/依赖管理.md"
```

### 第五步：提交到本地仓库

```
git commit -m "docs: add Maven dependency notes"
```

作用：将暂存区中的修改保存为一个新的版本。

建议提交信息：

| 提交信息                        | 含义            |
| ------------------------------- | --------------- |
| docs: add Java collection notes | 新增集合笔记    |
| docs: update Maven notes        | 更新 Maven 笔记 |
| docs: fix inheritance examples  | 修正继承示例    |

### 第六步：推送到 GitHub

```
git push
```

作用：将本地已经提交的版本推送到远程仓库。

完成后刷新 GitHub 页面，即可看到更新。

## 4. 每日常用命令清单

```
# 1. 查看修改
git status

# 2. 查看修改内容
git diff

# 3. 添加到暂存区
git add .

# 4. 检查即将提交的内容
git diff --staged

# 5. 提交版本
git commit -m "docs: update Java notes"

# 6. 推送到 GitHub
git push
```

**推荐习惯：以一个完整的知识点为单位进行 commit，不需要每修改一个字就提交一次。**

## 5. 多设备同步流程

如果以后换电脑，或者在另一台电脑上继续编辑笔记：

第一次使用：

```
git clone https://github.com/用户名/Java-Backend-Notes.git
```

作用：将远程仓库完整克隆到本地。

以后每次开始编辑之前：

```
git pull
```

作用：获取远程仓库的最新内容，并整合到本地。

编辑完成后：

```
git add .
git commit -m "docs: update Java notes"
git push
```

这样可以让不同电脑上的笔记保持同步。

# 二、Git 核心知识点

## 1. Git 是什么？

Git 是一个分布式版本控制系统（Distributed Version Control System，DVCS）。

它主要解决两个问题：

1. 记录文件的修改历史。
2. 管理不同版本之间的变化。

例如：

```
第一次提交：Java 基础笔记
第二次提交：补充集合框架
第三次提交：补充异常处理
第四次提交：修正代码示例
```

每一次 commit 都会形成一个版本，可以查看历史记录，必要时也可以恢复之前的版本。

## 2. Git 的四个重要区域

这是理解 Git 工作原理最重要的知识。

### 2.1 工作区（Working Directory）

工作区就是你实际操作文件的地方。

例如：

- 使用 Typora 修改 Markdown 文件。
- 新建 Java 笔记。
- 删除不需要的文件。

这些操作最初都发生在工作区。

### 2.2 暂存区（Staging Area）

暂存区是提交之前的准备区域。

通过：

```
git add .
```

将修改放入暂存区。

Git 不会自动把所有修改提交，你可以选择哪些修改需要保存到下一个版本。

### 2.3 本地仓库（Local Repository）

通过：

```
git commit -m "提交说明"
```

将暂存区中的修改正式提交到本地仓库。

commit 可以理解为一次版本存档。

### 2.4 远程仓库（Remote Repository）

远程仓库通常托管在 GitHub 上。

通过：

```
git push
```

将本地提交同步到远程仓库。

### 2.5 四个区域之间的关系

```
        修改文件
            ↓
         工作区
            ↓
         git add
            ↓
         暂存区
            ↓
        git commit
            ↓
         本地仓库
            ↓
         git push
            ↓
         远程仓库
          GitHub
```

特别注意：

- add 不等于 commit。
- commit 不等于 push。
- push 只推送已经提交的版本。

## 3. Git 常用命令

### 3.1 git status

```
git status
```

查看当前仓库状态。

例如：

```
On branch main

Changes not staged for commit:
  modified: Java基础.md

Untracked files:
  Maven基础.md
```

表示：

- Java基础.md 被修改了，但尚未添加到暂存区。
- Maven基础.md 是新文件，尚未被 Git 跟踪。

### 3.2 git diff

```
git diff
```

查看工作区与暂存区之间的差异。

```
git diff --staged
```

查看暂存区与最近一次 commit 之间的差异。

### 3.3 git add

```
git add .
```

将当前目录下的修改添加到暂存区。

注意：不要在没有检查的情况下随意添加包含密码、密钥或公司内部资料的文件。

### 3.4 git commit

```
git commit -m "update notes"
```

将暂存区的内容保存为一个新的版本。

其中：

- commit：提交。
- -m：message 的缩写，表示提交说明。

### 3.5 git push

```
git push
```

将本地提交推送到远程仓库。

第一次关联远程分支时，可能需要：

```
git push -u origin main
```

其中：

- origin：远程仓库的默认名称。
- main：本地分支名称。
- -u：设置上游分支（upstream），建立本地分支与远程分支的跟踪关系。

建立跟踪关系后，通常可以直接使用 git push。

### 3.6 git pull

```
git pull
```

从远程仓库获取最新内容，并整合到本地。

可以理解为：

```
git pull = git fetch + git merge
```

也可以使用：

```
git pull --rebase origin main
```

让本地尚未推送的提交重新应用到远程最新提交之后。

### 3.7 git fetch

```
git fetch origin
```

只获取远程仓库的最新信息，不会直接把远程修改合并到当前工作区。

与 pull 的区别：

| 命令  | 作用                       |
| ----- | -------------------------- |
| fetch | 获取远程更新，但不整合     |
| pull  | 获取远程更新，并整合到本地 |

# 三、Git 分支与远程仓库

## 1. 什么是分支（Branch）？

分支可以理解为一条独立的开发历史。

例如：

```
main
  |
  A
  |
  B
  |
  C
```

main 是一个分支，A、B、C 是提交记录。

实际项目中可能存在：

```
main
  |
  A
  |
  B
  |\
  | \
  |  feature
  |    |
  C    D
  |    |
  |    E
  |
```

开发者可以在 feature 分支上开发新功能，而不直接影响 main 分支。

对于目前的个人笔记仓库，暂时使用 main 一个分支就足够了。

## 2. origin 是什么？

origin 是 Git 为远程仓库设置的默认名称。

例如：

```
git remote -v
```

可以查看远程仓库地址。

输出可能是：

```
origin  https://github.com/用户名/Java-Backend-Notes.git (fetch)
origin  https://github.com/用户名/Java-Backend-Notes.git (push)
```

这里：

- origin：远程仓库名称。
- fetch：获取远程内容时使用的地址。
- push：推送内容时使用的地址。

origin 只是一个名称，不是 GitHub 的特殊服务器。

## 3. main 和 origin/main 有什么区别？

```
main
```

代表本地 main 分支。

```
origin/main
```

代表 Git 最近一次获取到的远程 main 分支状态，是一个远程跟踪分支。

它们可能存在差异。

例如：

```
本地 main：A → B → C
远程 main：A → B
```

表示本地比远程多了一个提交 C。

# 四、Git 提交状态与常见报错

## 1. Your branch is ahead of 'origin/main' by 1 commit

含义：

本地 main 分支比远程跟踪分支 origin/main 多一个提交。

例如：

```
本地：  A → B → C
远程：  A → B
```

本地多了 C，还没有推送到 GitHub。

解决方法：

```
git push
```

## 2. nothing to commit, working tree clean

含义：

当前没有尚未提交的文件修改。

这并不代表所有提交都已经上传到 GitHub。

还需要检查：

```
git status
```

如果出现：

```
Your branch is ahead of 'origin/main' by 1 commit
```

说明仍有提交需要 push。

## 3. rejected (fetch first)

曾经遇到的报错：

```
! [rejected] main -> main (fetch first)

error: failed to push some refs
```

含义：

远程仓库存在本地尚未获取或整合的提交，因此 Git 拒绝直接推送。

可能原因：

- 在 GitHub 创建仓库时自动生成了 README。
- 其他设备向远程仓库提交了内容。
- 远程仓库存在其他本地没有的提交。

### 正常情况下的处理方式

如果本地和远程拥有共同的提交历史，可以先执行：

```
git pull --rebase origin main
```

然后：

```
git push origin main
```

### 如果提示 unrelated histories

如果两个仓库分别独立初始化，没有共同的提交历史，rebase 可能无法直接完成。

可以在确认需要保留本地和远程两边内容后，使用：

```
git pull --no-rebase --allow-unrelated-histories origin main
```

如果出现冲突：

1. 执行 git status 查看冲突文件。
2. 打开冲突文件，检查冲突内容。
3. 删除冲突标记，保留需要的内容。
4. 执行 git add 添加解决后的文件。
5. 执行 git commit 完成合并。
6. 执行 git push origin main。

冲突标记通常长这样：

```
<<<<<<< HEAD
本地内容
=======
远程内容
>>>>>>> origin/main
```

需要手动决定最终保留什么内容，并删除这些标记。

**注意：不要随意使用 git push --force。它可能覆盖远程已有提交。**

# 五、less 分页器

## 1. 为什么 git diff 后出现 (END)？

Git 在展示较长的输出时，通常会调用一个分页器（Pager）。

默认常见的分页器是 less。

它让我们可以逐页查看大量输出，而不必一次性刷满终端。

例如：

```
git diff
```

可能会进入 less 界面。

## 2. less 常用操作

| 按键    | 作用           |
| ------- | -------------- |
| q       | 退出           |
| Space   | 向下翻一页     |
| b       | 向上翻一页     |
| ↑ / ↓   | 上下移动一行   |
| /关键词 | 搜索关键词     |
| n       | 下一个匹配结果 |
| g       | 跳到开头       |
| G       | 跳到结尾       |

最重要的是：

```
q = Quit
```

按 q 就能退出。

## 3. 不使用分页器

```
git --no-pager diff
```

直接在终端输出差异内容。

# 六、常用 Git 命令速查表

| 命令              | 作用                   |
| ----------------- | ---------------------- |
| git init          | 初始化本地仓库         |
| git clone         | 克隆远程仓库           |
| git status        | 查看仓库状态           |
| git diff          | 查看工作区修改         |
| git diff --staged | 查看暂存区修改         |
| git add .         | 添加修改到暂存区       |
| git commit -m     | 创建提交               |
| git log           | 查看提交历史           |
| git push          | 推送到远程仓库         |
| git pull          | 拉取并整合远程更新     |
| git fetch         | 获取远程更新           |
| git branch        | 查看分支               |
| git switch        | 切换分支               |
| git remote -v     | 查看远程仓库地址       |
| git rebase        | 变基，重新整理提交历史 |
| git merge         | 合并分支               |
| git diff --staged | 检查即将提交的内容     |

# 七、个人学习建议

对于目前的 Java 后端学习，我建议：

1. 每学习一个完整知识点，就整理成一份 Markdown 文件。
2. 使用 Typora 维护笔记。
3. 每次提交前检查 git status 和 git diff。
4. 按知识点进行 commit。
5. 定期 push 到 GitHub。
6. 逐步完善 README，建立自己的知识目录。

未来可以把知识库扩展为：

```
java-backend-notes/
├── Java基础
├── Maven
├── Java Web
├── Spring Boot
├── MySQL
├── Oracle
├── Redis
├── Git
└── 项目实践
```

**最终目标：让 GitHub 成为自己的长期技术知识库，而不仅仅是一个存放 Markdown 文件的地方。**

> 学习知识、整理知识、版本管理、持续积累，这本身就是一种很好的软件工程实践。