# Linux 服务器常用命令以及注意事项
主要记录在日常工作常用的一些Linux命令，以及服务器项目配置信息

> 1. 检查服务器中某个进程的内存以及CPU使用情况
     
     top -p $(pidof 进程名) 如:top -p $(pidof mongod) 

> 2. 搜索目录的文件内容中是否包括某个字符串

     grep -r -n "内容" .

     
> 3. 关于ubuntu文件句柄查看方法与总结
     查处与有系统条句柄:
     ```bash
     $ lsof | wc -l 
     ```

     将所有系统句柄打开数据进行排序:
     ```bash
     $ lsof -n|awk '{print $2}'|sort|uniq -c|sort -nr|more\
     // 131 24204　
     // 57 24244
     // 3   1
     ```

     其中第一列是打开的句柄数，第二列是进程ID。

     可以根据ID号来查看进程名。

     ```bash
       $ ps aef|grep 24204
     ``` 

     修改文件句柄限制
     ```bash
     # ulimit -HSn 4096
     ```

     以上命令中，H指定了硬性大小，S指定了软性大小，n表示设定单个进程最大的打开文件句柄数量。个人觉得最好不要超过4096，毕竟打开的文件句柄数越多响应时间肯定会越慢。设定句柄数量后，系统重启后，又会恢复默认值。如果想永久保存下来，可以修改.bash_profile文件，可以修改 /etc/profile 把上面命令加到最后.

     也可以永远修改

     ```bash
     $ vim /etc/security/limits.conf
     ```
