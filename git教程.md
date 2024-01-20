# 一，git上传GitHub

1. 在本地建立一个文件夹作为git本地仓库，在GitHub上建立一个同名的仓库。

2. 鼠标右击本地文件夹，选择open git bash ，进入git命令窗口。

3. 首先初始化，输入`git init`。

4. 然后链接到GitHub远程仓库，输入`git remote add origin 仓库http链接`。

5. 初始化邮箱和用户名：

   `git config user.email “vbfgde546@163.com”`
   `git config user.name “lpc”`

6. 添加文件夹下的文件到git的暂存区，输入`git add .`或者`git add 文件名`。

7. 新建一个分支main，输入`git checkout -b main`。

8. 然后从远程仓库拉代码（下载到本地并与本地文件合并），输入`git pull origin main`。

9. 加上标签，输入`git commit -m “这是第一次上传”`。

10. 然后上传至GitHub，输入`git push origin main`。

---

# 二，本地已存在文件更新

1. `git status`查看状态。
2. `git add .`或者`git add 文件名` 添加至git缓冲区（暂存区）。
3. `git commit -m “这是第n次提交”` 将文件提交至git本地仓库。
4. `git push origin main` 将文件上传至GitHub远程仓库。

注意：

​	若出现`fatal: unable to access 'https://github.com/lpcjack/typora-message.git/': Failed to connect to github.com port 443 after 21280 ms: Couldn't connect to server`报错，说明本机系统代理端口和git端口不一致。解决方案参考以下网址：[解决方案](http://t.csdnimg.cn/vHNyV)

---

# 三，删除GitHub仓库上的文件

1. 首先`git status` 查看状态，解决冲突。
2. `git rm 文件名.后缀` 将文件从本地仓库和工作目录中删除。
3. `git commit -m` “这是删除的文件” 提交删除操作。
4. `git push origin main` 推送删除操作到远程仓库。

---

# 四，删除或者添加GitHub仓库上的文件，更新到本地git仓库。

1. `git pull origin main` 拉取远程变更，更新本地分支。

2. `git status`查看状态，若出现`On branch main nothing to commit, working tree clean`，说明成功。

---

