---
layout:       post
title:        Mac中利用R、emacs、ess搭建R运行环境
category: usedtools
tags: [Mac,Emacs,R]

---

{% include JB/setup %}

###一、安装软件
1、 首先在[R官网](http://www.r-project.org/)，下载最新版本的R软件，注意选择相对应的系统版本（mac、linux、windows），安装过程一路next。
2、 在[emacsformac官网](http://emacsformacosx.com/)中下载emacs。安装过程一路next。

###二、配置R软件
R启动时会调用`R_HOME/etc/Renviron`中的配置信息来配置R运行环境，`R_HOME`在R终端使用`Sys.getenv("R_HOME")`命令可以得到。一般我们不直接修改`Renviron`，为了便于以后升级，我们在用户的家目录下新建`.Rprofile`文件，将我们自定义的启动环境放在里面。我的`.Rprofile`很简单，就以下几条命令

*     设置包的安装路径，以后R升级，不用重新一一安装所有包，使用`update.packages()`就能更新包了。注意`“~/.R/lib”`这个文件夹一定要是存在的，不存在要新建一个。
`.libPaths("~/.R/lib")`
* 设置包下载链接的站点，不用每次都去选择，这两个是国内较快的。
{% highlight cl %}            
	options(repos = c(CRAN = "http://mirrors.ustc.edu.cn/CRAN/",       
		              CRAN2 = "http://ftp.ctex.org/mirrors/CRAN/"))        
{% endhighlight %}	    
* 自动加载常用包，这里是ggpolt2包。       
{% highlight cl %}           
	if (interactive()) {        
	suppressMessages(require(ggpolt2))       
	options(warn = 1)      
	}       
{% endhighlight %}      
###三、配置emacs软件          
在家目录`“~/”`下找到`.emacs`文件和`.emacs.d`文件夹，没有就新建。再在`.emacs.d`中新建`plugins`文件夹，放手工下载安装的插件。以下就是我的`.emacs`的内容。    
* 设置用户细节           
{% highlight cl %}            
	(setq user-full-name "zysw")             
	(setq user-mail-address "zysw01@gmail.com")                
{% endhighlight %}               
* 设置环境                    
{% highlight cl %}        
	(setenv "PATH" (concat "/usr/local/bin:" (getenv "PATH")))      
	(require 'cl)       
{% endhighlight %}       
* 设置包管理工具     
{% highlight cl %}       	
	(load "package")       	    
	(package-initialize)    
	(add-to-list 'package-archives       
                '("marmalade" . "http://marmalade-repo.org/packages/"))       
		(add-to-list 'package-archives         
                '("melpa" . "http://melpa.milkbox.net/packages/") t)       
		(setq package-archive-enable-alist '(("melpa" deft magit)))        
{% endhighlight %}    
* 设置默认包列表     
{% highlight cl %}          
	(defvar zysw/packages '(ac-slime     
                          auto-complete     
                          smartparens    
                          magit     
                          markdown-mode       
                          org       
                          puppet-mode      
                          smex         
                          yaml-mode       
			  ess);;注意，ess是能够自动安装。      
		"Default packages")         
{% endhighlight %}         
* emacs启动时检测默认包是否安装，未安装则使用elpa安装       
{% highlight cl %}        
	(defun zysw/packages-installed-p ()          
		(loop for pkg in zysw/packages       
			when (not (package-installed-p pkg)) do (return nil)        
		    finally (return t)))            

   	(unless (zysw/packages-installed-p)      
	        (message "%s" "Refreshing package database...")        
	               (package-refresh-contents)        
		(dolist (pkg zysw/packages)       
		   (when (not (package-installed-p pkg))       
	         (package-install pkg))))          
{% endhighlight %}            
   
------------------------------
           设置启动操作
-----------------------------

* 去掉启动画面（Splash Screen）        
{% highlight cl %}        
	(setq inhibit-splash-screen t        
		initial-scratch-message nil)       
{% endhighlight %}      
* 去掉工具栏、滚动条        
{% highlight cl %}           
	(scroll-bar-mode -1)     
	(tool-bar-mode -1)       
	(menu-bar-mode 1)       
{% endhighlight %}      
* 设置显示括号的样式       
{% highlight cl %}        
	(setq show-paren-style 'mixed)    
{% endhighlight %}       
* 光标与鼠标重合时，移开鼠标    
{% highlight cl %}       
	(mouse-avoidance-mode 'animate)    
{% endhighlight %}     
* 设置其他的插件位置，这里设置在.emacs.d中新建的plugins文件夹中。  
{% highlight cl %}    
	(defvar zysw/plugins  (expand-file-name "plugins" "~/.emacs.d/"))   
	(add-to-list 'load-path  zysw/plugins)   
	(dolist (project (directory-files   zysw/plugins t "\\w+"))     
		(when (file-directory-p project)    
		(add-to-list 'load-path project)))   
{% endhighlight %}    
* 加载主题，Amelie-theme.el要下载放入plugins文件夹中。   
{% highlight cl %}    
	(add-to-list 'custom-theme-load-path zysw/plugins)      
	(load-theme 'Amelie t)       
{% endhighlight %}     
* 显示列号    
{% highlight cl %}      
	(column-number-mode 1)    
{% endhighlight %}    
* 启动 Emacs 服务，下次打开文件时使用同一个 Emacs    
{% highlight cl %}    
	(server-start)      
{% endhighlight %}     
* 设置自动补全      
{% highlight cl %}       
	(require 'smartparens-config)    
	(smartparens-global-mode t)      
	(show-smartparens-global-mode t)      
	(add-hook 'ess-R-post-run-hook 'smartparens-mode) ;;R专用   
{% endhighlight %}    
* 设置自动匹配   
{% highlight cl %}    
	(require `auto-complete-config)   
	(ac-config-default)   
{% endhighlight %}   
* 配置Smex,Smex非常必要，他可以提供历史和搜索当你M-X时。   
{% highlight cl %}   
	(require 'smex) ; Not needed if you use package.el   
	(smex-initialize) ; Can be omitted. This might cause a (minimal) delay   
	(global-set-key (kbd "M-x") 'smex)   
	(global-set-key (kbd "M-X") 'smex-major-mode-commands)    
{% endhighlight %}   
* 设置ido   
{% highlight cl %}    
	(ido-mode t)    
	(setq ido-enable-flex-matching t   
		ido-use-virtual-buffers t)    
{% endhighlight %}   

-------------------------------------------
                       ESS配置
-------------------------------------------

* 加载ESS   
{% highlight cl %}   
	(require 'ess-site)   
	(org-babel-do-load-languages  
	'org-babel-load-languages    
		'((R . t)))    
{% endhighlight %}    
* ESS的代码缩进设置    
{% highlight cl %}    
	(add-hook 'ess-mode-hook    
        (lambda ()    
            (ess-set-style 'C++ 'quiet)    
            (setq comment-column 4) ; 把以#开始的行缩进4空格，免得难看    
            (show-paren-mode t)     ; 自动加亮跟踪括号    
            ess-indent-level 2    
            ess-continued-statement-offset 2    
            ess-brace-offset 0    
            ess-arg-function-offset 4    
            ess-expression-offset 2   
            ess-else-offset 0   
            ess-close-brace-offset 0   
		))   
{% endhighlight %}   
* 运算符书写规范,smart-operator要先下载，放入plugins文件夹中。   
{% highlight cl %}   
	(require 'smart-operator)    
		(add-hook 'ess-mode-hook 'smart-operator-mode)   
			(add-hook 'inferior-ess-mode-hook 'smart-operator-mode)   
{% endhighlight %}   
#### 本记录是从网上众多的朋友分享的点滴中，选择我需要的汇集而成，在此，我谢谢这些朋友。  
#### 根据需要，还会继续添加。有需要的朋友。可以自由copy。   
#### 作者：zysw   
#### 时间：2013.9.16   

