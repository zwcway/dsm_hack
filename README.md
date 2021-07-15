# dsm_hack
synology DSM 6.1.7-15284 hack


### libsynoindex.so
群晖挂载另一台NAS的多媒体目录，比如照片。
```
mount -t nfs nas:/volume2/photo /volume1/photo
```
但是DSM 6.1版本无法索引远程文件夹，因此通过破解动态库，跳过远程文件夹的检查。libsynoindex.so 中 有两个函数
- IndexIsPathRemoteMount	000000000000A6D0	
- IndexIsPathRemoteOrImageMount	000000000000A740	

分别修改为返回 0 即可。
```
# 备份原始文件
cp /usr/lib/libsynoindex.so /usr/lib/libsynoindex.so.bak
# 替换库文件
cp -f xxxx/libsynoindex.so /usr/lib/libsynoindex.so
# 重启索引服务
synoservice --restart synoindexd
# 查看索引服务是否启动
synoindex_mgr --status
```
### libsynosdk.so.6
群晖挂载另一台NAS的多媒体目录，VideoStation 和 AudioStation 无法通过破解libsynoindex.so解决索引问题。可以直接破解 libsynosdk.so.6。只是会有点小问题，其他需要判断远程挂载的软件将会失效（比如 FileStation）。

libsynosdk.so.6 中 有一个函数
- SYNOFSIsRemoteFS	0000000000024225	

修改为返回 0 即可。
```
# 备份原始文件
cp /usr/lib/libsynosdk.so.6 /usr/lib/libsynosdk.so.6.bak
# 替换库文件
cp -f xxxx/libsynosdk.so.6 /usr/lib/libsynosdk.so.6
# 重启DSM
reboot
```
