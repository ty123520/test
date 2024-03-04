## 1、基本使用

### [1). 初始化本地用户](https://webtech.wiki/git/#/note/note-01?id=_1-初始化本地用户)

在使用 git 之前，我们先设置本地的账号，设置一下本地 `git`用户 的用户名和邮箱，如下命令

```shell
# 配置用户名
git config --global user.name "John Doe"
# 配置邮箱
git config --global user.email johndoe@example.com
```

配置好本地账号信息之后，就可以使用了。实际上本地的账号可以任意设置，与远端账号关系不大，只是记录我们在本地仓库操作的用户信息而已。

### [2). 创建本地仓库](https://webtech.wiki/git/#/note/note-01?id=_2-创建本地仓库)

我们在命令行终端上进入到自己的工作目录，假如自己的工作目录在 `/home/pan/work/src` ，我们先进入这个目录，再执行 git 初始化命令即可，如下命令

```shell
# 进入工作目录
cd /home/pan/work/src
# 初始化git仓库
git init
```

初始化完成后当前目录变成了 `git` 的工作目录，此时在这个目录下会生成名为 `.git` 的隐藏目录，这个目录就是 `git` 保存文件变动信息的目录，本地的所有变动记录都在这里。这个目录不能删除，如果删除之后，工作目录将不再是一个 `git` 的工作目录。

### [3). 将本地代码提交到 git 缓存区](https://webtech.wiki/git/#/note/note-01?id=_3-将本地代码提交到-git-缓存区)

我们可以在本地添加一个代码文件，如下命令

```shell
# 创建一个c++源代码文件，或者手动创建
touch test.cpp
```

这个时候，我们可以使用 git 将 `test.cpp` 源代码文件提交到 `git` 缓存区，使用以下命令

```shell
# 将 test.cpp 文件提交到 git 缓存区
git add test.cpp
```

或者使用另一个命令

```shell
# 将当前目录所有文件提交到 git 缓存区
git add .
```

### [4). 将缓存区的代码提交到本地仓库](https://webtech.wiki/git/#/note/note-01?id=_4-将缓存区的代码提交到本地仓库)

使用下面命令将代码提交到本地仓库，就完成托管了，如下命令

```shell
# 将本地git缓存区代码提交到本地仓库，-m 参数后面是提交备注，提交后才能传到远程仓库
git commit -m "first commit"
```

提交到 git 仓库的代码，我们就可以使用 `git` 的很多实用功能，如回退代码、查看代码变动历史等等。

### [5). 将代码提交到远程仓库](https://webtech.wiki/git/#/note/note-01?id=_5-将代码提交到远程仓库)

本地仓库的可以提交到远程仓库，远程仓库也可以实时同步最新代码到本地仓库。在实际项目中，一个稍微有规模的项目通常不是一个人开发的，而是多个人共同维护一套代码。下面是将本地代码同步到远端的基本操作：

假如我们在 github 仓库上创建了一个仓库，仓库的 git 地址为 https://github.com/ty123520/test.git,我们使用以下命令即可将本地代码提交到远端仓库。

```shell
# 将本地仓库绑定远程仓库
git remote add origin https://github.com/ty123520/test.git
# 将本地的当前master分支代码提交待远端master/main（根据自己想要创建的分支名称）
git push -u origin main
`fatal: unable to access ‘https://github.com/…/.git’: Could not resolve host: github.com 错误；提示无法解析主机github.com；`
```

进行账号验证之后，即可成功将代码推送到远端 git 仓库。

### [6）. 查看文件差异](https://webtech.wiki/git/#/./note/note-02?id=_4-查看文件差异)

在 git 仓库中的文件，都会被 git 跟踪，如文件修改历史、是否是新文件、文件提交历史等等。

`git diff` 命令用于查看的文件的差异，我们可以通过该指令对比文件的各种差异，以下是一些常用指令

```
# 比较所有文件与缓存区文件差异
git diff
# 比较当前文件和缓存区文件差异
git diff <file>
# 比较两次提交之间的差异
git diff <id1> <id2>
# 在两个分支之间比较
git diff <branch1> <branch2>
# 比较缓存区和版本库差异，与下一条指令的效果一样
git diff --staged
# 比较缓存区和版本库差异，与上一条指令的效果一样
git diff --cached
# 仅仅比较统计信息
git diff --stat
```

但要注意的是，只有使用 `git add` 指令将文件文件到缓存区之后，文件的信息才会被记录，接下来我们做个简单演示，修改`main.cpp`，添加以下代码

```cpp
#include <iostream>
using namespace std;

int main(){
  cout << "hello world" << endl;
}Copy to clipboardErrorCopied
```

通过`git diff`命令查看工作区与缓存区的文件差异，如下图：

<img src="C:\Users\田\AppData\Roaming\Typora\typora-user-images\image-20240304143624609.png" alt="image-20240304143624609" style="zoom:50%;" />

### [7）. 将本地仓库提交同步到远程仓库](https://webtech.wiki/git/#/./note/note-02?id=_5-将本地仓库提交同步到远程仓库)

我们使用以下命令将本地代码推送到远端仓库

```
# 将当前分支 (默认是master) 推送到远端仓库的main分支
git push -u origin main
```

实际上提交成功之后，远端的 `main.cpp` 文件是空的，是因为本地版本库还是一个空的 `main.cpp`文件。虽然`main.cpp` 文件被修改了，但还未提交到本地的版本库。所以还需要进行下面的操作

```shell
# 将所有变更添加的缓存区
git add .
# 将缓存区的变更提交到 本地git版本库
git commit -m "修改main.cpp文件"
# 将本地版本库提交到远端仓库
git push -u origin master
```

此时，我们再浏览远端仓库，可以看到代码已经同步过去了。

## 2、遇到的问题

### 1).无论是clone还是pull、push都会遇到以下问题：

```shell
`fatal: unable to access ‘https://github.com/…/.git’: Could not resolve host: github.com 错误；提示无法解析主机github.com；`
```

##### 问题原因

发现问题的根源出在使用了proxy代理，所以要解决该问题，核心操作就是要取消代理

##### 问题解决

Git取消代理
当出现以上报错的时候，你第一步可以采取的方法就是通过以下Git命令取消proxy代理，然后再回到你前面要执行的步骤.
解决：通过git取消代理：

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```



