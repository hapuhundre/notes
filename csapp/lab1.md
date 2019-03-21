# CS:APP Lab Notes 1

标签（空格分隔）： 计算机系统

---

[TOC]

## Lab 1:Data Lab 数值操作
### 1.1 位运算
**要求** 
在bits.c文件中编写以下函数，不能使用循环和条件语句，运算符只能使用```! ~ & ^ | ^ + << >>```

**代码与注释**
``` c
#include <stdio.h>

int bitXor(int x, int y) {
	return ~(x&y) & ~(~x & ~y);
}

int tmin(void) {
	return 1 << 31;
}

int isTmax(int x) {

	return !(x + 1 + x + 1) & !!(x+1);
}


int negate(int x) {
	return ~x + 1;
}

int allOddBits(int x) {
	return !(~(x | 0x55555555));
}


/*
* isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
*   Example: isAsciiDigit(0x35) = 1.
*            isAsciiDigit(0x3a) = 0.
*            isAsciiDigit(0x05) = 0.
*   Legal ops: ! ~ & ^ | + << >>
*   Max ops: 15
*   Rating: 3
*/
int isAsciiDigit(int x) {
	return (!((x+~0x30+1)>>31)) & ((x+~0x39+1)>>31);
}


/*
* conditional - same as x ? y : z
*   Example: conditional(2,4,5) = 4
*   Legal ops: ! ~ & ^ | + << >>
*   Max ops: 16
*   Rating: 3
*/
int conditional(int x, int y, int z) {
	return ((!x+~0)&y) | ((~!x+1)&z);
}

/*
* isLessOrEqual - if x <= y  then return 1, else return 0
*   Example: isLessOrEqual(4,5) = 1.
*   Legal ops: ! ~ & ^ | + << >>
*   Max ops: 24
*   Rating: 3
*/
// y-x然后看符号位
int isLessOrEqual(int x, int y) {
	return !((y+~x+1)>>31);
}

/*
* logicalNeg - implement the ! operator, using all of
*              the legal operators except !
*   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
*   Legal ops: ~ & ^ | + << >>
*   Max ops: 12
*   Rating: 4
*/
int logicalNeg(int x) {
	return x&1;
}

//---------------------------Test---------------------------
void Test(const char* testname, int output, int expect){
	printf("%s :", testname);
	if (output == expect)
		printf(" Passed.\n");
	else
		printf(" Failed.\n");
}

void Test1(){
	char* testname = "Test1";
	int output = logicalNeg(3);
	int expect = 0;
	Test(testname, output, expect);
}

void Test2(){
	char* testname = "Test2";
	int output = logicalNeg(0);
	int expect = 1;
	Test(testname, output, expect);
}

void Test3(){
	char* testname = "Test3";
	int output = logicalNeg(-1);
	int expect = 0;
	Test(testname, output, expect);
}

int main(){
	Test1();
	Test2();
	Test3();

	return 0;
}
```


***参考资料***
1. [深入理解计算机系统（CSAPP）实验二 datalab-handout][1]
2. [CSAPP深入理解计算机系统实验datalab解析][2]
3. [【不周山之读厚 CSAPP】I Data Lab][3]
  


  [1]: http://www.cnblogs.com/tenlee/p/4951639.html
  [2]: http://blog.csdn.net/u014124795/article/details/38471797
  [3]: http://wdxtub.com/2016/04/16/thick-csapp-lab-1/
