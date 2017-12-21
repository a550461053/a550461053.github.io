---
title: github_blog的使用
---
## github做blog方法比较
略

## github使用

0. 选择问题：
	- 上传代码库：github，完美
	- 写博客平台：
		+ csdn：交互多
		+ github.io：300M上限，贴图麻烦
		+ WordPress：需要搭建服务器，麻烦，但功能多，爬虫、SEO
	- 写博客目的：
		+ 如果为了提高知名度，网上都是团队来做，而且偏简单的教程反而受众
		+ 如果为了工作面试，那时间不如用来专做技术，面试一谈便知深浅
		+ 总结就是，技术没做好，没必要写，技术有了一定的积淀，再去写，反而有含金量，容易推广
1. 创建版本库:
	- cd mygit
	- git config --global user.name "yuziqi"
	- git config --global user.email "a550461053@gmail.com"
	- git config user.name 
	- git config user.email
	- git init
	- nautious .git
	- vim 1.py
2. 文件添加到版本库: 
	- 添加到暂存区: git add 1.py
	- 文件提交到仓库: git commit -m "注释信息"
	- 查看是否还有未提交的文件: git status
	- 查看修改内容: git diff 1.py
	- 提交修改:
		- git add 1.py
		- git status -s ; 或者 git diff
		- git commit -m "文件增加了一行"
		- git status
3. 版本回退:
	- 查看版本历史记录: git log --pretty=oneline
	- 直接回退版本: git reset --hard HEAD
		- git reset --hard HEAD^ 上个版本
		- git reset --hard HEAD~100 上100个
	- cat 1.py
	- git log --pretty=oneline
	- 查看回退的版本号为948a21b: git reflog
	- git reset --hard 948a21b
4. 撤销修改:
	- 法一: 重新提交源文件
	- 法二: git reset --hard HEAD~100
	- 法三: git checkout -- 1.py : 撤销工作区的修改
		- 1.py修改后还没放到暂存区
		- 1.py已经放到暂存区了,就回到添加暂存区后的状态
5. 删除文件:
	- rm 2.py
	- git add 2.py
	- git rm 2.py
	- 继续恢复: git checkout -- 2.py
6. 远程仓库github：
	- 1. 远程github创建一个repo, 勾选README.md
	- 2. 克隆到本地: 
		- git clone https://github.com/a550461053/matlab-seg.git
		- 指定深度为1: git clone git://xxoo --depth 1
	- 3. 上传文件: git init
		- touch README.md
		- git add README.md
		- git commit -m 'first test commit'
		- 执行一次就够了，git remote add origin https://github.com/a550461053/matlab-seg.git 
		- git push origin master
	- 4. push文件:
		- git add .
		- git commit -m 'first commit'
		- git remote add origin https://github.com/a550461053/matlab-seg.git
		- git push origin master
	- 5. 报错处理
		- fatal: remote origin already exists
			- 先执行: git remote rm origin
		- error:failed to push som refs to.......
			- 先执行: git pull origin master
	- 6. 设置免密码登录

7. 创建分支：
	- 创建分支：git branch dev
	- 切换分支：git checkout dev
	- 创建+切换：git checkout -b dev
	- 查看当前分支：git branch
	- 合并分支：在master下，git merge dev
	- 删除分支：git branch -d dev
	- 
8. 多人协作：
	- 查看远程库：git remote ； git remote -v
	- 推送分支：
		+ master始终要与远程同步
		+ bug分支可以先合并到master
		+ git push origin master
		+ git push origin dev
	- 抓取分支：
		+ 先获取最新提交：
		+ git branch --set-upstream dev origin/dev
		+ git pull
	- 一般协作过程：
		+ 1.视图git push origin branch dev推送自己的修改
		+ 2.若推送失败，则因为远程分支比你的本地更新早，需要先git pull合并
		+ 3.若合并有冲突，需要解决冲突，并本地提交；在用git push origin branch dev推送
9. 常用命令：
	- cd G:/
	- 查看历史记录：git log
	- 查看历史记录的版本号：git reflog
	- 对工作区的文件修改撤销：git checkout 2.py

## Hexo+github

