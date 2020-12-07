### 1 GCC 特性

编译过程分为 4 个阶段：

- 预处理：`gcc -E hello.c -o hello.cpp`
- 适当编译：
- 汇编
- 链接

**一次性编译命令：`gcc hello.c -o hello`**

### 2 GCC 常用命令选项

| 选项         | 说明                                      |
| ------------ | ----------------------------------------- |
| -o FILE      | 指定输出文件名，默认为 a.out              |
| -c           | 只编译不链接                              |
| -I DIRNAME   | 将 DIRNAME 加入到包含文件的搜索目录列表中 |
| -L DIRNAME   | 将 DIRNAME 加入到库文件的搜索目录列表     |
| -g           | 在可执行程序中包含标准调试信息            |
| -Wall        | 允许发出 GCC 能提供的所有有用的警告       |
| -O、-O2、-O3 | 优化编译过的代码，测试决定优化选项        |
| -lFOO        | 链接名为libFOO的函数库                    |

### 3 makefile

- 编写规则

  ```sh
  target ： dependency [dependency [...]]
  	command
   	command
  	[...]
          
  # dependency:创建 target 时需要输入的一个或多个文件的列表
  # command：创建 target 文件所需要的步骤
  
  howdy: howdy.o helper.o helper.h
  	gcc howdy.o helper.o -o howdy
  helper.o: helper.c helper.h
  	gcc -c helper.c
  howdy.o: howdy.c
  	gcc -c howdy.c
  all: howdy hello,o
  clean:
  	rm howdy hello *.o
  ```

  指定的  target 文件 make 找不到，就会自动生成

  如果 target 文件存在，make 会比较 target 文件和 dependency 文件的时间戳，如果有 dependency 比 target 新，就重新编译生成 target

- 伪目标

  伪目标不对应实际的文件，例如上述的 clean，规定了 make 应该执行的命令

- 变量

  ```sh
  VARNAME=some_text [...]
  # 把变量用括号括起来，并在前面加上 $ 符号，就可以引用变量的值
  $(VARNAME)
  ```

  ```sh
  OBJS = howdy.o helper.o
  HDRS = helper.h
  howdy: $(OBJS) $(HDRS)
  	gcc $(OBJS) -o howdy
  helper.o: helper.c $(HDRS)
  	gcc -c helper.c
  howdy.o: howdy.c
  	gcc -c howdy.c
  hello: hello.c
  	gcc hello.c -o hello
  all: howdy hello
  clean:
  	rm howdy hello *.o
  ```

  

  