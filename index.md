今天测试代码过程中无意间发现，代码如下：　　
复制代码

 1 #include <stdio.h>
 2 #include <string.h>
 3 #include <stdlib.h>
 4 #define SIZE 2
 5
 6 int main(int argc, char **argv)
 7 {
 8     char *s1 = "Hello, ";
 9     char *s2 = "world!";
10     char *s3 = (char*)malloc(SIZE * sizeof(char));
11
12     printf("s1: %s\n", s1);
13     printf("s2: %s\n", s2);
14     strcpy(s3, s1);
15     strcat(s3, s2);
16     printf("s1: %s\n", s1);
17     printf("s2: %s\n", s2);
18     printf("s3: %s\n", s3);
19
20     return 0;
21 }

复制代码

　　代码很简单，就是测试字符串的复制和连接，但是，无论第4行的代码中SIZE是多大，都可以出现如下结果：

s1: Hello,
s2: world!
s1: Hello,
s2: world!
s3: Hello, world!

　　感觉结果有点诡异，不是应该对指针的大小有要求的吗，怎么把指针外的东西也能输出呢。

　　以后分配内存就可以更加简单了，不需要设置大小了，自动识别大小的内存分配，代码如下：

　　
复制代码

 1 #include <stdio.h>
 2 #include <string.h>
 3 #include <stdlib.h>
 4
 5 void getName(char *name){
 6     printf("the name: %s\n", name);
 7
 8     return;
 9 }
10
11 char* setName(){
12     char *name = (char*)malloc(sizeof(char));
13     printf("please input your name: ");
14     scanf("%s", name);
15
16     return name;
17 }
18
19 int main(int argc, char **argv)
20 {
21     char *name = NULL;
22     name = setName();
23     getName(name);
24
25     return 0;
26 }

复制代码

　　代码结果为：

please input your name: zhangsan
the name: zhangsan

　　通过中间变量name指针，简单实现了java语言中的setter和getter，只不过是实现的思路稍微有点点变化而已。

　　代码如下：　　
复制代码

 1 #include <stdio.h>
 2
 3 int main(int argc, char **argv)
 4 {
 5     char cArray[] = "Hello, JJu!";
 6     //char cArray[20];
 7     //cArray = "Hello, JJu!";
 8     char *ptrArray= "Hello, World!";
 9　　　//*(ptrArray + 4) = 'e';
10     printf("cArray: %s\n", cArray);
11     printf("*ptrArray: %s\n", ptrArray);
12     printf("char size: %d\n", sizeof(char));
13     printf("'a' size: %d\n", sizeof('a'));
14
15     return 0;
16 }

复制代码

　　这段代码有两怪：

　　1、很多人看到这段代码的第一感觉就是第5行与第6行和第7行的效果是一样的，你确定吗，不信运行下就知道了，是不一样的。

　　2、第12行的代码的结果是1, 第13行代码的结果是4,a是char，但是char不是a呀；

　　3、第9行代码，如果不注释掉，程序就不能编译通过，说明数组无论是用数组本身表示，还是使用指针表示都是不能更改的；
