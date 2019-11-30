









1，在goland中下载用于与远程文件同步的工具，即SourceSynchronizer

![1575023355668](.\pic\1575023355668.png)

2，在远程服务端需要安装ftp服务并开启，注意503时修改vsfptd.conf 配置 pam_service_name=ftp 而不是vsftpd。不知道啥原因。

3，在goland  Tools--》SourceSyc配置响应的信息。并取一个名字，比如remoteServer

![1575023537579](.\pic\1575023537579.png)



4，选择需要同步的目录（package）右键弹出选择 project connection configration，来选择上面配置好的一个ftp服务。注意Root path，有时候同步的文件可能跑到其他目录去了，需要自行调整。



5，如果能够保证文件能够实时同步，就开始配置远程调试。



6，在远程服务端下载dlv工具，在同步的文件目录下开启dlv调式服务，执行命令 

dlv debug --headless --listen=:2345 --api-version=2 --accept-multiclient

这个在goland中也会看到，也可以配置其他的命令比如，go build -gcflags "all=-N -l" github.com/app/demo

但是我怎么样都成功不了。



7，在本地goland中配置 Host 和 Port 远程调试服务开启的ip和port，这个界面有一些note可以读一下。



![1575023930115](.\pic\1575023930115.png)



8，点击调式即可。如果结束调试后，再执行调式可能会报错。那是因为调试其实也启了一个进程，会一直运行着，需要人肉查看并终止这个进程，才可以进行下一次调试。按道理不应该这么繁琐的，但是还没找到其他办法。



9，文件同步后，自动重新启动dlv调试服务。这个就是用到go-reload.sh 脚本来监听gopath下面的文件变化，如果有变化，则重新编译文件，并重新开启dlv调试服务。感觉go-reload.sh很有用，可以自行修改，增加自己需要的功能。