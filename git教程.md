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

    # 二，本地文件更新

    1. 添加文件夹下的文件到git的暂存区，输入`git add .`或者`git add 文件名`。
    2. 加上标签，输入`git commit -m “这是第二次上传”`。
    3. 然后上传至GitHub，输入`git push origin main`。
    
    ---
    
    