1. Node.js:
	- sudo apt install nodejs
	- nodejs -v
	- sudo apt install npm
	- npm -v
2. git
	- sudo apt install git
3. Hexo：
	- mkdir hexo
	- npm install hexo-cli
	- hexo init blog
		+ hexo不能用，一般是没装好，
		+ npm不支持nodejsv9，所以需要npm uninstall -g npm删除原有的npm
		+ 然后再次执行hexo init blog即可
		+ 后来再次使用npm不行了，但sudo可以
		+ 使用hash -r即可
	- cd blog
	- npm install 安装依赖项
	- hexo clean # 删除hexo g生成的静态网页
	- hexo g # or: hexo generate生成静态网页，public文件夹
	- hexo s # or: hexo server,http://localhost:4000/运行本地服务器，查看网页
	- 此时打开：localhost:4000即可访问
4. Hexo-部署
	- 借用github pages：
		+ 仓库存放个人主页，但仓库名字得是username/username.github.io
		+ 通过http://username.github.io访问主页
		+ 个人主页的网站内容是在master分支下的
	- 创建自己的github pages：
		+ 也就是创建一个repo：batista.github.io
		+ 提交一次git commit
		+ 通过连接http://batista.github.io
	- 配置github：
		+ git config --global user.name "a550461053"
		+ git config --global user.email "550461053@qq.com"
		+ 创建公钥：
			* ssh-keygen -C '550461053@qq.com' -t rsa
			* 一直回车，直到生成~/.ssh/id_rsa.pub，打开
			* 在github的settings里面，直到id_rsa.pub文件的密钥复制进去即可
	- 部署hexo到github pages：
		+ 安装部署工具：
			* npm install hexo-deployer-git --save
		+ 修改_config.xml文件：
		+ 特别注意：type:这种写法的冒号后面一定要有空格，不然无效
		+ deploy:
			type: git
			repo: git@github.com:batista/batista.github.io.git
			branch: master
		+ 命令行：hexo d
	- 浏览器访问：https://a550461053.github.io/
	- 
5. Hexo-配置：
	- 修改主题：
		+ 安装主题：
			+ hexo clean 
			+ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
			+ 或者用next：
			+ git clone https://github.com/iissnan/hexo-theme-next themes/next
		+ 启动主题：
			* 修改hexo目录下的config.yml文件中的theme属性，设置为yilia或者为next
		+ 更新主题：
			* cd themes/yilia
			* git pull
			* hexo g
			* hexo d
		+ 修改主题配置：
			* next下面也有一个_config.yml
			* 修改scheme：可以切换不同的样式
		+ PS: 有时候有点延迟
6. hello world：
	- hexo的文件在/source/_post下
	- hexo new "firstblog"
	- 会自动在_post下生成一个firstblog.md
	- 然后添加相应的内容即可
	- hexo d
7. 添加评论
	- 
8. 添加打赏
	- 
9. 多终端同步：
	- 在username.github.io下新建一个hexo分支
		+ 在hexo的根目录下新建hexo分支
		+ git init 
		+ git add source # 这里不使用git add . 是因为文件夹下含有node_model文件夹比较大，所以有选择的添加文件
		+ git commit -m "hexo blog source file"
		+ git branch hexo # 新建hexo分支
		+ git checkout hexo # 切换到hexo分支
		+ git remote add origin git@github.com:username/username.github.io.git # 本地与github对接
		+ git push origin hexo # push到github的hexo分支上
	- 另一端clone和push更新：
		+ git clone -b hexo git@github.com:yourname/yourname.github.io.git  #将Github中hexo分支clone到本地
		+ cd  yourname.github.io  # 切换到刚刚clone的文件夹内
		+ npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
		+ hexo new post "new blog name"   //新建一个.md文件，并编辑完成自己的博客内容
		+ git add source  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
		+ git commit -m "XX"
		+ git push origin hexo  //更新分支
		+ hexo d -g   # push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
	- 更新hexo：
		+ B端更新后，A端也要更新：
		+ git pull origin hexo # pull完成本地与远程的融合
		+ 然后后面就一样了
		+ hexo new "..."
		+ git add source 
		+ git commit -m ".."
		+ git push origin hexo
		+ hexo d -g
		+ 
