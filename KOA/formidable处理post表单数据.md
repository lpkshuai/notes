一般来说，客户端向服务端提交数据有GET和POST这两种方式，在之前的文章node.js当中的http模块与url模块的简单介绍 当中我们可以知道通过req.url与url模块的配合处理可以快速得到客户端通过GET方式向服务端提交的数据。而原生的node.js在处理客户端以POST方式提交的数据时，比较复杂，要写两个监听，并且要处理上传的图片、文件也比较艰难。故我们常用第三方模块包formidable来处理客户端以POST方式提交的表单、文件、图片等。

# 一、formidable处理POST方式提交的表单数据 #
### 1、下载并引包 ###
在当前的项目文件夹下，用命令-> npm install formidable 来在当前文件夹下载formidable。再通过const formidable = require('formidable');来引包。

### 2、创建基本的表单结构 ###
我们新建一个表单页面form.html放在服务端，与主文件1.js放在同一个目录下。如下图所示：

![](https://segmentfault.com/img/bVV5MW?w=1328&h=735)

在主文件1.js当中，我们设计路由为，当用户访问根目录时，呈递该表单页面，当用户完成表单信息填写，用submit进行提交之后，默认跳转至路由http://192.168.155.1:3000/dopost，(一定要加上指定的端口号)，则我们设计在这一条路由当中来使用formidable来进行处理。则1.js主文件的骨架代码为：

    const formidable = require('formidable');
    const fs = require('fs');
    const path = require('path');
    const http = require('http');
    var server = http.createServer((req,res)=>{
    	if(req.url == '/'){
    		var target = path.join(__dirname,'./form.html');
    		fs.readFile(target,(err,data)=>{
    		if(err) throw err;
    			res.writeHead(200,{"Content-Type":"text/html;charset=UTF8"});
    			res.end(data);
    		});
    	}else if(req.url == '/dopost' && req.method.toLowerCase() == 'post'){
    
    	}else{
    		res.writeHead(404,{"Content-Type":"text/html;charset=UTF8"});
    		res.end("找不到该页面！");
    	}
    });
    server.listen(3000,'192.168.155.1');
### 3、使用formidable来处理表单数据 ###
常用代码段为
    
    var form = new formidable.IncomingForm();
    form.parse(req,function(err,fields,files){});
其中当服务端全部接收完客户端用post方式提交的表单数据之后，触发执行该回调函数。以post方式提交的表单域数据都放在fields这个对象当中，以post方式上传的文件、图片等文件域数据都放在files这个对象当中。
则我们在第二条路由选择的分支当中的代码示例为：


    else if(req.url == '/dopost' && req.method.toLowerCase() == 'post'){
	    var form = new formidable.IncomingForm();
	    form.parse(req,function(err,fields,files){
	        if(err) throw err;
	        console.log(fields);
	        res.writeHead(200,{"Content-Type":"text/html;charset=UTF8"});
	        res.end('表单提交成功！');
	    });
    }
此时完成表单数据填写并提交之后，结果为：

![](https://segmentfault.com/img/bVV5SX?w=713&h=269)

# 二、formidable处理POST方式上传的文件或图片 #
通过上述方式进行下载与引包之后，form.html文件与主文件1.js仍处于同一目录下，form.html当中的示例代码为：

    <!DOCTYPE html>
    <html lang="en">
    <head>
	    <meta charset="UTF-8">
	    <title>form表单</title>
    </head>
    <body>
    <form action="http://192.168.155.1:3000/dopost" method="POST" enctype="multipart/form-data">
        <p><input type="file" name="uploadImg"></p>
        <p><input type="submit" value="提交"></p>
    </form>
    </body>
    </html>
> 当表单提交的过程中涉及文件或图片上传，则一定要设置表单头，即在form标签上加上固定写法的属性为enctype="multipart/form-data"，否则文件或图片会上传失败。其中` <input type="file" name="uploadImg"> `，当中的name属性一定要赋值。

其中主文件1.js的骨架代码也与上述的相似。当要使用formidable来处理上传的图片时，常用的代码段为：
    
    var form = new formidable.IncomingForm();
    form.uploadDir = targetFile;
    form.parse(req,function(err,fields,files){});
其中targetFile为一个自定义的变量，用于设置文件或图片上传的存放路径，为绝对物理路径。该目标地址的文件夹必须能实现存在，这样才能确保文件能顺利上传。其中当服务端全部接收完客户端用post方式提交的文件或图片之后，触发执行该回调函数。以post方式提交的表单域数据都放在fields这个对象当中，以post方式上传的文件、图片等文件域数据都放在files这个对象当中。则我们在第二条路由选择的分支当中的代码示例为：


    else if(req.url == '/dopost' && req.method.toLowerCase() == 'post'){
        var form = new formidable.IncomingForm();
        var targetFile = path.join(__dirname,'./upload');
        form.uploadDir = targetFile;
        form.parse(req,function(err,fields,files){
            if(err) throw err;
            console.log(files);
            res.writeHead(200,{"Content-Type":"text/html;charset=UTF8"});
            res.end('表单提交成功！');
        });
    }
在完成图片的选择与上传之后，结果为：

![](https://segmentfault.com/img/bVV6aH?w=781&h=707)

此时可以看到在主文件1.js的同一目录下的upload文件夹当中多了一个随机名字，并且没有后缀名的文件。由于没有后缀名，所以该图片在编辑器当中无法正常显示。

![](https://segmentfault.com/img/bVV6bb?w=729&h=251)

我们可以在接收到上传的图片之后，使用fs.rename()方法对其进行改名的操作，使其上传之后的文件名与之前的保持一致，并且包含后缀名的部分。
我们从控制台打印的信息可以看出其中files.uploadImg.path代表该图片上传之后存放的绝对物理路径。其中files.uploadImg.name代表该图片原来的文件名部分（包含扩展名的信息）。我们加入了上传改名的功能之后，第二条路由选择分支当中的示例代码为：

    else if(req.url == '/dopost' && req.method.toLowerCase() == 'post'){
        var form = new formidable.IncomingForm();
        var targetFile = path.join(__dirname,'./upload');
        form.uploadDir = targetFile;
        form.parse(req,function(err,fields,files){
            if(err) throw err;
            var oldpath = files.uploadImg.path;
            var newpath = path.join(path.dirname(oldpath),files.uploadImg.name);
            fs.rename(oldpath,newpath,(err)=>{
                if(err) throw err;
                res.writeHead(200,{"Content-Type":"text/html;charset=UTF8"});
                res.end('图片上传并改名成功！');
            })
        });
    }