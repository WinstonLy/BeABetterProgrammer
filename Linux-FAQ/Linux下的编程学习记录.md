## int main()

1. Linux下的进程运行完毕都会有一个返回值，范围[0~255]，int main() 就是为了对应这个返回值。

2. 使用int main()主要是可以给操作系统返回一个值，让操作系统明白这个程序执行的状态，比如执行这个程序后下一步可能要根据这个返回值做分支处理，如果是void的话就是一个哑巴程序，返回值不确定，异常退出和正常推出无法区别，的确移植性很差。`int mian`是C++标准的写法。

3. main函数可以看成是操作系统期待的一个入口函数，操作系统认为这个函数应该是返回一个int返回值的，并且带有三个参数，它的原型应该是：

`int main(int argc, char *argv[]){}`

`argc`:命令行传给main函数的参数总个数

`*argv`字符串数组，第0个参数是程序的全名，以后的参数是命令行跟的用户输入的参数,这些参数在main函数中起作用。

## `Gcc `编译命令

```shell
**查看g++版本**      
	`g++ -v` 

**编译单个文件**      
	`g++ hello.cpp -o hello` 

**编译多个文件**       
	如有头文件（header.h）,函数定义文件（func.cpp）,主函数文件（main.cpp）

	 `g++ -c func.cpp` `g++ -c main.cpp` 

	编译完成生成两个文件：func.o main.o 通过链接可得到最终的可执行文件 `g++ main.o func.o -o test

	`g++ -c func.cpp -I ../include` `g++ func.o main.o -o ../testOutput/test`

**查看g++版本**
`g++ -v`
**编译单个文件**
`g++ hello.cpp -o hello`
**编译多个文件**
如有头文件（header.h）,函数定义文件（func.cpp）,主函数文件（main.cpp）
`g++ -c func.cpp`
`g++ -c main.cpp`
编译完成生成两个文件：func.o main.o
通过链接可得到最终的可执行文件
`g++ main.o func.o -o test


`g++ -c func.cpp -I ../include`
`g++ func.o main.o -o ../testOutput/test`
```

## Makefile

学习链接如下：

