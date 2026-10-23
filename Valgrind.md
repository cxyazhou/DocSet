# 交叉编译及运行
## 源码下载后解压后进入源码目录
valgrind-3.17.0.tar.bz2
## 配置及编译
1. apt install automake
2. ./autogen.sh
3. NT98539A
```
./configure --host=aarch64-linux-gnu CC=/opt/ivot/aarch64-ca53-linux-gnueabihf-10.4.0/bin/aarch64-ca53-linux-gnu-gcc CPP=/opt/ivot/aarch64-ca53-linux-gnueabihf-10.4.0/bin/aarch64-ca53-linux-gnu-cpp CXX=/opt/ivot/aarch64-ca53-linux-gnueabihf-10.4.0/bin/aarch64-ca53-linux-gnu-g++ --prefix=/opt/valgrind_install
```
4. make
5. make install
## 运行
1. 设备需要未strip的ld动态库，一般在交叉编译链下有，把该动态库替换掉设备里strip过的库。
```
/opt/ivot/aarch64-ca53-linux-gnueabihf-10.4.0/aarch64-ca53-linux-gnu/sysroot/lib/ld-linux-aarch64.so.1
```
2. 待定位的可执行程序可以用Release版本，但是不可以strip。
3. 运行valgrind定位内存问题
```
./valgrind --tool=memcheck --gen-suppressions=all --track-origins=yes ./smart_sdk_demo ./auth ./test.avi 3 2>&1
```
4. 日志分析
```
Invalid write of size 1
   at strcat
   by SmartGlobalTypeCheckLicense
 Address 0x... is 0 bytes after a block of size 39 alloc'd
   at malloc
   by SmartGlobalTypeCheckLicense
```