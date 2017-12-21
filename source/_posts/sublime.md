---
title: sublime
date: 2017-12-21 10:41:32
tags:
---



## tools
1. tools>snippet:
	- new file: lorem + tab
	- new python file: !env + tab

## package
1. subl:
	- subl $PWD
2. package control:
	- key: ctrl+shift+p
3. theam: flatland	
	"theme": "Flatland Dark.sublime-theme",
	"color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme",
	"flatland_sidebar_tree_medium" : true,
4. sidebar: sidebarEnhancements
	- 同步颜色：SyncedSidebarBg
5. pythonIDE: Anaconda
	- pep8 :
		- sublime>preferences>package Settings>anaconda>settings  user: 
		- add : {"anaconda_linting": false,}
6. python version:
	- tools>Build System>New Build System:
	- add: 
{ 
	"cmd": ["/home/yuziqi/.local/virtualenvwrapper/env35/bin/python", "-u", "$file"], 
	"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)", 
	"selector": "source.python"
}
	- and use virtualenvwrapper:
		- workon env35
		- which python 
		- pip install ...
7. utf8:
	- linux : package install : Codecs33
		- ConvertToUFT8
	
8. markdown:
	- markdownEditing	
	- markdownPreview
		+ Preferences>keybindings:
			*  { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  }
		+ Preferences>Color Scheme
	- OmniMarkupPreviewer
		+ 一个顶俩
9. solidity:
	- ethereum: 安装后并修改view
作者：知乎用户
链接：https://www.zhihu.com/question/50098807/answer/133097092
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

10. InsertDate
	- 可以在文档中快速插入时间，初始快捷键是Ctrl+F5。

11. DocBlockr
	- 一款支持自动生成注释的插件，比如下面给main函数添加注释，先按/**后敲击回车，会出现如下注释.

## settings
1. user:
	- preferences>settings>user
{
"auto_complete": false,
"sublimelinter": false,
"tab_size":4,
"dpi_scale": 1.0,
"font_face": "Sans Mono",
"font_size": 13,
"word_wrap": true
}	

双击侧边栏箭头：对齐侧边栏
theme: flatland
