## git 相关

git 是 版本控制工具

### 一、下载安装

1. 先下载安装 git 

2. 去 [码云](https://gitee.com/) 或者[github](https://github.com/) 注册账号

3. git 全局设置

    ```
    git config --global user.name '你的用户名'

    git config --global user.email '注册时使用的邮箱'

    ```

### 二、链接 本地仓库 和 远程仓库

1. 本地设置和生成 公钥 ：[教程](https://help.gitee.com/base/account/SSH%E5%85%AC%E9%92%A5%E8%AE%BE%E7%BD%AE)

2. 复制生成的公钥到对应的网站上进行绑定配置：[码云公钥](https://gitee.com/profile/sshkeys)、[github公钥](https://github.com/settings/keys)

3. 完成上述步骤之后，就跟远程仓库做了一个链接了

4. 上传代码：

    - 创建远程仓库，到码云或者github网站上创建一个仓库并完成初始化操作

    - 下载仓库代码：``git clone https://xxname/仓库名字``
    
    - 写完代码之后，提交所有代码到本地暂存区：  ``git add .``

    - 提交所有代码到本地仓库：``git commit -m '注释（写提交的说明）'``
    
    - 上传本地仓库到网站：``git push``


### 三、git 本地仓库使用

#### 3.1. 操作命令

1. 初始化仓库：``git init``

2. 查看配置：``git config -l``

3. 查看暂存状态：``git status``

    - 红色的文件：还没有被git管理的，处于未暂存状态

    - 绿色的文件：存储在了 本地仓库暂存区

4. 添加所有文件到暂存区：``git add .``，添加指定文件：``git add ./xx``

5. 添加到分支中：``git commit -m '注释说明'``

6. 查看修改文件：

    - 查看所有文件修改内容：``git diff``

    - 查看某个文件的修改内容：``git diff xx.js``

7. 查看修改历史(用户、时间、具体文件)：

    - 查看所有文件修改历史：``git log``

    - 查看某个文件的修改历史：``git log xx.js``

8. 查看修改历史(简单形式)：

    - 简单查看所有文件修改历史：``git reflog``

    - 简单查看某个文件的修改历史：``git reflog xx.js``

9. 回到之前的版本（每次 commit 都生成一个唯一版本号）：

    - 回到上一个版本：  ``git reset --hard HEAD^``

    - 回到指定的版本：   ``git reset --hard 版本号``



### 四、多人开发

1. 克隆下载代码：``git clone xxx``

2. 配置（下面是局部配置，主要是用于标识代码提交者）：

    ``git config user.name '你的用户名'``

    ``git config user.email '邮箱'``

3. 提交本地仓库：``git add .`` 、 ``git commit -m 'xxx'``

4. 提交远程仓库：``git push``，但是可能会报错：

    - 因为已经有人更新了远程仓库的代码，因此不能直接 push

    - 先下拉代码：``git pull``，然后再进行提交：``git push``

    - 如果下拉的代码有冲突，要先手动解决代码冲突问题，然后再 push


### 五、分支操作

默认主分支：master， 并且各分支之间是独立的，不会互相影响

***注意***：在一个分支下创建一个新的分支，新的分支会继承旧分支的操作（commit提交记录等）

1. 查看分支：

    - 本地仓库：``git branch``

    - 远程仓库：``git branch -r``

2. 新建分支：``git branch 分支名称``

3. 切换分支：``git checkout 分支名称``

    - 创建新分支并切换到新分支：``git checkout -b 分支名称``

4. 提交本地分支到 github\gitee 网站：``git push --set-upstream origin 分支名称``

5. 删除分支：

    - 删除本地分支（先切换其他分支）：``git branch -d 分支名称``

    - 删除远程仓库分支：``git push origin --delete 分支名称 ``

6. 合并分支：``git merge 分支名称``

    - 当前分支为 master，假如合并 dev 分支，就是：``git merge dev``

        相当于： master += dev

    - 然后手动解决冲突问题 


### 六、gitflow 工作流

- master        用于保存上线版本代码

- develop       用于保存相对稳定版本的代码，该分支由 master 分支创建

- feature       用于开发某几个功能，不同的功能可能会创建不同的分支，所有的feature都是由 dev 分支创建的

- release       用于代码上线前的准备 (测试，bug修复) ，也是由 dev 分支创建

- bugfix        用于修复不紧急 bug

- hotfix        用于修复紧急 bug


1. 领导（上级）创建项目，然后就有了 master 分支，然后再创建 dev 分支

2. 根据开发需求，需要开发 a、b 功能，然后领导从 dev 分支中 创建了 feature/a、feature/b 两个分支

3. 然后两个前端仔接到任务，分别 clone 项目到各自本地，然后使用 ``git checkout -b feature/a、feature/b`` 分支

4. 两人分别进行开发、最后提交 ``git push ----set-upstream origin feature/a（b同理）``

5. 领导 ``git pull`` 拉取代码，然后 ``git merge origin/feature/b(a)`` 进行代码合并，并且检查冲突问题，测试代码的时候，创建 release 分支

6. 由测试同学 对测试分支 release 进行测试，如果有 bug，再创建 bugfix 或者 hotfix 分支进行修复 bug

7. 改完 bug 之后，合并到 dev 分支，最后领导检查 dev 分支， 最后上线