[Makefile学习](https://seisman.github.io/how-to-write-makefile/introduction.html)



### 程序的编译和链接

- 编译的时候首先将原文件编译成中间代码（在Windows下就是` .obj`文件，在Linux下是` .o文`件，即Object File）这个动作叫做编译（compile），一般来说每个源文件都应该对应一个中间目标文件。
- 链接时主要是链接函数和全局变量，所以要用这些中间文件来链接应用程序。由于中间目标文件太多，需要给这些文件打包，这种包在Windows下叫做库文件（Library File），也就是` .lib`文件，在Linux下是Archive File，也就是 `.a`文件。
- 总结，源文件首先生成中间文件，然后由中间文件生成执行文件。

### Makefile介绍

make命令执行时，需要一个 Makefile 文件，以告诉make命令需要怎么样的去编译和链接程序。编写Makefile的原则：

>1）如果这个工程没有编译过，那么我们的所有C文件都要编译并被链接。
>2）如果这个工程的某几个C文件被修改，那么我们只编译被修改的C文件，并链接目标程序。
>3）如果这个工程的头文件被改变了，那么我们需要编译引用了这几个头文件的C文件，并链接目标程序。

#### 1 Makefile的规则

```shell
target  ... : prerequisites ...
		command
		...
		...
` target ` 也就是一个目标文件，可以使Object File，也可以是执行文件
` prerequisites ` 就是要生成target所需要的文件或者目标 
` command `  就是make需要执行的命令(任意的shell命令)  
`这是一个依赖关系，target依赖于prerequisites中文件，其生成规则定义在command中。也就是说，如果     	    prerequisites中有一个及以上的文件比target文件新的话，command定义的命令就会被执行。`
```

示例

```shell
edit : main.o kbd.o command.o display.o \
    	insert.o search.o files.o utils.o
	gcc -o edit main.o kbd.o command.o display.o \
    	insert.o search.o files.o utils.o

main.o : main.c defs.h
	gcc -c main.c
kbd.o : kbd.c defs.h command.h
	gcc -c kbd.c
command.o : command.c defs.h command.h
	gcc -c command.c
display.o : display.c defs.h buffer.h
	gcc -c display.c
insert.o : insert.c defs.h buffer.h
	gcc -c insert.c
search.o : search.c defs.h buffer.h
	gcc -c search.c
files.o : files.c defs.h buffer.h command.h
	gcc -c files.c
utils.o : utils.c defs.h
	gcc -c utils.c
clean :
	rm edit main.o kbd.o command.o display.o \
		insert.o search.o files.o utils.o
               
 # 定义好依赖文件后，后续的一行定义系统操作文件命令，一定要以一个Tab键作为开头 
 # `\`是换行的意思
 # 将这个文件保存为名字为“makefile”或者“Makefile”的文件中，然后在该目录下直接输入make就可以生成执行文件	edit，输入make clean就可以删除执行文件和中间目标文件
```

#### 2 make的工作过程

在上面的例子中，我们默认输入`make`命令，那么：

1. make会在当前目录下找名字叫“Makefile”或“makefile”的文件。
2. 如果找到，它会找文件中的第一个目标文件（target），在上面的例子中，他会找到“edit”这个 文件，并把这个文件作为最终的目标文件。
3. 如果edit文件不存在，或是edit所依赖的后面的 `.o` 文件的文件修改时间要比 `edit` 这个 文件新，那么，他就会执行后面所定义的命令来生成 `edit` 这个文件。
4. 如果 `edit` 所依赖的 `.o` 文件也不存在，那么make会在当前文件中找目标为 `.o` 文件 的依赖性，如果找到则再根据那一个规则生成 `.o` 文件。（这有点像一个堆栈的过程）
5. 当然，你的C文件和H文件是存在的啦，于是make会生成 `.o` 文件，然后再用 `.o` 文件生 成make的终极任务，也就是执行文件 `edit` 了。



#### 3 makefile中使用变量

` .o`文件总是可能被多次重复使用，为了makefile的易维护，在makefile中我们可以使用变量。makefile的变量也 就是一个字符串，理解成C语言中的宏可能会更好。

例如声明一个变量，叫做`objects`，`OBJECTS`，`objs`，`OBJS`或是`OBJ`，在makefile的一开始这样定义：

```shell
objects = main.o kbd.o command.o dispaly.o 
	\insert.o search.o files.o utils.o
```

然后我们可以在makefile中以<font color=#0099ff>`$(objects)`</font>的方式来使用这个变量了，于是makefile可以改为下面这个样子：

```shell
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    gcc -o edit $(objects)
main.o : main.c defs.h
    gcc -c main.c
kbd.o : kbd.c defs.h command.h
    gcc -c kbd.c
command.o : command.c defs.h command.h
    gcc -c command.c
display.o : display.c defs.h buffer.h
    gcc -c display.c
insert.o : insert.c defs.h buffer.h
    gcc -c insert.c
search.o : search.c defs.h buffer.h
    gcc -c search.c
files.o : files.c defs.h buffer.h command.h
    gcc -c files.c
utils.o : utils.c defs.h
    gcc -c utils.c
clean :
    rm edit $(objects)
    
# 如果有新的 .o文件加入，简单地修改 objects 就行了
```

#### 4 make的自动推导

make会自动识别并推导命令，例如看到一个` .o`文件，就会自动的把` .c`文件加在依赖关系中，并且`gcc -c .c`也会被推导出来。修改后的makefile如下：

```shell
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean
clean :
    rm edit $(objects)
```

#### 5 清除目标文件的规则

```shell
.PHONY : clean
clean :
    -rm edit $(objects)
```

`.PHONY` 表示 `clean` 是一个“伪目标”。而在 `rm` 命令前面加了一个小减号的 意思就是，也许某些文件出现问题，但不要管，继续做后面的事。当然， `clean` 的规则不要放在文件 的开头，不然，这就会变成make的默认目标，相信谁也不愿意这样。不成文的规矩是——“clean从来都是放 在文件的最后”。

**注：**

>1.  makefile中的注释用`#`字符
>
>2. 并且Makefile中的命令必须要以`Tab`键开始
>
>3. 一般讲Makefile的文件名命名为Makefile
>
>4. 如果命令太长，用反斜杠`\`作为换行符

#### 6 引用其他的Makefile

用`include`关键字可以把别的Makefile包含进来。

```shell
include <filename>
filename是当前操作系统shell的文件模式
```

### 书写命令

每条规则中的命令和操作系统Shell的命令行是一致的。make的命令默认是被 `/bin/sh`，`#` 是注释符。

#### 1 显示命令

用 `@` 字符在命令行前，那么， 这个命令将不被`make`显示出来。

```shell
@echo 正在编译xxx模块
输出：正在编译xxx模块......

echo 正在编译xxx模块
输出：echo 正在编译xxx模块......
	 正在编译xxx模块......
```

`make`执行时，带入`make`参数 `-n` 或 `--just-print` ，那么其只是显示命令，但不会执行命令。

`make`参数 `-s` 或 `--silent` 或 `--quiet` 则是全面禁止命令的显示。

#### 2 命令执行

当需要更新的时候就要进行命令执行，需要注意的是如果想让上一条命令的结果应用在下一条命令时，应该用分号分割这两条命令。示例如下：

```shell
exec:
    cd /home/hchen
    pwd
执行make exec的时候，cd没有起作用    
exec:
    cd /home/hchen; pwd
执行make exec的时候，cd起作用了，会打印处”/home/hchen"
```

#### 3 命令出错

可以在Makefile的命令行前加一个减号 `-`（在Tab键之后） ，标记为不管命令出不出错都认为是成功的，忽略命令的出错。

一个全局的办法是，给make加上 `-i` 或是 `--ignore-errors` 参数，那么，Makefile中 所有命令都会忽略错误。

### 使用变量

变量的命名字可以包含字符、数字，下划线（可以是数字开头），但不应该含有 `:` 、 `#` 、 `=` 或是空字符（空格、回车等）。

变量在声明时需要给予初值，而在使用时，需要给在变量名前加上 `$` 符号，但最好用小括号 `()` 或是大括号 `{}` 把变量给包括起来。如果你要使用真实的 `$` 字符，那么你需要用 `$$` 来表示。

```shell
objects = program.o foo.o utils.o
program : $(objects)
    cc -o program $(objects)

$(objects) : defs.h
```

#### 1 变量中的变量

第一种方式，也就是简单的使用 `=` 号，在 `=` 左侧是变量，右侧是变量的值，右侧变量的值 可以定义在文件的任何一处，也就是说，右侧中的变量不一定非要是已定义好的值，其也可以使用后面定义的值。

可以使用make中的另一种用变量来定义变量的方法。这种方法使用的是 `:=` 操作符。这种方法，前面的变量不能使用后面的变量，只能使用前面已定义好了的变量。