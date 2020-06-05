# using

## gcc_ using

> 1.  -E 预处理: 把 .h .c 展开形成一个文件. 宏定义直接替换头文件 生成.i文件
>
>    > ~~~Linux
>    > gcc -E hello.c -o hello.i
>    > ~~~
>
> 2. -S 编译    将.i 文件生成一个汇编文件 .s
>
>    **补充:**
>
>    ​		.s 汇编语言源程序;  操作: 汇编 
>
>    ​		.S汇编语言源程序;  操作: 预处理 + 汇编
>
>    > ~~~Linux
>    > gcc -S hello.i -o hello.S
>    > ~~~
>
> 3. 汇编 将汇编文件经过汇编,将.S文件生成 .o .obj文件(微软的编译器文件为.obj, gcc为.o)
>
>    > ~~~linux
>    > gcc -c hello.S -o hello.o
>    > ~~~
>
> 4. 链接  将各个模块的.o文件链接可执行文件
>
>    > ~~~linux
>    > gcc hello.o -o hello
>    > ~~~
>
>    
>
>    ![GCC编译流程](D:\我的坚果云\考研笔记\专业课\1-1P921162IV92.jpg)
>
>    
>
>    

## Makefile_using

> Makefile C/C++ 必须要使用的编译脚本
>
> 1. 第一层: 显示文件
>
>    > 目标文件:依赖文件
>    >
>    > [TAB] 指令
>    >
>    > ***第一个目标文件就是最终文件!!!   递归***
>    >
>    > 伪目标: .PHONY:
>    >
>    > 
>    >~~~makefile
>    >hello:hello.i
>    > 	gcc hello.o -o hello
>    > 	
>    > hello.o:hello.s
>    > 	gcc -c hello.S -o hello.o
>    > 	
>    > hello.s:hello.i
>    > 	gcc -S hello.i -o hello.s
>    > 	
>    > hello.i:hello.c
>    > 	gcc -E hello.c -o hello.o
>    > 
>    > .PHONY:
>    >  clear:
>    > 	rm -rf hello.o hello.S hello.i
>    > clearall:
>    > 	rm -rf hello.o hello.S hello.i hello
>    > ~~~
>    > 
>    
> 2. 第二层:变量       =(替换)     += (追加)     :=(恒等于)
>
>    > 使用变量 $(变量名)  替换
>    >
>    > ~~~~makefile
>    > #将circle.h circle.c main.h main.c 生成可执行文件
>    > 
>    > text: circle.o main.o
>    > 	gcc circle.o main.o -o text
>    > 
>    > circle.o:circle.c
>    > 	gcc -c circle.c -o circle.o
>    > 
>    > circle.o:circle.c
>    > 	gcc -c circle.c -o circle.o
>    > 	
>    > .PHONY:
>    > clear:
>    > 	rm -rf circle.o main.o 
>    > clearall:
>    > 	rm -rf circle.o main.o text	
>    > ~~~~
>    >
>    > 使用变量
>    >
>    > ~~~makefile
>    > # 使用变量替换
>    > TAR = text
>    > OBJ = circle.o main.o
>    > cc := gcc
>    > 
>    > $(TAR):$(OBJ)
>    > 	$(cc) $(OBJ) -o $(TAR)
>    > 	
>    > circle.o:ciecle.c
>    > 	$(cc) -c circle.c -o circle.o
>    > 
>    > main.o:main.c
>    > 	$(cc) -c main.c -o main.o
>    >  
>    > .PHONY:
>    > clear:
>    > 	rm -rf circle.o main.o 
>    > clearall:
>    > 	rm -rf circle.o main.o text	
>    > ~~~
>
> 3. 第三层:隐含规则   %.c   %.o  任意的.c .o文件      *.c   *.o   所有的.o .c文件
>
>    > ~~~makefile
>    > # 使用隐含规则
>    > 
>    > Tar = text
>    > Obj = circle.o main.o
>    > cc := gcc
>    > 
>    > $(Tar):$(Obj)
>    > 	$(cc) $(Obj) -o $(Tar)
>    > 
>    > %.o:%.c
>    > 	$(cc) -c %.c -o %.o
>    > 	
>    > .PHONY:
>    > clear:
>    > 	rm -rf circle.o main.o 
>    > clearall:
>    > 	rm -rf circle.o main.o text	
>    > ~~~
>
> 4. 第四层:通配符  $^ 所有的依赖文件     $@ 所有的目标文件     $<  所有的依赖文件的第一个文件
>
>    > ~~~makefile
>    > #使用通配符
>    > 
>    > Tar = text
>    > Obj = circle.o main.o
>    > cc := gcc
>    > Remove = rm -rf
>    > 
>    > $(Tar):$(Obj)
>    > 	$(cc) $(Obj) -o $@
>    > %.o:%.c
>    > 	$(cc) -c $^ -o $@
>    > 	
>    > .PHONY:
>    > clean:
>    > 	$(Remove) $(Obj)
>    > cleanall:
>    > 	$(Remove) $(Obj) $(Tar)
>    > ~~~
>
> 5. 第五层
>
>    >使用函数  
>    >
>    >[https://wiki.ubuntu.org.cn/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E5%86%99Makefile:%E4%BD%BF%E7%94%A8%E5%87%BD%E6%95%B0](https://wiki.ubuntu.org.cn/跟我一起写Makefile:使用函数)
>









