用于在qqwry.dat里查找IP地址归属地。  
另提供一个从纯真网络更新qqwry.dat的小工具。

本工具已上传到pypi：https://pypi.python.org/pypi/qqwry-py3  
Python 3.4及以上版本自带了pip工具，执行此命令既可安装：

    pip install qqwry-py3

特点：

1. for Python 3.0+。
2. 提供两套实现供选择。有一个查找速度更快，但加载稍慢一点、占用内存稍多一点。
3. 在i3 3.6GHz，Python 3.5上查询速度达10.2万次/秒。
4. 仅import array和bisect这两个Python自带的模块。
5. 经过较严格测试。
6. 提供一个从纯真网络(cz88.net)更新qqwry.dat的小工具，用法见本文最后一部分。

用法
============
    from qqwry import QQwry
    
    q = QQwry()
    q.load_file('qqwry.dat', loadindex=False)
    result = q.lookup('8.8.8.8')

解释q.load_file(filename, loadindex=False)函数
--------------
加载qqwry.dat文件。成功返回True，失败返回False。

参数filename可以是qqwry.dat的文件名（str类型），也可以是bytes类型的文件内容。

﻿当参数loadindex=False时（默认）：  
﻿程序行为：把整个文件读入内存，从中搜索。  
﻿加载速度：很快  
﻿进程内存：较少，12.6 MB  
﻿查询速度：较慢，3.9 万次/秒  
﻿使用建议：适合桌面程序、中小型网站。  

﻿﻿当参数loadindex=True时：  
﻿程序行为：把整个文件读入内存。额外加载索引，把索引读入更快的数据结构。  
﻿加载速度：慢，因为要额外加载索引。  
﻿进程内存：较多，17.7 MB  
﻿查询速度：较快，10.2 万次/秒  
﻿使用建议：适合高负载服务器。  

（以上是在i3 3.6GHz, Win10, Python 3.5.0rc2 64bit，qqwry.dat 8.85MB时的数据）

解释q.lookup('8.8.8.8')函数
--------------
﻿﻿找到则返回一个含有两个字符串的元组，如：('国家', '省份')  
﻿没有找到结果，则返回一个None

解释q.get_lastone()函数
--------------
﻿返回最后一条数据，最后一条通常为数据的版本号  
﻿没有数据则返回None

解释q.is_loaded()函数
--------------
q对象是否已加载数据，返回True或False

解释q.clear()函数
--------------
清空已加载的qqwry.dat  
再次调用load_file时不必执行q.clear()

从纯真网络(cz88.net)更新qqwry.dat的小工具
============
    from qqwry import updateQQwry
    
    result = updateQQwry(filename)

参数filename可以是要保存的文件名（str类型）；  
参数filename也可以是None，这时函数直接返回qqwry.dat的文件内容，一个bytes对象。  

updateQQwry函数返回值：  
正整数：表示已成功更新，是保存到磁盘的文件字节数。  
一个bytes对象：表示已成功更新，返回的是文件的内容。  

如果返回负数，表示更新失败：  
-1：下载copywrite.rar时出错  
-2：解析copywrite.rar时出错  
-3：下载qqwry.rar时出错  
-4：qqwry.rar文件大小不符合copywrite.rar的数据  
-5：解压缩qqwry.rar时出错  
-6：保存到最终文件时出错
