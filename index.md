今天测试代码过程中无意间发现，代码如下：
    

    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>
    #define SIZE 2
    int main(int argc, char **argv)
    {
        char *s1 = "Hello, ";
        char *s2 = "world!";
        char *s3 = (char*)malloc(SIZE * sizeof(char));
        printf("s1: %s\n", s1);
        printf("s2: %s\n", s2);
        strcpy(s3, s1);
        strcat(s3, s2);
        printf("s1: %s\n", s1);
        printf("s2: %s\n", s2);
        printf("s3: %s\n", s3);
    
    return 0;
    }

	代码很简单，就是测试字符串的复制和连接，但是，无论第4行的代码中SIZE是多大，都可以出现如下结果：
	s1: Hello,
	s2: world!
	s1: Hello,
	s2: world!
	s3: Hello, world!

感觉结果有点诡异，不是应该对指针的大小有要求的吗，怎么把指针外的东西也能输出呢。

　　以后分配内存就可以更加简单了，不需要设置大小了，自动识别大小的内存分配，代码如下：

	#include <stdio.h>
	#include <string.h>
	#include <stdlib.h>
	
	void getName(char *name){
		printf("the name: %s\n", name);
		
		return;
	}
	char* setName(){
		char *name = (char*)malloc(sizeof(char));
		printf("please input your name: ");
		scanf("%s", name);
	
		return name;
	}
	int main(int argc, char **argv)
	{
    	char *name = NULL;
    	name = setName();
    	getName(name);
    	
      	return 0;
	}

代码结果为：

	please input your name: zhangsan
	the name: zhangsan

	通过中间变量name指针，简单实现了java语言中的setter和getter，只不过是实现的思路稍微有点点变化而已。

　　代码如下：

	#include <stdio.h>
	int main(int argc, char **argv)
	{
    	char cArray[] = "Hello, JJu!";
    	//char cArray[20];
    	//cArray = "Hello, JJu!";
    	char *ptrArray= "Hello, World!";

    	printf("cArray: %s\n", cArray);
    	printf("*ptrArray: %s\n", ptrArray);
    	printf("char size: %d\n", sizeof(char));
    	printf("'a' size: %d\n", sizeof('a'));
    	
		return 0;
	}

这段代码有两怪：

	1、很多人看到这段代码的第一感觉就是第5行与第6行和第7行的效果是一样的，你确定吗，不信运行下就知道了，是不一样的。
	2、第12行的代码的结果是1, 第13行代码的结果是4,a是char，但是char不是a呀；
	3、第9行代码，如果不注释掉，程序就不能编译通过，说明数组无论是用数组本身表示，还是使用指针表示都是不能更改的；
