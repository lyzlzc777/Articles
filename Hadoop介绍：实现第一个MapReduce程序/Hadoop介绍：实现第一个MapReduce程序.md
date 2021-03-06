# Hadoop介绍：实现第一个MapReduce程序for mac
本文所有流程、代码均在mac 10.13.2系统版本下测试通过。

## 1.下载并安装Cloudera VM和VirtualBox
### 安装VirtualBox
下载地址：https://www.virtualbox.org/wiki/Downloads

也可以直接下载本文使用的版本 [VirtualBox 5.1 builds](https://www.virtualbox.org/wiki/Download_Old_Builds_5_1)

### 下载Cloudera VM
Cloudera集成了hadoop生态环境中的大部分常用框架。
点击下面的链接安装：https://downloads.cloudera.com/demo_vm/virtualbox/cloudera-quickstart-vm-5.4.2-0-virtualbox.zip.
该程序大约4GB。下载后请解压

### 导入程序
打开VirtualBox，点击菜单栏File-> Import Appliance，然后点击文件夹图标
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/import.png?raw=true)
选择解压缩后的Cloudera文件‘cloudera-quickstart-vm-5.4.2-0-virtualbox.ovf’，然后一直点击继续。

### 启动Cloudera VM
当导入程序完成后，点击绿色Start按钮：
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/start.png?raw=true)
启动完成后进入桌面
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/desktop.png?raw=true)

## 2. 复制文件到HDFS  
打开浏览器，输入网址http://ocw.mit.edu/ans7870/6/6.006/s08/lecturenotes/files/t8.shakespeare.txt
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/menu.png?raw=true)
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/save%20page.png?raw=true)
点击菜单按钮下的Save Page，将文件保存在Downloads文件夹，文件名为words.txt

### 打开terminal
点击桌面菜单栏的黑色图标打开terminal
![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/terminal.png?raw=true)

输入命令进入Downloads文件夹：`cd Downloads`
输入ls确认'words.txt'文件存在：`ls`

### 拷贝文件
拷贝命令:`hadoop fs –copyFromLocal words.txt `
确认文件拷贝成功：`hadoop fs –ls`

### HDFS文件拷贝
命令：`hadoop fs -cp words.txt words2.txt `
查看：`hadoop fs -ls`

### 拷贝文件到本地
命令：`hadoop fs -copyToLocal words2.txt `
查看：`ls`

### 删除HDFS内的文件
命令:`hadoop fs -rm words2.txt`
查看：`hadoop fs -ls`

## 3. 运行第一个Hadoop程序：WordCount
该程序实现的功能是计算文件中单词出现的次数。

### 查看示例程序
虚拟机中打开terminal，执行命令：`hadoop jar /usr/jars/hadoop-examples.jar`
结果中列出了所有示例程序，其中包括我们要使用的wordcount程序

### 查看程序参数
执行命令：`hadoop jar /usr/jars/hadoop-examples.jar wordcount`
结果列出了该程序的参数，可以有一个或多个输入，得到一个输出。

### 执行程序
运行：`hadoop jar /usr/jars/hadoop-examples.jar wordcount words.txt out`
我们以之前下载的word.txt文件作为输入
得到输出文件out
等待map和reduce分别执行到100%即运行完成。

### 查看结果
运行：`hadoop fs –ls`
我们发现HDFS中多了一个文件夹out
进入文件件` hadoop –fs ls out`
文件‘part-r-00000’包含了程序运行的结果
我们首先将文件拷贝到本地`hadoop fs –copyToLocal out/part-r-00000 local.txt`
运行以下命令查看结果：`more local.txt`

![](https://github.com/freefrog1986/Articles/blob/master/Hadoop%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%AC%AC%E4%B8%80%E4%B8%AAMapReduce%E7%A8%8B%E5%BA%8F/result.jpeg?raw=true)

我们看到文件中记录了所有单词出现的次数！

至此，第一个MapReduce程序运行完成。



