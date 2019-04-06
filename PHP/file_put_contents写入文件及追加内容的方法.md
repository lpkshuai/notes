# **以追加形式写入内容** #

**当设置 flags 参数值为 FILE_APPEND 时，表示在已有文件内容后面追加内容的方式写入新数据：**



    <?php
    file_put_contents("test.txt", "This is another something.", FILE_APPEND);
    ?>
> file_put_contents() 的行为实际上等于依次调用 fopen()，fwrite() 以及 fclose() 功能一样。

*FILE_APPEND：在文件末尾以追加的方式写入数据*

**参数说明：**


- filename 要写入数据的文件名 
- data 要写入的数据。类型可以是 string，array（但不能为多维数组），或者是 stream 资源 
- flags 可选，规定如何打开/写入文件。
可能的值：	1.**FILE_USE_INCLUDE_PATH**：检查 filename 副本的内置路径
  			2.**FILE_APPEND**：在文件末尾以追加的方式写入数据
  			3.**LOCK_EX**：对文件上锁

- context 可选，Context是一组选项，可以通过它修改文本属性
### PHP 内置函数 file_put_contents 用于写入文件： ###

file_put_contents 函数最简单的写法，可以只用两个参数，一个是文件路径，一个是要写入的内容，语法如下：

**file_put_contents(filepath,data)**

> 如果文件不存在，file_put_contents 函数会自动创建文件；如果文件已存在，原有文件被重写。

> 你可以利用 file_put_contents 函数创建并写入一个新文件，或者重写一个原有文件。

**下面是一个使用 file_put_contents 函数的 PHP 代码示例：**

    <html>
    <body>
    <?php
    $path ="C:\\blabla\\filesys\\one.txt";
    $content = "one for all";
    file_put_contents($path,$content);
    if (file_exists($path))
     {echo "ok";}
    else
     {echo "ng";}
    ?>
    </body>
    </html>
该 PHP 代码示例会创建一个路径为 C:\blabla\filesys\one.txt 的文件，该文件的内容是 one for all 。

### PHP 内置函数 file_put_contents 用于追加内容： ###

如果你想在一个已有文件上追加内容，你也可以使用file_put_contents 函数，只需要加一个参数即可。

**file_put_contents(filepath,data,flags)**

> 当 flags 的值为 FILE_APPEND 时，表示在已有文件上追加内容。即：第三个参数flags实现将内容追加到文件的后面，如果没有这个参数会直接覆盖以前的数据。

比如我们要在上面示例的C:\blabla\filesys\one.txt 文件上追加内容，我们可以这样写：


    <html>
    <body>
    <?php
    $path ="C:\\blabla\\filesys\\one.txt";
    $content = " all for one";
    file_put_contents($path,$content,FILE_APPEND);
    if (file_exists($path))
     {echo "ok";}
    else
     {echo "ng";}
    ?>
    </body>
    </html>
执行该 PHP 文件之后，我们再看 C:\blabla\filesys\one.txt 文件，发现文件内容增加了，变成了：

one for all all for one

> file_put_contents 函数返回写入文件的字节数 (number of bytes) ，如果出错，返回 FALSE。