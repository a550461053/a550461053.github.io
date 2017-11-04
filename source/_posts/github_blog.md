# github blog

## github做blog方法比较

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
		+ hexo d -g   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master

