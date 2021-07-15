# dsm_hack
synology DSM 6.1.7-15284 hack

### libsynoindex.so
群晖挂载另一台NAS的照片目录
```
mount -t nfs nas:/volume2/photo /volume1/photo
```
但是DSM 6.1版本无法索引远程文件夹，因此通过破解动态库，跳过远程文件夹的检查
libsynoindex.so 中 有两个函数
- IndexIsPathRemoteMount	000000000000A6D0	
- IndexIsPathRemoteOrImageMount	000000000000A740	
分别修改为返回 0 即可
