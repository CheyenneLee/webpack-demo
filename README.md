上面讲到的解决方案，目的是为了建立一个匹配本地和远端git的钥匙，这把钥匙是一串密码，本地使用这串要是只能开一把某一个 github 用户下的仓库。

情景二： 如果我们有两个 github 的用户 A 和 B， 当切换到另外一个用户 B，并在本地 clone 了这个用户下的一个仓库， 这时当 `git pull ` 的时候又会遇到 同样的 权限问题： `ERROR: Permission to XXX.git` 或者 `master -> master (Permission denied)`.

这是因为在本地中已经配置了一个 ssh的 公钥（id_rsa.pub）， 而这个公钥是 匹配的是 github 用户 A 的锁, 当我们切换到 github 用户 B，的时候，这把公钥没有权限的。

### 解决方案；
1. 生成一个新的 SSH KEY （id_rsa  id_rsa.pub）
```bash
# 进入 .ssh 目录
cd ~/.ssh/
# 新建另外一个 ssh key
ssh-keygen -t rsa -C "your_email@example.com"
```
这里不要一路回车；会有一个提醒 去填写一个 key 的保存路径， 填写一个新的 id_rsa 的路径


2. 查看新的 id_rsa_XX, 并复制 
3. 在 切换后的 github 上面新加一个 ssh key （settings-> ）

这时候虽然已经新建一把公钥连接了新的github 用户和本地，但是问题还没有解决，ssh 并不能区分两把钥匙，可以做一下测试： 
ssh -T github



下面需要将本地的两把钥匙做一下区分，以便在使用 `git pull ` 的时候，让 git 知道该使用那一把钥匙
1. 打开 `~/.ssh/config` 文件  如果没有就新建一个。
2. 编辑 config 内容：
```bash 
# default github setting 
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa

# another user rsa pub 
Host github-cheyenne
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_cheyenne
```

3. 替换 git 仓库的地址。
```bash 
git remote -v # 查看仓库地址
```

git@github.com:CheyenneLee/webpack-demo.git

https://github.com/CheyenneLee/webpack-demo.git