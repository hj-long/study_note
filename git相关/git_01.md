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

10. 