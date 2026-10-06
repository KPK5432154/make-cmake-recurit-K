# note  

## 环境  
```bash
sudo apt update
sudo apt install build-essential cmake
```
目的是：
1.  以管理员权限查询包是否可以更新  
2.  以管理员权限下载包

## Task 02  
Makefile 就像是倒推，我要得到的东西依赖于什么东西，需要什么指令，从可执行程序一步步推回.c文件  
```  
make
Makefile:6: *** 缺失分隔符。 停止。  
```  
命令前必须是**tab**  
---

```  
# TODO: compile src/main.c
gcc -c src/main.c -o main.o
src/main.c:2:10: fatal error: calculator.h: 没有那个文件或目录
    2 | #include "calculator.h"
      |          ^~~~~~~~~~~~~~
compilation terminated.
make: *** [Makefile:9：main.o] 错误 1  
```  
`CFLAGS = -Wall -Wextra -Iinclude`意思是开启警告以及添加“include”作为路径
---  
.h文件已经在依赖路径中  
--- 
```
make
# TODO: compile src/main.c
gcc -Wall -Wextra -Iinclude -c src/main.c -o main.o
# TODO: compile src/calculator.c
gcc -Wall -Wextra -Iinclude -c src/calculator.c -o calculator.o
# TODO: compile src/logger.c
gcc -Wall -Wextra -Iinclude -c src/logger.c  -o logger.o
# TODO: link object files into the calculator executable
gcc -Wall -Wextra -Iinclude main.o calculator.o logger.o -o calculator 

touch src/calculator.c
make
# TODO: compile src/calculator.c
gcc -Wall -Wextra -Iinclude -c src/calculator.c -o calculator.o
# TODO: link object files into the calculator executable
gcc -Wall -Wextra -Iinclude main.o calculator.o logger.o -o calculator 
```  
touch的意思是更新文件时间，在编译时检测到calculator.c文件更新，就只会重新编译与它相关的指令  
## Task 03  
```cmake
add_executable(...) 
``` 
```
add_executable(calculator
    src/main.c
    src/calculator.c
    src/logger.c
)
```  
生成一个 `calculator`的程序 由src/main.c src/calculator.c src/logger.c组成  
  
关于`target_include_directions(...)`
``` 
target_include_directories(calculator PRIVATE include)  
```  
目标程序**calculator** 需要的头文件在include中  

关于  
```  
cmake -S . -B build
cmake --build build
./build/calculator  
```  
意思是  
1. 程序源码在当前文件夹，构建相关关系并且放在build文件夹中  
2. 在放了构建关系的文件夹中开始构建，并且进行编译、链接
3. 运行
## Task 04 
make这些东西都是为了更加高效和自动化，就像是有些宏定义和变量也是为了便于修改一样