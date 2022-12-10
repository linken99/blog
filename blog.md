## 本地 Git ssh 连接自己的 github repository

#### 1.注册 git 账号并创建 repository

#### 2.本地下载安装 [git](https://git-scm.com/download/win)

#### 3.本地创建一个目录作为本地仓库，并在该目录中右键->"git bash here"并执行下列命令：

```shell
#直接三次回车创建默认的公私钥匙，并将公钥copy到 git hub上
ssh-keygen -t rsa -C "308134042@qq.com"	
ssh -T git@github.com	#测试公钥是否配置成功

git config user.name		#查看用户名
git config user.password	#查看密码
git config user.email	#查看email

git config --global user.name "Monkey"	#修改/配置全局用户名
git config --global user.password "xxxx"	#修改/配置全局密码
git config --global user.email "308134042@qq.com"	#修改/配置全局emai

git config --list	#查看全局配置

git clone git@github.com:linken99/blog.git
git add xxx
git commit -m "xxx"
git push
```















