原文链接： [[Jekyll + Github Pages 搭建个人免费博客 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/87225594)]

**Jekyll** 的核心是一个文本转换引擎。它的方便之处在于支持多种文本标记语言：Markdown，Textile，HTML，然后 Jekyll 就会帮你加入你选择主题的样式的布局中。最终生成你自己的静态博客网站。

##   
**博客搭建步骤**

**1.安装环境**  
**1.1 安装Ruby**  
官网地址：[https://rubyinstaller.org/downloads/](https://link.zhihu.com/?target=https%3A//rubyinstaller.org/downloads/)  
根据自己的需要下载不同版本，直接点击“下一步”就可轻松安装成功。

**1.2 安装RubyGems**  
官网地址：[https://rubygems.org/pages/download](https://link.zhihu.com/?target=https%3A//rubygems.org/pages/download)  
据自己的需要下载不同版本。解压压缩文件到本地。在 CMD 命令窗口执行如下命令：

```text
cd D:\下载\rubygems-3.0.6\rubygems-3.0.6	#切换文件目录 
ruby setup.rb        #安装
rubygems ruby -v     #查看rubygems版本号
```

  
**1.3 安装Jekyll**  
以上两个步骤操作完成后，在 CMD 窗口执行如下命令安装Jekyll：

```text
gem install jekyll   #安装jekyll  
jekyll -v    #查看jekyll版本号
```

  
**2.本地搭建博客  
2.1 项目启动**

```text
jekyll new restlessManBlog   #新建博客 
cd restlessManBlog           #切换目录 
jekyll server                #启动项目
```

  
项目启动日志如下：  

![](https://pic3.zhimg.com/80/v2-5d3fe0290ff5cdfa5dea3fe01a69cb6a_1440w.webp)

  
在浏览器访问：[http://localhost:4000/](https://link.zhihu.com/?target=http%3A//localhost%3A4000/)

  
**2.2 添加 MarkDown 文档**  
在项目根目录下的 _posts 目录创建 markdown 文档。这里注意 md 文档命名要添加 “yyyy-mm-dd”的前缀。  
例如：2019-10-11-5分钟搭建博客.md

  
**2.3 部署代码到 Github**  
**2.3.1 创建 Github 账号**  
注：这里我使用的 Github 托管静态博客的，你也可以选择把代码托管到 **码云** 或者其他平台上。  
没有 Github 账号的朋友可以注册一个账号，有账号的朋友可跳过。

  
**2.3.2 创建代码仓库**  
创建一个名称为 ‘账号名称.[http://github.io](https://link.zhihu.com/?target=http%3A//github.io)’。例如：我的账号名是**helloRestlessMan**，仓库名就是 [helloRestlessMan.github.io](https://link.zhihu.com/?target=http%3A//helloRestlessMan.github.io)

  
**2.3.3 部署代码到Github**  
在我们创建的博客的目录找到 _site 目录，将 _site 目录下的所有文件都提交到Github上。  

![](https://pic2.zhimg.com/80/v2-a4dc583cdc7c297a3e653e333474d79d_1440w.webp)

  
  
操作步骤：

```text
git clone https://github.com/helloRestlessMan/helloRestlessMan.github.io.git    
#克隆远程代码到本地 
拷贝_site 文件到 helloRestlessMan.github.io 
cd helloRestlessMan.github.io 
git add .   #git 命令添加所有文件 
git commit -m "创建 Jekyll 个人博客"      #git 提交文件 
git push    #git 推送代码到远程
```


  
**2.5 访问自己的博客网站**  
效果如下图：  

![](https://pic4.zhimg.com/80/v2-b9c10a847764e33f0aec50d419f903c3_1440w.webp)
#blog #博客 