# 安装Sass和Compass #
> sass基于Ruby语言开发而成，因此安装sass前需要安装Ruby。（注:mac下自带Ruby无需在安装Ruby!）

#### window下安装SASS首先需要安装Ruby，先从官网下载Ruby并安装。安装过程中请注意勾选Add Ruby executables to your PATH添加到系统环境变量。 ####
**测试安装有没有成功,运行CMD输入以下命令：**

    ruby -v
    //如安装成功会打印
    ruby 2.2.2p95 (2015-04-13 revision 50295) [i386-mingw32]
**如上已经安装成功。但因为国内网络的问题导致gem源间歇性中断因此我们需要更换gem源。（使用淘宝的gem源https://ruby.taobao.org/）如下：**

    //1.删除原gem源
    gem sources --remove https://rubygems.org/
    
    //2.添加国内淘宝源
    gem sources -a https://ruby.taobao.org/
    
    //3.打印是否替换成功
    gem sources -l
    
    //4.更换成功后打印如下
    *** CURRENT SOURCES ***
    https://ruby.taobao.org/
**Ruby自带一个叫做RubyGems的系统，用来安装基于Ruby的软件。我们可以使用这个系统来 轻松地安装Sass和Compass。要安装最新版本的Sass和Compass，你需要输入下面的命令：**

    //安装如下(如mac安装遇到权限问题需加 sudo gem install sass)
    gem install sass
    gem install compass
**在每一个安装过程中，你都会看到如下输出：**

    Fetching: sass-3.x.x.gem (100%)
    Successfully installed sass-3.x.x
    Parsing documentation for sass-3.x.x
    Installing ri documentation for sass-3.x.x
    Done installing documentation for sass after 6 secon
    1 gem installed
**安装完成之后，你应该通过运行下面的命令来确认应用已经正确地安装到了电脑中：**

    sass -v
    Sass 3.x.x (Selective Steve)
    
    compass -v
    Compass 1.x.x (Polaris)
    Copyright (c) 2008-2015 Chris Eppstein
    Released under the MIT License.
    Compass is charityware.
    Please make a tax deductable donation for a worthy cause: http://umdf.org/compass
**如下sass常用更新、查看版本、sass命令帮助等命令：**

    //更新sass
    gem update sass
    
    //查看sass版本
    sass -v
    
    //查看sass帮助
    sass -h
# 编译sass #
> sass编译有很多种方式，如命令行编译模式、sublime插件SASS-Build、编译软件koala、前端自动化软件codekit、Grunt打造前端自动化工作流grunt-sass、Gulp打造前端自动化工作流gulp-ruby-sass等。

**命令行编译;**

    //单文件转换命令
    sass input.scss output.css
    
    //单文件监听命令
    sass --watch input.scss:output.css
    
    //如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
    sass --watch app/sass:public/stylesheets

#### 命令行编译配置选项; ####
**命令行编译sass有配置选项，如编译过后css排版、生成调试map、开启debug信息等，可通过使用命令sass -v查看详细。我们一般常用两种--style``--sourcemap。**

    //编译格式
    sass --watch input.scss:output.css --style compact
    
    //编译添加调试map
    sass --watch input.scss:output.css --sourcemap
    
    //选择编译格式并添加调试map
    sass --watch input.scss:output.css --style expanded --sourcemap
    
    //开启debug信息
    sass --watch input.scss:output.css --debug-info
- --style表示解析后的css是什么排版格式;
sass内置有四种编译格式:nested` `expanded` `compact` `compressed。
- --sourcemap表示开启sourcemap调试。开启sourcemap调试后，会生成一个后缀名为.css.map文件。

**四种编译排版演示;**

    //未编译样式
    .box {
      width: 300px;
      height: 400px;
      &-title {
    height: 30px;
    line-height: 30px;
      }
    }
**nested 编译排版格式**

    /*命令行内容*/
    sass style.scss:style.css --style nested
    
    /*编译过后样式*/
    .box {
      width: 300px;
      height: 400px; }
      .box-title {
    height: 30px;
    line-height: 30px; }
**expanded 编译排版格式**

    /*命令行内容*/
    sass style.scss:style.css --style expanded
    
    /*编译过后样式*/
    .box {
      width: 300px;
      height: 400px;
    }
    .box-title {
      height: 30px;
      line-height: 30px;
    }
**compact 编译排版格式**

    /*命令行内容*/
    sass style.scss:style.css --style compact
    
    /*编译过后样式*/
    .box { width: 300px; height: 400px; }
    .box-title { height: 30px; line-height: 30px; }
** compressed 编译排版格式**

    /*命令行内容*/
    sass style.scss:style.css --style compressed
    
    /*编译过后样式*/
    .box{width:300px;height:400px}.box-title{height:30px;line-height:30px}
**软件方式编译;**

> 推荐koala&codekit,它们是优秀的编译器，界面清晰简洁，操作起来也非常简单。

**LESS/Sass 编译工具Koala介绍**
> 易上手的Sass编译工具koala支持多个环境的安装文件 下载Koala
> 
> koala是一个国产免费前端预处理器语言图形编译工具，支持Less、Sass、Compass、CoffeeScript，帮助web开发者更高效地使用它们进行开发。跨平台运行，完美兼容windows、linux、mac。
